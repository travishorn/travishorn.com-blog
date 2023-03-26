---
title: "parallaxBG"
datePublished: Fri Apr 06 2012 05:00:00 GMT+0000 (Coordinated Universal Time)
cuid: ckrnhyhf90bxeems13oov1n65
slug: parallaxbg-d705ae8a7daf
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1627409930926/lUHj8HxiGD.png
tags: javascript, web-design, web-development

---


I wanted to try my hand at developing a packaged script that you could include on a page with no dependencies (such as jQuery). Today, I’m presenting parallaxBG: a small piece of JavaScript that will appear to scroll the background of a page slower than the content to create a parallax effect.

![“Parallax Example” by Booyabazooka — Parallax Example.png. Licensed under CC BY-SA 3.0 via Commons — [https://cdn.hashnode.com/res/hashnode/image/upload/v1627409929412/NlkzTR3r_U.html](https://commons.wikimedia.org/wiki/File:Parallax_Example.svg#/media/File:Parallax_Example.svg)](https://cdn-images-1.medium.com/max/2000/1*FgaE6dpDUhSgv7eQltSQKw.png)*“Parallax Example” by Booyabazooka — Parallax Example.png. Licensed under CC BY-SA 3.0 via Commons — [https://commons.wikimedia.org/wiki/File:Parallax_Example.svg#/media/File:Parallax_Example.svg](https://commons.wikimedia.org/wiki/File:Parallax_Example.svg#/media/File:Parallax_Example.svg)*

The basis is simple. When the user scrolls the page, move the background image’s position the opposite direction. As long as the amount the background position is moved is less than the amount of space the user scrolled, it should appear as though the background is still moving in the same direction, just a bit slower. Here it is in pseudo-code:

```
when window is scrolled {
  verticalBackgroundPosition = topOfWindow * -0.5
  horizontalBackgroundPosition = leftOfWindow * -0.5
}
```


Of course, it won’t be quite that easy because of inconsistencies between browsers. Every major browser has a window.onscroll event, so that part is easy.

```
window.onscroll = function() { }
```


Now we want to store the values of where the new background position will be for both top and left. For this step we’re going to need to store the html DOM element to save space and so we’re not repeatedly jumping into the DOM. We can do this by getting the html element by it’s tag name. You’ll notice we’re using Math.round to get nice integers for each value.

```
var html = document.getElementsByTagName('html')[0];

window.onscroll = function() {
  var top = Math.round(html.scrollTop * -0.5);
  var left = Math.round(html.scrollLeft * -0.5);
}
```


Now it’s only a matter of setting the background position itself using the new values. We’ll also going to store the body element here using the same technique we did for html.

```
var html = document.getElementsByTagName('html')[0];
var body = document.getElementsByTagName('body')[0];

window.onscroll = function() {
  var top = Math.round(html.scrollTop * -0.5);
  var left = Math.round(html.scrollLeft * -0.5);

  body.style.backgroundPosition = left + "px " + top + "px";
}
```


At this point everything should be working!… except in [Chrome](https://www.google.com/chrome). Chrome doesn’t update the html.scrollTop and html.scrollLeft attributes when scrolling. Instead, it updates body.scrollTop and body.scrollLeft. We can solve this easily by testing whether the body attributes have been updated onscroll. If they have, we’ll use those values, otherwise we’ll use html.

```
var html = document.getElementsByTagName('html')[0];
var body = document.getElementsByTagName('body')[0];

window.onscroll = function() {
  if (body.scrollTop == 0) {
    var top = Math.round(html.scrollTop * -0.5);
  } else {
    var top = Math.round(body.scrollTop * -0.5);
  }

  if (body.scrollLeft == 0) {
    var left = Math.round(html.scrollLeft * -0.5);
  } else {
    var left = Math.round(body.scrollLeft * -0.5);
  }

  body.style.backgroundPosition = left + "px " + top + "px";
}
```


That should do it! A dependency-free script that creates a parallax effect on the page background. Feel free to [fork parallaxBG on GitHub](https://github.com/travishorn/parallaxBG) if you have a feature idea or want to patch a bug you find.

*This post first appeared on my old blog on April 6, 2012.*