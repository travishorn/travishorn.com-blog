---
title: "JavaScript Slideshow with No Dependencies"
datePublished: Mon Aug 08 2016 22:01:03 GMT+0000 (Coordinated Universal Time)
cuid: ckrnia96m0c1gems18xnbdu59
slug: javascript-slideshow-with-no-dependencies-8089d6c16511
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1627409884577/9z8GE4F38.jpeg
tags: javascript, web-development

---


There’s no shortage of slideshow plugins and frameworks for JavaScript. But how simple would it be to code one using plain old JavaScript? Warning: This slideshow will be pretty bare-bones. I didn’t spend any time styling it or adding any extra features. However, there is a list of possible feature enhancement ideas at the end of this article.

![Photo by [Nathan Anderson](https://cdn.hashnode.com/res/hashnode/image/upload/v1627409882977/GI7ka43cy.html)](https://cdn-images-1.medium.com/max/12032/1*xzDHUkDpNsqN_HkbtaHExg.jpeg)*Photo by [Nathan Anderson](https://unsplash.com/@nathananderson)*

Let’s assume we have three images:

* butterfly.jpg

* ship.jpg

* roof.jpg

While not required, the slideshow will look the best if each of these images have the same dimensions.

Using the principle of progressive enhancement, let’s make sure all our images look nice before adding any JS.

**slideshow.html**

```
<!doctype html>
<html lang="en">
  <head>
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1">

    <title>Slideshow</title>

<link rel="stylesheet" href="slideshow.css">
  </head>
  <body>
    <ul id="myImageList" class="slideshow">
      <li><img alt="A pretty butterfly." src="butterfly.jpg"></li>
      <li><img alt="A navy command ship." src="ship.jpg"></li>
      <li><img alt="The roof of an old church." src="roof.jpg"></li>
    </ul>

    <script src="slideshow.js"></script>
  </body>
</html>
```


**slideshow.css**

```
.slideshow {
  list-style-type: none;
  padding: 0;
  margin: 0;
}
```


Even with an empty **slideshow.js**, the images are displayed in a neat column for viewing. Now let’s add a little JS.

**slideshow.js**

```
// Get the list of images
var myImageList = document.getElementById('myImageList');

// A function that will convert a list of images into a slideshow
var slideshow = function(imageList) {
  // skip for now
}

// Pass the image list into the function
slideshow(myImageList);
```


Inside the **slideshow()** function, let’s start by setting up some variables.

```
var count = imageList.children.length; // Number of images
var current = 0; // The currently displayed image
var ONE_SECOND = 1000; // 1 second represented as milliseconds
var i; // Used later for looping through images
var nextSlide; // Used later to store our slide advance function
```


After setting the necessary variables, the first order of business is to hide all but the first image. We can do this by looping through the list of images and setting their display property to ‘none’. Notice we are starting the index of this loop on 1 instead of 0. This will skip our first image and process all the remaining ones.

```
for (i = 1; i < count; i++) {
  imageList.children[i].style.display = 'none';
}
```


With all but the first image hidden, we need a function that will advance through the slides one at a time. This function will need to fire every second (or however long you wish).

```
nextSlide = function() {
  // Skip for now
}

window.setInterval(nextSlide, ONE_SECOND);
```


We’re all done except for defining the **nextSlide()** function. It does most of the heavy lifting here. Inside the **nextSlide()** function, let’s set up any variables we’ll need later.

```
var next; // Used as an index on the next image to be shown
```


That’s the only variable in this function’s scope. We can begin with the logic. We want to set **next** to the index of the next image to be shown. Usually, it will just be the current image’s index + 1. But we have to handle getting to the end of the list. In that case, we’ll set the next image back to index 0.

```
if (current + 1 > count - 1) {
  next = 0;
} else {
  next = current + 1;
}
```


Now we’ll actually hide the current image and show the next one.

```
imageList.children[current].style.display = 'none';
imageList.children[next].style.display = 'block';
```


Finally, since we’ve just advanced our slide, we need to update the current image index;

```
current = next;
```


We’re done. The whole **slideshow.js** looks like this:

```
var myImageList = document.getElementById('myImageList');

var slideshow = function(imageList) {
  var count = imageList.children.length;
  var current = 0;
  var ONE_SECOND = 1000;
  var i;
  var nextSlide;
  
  for (i = 1; i < count; i++) {
    imageList.children[i].style.display = 'none';
  }
  
  nextSlide = function() {
    var next;
    
    if (current + 1 > count - 1) {
      next = 0;
    } else {
      next = current + 1;
    }
    
    imageList.children[current].style.display = 'none';
    imageList.children[next].style.display = 'block';
    
    current = next;
  }
  
  window.setInterval(nextSlide, ONE_SECOND);
}

slideshow(myImageList);
```


If you load up **slideshow.html** in your browser, you’ll see the first image in the list, followed by the next one a second later, and so on.

%[https://codepen.io/travishorn/pen/rLvQEE]

There are many ways to extend this function. Here are a few that would be good practice for a JS developer:

* Allowing configuration options for things such as the delay before advancing the slide

* Advance on click

* Visible slideshow controls

* Animations between slides

* Deal with images of different dimensions