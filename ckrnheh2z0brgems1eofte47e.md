---
title: "Increasingly Higher Level “Hello World” with Node"
datePublished: Mon Apr 23 2012 05:00:00 GMT+0000 (Coordinated Universal Time)
cuid: ckrnheh2z0brgems1eofte47e
slug: increasingly-higher-level-hello-world-with-node-1d31cb14b564
tags: javascript, nodejs

---


This is a small experiment that I found a little neat and it may give some clarity to the layers of some Node stacks. I’m going to use Node to send “Hello world!” to the browser in increasingly higher level stacks; starting all the way from TCP and moving up to Zappa. Each example has the same goal: to send the text “Hello world!” to the browser. Even though, we’re not using them, each higher level adds enormous capabilities that I’ll go over at each step.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627410016716/hLi3AHU21.png)

To run the code in this post, you need to [have Node installed](https://github.com/joyent/node/wiki/Installation). Let’s start with…

### TCP

This is about as low-level as we can go here seeing as the web runs on [TCP](http://en.wikipedia.org/wiki/Transmission_Control_Protocol). Everything else will be built upon this layer. There is no cruft here: no HTML, no CSS, not even headers will be sent.

```
var net = require('net');

var server = net.createServer(function (socket) {
  socket.addListener('connect', function () {
    socket.end('Hello world!');
  });
});

server.listen(3000, 'localhost');
```


Save this snippet as tcp.js and run it with node tcp.js. Then visit localhost:3000 in your browser.

The net module is a part of Node and its createServer method creates a new TCP server. The server’s socket then listens for a connection. When a client connects, the “Hello world!” text is emitted and the connection is closed. If the connected client is a browser, it will display whatever is sent over TCP.

### HTTP

[HTTP](http://en.wikipedia.org/wiki/HTTP) is built upon TCP. It defines a communication structure for displaying web pages. Headers are now sent along with content.

```
var http = require('http');

var server = http.createServer(function (req, res) {
  res.writeHead(200, {'Content-Type': 'text/plain'});
  res.end('Hello world!');
});

server.listen(3000);
```


Same deal: Save this snippet as http.js and run it with node http.js. Browse to localhost:3000.

The http module is another built-in piece of Node. Its createServer method is more robust than net’s, though. In this case, we specify a request and response, rather than a single socket. When a client connects, we tell it through the header that the connection has been accepted and we’re sending plain text data in this transmission.

### Connect

Now things are starting to get powerful. You can already do a lot with just the http module, but now we’re throwing in [Connect](http://www.senchalabs.org/connect/), whose middleware provides some pretty powerful low-level functionality such as logging, serving static files, and authentication just to name a few. Connect basically extends the http module.

Connect is not a native part of Node, so you must install it first with npm install connect.

```
var connect = require('connect');

var app = connect()
  .use(function (req, res) { res.end('Hello world!'); })
  .listen(3000);
```


In the code above, the app object becomes a connect instance. In addition to using use just to handle requests, we could also tell Connect to pass the request through any of its middleware. But that’s beyond the scope of this article. Instead we’re just sending “Hello world!” again, then ending the response.

### Express

Moving up, let’s try it with [Express](http://expressjs.com/) now. Express is built on top of Connect, so it provides the same functionality, plus more robust features such as dynamic views and environment-based configurations.

Express can be installed by running npm install express.

```
var express = require('express');

var app = express.createServer();

app.get('/', function(req, res){
  res.send('Hello world!');
});

app.listen(3000);
```


This time app becomes an instance of express, which we use to create a server. Then when a client tries to GET the default resource (/), Express responds with “Hello world!”

### Zappa

Finally, we have [Zappa](http://zappajs.org/). This platform extends Express, providing support for CoffeeScript and Socket.IO. If you’ve never used CoffeeScript, it’s an optimized language that directly compiles down to plain JavaScript.

You’ll need to install Zappa with npm install zappa.

```
require('zappa') -> @get '/': 'Hello world!'
```


This is about as simple as you can get as far as simple web servers go. The first line requires Zappa, while the second tells Zappa to respond to GETs with “Hello world!”.

It’s interesting to see how all of these technologies are built upon each other. You could certainly write you’re own code with the net module to do what the http module does, just as you can easily write your own code with the http module to do what Connect does. But since the groundwork is already there, you might as well use what’s already freely available. And even if you do want to code it on your own, why not fork and contribute to these projects on GitHub?

* [net.js](https://github.com/joyent/node/blob/master/lib/net.js) (part of [Node](https://github.com/joyent/node))

* [http.js](https://github.com/joyent/node/blob/master/lib/http.js) (part of [Node](https://github.com/joyent/node))

* [Connect](https://github.com/senchalabs/connect)

* [Express](https://github.com/visionmedia/express)

* [Zappa](https://github.com/mauricemach/zappa)

*This post first appeared on my old blog on April 23, 2012.*