---
title: "I started naming all my anonymous functions (and you should, too)"
datePublished: Thu Jan 25 2018 23:01:02 GMT+0000 (Coordinated Universal Time)
cuid: ckrox3try0p6ffws1gv6j60tn
slug: i-started-naming-all-my-anonymous-functions-and-you-should-too-67c960a0fcd3
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1627409790109/Qp2WdcB9V.jpeg
tags: javascript, web-development

---


JavaScript allows anonymous functions. Wikibooks describes them as…
> …a function that was declared without any named identifier to refer to it. As such, an anonymous function is usually not accessible after its initial creation.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627409788164/ok-F-5aAv.jpeg)

They are used all over the place in JS. Most of the time, you will see them as arguments to other functions.

For example, the `[setTimeout()`](https://developer.mozilla.org/en-US/docs/Web/API/WindowOrWorkerGlobalScope/setTimeout) function takes two arguments:

* a function to execute, and

* a number of milliseconds to wait before execution

Here’s a simple demonstration.

```
<!-- HTML -->

<div id="message">Wait...</div>


/* JavaScript */

const message = document.getElementById('message');

setTimeout(function () {
  message.textContent = 'Hello, world.';
}, 1000);
```


Upon load, the webpage displays “Wait…”. After one second, it displays “Hello, world.”

See it in action here.

%[https://codepen.io/travishorn/pen/PEmgzG]

See the function inside `setTimeout()`? It doesn’t have a name; it is *anonymous*.

Although the code works, what happens when we introduce a bug? Say we made a typo on the symbol `message`.

```
setTimeout(function () {
  mesage.textContent = 'Hello, world.';
}, 1000);
```


*Notice I forgot an “s” in “message”.*

%[https://codepen.io/travishorn/pen/vpmMXO]

The message on the page never changes. At this point, as a developer, you see that something is wrong and would probably be looking towards the [DevTools Console panel](https://developers.google.com/web/tools/chrome-devtools/console/) (In Chrome, you could press Ctrl+Shift+J or Cmd+Shift+J). There, you would indeed find an error:

```
Uncaught ReferenceError: mesage is not defined
    at app.js:3
```


DevTools is very nice in that it provides the type of error, the error message, the filename, and the line number. This is definitely enough to solve our simple problem.

But imagine for a minute that the issue was with a much larger application, with much more going on. Things like multiple files, build tools, minification, and concatenation all compound the complexity of fixing bugs. In these cases, **I like to give myself every advantage to find where the problem lies**.

Here’s where naming anonymous functions can be helpful.

While the error above is fairly descriptive, we can go further. Go ahead and name that anonymous function.

```
setTimeout(function **sayHello**() {
  mesage.textContent = 'Hello, world.';
}, 1000);
```


*To illustrate the point, I’m keeping the typo in for right now.*

Here’s the code now.

%[https://codepen.io/travishorn/pen/baWJgR]

Now look at the error:

```
Uncaught ReferenceError: mesage is not defined
    at sayHello (app.js:3)
```


It is *almost* identical. But with one big difference: **the name of the function that produced the error**.

In a large project, this extra piece of information could make it that much quicker to find and fix the bug you’re hunting.

### Bonus

At this point, you may as well consider cutting & pasting this named function outside the `setTimeout()` call.

```
function sayHello() {
  message.textContent = 'Hello, world.';
}

setTimeout(sayHello, 1000);
```


*I fixed the typo now, to demonstrate working code.*

%[https://codepen.io/travishorn/pen/jYmRBY]

There are a couple of advantages to this style:

1. It avoids [callback hell](http://callbackhell.com/). Your code becomes “flatter”. Imagine multiple callbacks and `setTimeout`s, all nested inside each other. Before you know it, you will be working with some seriously terrible indentation.

1. `sayHello()` can now be called from other places in your code, instead of being locked down inside this single `setTimeout`.

By the way, you can still use this style if you like or need to use [arrow functions](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions).

```
const sayHello = () => {
  message.textContent = 'Hello, world.';
}

setTimeout(sayHello, 1000);
```

%[https://codepen.io/travishorn/pen/ZvKZJM]

If you start naming your anonymous functions, I guarantee you’ll be thankful for it at some point down the road. I hope this little tip helps you next time you’re bug-fixing.