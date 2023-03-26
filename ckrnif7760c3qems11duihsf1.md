---
title: "Getting Browser Width & Height"
datePublished: Thu Aug 17 2017 22:01:01 GMT+0000 (Coordinated Universal Time)
cuid: ckrnif7760c3qems11duihsf1
slug: getting-browser-width-height-16ef95a9e444
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1627409858790/On4njuSeR.jpeg
tags: javascript, web-design, browsers

---


A while back, a user on Stack Overflow asked the question “How to get browser width using JavaScript code?” The accepted answer (from 2009) recommends using the width() method in jQuery, citing this article from howtocreate.co.uk as proof that it’s not easy to write the code yourself.

Eight years later, is this still the case? Is using jQuery still the best practice?

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627409856912/9Reff-9m6.jpeg)

There’s still no single reliable property to get browser width (or height) in all browsers. I believe jQuery is actually a great option if you are already using it on your site. If so, you can easily get the browser’s width like so:

```
$(window).width()
```


But if you’re not using jQuery already, you don’t need to include an 85 KB library just to use one function. It’s not as hard as you think to code up your own small function.

### Where to start?

There are so many properties in JavaScript that could possibly give you the browser’s width. The developers of jQuery have already thought about this and have had numerous discussions on how to solve this problem. In fact, [you can see some of this discussion right in their GitHub issues](https://github.com/jquery/jquery/issues?utf8=%E2%9C%93&q=is%3Aissue%20width). For that reason, let’s first look at how jQuery does it.

Through the beauty of open source code, we can look into [the jQuery source](https://github.com/jquery/jquery/tree/master/src). Specifically, there is a file called [dimensions.js](https://github.com/jquery/jquery/blob/master/src/dimensions.js). Around [line 36](https://github.com/jquery/jquery/blob/f71eeda0fac4ec1442e631e90ff0703a0fb4ac96/src/dimensions.js#L36) (in version 3.2.1), you can see where it actually determines the width. To simplify; it’s getting the greatest value of `scrollWidth`, `offsetWidth`, or `clientWidth` on both `document.body` and `document.documentElement`.

### Building our own

With that in mind, we can build our simple function using `[Math.max()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math/max). `Math.max()` is a function that takes zero or more arguments and returns the greatest of them.

```
const getWindowWidth = () => {
  return Math.max(
    document.body.scrollWidth,
    document.documentElement.scrollWidth,
    document.body.offsetWidth,
    document.documentElement.offsetWidth,
    document.documentElement.clientWidth
  );
};
```


And a similar function for height:

```
const getWindowHeight = () => {
  return Math.max(
    document.body.scrollHeight,
    document.documentElement.scrollHeight,
    document.body.offsetHeight,
    document.documentElement.offsetHeight,
    document.documentElement.clientHeight
  );
};
```


Or even a single function that returns both:

```
const getWindowDimensions = () => {
  const width = Math.max(
    document.body.scrollWidth,
    document.documentElement.scrollWidth,
    document.body.offsetWidth,
    document.documentElement.offsetWidth,
    document.documentElement.clientWidth
  );
  
  const height = Math.max(
    document.body.scrollHeight,
    document.documentElement.scrollHeight,
    document.body.offsetHeight,
    document.documentElement.offsetHeight,
    document.documentElement.clientHeight
  );
  
  return { width, height };
};
```


The `getWindowDimensions()` function above returns an object like this:

```
Object {
  width: 2198,
  height: 3589
}
```

%[https://codepen.io/travishorn/pen/brabWp]

This function should *at least* work across [browsers that are currently supported by jQuery](https://jquery.com/browser-support/). Which, at the time of this writing are…

* Chrome: (Current-1) and Current

* Edge: (Current-1) and Current

* Firefox: (Current-1) and Current

* Internet Explorer: 9+

* Safari: (Current -1) and Current

* Opera: Current

* Stock browser on Android 4.0+

* Safari on iOS 7+