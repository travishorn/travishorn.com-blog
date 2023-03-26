---
title: "Shorten functions with ES6 features"
datePublished: Thu Jan 03 2019 22:01:02 GMT+0000 (Coordinated Universal Time)
cuid: ckroxoyly0pdsfws12a5ofgst
slug: shorten-functions-with-es6-features-ffd5e158c58a
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1627409595215/QxgJH1sCE.png
tags: javascript, web-development, es6

---


ES6 implements features which use a more expressive closure syntax and more intuitive expression interpolation that can shorten your functions.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627409593285/LeyQf446A.png)

Consider this function.

```
function sayHello(name) {
  return 'Hello, ' + name + '.';
}
```


Using [template literals string interpolation](http://es6-features.org/#StringInterpolation), we can re-write the function like so.

```
functon sayHello(name) {
  return `Hello, ${name}.`;
}
```


Notice the backticks instead of quotes around the string.

We can further shorten it using [arrow functions](http://es6-features.org/#ExpressionBodies).

```
var sayHello = name => `Hello, ${name}.`;
```


Using this syntax, the function is shortened drastically, without losing any code clarity.

The function above looks nice, but there’s one last change I would personally make to make the code even easier to reason about.

```
const sayHello = name => `Hello, ${name}.`;
```


I swapped `var` for `const`. Constants cannot be re-assigned, so this function will always do the same thing no matter when it’s called during run-time.