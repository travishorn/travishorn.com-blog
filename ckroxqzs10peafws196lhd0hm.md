---
title: "3 ways a front-end developer can reduce page load"
datePublished: Thu Dec 20 2018 11:01:00 GMT+0000 (Coordinated Universal Time)
cuid: ckroxqzs10peafws196lhd0hm
slug: 3-ways-a-front-end-developer-can-reduce-page-load-5f6c81797d44
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1627409559060/kegKg1kgm.png
tags: optimization, javascript, performance, web-development

---


Loading times are a big pain for users. You have to tackle the problem from many angles. Some tweaks can be made on the back-end; with hardware bottlenecks and GZIP compression playing big roles.

There are however, small changes that front-end developers can make to scrape up some extra performance. Added altogether, these changes can make a big difference.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627409552004/020Ho6hxc.png)

### Minify resources

One of the biggest things you can do is to minify HTML, CSS, and JavaScript. To demonstrate, take a look at this simple HTML file.

```
<!doctype html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <meta name="viewport"
          content="width=device-width,
                   initial-scale=1,
                   shrink-to-fit=no">

<link rel="stylesheet" href="styles/main.css">

<title>Hello, world!</title>
  </head>
  <body>
    <div class="container my-5">
      <div class="row justify-content-center">
        <div class="col-md-6">
          <h1>Hello, world!</h1>
          
          <p>
            Lorem ipsum dolor sit amet, consectetur adipiscing elit.
          </p>
        </div>
      </div>
    </div>    
  </body>
</html>
```


There is a ton of whitespace in that file. The whitespace is crucial while developing the page. But when serving it? It’s unnecessary. Running this file through [HTML Minifier](http://kangax.github.io/html-minifier/) gets rid of all the whitespace.

```
<!doctype html><html lang=en><meta charset=utf-8><meta content="width=device-width,initial-scale=1,shrink-to-fit=no"name=viewport><link href=styles/main.css rel=stylesheet><title>Hello, world!</title><div class="container my-5"><div class="justify-content-center row"><div class=col-md-6><h1>Hello, world!</h1><p>Lorem ipsum dolor sit amet, consectetur adipiscing elit.</div></div></div>
```


The code above is the exact same page, but 37% smaller!

Obviously, the minified code is much harder to read, but that’s why you keep one version with whitespace for development, and one version minified for production.

Minifying resources one at a time works well if you only have a few files and only make updates infrequently. In the real world, you’d probably want to use a build tool like [Webpack](https://webpack.js.org/), which, among other things, [automatically minifies your code](https://webpack.js.org/guides/production/).

### Move JavaScript to the bottom

When a browser parses a page and encounters a `&lt;script&gt;` tag, it stops rendering the page and starts parsing the script instead.

This is typically a bad thing, as scripts only need to be executed once the page is fully rendered. Only in extremely rare cases will you need the behavior of JavaScript before the page is fully loaded.

Instead, place scripts at the bottom so your page can load and the user can begin viewing it **before** scripts are requested, pull from disk, sent over the network, and executed by the client.

Basically, instead of this…

```
<!doctype html>
<html>
  <head>
    <script src="scripts/main.js"></script>
    <title>Hello, world!</title>
  </head>
  <body>
    <h1>Hello, world!</h1>
    <!-- snip -->
  </body>
</html>
```


Do this…

```
<!doctype html>
<html>
  <head>
    <title>Hello, world!</title>
  </head>
  <body>
    <h1>Hello, world!</h1>
    <!-- snip -->
    **<script src="scripts/main.js"></script>**
  </body>
</html>
```


This is considered best practice. This simple change can make a huge difference. Don’t block your page loads with your JavaScript.

### Optimize images

Images are highly compressible, often with minimal quality loss. Take this PNG for example. It is 126 KB.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627409553991/n3eJyqcKd.png)

If we run it through an optimizer, the result is a much smaller 52 KB image!

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627409555702/YrmnD3h_2.png)

That’s 60% smaller with virtually no noticeable difference is quality.

The two predominant image formats on the web, PNGs and JPGs, [can be optimized automatically with build tools like Webpack](https://iamakulov.com/notes/optimize-images-webpack/).

### Bonus! Reduce perceived page load with a loading animation

Besides *actually* reducing the load time, you can also reduce the *perceived** ***load time. Users hate when they’re not sure if a page is doing anything. If the page doesn’t load or appear to load right away, many users might even click away.

In the same way they put mirrors in elevators to give people something to look at, adding some sort of indicator on your page that something is happening can make a difference in how users perceive your site.

If your page takes a long time to load because of external requests or heavy use of JavaScript, consider adding something like this at the forefront of your site or app.

```
<img id="loading" src="loading.gif">
```


![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627409557349/Ffw6QymNY.gif)

In your JavaScript, when you know your page is ready to be viewed, simply hide the image.

```
document.onreadystatechange = () => {
  if (document.readyState === 'complete') {
    document.getElementById('loading').style.display = 'none';
  }
};
```


These tweaks are just the tip of the iceberg. For more information, you can check out [Google’s PageSpeed Insights guide](https://developers.google.com/speed/docs/insights/about?hl=en-US&utm_source=PSI&utm_medium=incoming-link&utm_campaign=PSI) and [Mozilla’s optimization and performance guide](https://developer.mozilla.org/en-US/docs/Web/Guide/Performance).

You may also consider running Google Chrome’s audits on your site. Open your site in Google Chrome, press **F12**, go to the **Audits** tab, make sure **Performance** is checked, then click **Run audits**. This tool can give you some insights into what might be slowing your site down, as well as some guidance on how to speed it up.