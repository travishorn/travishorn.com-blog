---
title: "jQuery"
datePublished: Fri Apr 13 2012 05:00:00 GMT+0000 (Coordinated Universal Time)
cuid: ckrnhqylo0bvcems148o9fx0f
slug: jquery-f35f0b101a21
tags: jquery, javascript, web-development

---


jQuery is an extremely popular front-end library for web development due to its simplicity and power. According to builtwith.com, jQuery is used in over half of the top 10,000 websites on the Internet. The more popular it gets, the more plugins are written to extend it’s functionality. Let’s take a look at what it can do.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627409957038/vRj9CQ5CW.png)

Every web page is made up of a collection of elements called the [Document Object Model](http://en.wikipedia.org/wiki/Document_Object_Model) (DOM). The window itself, history & location information, links, images, forms, and every other element are all part of the DOM. jQuery provides easy methods to traverse and modify the DOM. For example, let’s take a look at how you could easily stripe rows on a table.

HTML:

```
<table>
  <tbody>
    <tr>
      <td>Row 1</td>
    </tr>
    <tr>
      <td>Row 2</td>
    </tr>
    <tr>
      <td>Row 3</td>
    </tr>
    <tr>
      <td>Row 4</td>
    </tr>
    <tr>
      <td>Row 5</td>
    </tr>
    <tr>
      <td>Row 6</td>
    </tr>
  </tbody>
</table>
```


CSS:

```
tr.stripe td {
  background-color: #CCC;
}
```


[Download the jQuery file](http://code.jquery.com/jquery-latest.min.js) and include it:

```
<script src="jquery.js"></script>
```


JavaScript:

```
<script>
  $(document).ready(function(){
    $('tr:even').addClass('stripe');
  });
</script>
```


If you want to see this code in action, [view this jsFiddle](http://jsfiddle.net/cHVQP/).

With the first line of JavaScript, we’re just telling jQuery to execute the rest of the code after the document is ready (when the page is done loading). This is always a good idea because it prevents jQuery from trying to modify DOM elements that haven’t been created in the browser, yet. The second line is where the magic happens. We’re selecting even rows (&lt;tr&gt;s) with $(‘tr:even’) and then adding the stripe class to them with .addClass(‘stripe’). The CSS takes over from there, applying a background color to all &lt;td&gt;s on rows with the stripe class.

Now that you know how it works, [take a look at what functions you can perform on DOM elements](http://apijquery.com/category/manipulation/). You can quickly see how powerful this library is. Of course, this is only the tip of the iceberg; With other features such as ajax, events, effects, and plugins, there’s a lot of possibility here.

While it’s true that jQuery is just a collection of extensive JavaScript functions, and you could do anything in plain JavaScript that you could with jQuery, it makes it easy on you because it’s a standardized format that is tested to work in all major browsers. You may discover that a lot of what you’ve been trying to do with JavaScript has already been done for you in jQuery.

If you want to continue learning, look at [this tutorial](http://docs.jquery.com/Tutorials:How_jQuery_Works) by jQuery’s creator, John Resig and the official [jQuery Tutorials](http://docs.jquery.com/Tutorials) section.