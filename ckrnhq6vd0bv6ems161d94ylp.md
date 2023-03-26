---
title: "Node.js"
datePublished: Mon Mar 26 2012 05:00:00 GMT+0000 (Coordinated Universal Time)
cuid: ckrnhq6vd0bv6ems161d94ylp
slug: node-js-1ae05d0fe66c
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1627409961111/y2RVDXyS7t.png
tags: javascript, nodejs

---


From Nodejs.org:
> *“Node.js is a platform built on [Chrome’s JavaScript runtime](http://code.google.com/p/v8/) for easily building fast, scalable network applications. Node.js uses an event-driven, non-blocking I/O model that makes it lightweight and efficient, perfect for data-intensive real-time applications that run across distributed devices.”*

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627409959673/eUz9bv2RO.png)

If you know anything about Node, you already know it’s an application framework. If you don’t anything about it, it’s basically a platform that you can use to develop applications using JavaScript. It’s new and exciting for the JavaScript community. So, let’s take a look at what the above quote means.

The first thing you’ll notice is that it’s built on Chrome’s JavaScript runtime, which is called V8. V8 is an open-source JavaScript engine written in C++ by Google. It is included in Chrome, Node, and any other project that wants to use it. It’s known for its [fast performance](https://developers.google.com/v8/design). In a way, Node is just a JavaScript processor that comes with built-in functions for common application tasks, such as serving web pages. In fact, much of the extended functionality is network-oriented.

Which brings us to how Node allows for easily building fast, scalable network applications. It’s speed can be attributed to using V8 and a non-blocking I/O model (a key component I’ll talk about further in this post), while it’s scalability comes from the fact that you can spawn your Node application in as many processes, on as many machines as necessary, as long as you have the hardware. A popular approach to this sort of scalability is to run multiple Node processes behind [Nginx](http://nginx.org/) to load-balance the requests. Another approach is to use [Fugue](https://github.com/pgte/fugue) to manage multiple Node processes across multiple cores.

Going back to the topic of Node’s non-blocking I/O model, you’ll also note that Node is event-driven. What this means for web developers and system administrators is that Node apps can be very fast if written correctly. It takes a little change in your thinking to write effective event-driven applications. Everything in Node is a reaction from an event. When that event (such as a page being requested) is triggered, Node will perform an action on that event, then continue waiting for the next event. When the action is complete, Node will issue a callback to the originating function. This is the essence of the “non-blocking” model. Node should almost never be “waiting” for a function to complete. Instead, it calls the function, moves on, and receives the callback later. When the callback is received, Node can do something with it (such as serving the requested page).

The above features make Node into the lightweight and efficient platform, perfect for data-intensive real-time applications that the official homepage claims it to be. In addition to these features, I’d like to point out one of my favorite advantages to this platform: everything is in JavaScript. With Node, it is entirely possible to write an entire web app (front-end **and** back-end) in the same language. This reduces the need for proficiency in multiple languages, as well as the need for context-switching for front and back-end development.

For more information, the [Node.js](http://nodejs.org/) homepage is a plentiful resource.