---
title: "A small function to convert an array of ID strings into a dictionary of jQuery objects"
datePublished: Sat Jul 16 2016 00:12:52 GMT+0000 (Coordinated Universal Time)
cuid: ckroxesy80orqems1f1eodvv0
slug: a-small-function-to-convert-a-list-of-id-strings-into-a-dictionary-of-jquery-objects-a68c89facb37
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1627409712178/KiYqTh4QT.jpeg
tags: jquery, javascript, web-development

---


I often use jQuery to modify the DOM. The simplest way to do this is to use jQuery’s query selector and a modifier function.

```
$('#intro').css('background-color', 'green');
```


![Photo by [Kalliop3](https://cdn.hashnode.com/res/hashnode/image/upload/v1627409710288/dDfrmP1d_.html) under [CC BY-SA 2.0](https://creativecommons.org/licenses/by-sa/2.0/)](https://cdn-images-1.medium.com/max/8576/1*NnXo-KrTwltkqdL77lkubg.jpeg)*Photo by [Kalliop3](https://www.flickr.com/photos/ubicumque/) under [CC BY-SA 2.0](https://creativecommons.org/licenses/by-sa/2.0/)*

If you have to perform multiple actions on the same element, you can keep using this selector.

```
$('#intro').css('background-color', 'green');

// Later...

$('#intro').hide();
```


Instead of searching the DOM every time we want to access this element, it is much more efficient to search for it once and store it in a variable.

```
var intro = $('#intro');

intro.css('background-color', 'green');

// Later...

intro.hide();
```


If you have many elements to access…

```
var intro = $('#intro');
var header = $('#header');
var button = $('#button');
var form = $('#form');
var alert = $('#alert');
var footer = $('#footer');
// etc...
```


it may be wise to put them all into a “dictionary” object.

```
var dom = {
  intro: $('#intro'),
  header: $('#header'),
  button: $('#button'),
  form: $('#form'),
  alert: $('#alert'),
  footer: $('#footer'),
  /// etc...
};
```


This way they can be accessed by dot (or bracket) notation from the main object.

```
dom.intro.css('background-color', 'green');
dom.header.hide();

dom['outro'].text('Goodbye.');
dom['alert'].show();
```


Writing the dictionary seems inefficient because there’s a lot of duplicated words. As long as your element IDs are also valid JavaScript variable names, we can automatically convert the IDs into variables with a small function.

### The idsToJqObjects function

```
var idsToJqObjects = function(arr) {
  return arr
    .reduce(function addJqToObject(obj, newKey) {
      obj[newKey] = $('#' + newKey);
      return obj;
    }, {});
};
```


Use it in this way:

```
var dom = idstoJqObjects([
  'intro',
  'header',
  'button',
  'form',
  'alert',
  'footer',
  // etc...
]);
```


Your jQuery objects can be accessed with the same dot or bracket notation as before, without having to write duplicate code for each ID/variable.