---
title: "Backbone.js"
datePublished: Fri Mar 30 2012 05:00:00 GMT+0000 (Coordinated Universal Time)
cuid: ckrnhcv300c3pfws1fqzg3000
slug: backbone-js-3fb2a99b7b03
tags: javascript, backbonejs

---


It becomes apparent that as you start building more and more client-side JavaScript functionality to an application, the code can quickly become a hard-to-maintain mess. You will start to notice yourself trying to build a consistent structure for tying data and events to page elements. Most of the time, for small projects, this is fine. For large projects, however, I’d recommend a supplimental library such as Backbone.js.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627410024001/15PNdoGLj.png)

Backbone provides a structured platform for developing your JavaScript-heavy applications. It is similar to an [MVC](http://en.wikipedia.org/wiki/Model%E2%80%93view%E2%80%93controller) and, in fact, has the same goal as a traditional MVC in mind. However, it uses [models](http://documentcloud.github.com/backbone/#Model), [collections](http://documentcloud.github.com/backbone/#Collection), [routers](http://documentcloud.github.com/backbone/#Router), and [views](http://documentcloud.github.com/backbone/#View).

From [http://documentcloud.github.com/backbone/](http://documentcloud.github.com/backbone/):
> *With Backbone, you represent your data as Models, which can be created, validated, destroyed, and saved to the server. Whenever a UI action causes an attribute of a model to change, the model triggers a “change” event; all the Views that display the model’s state can be notified of the change, so that they are able to respond accordingly, re-rendering themselves with the new information.*

By writing your application with Backbone, you’re keeping your code in a clean, strict environment, using [RESTful](http://en.wikipedia.org/wiki/Representational_state_transfer) services. It also takes care of much of the boilerplate code you’d have to write for interactive apps, and provides you with a best-practice environment that will be easier for you or any collaborators to maintain.

If you’re looking for more information, a good way to familiarize yourself with Backbone is by checking out [real-world examples](http://documentcloud.github.com/backbone/#examples). If you’re looking to learn how to get started, I like [this tutorial by Thomas Davis](http://thomasdavis.github.com/2011/02/01/backbone-introduction.html).