---
title: "Integrating multiple stylesheets in a single site"
datePublished: Wed Aug 17 2016 22:01:01 GMT+0000 (Coordinated Universal Time)
cuid: ckrox7zxw0p7lfws1ehsmfe1m
slug: integrating-multiple-stylesheets-in-a-single-site-e720256edcee
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1627409759754/jsNNHWeY4.jpeg
tags: css, javascript, gulp

---


The question is: “If you have 5 different stylesheets, how would you best integrate them into a site?”

The answer will depend on how your site is laid out and how users use your site. Every linked stylesheet is another HTTP request. Performance-wise, HTTP requests are expensive. Developers would do well to minimize the number of stylesheets, therefor minimizing HTTP requests.

![Photo by [Frankie K.](https://cdn.hashnode.com/res/hashnode/image/upload/v1627409758020/lRRGiiquV.html)](https://cdn-images-1.medium.com/max/10368/1*WTEvZDuYeR_54XUfXPQOBQ.jpeg)*Photo by [Frankie K.](https://unsplash.com/@frankie_k)*

So one big stylesheet is the way to go, right? In my opinion, in most cases, yes. However, consider the case that perhaps half of your styles are for an administration panel pages that 99% users will never see. In that case, it would be better to go with two stylesheets: one for public-facing pages and one for the administration panel. Again, the way you split up styles is dependent on your site. Ultimately, I’d say going with one concatenated stylesheet that contains all your styles will be perfectly fine.

### Working with one stylesheet

What kind of workflow works best for one stylesheet then? The simplest answer would be just one file called styles.css (or similar). When you want to change the style, you just open up styles.css, scroll until you find the relevant section, and modify the CSS. In your HTML, you simply link the sheet like so:

```
<link rel="stylesheet" href="styles.css">
```


This will work fine unless/until your site grows so big that finding the relevant section of CSS requires a bunch of scrolling or Ctrl + F’ing.

### Concatenating multiple stylesheets

One solution to the problem above is to have multiple stylesheets in development, but concatenate them into a single sheet for serving in production.

In this case, let’s assume I have an `index.html` and a folder called `css`. Inside the `css` folder there are 4 files:

* bootstrap.css — for my basic boilerplate styles

* header.css — for my page header

* main.css — for the main content of my site (between the header & footer)

* footer.css — for the small footer at the bottom of every page

I could link all these stylesheets in my `index.html` like so:

```
<link rel="stylesheet" href="css/bootstrap.css">
<link rel="stylesheet" href="css/header.css">
<link rel="stylesheet" href="css/main.css">
<link rel="stylesheet" href="css/footer.css">
```


But I want one stylesheet, not four. For this, I’ll add a single build step using Gulp.

If you don’t already have it, you have to install the Gulp CLI globally. This will only have to be done once on a machine.

```
npm install -g gulp-cli
```


The next step is optional, but since we are working with npm modules, I highly recommend initiating npm on your project to keep track of dependencies:

```
npm init
```


Answer all prompts from `npm init`. When you’re finished, install gulp and the gulp-concat-css plugin as development dependencies:

```
npm install --save-dev gulp gulp-concat-css
```


Now, create a new file called gulpfile.js in your project’s root directory. This file will describe the style concatenation build step as well as any future build steps you may add later.

```
'use strict';

var gulp = require('gulp');
var concatCss = require('gulp-concat-css');

gulp.task('css', function() {
  return gulp.src('css/*.css')
    .pipe(concatCss('style.css'))
    .pipe(gulp.dest('.'));
});

```


This defines a new gulp task called `css`. The task takes any file in the `css` folder with a `.css` extension and pipes it to `gulp-concat-css`, which concatenates them all into a `style.css` file, then pipes **that** to a destination directory. In the code above, the destination directory is our project’s root.

Basically, when you run

```
gulp css
```


a new file will appear called `styles.css` which contains all of your styles. In `index.html`, you can replace all those stylesheet links with one line:

```
<link rel="stylesheet" href="styles.css">
```


If (when) you change the CSS and are ready to push your updates to production, just run `gulp css` again to re-concatenate the styles.

### Final steps

At this point you are done. However, I would personally take an extra step and create a default gulp task that includes the `css` task. Add this to `gulpfile.css`:

```
gulp.task('default', ['css']);
```


Now, you can run the build by simply running…

```
gulp
```


If you ever add more build steps (gulp tasks), you can add them to the array we just created. Other build steps you may consider are:

* minifying the CSS file

* concatenating & minifying JS files

* minifying HTML

* compressing image files

These can all be done with various [Gulp plugins](http://gulpjs.com/plugins/).