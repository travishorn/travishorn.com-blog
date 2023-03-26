---
title: "Simple jQuery"
datePublished: Thu Aug 04 2016 22:01:01 GMT+0000 (Coordinated Universal Time)
cuid: ckrowqks30ojwems13ij8degr
slug: simple-jquery-40e9b0a7a41a
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1627409844747/cFhoxcpuD.jpeg
tags: jquery, javascript, web-development

---


This post is part of a project to move my old reference material to my blog. Before 2012, when I accessed the same pieces of code or general information multiple times, I would write a quick HTML page for my own reference and put it on a personal site. Later, I published these pages online. Some of the pages still get used and now I want to make them available on my blog.

![Photo by [Charlie Harutaka](https://cdn.hashnode.com/res/hashnode/image/upload/v1627409843099/VKBja3zLP.html)](https://cdn-images-1.medium.com/max/9216/1*XYxrA80uZ03vV6xQnENNvA.jpeg)*Photo by [Charlie Harutaka](https://unsplash.com/@charlieharutaka)*

jQuery is a fast, concise, JavaScript library that simplifies how to traverse HTML documents, handle events, perform animations, and add AJAX.

### Including it in a Page

The simplest method is to use the [jQuery CDN](https://code.jquery.com/) and include jQuery just before the closing &lt;/body&gt; tag.

```
<!doctype html>
<html>
  <head>
    <title>[TITLE]</title>
  </head>
  <body>
    [CONTENT]

  <script src="https://code.jquery.com/jquery-3.1.0.min.js"></script>
    <script>
      $(function() {
        // Put your jQuery code here.
      }
    </script>
  </body>
</html>
```


### Selecting Elements

Most methods in jQuery require you to target an element(s) on the page. Selection looks like this:

```
$('p').doSomething();
```


Selects all &lt;p&gt; elements.

```
$('#target').doSomething();
```


Selects an element that has the id “target”. For example &lt;div id=”target”&gt;.

```
$('.selectme').doSomething();
```


Selects all elements with the class “selectme”. For example &lt;div class=”selectme”&gt; or &lt;li class=”selectme”&gt;.

For more information about selecting elements, visit [https://api.jquery.com/category/selectors/](https://api.jquery.com/category/selectors/).

### Manipulating Selected Elements

In all three of the examples above, the method being called was doSomething(). There’s no such method as doSomething() in the default jQuery library, but here are some real ones you can use:

```
$('span').addClass('highlight');
```


Adds the class “highlight” to every span on the page.

```
$('p').html('Replacement text.');
```


Sets the HTML inside every &lt;p&gt; to “Replacement text.”

```
$('img#header').attr('alt', 'Sky with clouds');
```


Changes the “alt” attribute of &lt;img id=”header” …&gt; to “Sky with clouds.”

For more information about manipulation, visit [https://api.jquery.com/category/manipulation/](http://api.jquery.com/category/manipulation/).

### Events

Events are a way to perform an action when a condition is met. Some examples are:

```
$('#target').click(function() {
  // Do something.
});
```


Executes when the user clicks on “target.”

```
$('#target').hover(function() {
  // Do something.
});
```


Executes when the user hovers over “target.”

```
$('#target').focus(function() {
  // Do something.
});
```


Executes when “target” gains focus (user clicks or tabs into it).

Learn more about events at [https://api.jquery.com/category/events/](http://api.jquery.com/category/events/).