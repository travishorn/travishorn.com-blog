---
title: "Creating a Node.js proxy to secure your third-party API key"
datePublished: Fri Apr 01 2016 22:31:01 GMT+0000 (Coordinated Universal Time)
cuid: ckrowr93d0ok4ems18bhn7cyt
slug: creating-a-node-js-proxy-to-secure-your-third-party-api-key-e305f2c951da
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1627409839545/Hm4qzc__g.jpeg
tags: javascript, security, nodejs, apis

---


Today I am creating a mailing list signup form that will post data to a sheet on Fieldbook. We could give our form the API key but this is a terrible idea as it gives anyone who visits the form the key to do anything they want to your database (Fieldbook book).

Instead, we’ll create a proxy in Node.js in less than 50 lines of code.

![Photo by [Juan D Nella](https://cdn.hashnode.com/res/hashnode/image/upload/v1627409837686/-nGvO5Nke.html)](https://cdn-images-1.medium.com/max/7858/1*XviICXh4doDeG_ZyDB6Tkw.jpeg)*Photo by [Juan D Nella](https://unsplash.com/juandinella)*

First let’s set up the database. Log into Fieldbook and create a new book. In that book, make a sheet named **Contacts**. This sheet will contain these three columns

* **Contact ID** (I am just using Fieldbook’s built in numeric ID)

* **Email address**, and — just for the fun of it —

* **Signup date**.

Now, click on **API**, then **Manage API access**, then **Generate a new API key**. Make sure to copy down the **Username** and **Password**.

Now for the code. Let’s create a new directory and cd into it.

```
mkdir fbproxy
cd fbproxy
```


Next, let’s initialize our npm project.

```
npm init
```


Answer any prompts.

Now we can install dependencies.

```
npm install express body-parser requestify moment --save
```


The proxy itself is simple enough. In a new file called index.js, we’ll first put the parser in strict mode and require our modules.

```
'use strict';

var express = require('express');
var bodyParser = require('body-parser');
var requestify = require('requestify');
var moment = require('moment');
```


Now set up the constants.

```
var PORT = process.env.PORT || 3000;
var BASE_URL = 'https://api.fieldbook.com/v1/[your book ID]';
var API_OPTIONS = {
  headers: { accept: 'application/json' },
  auth: {
    username: process.env.FB_API_USERNAME,
    password: process.env.FB_API_PASSWORD,
  },
};

var app = express();
```


Since the form will be running from a separate service as our proxy, we’ll need to set up CORS. And since we’ll be accepting form data, we’ll set up the body-parser, too.

```
app.use(function(req, res, next) {
  res.header('Access-Control-Allow-Origin', '*');
  res.header('Access-Control-Allow-Headers', 'Origin, X-Requested-With, Content-Type, Accept');
  next();
});

app.use(bodyParser.urlencoded({ extended: true }));
```


Finally, we get to the meat of the proxy: a single route for handling POST requests from our form.

```
app.post('/contacts', function(req, res) {
  var URL = BASE_URL + '/contacts';

  var record = {
    email_address: req.body.emailAddress,
    signup_date: moment().format('M/D/YYYY');
  };

  requestify
    .post(URL, record, API_OPTIONS)
    .then(function(fbres) {
      res.json(fbres);
    })
    .catch(function(err) {
      res.json(err);
      // you should also probably set up some logging here.
    });
});
```


With everything else set up, we will simply tell our proxy to listen for incoming connections.

```
app.listen(PORT, function() {
  console.log('Fieldbook proxy listening on port %s.', PORT);
});
```


In order to run the proxy, we’ll set our environment variables. This will depend on your operating system. But if you’re on Windows like me, open the command prompt and run these commands:

```
SET PORT=3000
SET FB_API_USERNAME=[your API username]
SET FB_API_PASSWORD=[your API password]
```


And simply start the proxy:

```
node index.js
```


This proxy will accept form data with an emailAddress key, add the sign up date, and post it to your Fieldbook sheet securely. However, it’s worth mentioning that, as it is, this proxy accepts form data from any origin.

[Read part two](https://travishorn.com/creating-a-front-end-for-working-with-your-api-proxy-7843670c6022#.3uax22l00) to see how to write up a quick front-end form for posting data to this proxy. The code for both this proxy and the front-end form is located in [this GitHub repository](https://github.com/travishorn/fieldbook-proxy-example/).