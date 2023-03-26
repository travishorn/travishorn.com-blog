---
title: "Delaying forEach() Iterations"
datePublished: Thu Jul 04 2019 11:01:01 GMT+0000 (Coordinated Universal Time)
cuid: ckroxve2z0pf8fws11ll4fwft
slug: delaying-foreach-iterations-2ebd4b29ad30
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1627409500890/g2XMMqjMv.png
tags: javascript, web-development, functional-programming

---


I recently ran into the problem of having to loop over an array, but with a delay between iterations. This functionality isn’t built in to JavaScript and I couldn’t find any libraries that provided it out-of-the-box. So I decided to code my own solution. Keep reading and we’ll build it together.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627409482211/nxUNuTxxF.png)

I want to loop through an array of strings and display one string at a time on the page.

We’ll start with a basic HTML template.

```
<p id="output"></p>
```


First, let’s create a function which displays text in this `<p>`.

```
const output = document.querySelector("#output");
const display = s => output.innerText = s;
```


Text can be displayed on the page by calling `display("Hello World");`.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627409484360/3q0HD4m6y.png)

Next, let’s define an array of strings.

```
const names = [
  "Alberta",
  "Barry",
  "Charley",
  "Christopher",
  "Dianne",
  "Ellen",
  "Ethel",
  "James",
  "Jodee",
  "Joseph",
  "Lilia",
  "Mark",
  "Mary",
  "Merri",
  "Michael",
  "Mildred",
  "Randall",
  "Roy",
  "Thomas",
  "Venus"
];
```


To loop over this array and display each name, we can use [Array.prototype.forEach()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach).
> The `forEach()` method executes a provided function once for each array element.

We can loop over the array and call `display()` on each name.

```
names.forEach(display);
```


The problem is that the browser loops over the array so quickly, that only our last element is shown.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627409485823/yEh4QVJFk.png)

We could try slowing it down by adding a simple [setTimeout()](https://developer.mozilla.org/en-US/docs/Web/API/WindowOrWorkerGlobalScope/setTimeout).
> The `setTimeout()` method sets a timer which executes a function or specified piece of code once the timer expires.

```
names.forEach((name) => {
  setTimeout(() => {
    display(name);
  }, 1000);
});
```


But all that happens here is that *nothing* is displayed for 1000 milliseconds (1 second), then we see the last name again!

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627409488021/eVU_C_4iM.png)

But why?

`forEach()` is looping over our array as fast as possible and setting a 1 second timeout on each item. So they are all going to be executed *at the same time:* 1 second from now.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627409489689/o5Zie9onH.png)

What we really need is to set a 0 second timeout on the first item, a 1 second timeout on the second item, a 2 second timeout on the third item, and so on.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627409491180/W55mnOwEW.png)

To do this, re-write the loop like so.

```
names.forEach((name, i) => {
  setTimeout(() => {
    display(name);
  }, i * 1000);
});
```


You can see that instead of setting a static 1000 millisecond delay for *all* iterations like before, we now *multiply* the iteration index by 1000 milliseconds.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627409492874/c0YrXUn-0.png)

And it works!

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627409496708/UEQFMxNca.gif)

We’ve reached our goal so we *could* stop here. But we’re *really* smart so let’s make this more re-usable and functional by moving the function into a delayed-loop factory function.

Start with a blank function that accepts two arguments:

* another function

* a delay length

This function will return a function itself.

```
const delayLoop = (fn, delay) => {
  return 'some function';
};
```


Copy the existing function we just created earlier into this function as the return value.

```
const delayLoop = (fn, delay) => {
  return (name, i) => {
    setTimeout(() => {
      display(name);
    }, i * 1000);
  }
};
```


Instead of the hard-coded `display()` function call, let’s call whatever function was passed in.

```
const delayLoop = (fn, delay) => {
  return (name, i) => {
    setTimeout(() => {
      fn(name);
    }, i * 1000);
  }
};
```


Also instead of naming our passed-in argument `name` we can name it something more modular, like `x`.

```
const delayLoop = (fn, delay) => {
  return (x, i) => {
    setTimeout(() => {
      fn(x);
    }, i * 1000);
  }
};
```


Finally, instead of hard-coding the value `1000` for the delay length, we can use whatever delay was passed in.

```
const delayLoop = (fn, delay) => {
  return (x, i) => {
    setTimeout(() => {
      fn(x);
    }, i * delay);
  }
};
```


Our delayed loop function is done. We can pass it to any `forEach` loop like so. Remember to give it a reference to the `display` function, as well as an appropriate delay length like `1000`.

```
names.forEach(delayLoop(display, 1000));
```


All of the JavaScript together, looks like this:

```
const output = document.querySelector("#output");
const display = s => output.innerText = s;

const names = [
  "Alberta",
  "Barry",
  "Charley",
  "Christopher",
  "Dianne",
  "Ellen",
  "Ethel",
  "James",
  "Jodee",
  "Joseph",
  "Lilia",
  "Mark",
  "Mary",
  "Merri",
  "Michael",
  "Mildred",
  "Randall",
  "Roy",
  "Thomas",
  "Venus"
];

const delayLoop = (fn, delay) => {
  return (name, i) => {
    setTimeout(() => {
      display(name);
    }, i * 1000);
  }
};

names.forEach(delayLoop(display, 1000));
```


The end result is the same as before.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627409498105/uetasqs81.gif)

Now you can use `delayLoop()` on any `forEach()` loop. Simply pass the function to use and the amount of time — in milliseconds — to delay between each iteration.

You can see the all the code and a demo on CodePen below.

%[https://codepen.io/travishorn/pen/pBbrEL]
