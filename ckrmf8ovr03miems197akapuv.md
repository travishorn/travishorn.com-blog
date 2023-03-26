---
title: "Creating a system tray icon to display the current Ether-USD market price"
datePublished: Thu Aug 24 2017 22:01:01 GMT+0000 (Coordinated Universal Time)
cuid: ckrmf8ovr03miems197akapuv
slug: creating-a-system-tray-icon-to-display-the-current-ether-usd-market-price-404ee7df4242
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1627410392039/fB_xutVL7.jpeg
tags: javascript, electron, ethereum

---


I thought a fun side project would be creating a desktop app that runs in the system tray and displays the market price of ETH in USD when you hover over it. Follow along and I’ll show you how I built this simple desktop app.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627410387229/MALQqAVLt.jpeg)

### Just Install

If you would just like to run this app and not learn how it’s built, just follow these steps:

1. Install [git](https://git-scm.com/) and [Node.js](https://nodejs.org/)

1. Run `git clone [https://github.com/travishorn/ethtray`](https://github.com/travishorn/ethtray)

1. Run `npm install`

1. Run `npm start`

Look for the Ethereum icon in the system tray. Hover your mouse cursor over it to see the market price in USD. Right-click to bring up the context menu. Choose **Update** to request a new current price. Choose **Quit** to quit the application.

### Let’s Build It

Start off by creating a new directory for our project and initializing an npm module in it.

```
> mkdir ethtray
> cd ethtray
> npm init -y
```


We are going to utilize a few npm packages that others have created for us already. Namely:

* [Electron](https://www.npmjs.com/package/electron) — the `Menu` and `Tray` components will be very helpful in creating an app that runs in the system tray

* [Moment ](https://www.npmjs.com/package/moment)— great for formatting dates & times

* [Request](https://www.npmjs.com/package/requests) — the most popular package for making and handling HTTP requests

Install and save them in the npm package.

```
> npm install --save electron moment request
```


Our app is pretty small. We can fit it into a single file. Create a new file called `index.js`.

Inside this file, let’s set up the variables and packages we’ll need later on. Get an instance of the `app`, `Menu`, and `Tray` components from `Electron`. Then, pull in `request` and `moment`. We’ll also create a `tray` variable to hold our tray component. We want this variable to be at the top-level scope so we can reference it at any point of our app lifecycle.

```
const { app, Menu, Tray } = require('electron');
const request = require('request');
const moment = require('moment');

let tray = null;
```


When the script is run, Electron will start the `app`. We can listen for the `ready` event and instantiate the rest of the app.

```
app.on('ready', () => {
  // All the remaining code will go here
});
```


Once the app is ready, we can create our tray.

```
tray = new Tray('ethereum-logo.ico');
tray.setToolTip('Getting market price...');
tray.setContextMenu(contextMenu);
```


The first line creates the tray. `Tray`'s constructor accepts a string pointing to an icon file. I’m simply using the Ethereum logo for [my icon](https://github.com/travishorn/ethtray/raw/master/ethereum-logo.ico). Make sure it’s in the same directory as `index.js` or that you use the correct path when creating the tray.

The next line sets the tooltip. This is the text that will be shown to the user on mouseover. For now, it will just say “Getting market price…”. We’ll update it once the price has been gathered.

The last line sets the context menu. This is the menu that pops up when the user right-clicks on the icon. But we’re passing a variable that doesn’t exist, yet! Let’s create that variable. It should be an instance of `Electron.Menu`. It is necessary to place variable declarations *before* any portion of the code that uses them. This means that I’ll write the next few lines of code *above* `tray.setContextMenu(contextMenu).`

```
const contextMenu = Menu.buildFromTemplate([
  { label: 'Update', click: updatePrice },
  { label: 'Quit', click: () => { app.quit(); } },
]);
```


We’re using `Menu`'s `buildFromTemplate()` function to create the menu. It accepts an array of objects defining the menu items. You’ll notice two menu items: **Update** and **Quit**. **Quit** is pretty self-explanatory. It calls `app.quit()` on click which exits the app. **Update** calls `updatePrice()` on click, which we haven’t written, yet. Let’s write it now. Again, these lines should be written *above* where it’s called.

```
const updatePrice = () => {
  // Request price and set tooltip
};
```


Inside this function will be a single command: a request to [Coinbase’s API](https://developers.coinbase.com/) for the market price of ETH in USD. Note that Coinbase requires a `CB-VERSION` header that specifies the date you wrote your app. This ensures the API will always respond in a way your app expects.

```
request({
  url: 'https://api.coinbase.com/v2/prices/ETH-USD/spot',
  headers: { 'CB-VERSION': '2017-08-04' },
}, (err, res, body) = > {
  // Do something with the response
});
```


So how do we handle the response from Coinbase’s API? First we check for an error and notify our user. We could get detailed by looking at the `err` object and handling various errors in different ways. But for now I’ll just display a simple error message.

```
if (err) {
  tray.setTooltip('Error getting market price.');
} else {
  // No error. Price is in the response body
}
```


If there are no errors, we can go ahead and update the tooltip to show the price. The price can be found in the response body, which looks like this:

```
{ "data":
  {
    "amount": "300.71",
    "currency":"USD"
  }
}
```


But that’s only half the information we want to show the user. We also want to show the date and time that this price was fetched. Add these two lines to get both pieces of information:

```
const amount = JSON.parse(body).data.amount;
const timestamp = moment().format('YYYY-MM-DD HH:mm');
```


Now set the tooltip.

```
tray.setToolTip(`$${amount} as of ${timestamp}`);
```


So now we have a system tray icon that displays a menu when right-clicked. The menu can update the price and quit the application. The only thing left is to make sure the price is updated as soon as the application is ready. For that, you just have to call the `updatePrice()` function at the bottom of the `app.on('ready')` event.

```
updatePrice();
```


The whole app is 36 lines:

```
const { app, Menu, Tray } = require('electron');
const request = require('request');
const moment = require('moment');

let tray = null;

app.on('ready', () => {
  const updatePrice = () => {
    request({
      url: 'https://api.coinbase.com/v2/prices/ETH-USD/spot',
      headers: { 'CB-VERSION': '2017-08-04' },
    }, (err, res, body) => {
      if (err) {
        tray.setToolTip('Error getting market price.');
      } else {
        const amount = JSON.parse(body).data.amount;
        const timestamp = moment().format('YYYY-MM-DD HH:mm');

        tray.setToolTip(`$${amount} as of ${timestamp}`);
      }
    });
  };

  const contextMenu = Menu.buildFromTemplate([
    { label: 'Update', click: updatePrice },
    { label: 'Quit', click: () => { app.quit(); } },
  ]);

  tray = new Tray('ethereum-logo.ico');

  tray.setToolTip('Getting market price...');
  tray.setContextMenu(contextMenu);

  updatePrice();
});
```


The app can be ran by calling `node_modules\.bin\electron .` (The dot on the end is important).

We can shorten that by adding a script to `packages.json`:

```
"scripts": {
    "start": "electron ."
  },
```


With that in place, you can start the app by running `npm start`.

![When you hover, the price is shown.](https://cdn.hashnode.com/res/hashnode/image/upload/v1627410389104/GbsdIEdpd.png)*When you hover, the price is shown.*

![When you right-click, the menu appears.](https://cdn.hashnode.com/res/hashnode/image/upload/v1627410390469/5IYJt976Z.png)*When you right-click, the menu appears.*

All of the code (and more) is available in [this GitHub repository](https://github.com/travishorn/ethtray).