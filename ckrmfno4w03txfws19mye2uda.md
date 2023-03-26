---
title: "Dealing with asynchronous functions in JavaScript"
datePublished: Thu Feb 28 2019 11:01:00 GMT+0000 (Coordinated Universal Time)
cuid: ckrmfno4w03txfws19mye2uda
slug: dealing-with-asynchronous-functions-in-javascript-33301291b6db
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1627410313159/IUpf5F6qZ.png
tags: javascript, asynchronous, web-development, nodejs, promises

---


Writing non-blocking, asynchronous code is a staple of Node.js development and the broader JavaScript world. JavaScript itself uses an event loop which makes writing asynchronous functions more difficult by default. Let’s look at examples of synchronous and asynchronous code; as well as some methods for programming asynchronously.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627410311449/d3QhMesKu.png)

### A regular synchronous function

Here is a simple script.

```
const output = document.getElementById('output');
const button = document.getElementById('button');

const greet = (subject) => {
  return `Hello, ${subject}!`;
};

button.addEventListener('click', () => {
  output.textContent = 'Loading...';
  output.textContent = greet('World');
});
```


When the button is clicked, the output message temporarily becomes “Loading…” and then **immediately** becomes “Hello, World!”.


`greet()` is a synchronous function. When it’s called, the parser steps into the function and immediately returns the resulting string.

But what about cases where the result cannot be returned immediately? A good example of this would be a call to an external API. Another way to simulate this would be setting a timeout so the result is **not returned right away**.

What if we change our function to look like this?

```
const output = document.getElementById('output');
const button = document.getElementById('button');

const greet = (subject) => {
  setTimeout(() => {
    return `Hello, ${subject}!`;
  }, 1000);
};

button.addEventListener('click', () => {
  output.textContent = 'Loading...';
  output.textContent = greet('World');
});
```


*Note the timeout added in bold above.*

You might expect this code to work just as before, but with a 1000 millisecond (1 second) delay.

In reality, however, the output message is blank!


This is because the `greet()` call is looking for a result to be returned immediately. Since there was a 1 second delay, the parser just kept chugging along after calling the function and not getting an immediate return value.

### Callbacks

So how do we fix it? One method is using a callback. We modify our function to accept a second argument. This second argument is expected to be a function that our function will call when it’s ready.

```
const greet = (subject**, callback**) => {
  setTimeout(() => {
    **callback(null, **`Hello, ${subject}!**`);**
  }, 1000);
};
```


*One interesting thing to note about the function above is that, when calling the callback, the first argument we’re passing is `null`. If our function encountered an error while executing, we would pass an error message in place of this `null` value. This isn’t necessarily required but it is a strong convention, so I definitely recommend it.*

Now that we’ve modified our function, we have to modify how we call it.

```
button.addEventListener('click', () => {
  output.textContent = 'Loading...';
  
  greet('World'**, (error, message) => {
    output.textContent = message;
  }**);
});
```


In addition to the first `subject` argument, `greet()` now takes a second argument, which is a function. This function is the callback that gets called when `greet()` is done. *That* is where we set the output message.

It’s also where you’d handle any errors that occurred. This can be done with a simple `if` statement.

```
greet('World', (error, message) => {
  **if (error) {
    output.textContent = `There was a problem: ${error}`;
  } else {**
    output.textContent = message;
  **}**
});
```


The code above says: If there was an error, show it. Otherwise, show the returned message.

Now, our code works as expected. Callbacks are a tried and true pattern for asynchronous functions.


### Promises

Another popular method for dealing with asynchronous code is called **promises**. [MDN web docs defines a promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise) as…
> …a proxy for a value not necessarily known when the promise is created. It allows you to associate handlers with an asynchronous action’s eventual success value or failure reason. This lets asynchronous methods return values like synchronous methods: instead of immediately returning the final value, the asynchronous method returns a *promise* to supply the value at some point in the future.

We can re-write our function to use promises like so.

```
const greet = (subject) => {
  **return new Promise((resolve, reject) => {**
    setTimeout(() => {
      **resolve(**`Hello, ${subject}!`**);**
    }, 1000);
  **});**
}
```


The function is back to only needing one argument. And instead of returning a value, we return a `new Promise` that can `resolve()` to a value (or `reject()` to an error).

Of course, this means we must change how it’s called.

```
button.addEventListener('click', () => {
  output.textContent = 'Loading...';
  
  **greet('World').then((message) => {**
    output.textContent = message;
  **});**
});
```


Just as with callbacks, we can handle errors in the call. It looks a little different, though.

```
button.addEventListener('click', () => {
  output.textContent = 'Loading...';
  
  greet('World')
    .then((message) => {
      output.textContent = message;
    })
    **.catch((error) => {
      output.textContent = `There was a problem: ${error}`;
    });**
});
```


Testing our code, it does indeed work with this pattern.


Promises have several advantages over callbacks, which you can read about in [Oscar Paz’s answer to the StackOverflow question “Aren’t promises just callbacks?”](https://stackoverflow.com/a/22540276/307338)

### Async/await

It gets even better. As long as the function is written as a promise, we can call it in the same style as a synchronous function; we just have to add a couple keywords.

```
asyncButton.addEventListener('click', **async** () => {
  output.textContent = 'Loading...';
  output.textContent = **await** greet('World');
});
```


Look how similar this is to our original code! The only differences are

1. We added `await` before the function is called

1. In order for `await` to work, we added `async` to the parent function

As with all asynchronous functions, we need a way to report if an error occurred. In this case, we can use `try` and `catch`.

```
asyncButton.addEventListener('click', async () => {
  output.textContent = 'Loading...';
  
  **try {**
    output.textContent = await greet('World');
  **} catch(error) {
    output.textContent = `There was a problem: ${error}`;
  }**
});
```



This is the latest and most trendy way to use asynchronous functions. It has advantages over previous methods you can read about in Mostafa Gaafar’s post titled [6 Reasons Why JavaScript’s Async/Await Blows Promises Away](https://hackernoon.com/6-reasons-why-javascripts-async-await-blows-promises-away-tutorial-c7ec10518dd9).