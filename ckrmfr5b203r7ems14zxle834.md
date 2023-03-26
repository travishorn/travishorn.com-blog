---
title: "Creating a responsive header image"
datePublished: Thu Mar 14 2019 11:01:02 GMT+0000 (Coordinated Universal Time)
cuid: ckrmfr5b203r7ems14zxle834
slug: creating-a-responsive-header-image-8ce1d2ed8a4c
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1627410295281/D12mIhh7P.png
tags: web-design, web-development

---


Sometimes it’s best to load and display different images for different screen sizes. For example, a very large header image that works for a large desktop screen may not be necessary or even look correct on a phone in portrait mode.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627410267456/GWw8uYX3h.png)

Say I want to create a header for my new website. I’m going to start with this high-resolution photo by [Graham Holtshausen](https://unsplash.com/photos/fUnfEz3VLv4?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText).

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627410270339/ToasdUsf5.jpeg)

The first thing I’m going to do is shrink the image down so the width is 2560 pixels. Most screens won’t go higher than this so this is a good maximum.

Since this is a header image, I’m going to crop it so it’s short and wide. I’m cropping on center with the height at 100 pixels.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627410273192/gvAMp-H08.png)

*Note that the image may appear less than 2560 x 100 on this blog post.*

Finally, I’ll darken the center, add a logo, and add text in Photoshop to create the header.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627410275263/4-ufESE1P.png)

Now I can create a page using this header at the top.

```
<!doctype html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">

    <link rel="stylesheet" href="styles/normalize.css">
    <link rel="stylesheet" href="styles/main.css">

    <title>My Site</title>
  </head>
  <body>
    <img src="images/header.png">

    <div class="container">
      <!-- The contents of the page will go here. -->
    </div>
  </body>
</html>
```


Notice I’m using [Normalize.css](https://necolas.github.io/normalize.css/) (to make browsers render all elements more consistently). And I’ve included another stylesheet. Right now the styles are pretty minimal.

```
html { font-size: 22px; }

.container {
  width: 80%;
  margin: 0 auto;
}
```


The site looks like this.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627410277443/AAzs7McDj.png)

Oh no. The main part of the header isn’t visible. Plus, the huge width makes the site scroll horizontally. We can fix it with a little bit of HTML…

```
<header>
  <img src="images/header.png">
</header>
```


…and a little bit of CSS.

```
header {
  overflow: hidden;
}

header img {
  margin-left: 50%;
  transform: translateX(-50%);
}
```


With those changes in place, the header looks better!

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627410279317/1apjtwN7z.png)

But look what happens on smaller screens.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627410281437/Ps3rEpvyn.png)

The header is cut off. On extra small devices like this (portrait phones) with a max width of 576 pixels, it might be nice to just show the ice cream logo. To do that, I create a smaller header image with only the logo. This image is 576 x 100 pixels.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627410283132/UPicdatP3.png)

To get *this* image to show on extra small devices, but the *other* image on larger screens, we can use the `&lt;picture&gt;` and `&lt;source&gt;` elements.

I replace the `&lt;img&gt;` in the `&lt;header&gt;` with a `&lt;picture&gt;` element.

```
<header>
  <picture>
    <!-- Our sizes and image sources go here -->
  </picture>
</header>
```


Inside the `&lt;picture&gt;`, we’ll place a couple `&lt;sources&gt;`.

```
<header>
  <picture>
    <source media="(max-width: 576px)"
            srcset="images/header-576.png">
    <source media="(min-width: 577px)"
            srcset="images/header-2560.png">
  </picture>
</header>
```


You can see that `header-576.png` will be displayed on screens up to 576 maximum width. The other image — `header-2560.png` — will be shown on devices 577 pixels and up.

In addition to these two sources, I’ll also add the big original header as a fallback in an `&lt;img&gt;` element.

```
<header>
  <picture>
    <source media="(max-width: 576px)"
            srcset="images/header-576.png">
    <source media="(min-width: 577px)"
            srcset="images/header-2560.png">
    **<img src="images/header-2560.png">**
  </picture>
</header>
```


I add this fallback because `&lt;picture&gt;` isn’t quite supported across the board. Here’s the compatibility table at the time of this writing.

![[Can I use… compatibility table for &lt;picture&gt; element](https://cdn.hashnode.com/res/hashnode/image/upload/v1627410284591/YImxvRHa2.html)](https://cdn-images-1.medium.com/max/2508/1*Ix6ozcuhu5k0I7NFIvXQKQ.png)*[Can I use… compatibility table for &lt;picture&gt; element](https://caniuse.com/#feat=picture)*

Now that we’ve made our header more responsive, here’s how it looks on extra small devices.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627410286056/T5SJwEBaD.png)

On larger devices it still looks good.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627410287660/XnXhL0eX5.png)

How about medium devices (up to 922 pixels)? Simple. It’s the same process.

Create a 922 x 100 pixel header.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627410289669/AYpNlMdXl.png)

And add the `&lt;source&gt;`.

```
<picture>
  <source media="(max-width: 576px)"
          srcset="images/header-576.png">
  **<source media="(max-width: 922px)"
          srcset="images/header-922.png">**
  <source media="(min-width: **923px**)"
          srcset="images/header-2560.png">
  <img src="images/header-2560.png">
</picture>
```


Note the additional change to the `min-width` of the largest `&lt;source&gt;`. It’s always 1 pixel higher than the previous `max-width`.

The images are dynamic, too. If the user’s viewport changes, the image automatically changes.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627410292053/ulF01f1Xw.gif)

Your needs will dictate which breakpoints you should include; In general, you might consider…

* 576px for extra small devices like portrait mode phones

* 768px for small devices like landscape phones

* 992px for medium devices like tablets

* 1200px for large devices like desktops

* 2560px for extra large devices like large desktops

These are the same [breakpoints used by Bootstrap 4](http://getbootstrap.com/docs/4.1/layout/overview/#responsive-breakpoints).

Finally, there is another benefit to using responsive images in this way: The only image(s) that get downloaded are the ones that meets the breakpoint media rules. This means that, on extra small devices, only `header-576.png` gets downloaded. In my case, it’s five times smaller than the original `header-2560.png`; A savings of 465 kilobytes!

See the pen below for a live version of this project that you can play around with.

https://codepen.io/travishorn/pen/MPWLWB