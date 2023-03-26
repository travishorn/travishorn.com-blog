---
title: "Access the browser’s query string as a JavaScript Object"
datePublished: Thu Sep 08 2016 22:01:01 GMT+0000 (Coordinated Universal Time)
cuid: ckrox7b0e0p7ffws1gb6ahcfg
slug: access-the-browsers-query-string-as-a-javascript-object-5bc058828241
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1627409765534/CMR8H17zH.jpeg
tags: javascript, browsers, web-development, frontend-development

---


I recently read a post on David Walsh’s blog called Get Query String Parameters with JavaScript. He provides a simple function for getting parts of the URL query string. However, he mentions:
> I’d love a more modern method of query string parameter value retrieval but we still only have one string property to pick apart.

I wrote a (very) small library to do just that. It provides a method to get the query string as a JavaScript object. If you just want to see the code or use the project, [head over to the GitHub repository](https://github.com/travishorn/qs). Below, I will explain how it works.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627409763354/cz-U31wiS.jpeg)

To start out with, it’s important to know that the query string can be accessed natively by using

```
window.location.search

'?post=1234&action=edit'
```


Ultimately, we want to be able to access it like this:

```
qs.get();

{
  post: "1234",
  action: "edit"
}
```


I always like to start with an [IIFE](http://benalman.com/news/2010/11/immediately-invoked-function-expression/) to keep my code contained. This also allows me to put my code in strict mode without affecting other included scripts.

```
(function () {
  'use strict';

});
```


In this library, I am going to be using [Array.isArray()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/isArray). It has pretty good browser support, but I’ll include a [polyfill](https://remysharp.com/2010/10/08/what-is-a-polyfill) anyway since it is so small.

```
(function () {
  'use strict';

  if (!Array.isArray) {
    Array.isArray = function (arg) {
      return Object.prototype.toString.call(arg) === '[object Array ]';
    };
  }

});
```


Now let’s start writing a main object that will be exported as a global. This will go inside the IIFE, underneath the isArray() polyfill.

```
var qs = {};
```


This object will include any methods, such as qs.get(), that we want to be a part of our library. This object should be a global in the browser. We can make it global by attaching it to the window object.

First, we’ll check that the window object is accessible, then we’ll make sure it doesn’t have a qs property already. Underneath the declaration of the qs object:

```
if (window) {
  if (!window.qs) {
    window.qs = qs;
  } else {
    throw new Error('Error bootstrapping qs: window.qs already set.');
  }
}
```


Now we can add our first (and only, for now) method to the qs object:

```
var qs = {
  get: function () {

  }
};
```


Inside this function, we’ll first capture the query string, then we’ll initialize an empty object that we’ll eventually fill with our properties and return.

```
var query = window.location.search;
var obj = {};
```


If there’s no query string, we can just return the empty object:

```
if (query === '') return obj;
```


If the query isn’t empty and we don’t return the empty object, we can use String.slice() to remove the starting question mark from the query string.

```
query = query.slice(1);
```


Now, we’ll split the string into key/value pairs. In the query string, these pairs are ampersand-separated.

```
query = query.split('&');
```


Now we’ll loop through our key/value pairs:

```
query.map(function (part) {

});
```


Inside this loop (for each key/value pair), let’s assign the key and value to some easy to access variables:

```
var key;
var value;

part = part.split('=');
key = part[0];
value = part[1];
```


If the key doesn’t exist in our object, yet, we can set it.

```
if (!obj[key]) {
  obj[key] = value;
} else {
  // Key already exists!
}
```


If it does already exist, that means there is already a value assigned to it. In this case, we need a way to show the current value(s) *and* the new value. An array is a great option here. If the current value isn’t already an array, we need to *make* it an array.

```
if (Array.isArray(obj[key])) {
  obj[key] = [obj[key]];
}
```


Once we are sure the value is an array, we can push the new value into that array.

```
obj[key].push(value);
```


After the loop, our object contains all the key/value pairs. So outside the loop, we can return it.

```
return obj;
```


That’s it. If you save the code as qs.js, you can include it on any HTML page:

```
<script src="qs.js"></script>
```


Once it’s included, you can call it like so:

```
qs.get();
```


If, for example, the URL is [https://example.com/?post=1234&action=edit](https://example.com/?post=1234&action=edit) the returned object will look like

```
{
  post: "1234",
  action: "edit"
}
```


If the URL is [https://example.com/?post=1234&post=4321](https://example.com/?post=1234&post=4321) the returned object will be

```
{
  post: ["1234", "4321"]
}
```


The fully commented code and minified version are available at the [qs GitHub repository](https://github.com/travishorn/qs). If you find any bugs or would like to add features, please open an issue and/or send a pull request.