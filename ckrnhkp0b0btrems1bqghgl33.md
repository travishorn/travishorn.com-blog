---
title: "Should you name variables based on their content or their purpose?"
datePublished: Tue Jul 12 2016 22:01:02 GMT+0000 (Coordinated Universal Time)
cuid: ckrnhkp0b0btrems1bqghgl33
slug: should-you-name-variables-based-on-their-content-or-their-purpose-8975e4cec9f8
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1627410001240/_RE1Zhn_N.jpeg
tags: javascript, programing

---


Before I start, I should say that this is a question without a definite answer. You will not find one in this article. Instead, I’ll explore the subject and pose some questions at the end.

Imagine a scenario where you want to repeatedly call a function with a one minute delay between each call. Using JavaScript, window.setInterval() is a perfect solution. Its two required parameters, in order, are…

* a function to repeatedly call, and

* a number of milliseconds to delay between each call.

![Photo by [Drunk Photographer](https://cdn.hashnode.com/res/hashnode/image/upload/v1627409999520/w1aXEVNPY.html) under [CC BY-SA 2.0](https://creativecommons.org/licenses/by-sa/2.0/)](https://cdn-images-1.medium.com/max/7776/1*a_X0SMnFUjxUccz0hEqUtA.jpeg)*Photo by [Drunk Photographer](https://www.flickr.com/photos/ksyz/) under [CC BY-SA 2.0](https://creativecommons.org/licenses/by-sa/2.0/)*

For the sake of example let’s say we have already defined the function we want to call as someFunc(). Here is a simple way to write the code we want to execute:

```
let timer = window.setInterval(someFunc, 60000);
```


This will work, but the second parameter is a [magic number](https://en.wikipedia.org/wiki/Magic_number_(programming)#Unnamed_numerical_constants). Magic numbers occur when you use unnamed constant numbers directly in source code. According to some of the oldest rules in programming (and my opinion), they should be avoided because
> The use of unnamed magic numbers in code obscures the developers’ intent in choosing that number, increases opportunities for subtle errors and makes it more difficult for the program to be adapted and extended in the future.

If we replace this magic number with a variable that describes it or it’s purpose, those problems disappear. Here are two methods:

### Method #1

```
const ONE_MINUTE = 60 * 1000;
let timer = window.setInterval(someFunc, ONE_MINUTE);
```


This solves the problem of “What does 60000 mean in this context?” Even someone unfamiliar with window.setInterval() will probably understand that someFunc() will get called on a one minute interval.

With this method, the variable is named to describe its **content**.

### Method #2

```
const DELAY = 60 * 1000; // One minute
let timer = window.setInterval(someFunc, DELAY);
```


This also solves the magic number problem. Someone unfamiliar with window.setInterval() will definitely know that the second parameter is the delay between calls of someFunc(). However, to see what the actual delay is (in milliseconds), we have to look to the declaration. In this case, I feel like a nice comment about is still necessary to describe what is meant by 60 * 1000.

With this method, the variable is named to describe its **purpose**.

Which method do you prefer? Does it change depending on the context?