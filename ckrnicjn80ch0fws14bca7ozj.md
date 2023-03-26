---
title: "Building a Random-Within-Range Function"
datePublished: Tue Aug 09 2016 22:01:03 GMT+0000 (Coordinated Universal Time)
cuid: ckrnicjn80ch0fws14bca7ozj
slug: building-a-random-within-range-function-9b9037ec2c97
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1627409874343/TnQRM7nAb.jpeg
tags: javascript

---


Let’s walk through building a function in JavaScript that returns a random number within a given range.

![Photo by [Don DeBold](https://cdn.hashnode.com/res/hashnode/image/upload/v1627409872757/of3-6rTch.html) under [CC BY 2.0](https://creativecommons.org/licenses/by/2.0/)](https://cdn-images-1.medium.com/max/6660/1*bEdolomDTqlS6cODCClqYA.jpeg)*Photo by [Don DeBold](https://www.flickr.com/photos/ddebold/) under [CC BY 2.0](https://creativecommons.org/licenses/by/2.0/)*

JavaScript provides an object called Math that contains objects and methods for working with mathematics. The particular method we are interested in today is Math.random(). It returns a pseudo-random number from 0 (inclusive) to 1, but not including 1.

```
var getRandom = function() {
  return Math.random();
};

getRandom();

// 0.30283920327201486
```


We can scale this function to a maximum using multiplication.

```
var getRandomMax = function(max) {
  return Math.random() * max;
};

getRandomMax(10);

// 8.227049359120429;
```


Since we want an integer, we can use another method of the Math object called Math.floor(). It returns the largest integer less than or equal to a given number.

```
Math.floor(8.227049359120429);

// 8
```


If we use this method in our function, we get…

```
var getRandomIntMax = function(max) {
  return Math.floor(Math.random() * max);
};

getRandomIntMax(10);

// 2
```


There is an issue with the above code, however. Our function will never return the exact maximum, which is 10 in this case. It will return 0 through 9, but never 10.

We can fix this by adding 1 to our max internally.

```
var getRandomIntMax = function(max) {
  return Math.floor(Math.random() * (max + 1));
};

getRandomIntMax(10);

// (could be anywhere from 0 to 10, inclusive)
```


Now let’s add a minimum. One way to make sure we are always above our minimum to to add the minimum before returning.

```
var getRandomIntRange = function(min, max) {
  return Math.floor(Math.random() * (max + 1)) + min;
};

getRandomIntRange(5, 10);

// 13
```


Uh oh. We will never drop below our minimum, but we could rise above our maximum. But this is an easy fix. Just subtract the minimum from the maximum and we’re good!

```
var getRandomIntRange = function(min, max) {
  return Math.floor(Math.random() * (max - min + 1)) + min;
};

getRandomIntRange(5, 10);

// 9
```


Excellent. Our function works as expected… unless you don’t give it one or both of the expected arguments. If you’d like, you can add some sensible defaults. These defaults might depend on the context of what you’re building. In our case, I’ll just make the default minimum 0 and the maximum 100. However, another good option would be to return an error if any argument is missing.

```
var getRandomIntRange = function(min, max) {
  min = min || 0;
  max = max || 100;
  
  return Math.floor(Math.random() * (max - min + 1)) + min;
};

getRandomIntRange();

// 31
```


What if we pass an argument that is not an integer?

```
getRandomIntRange(5, 'a');

// NaN
```


Oops. Let’s put in some checks to make sure our arguments are integers.

### THE FINAL FUNCTION

```
var getRandomIntRange = function(min, max) {
  min = min ? min : 0;
  max = max ? max : 100;
  
  if (!Number.isInteger(min)) {
    return new Error('Minimum is not an intenger.');
  }
  
  if (!Number.isInteger(max)) {
    return new Error('Maximum is not an integer.');
  }
  
  return Math.floor(Math.random() * (max - min + 1)) + min;
};

getRandomIntRange(5, 'a');

// Error: Maximum is not an integer.
```


Number.isInteger() will work in most modern browsers *except* Internet Explorer. For more information, check out the compatibility table for [ES6 Number](http://caniuse.com/#feat=es6-number). If you need to support Internet Explorer, you can use this handy function:

```
function isInteger(num) {
  return (typeof num === 'number' && (num % 1) === 0);
}
```


That’s all I have for now. There are undoubtedly ways to improve this function to make it more efficient and robust. But this will work for 99% of use-cases.

%[https://codepen.io/travishorn/pen/bZKyGP]