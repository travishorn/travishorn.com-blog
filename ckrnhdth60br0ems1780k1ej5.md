---
title: "Web Fonts"
datePublished: Fri Apr 20 2012 05:00:00 GMT+0000 (Coordinated Universal Time)
cuid: ckrnhdth60br0ems1780k1ej5
slug: web-fonts-ebcfe913dce8
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1627410020958/mHiZY5TFc.png
tags: typography, web-development

---


There’s always been a problem with fonts on the Internet. It’s always been possible to send text, images, and videos, but until recently there’s been no reliable way to send fonts to the browser. Sure, you can define all the fonts you want on your page, but they won’t show up if the end-user doesn’t have them installed on their local machine. This means your page could look beautiful on one person’s screen, but terrible on another’s. Browsers have attempted to compensate for this shortcoming by implementing embeddable font systems, but so far compatibility has been limited.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627410019499/3enHk8klC.png)

However, with CSS3, web fonts are coming back with great support. As long as you define them correctly, you can have web fonts compatible with [every major browser today](http://webfonts.info/wiki/index.php?title=@font-face_browser_support). To make things even easier, there are free font services such as [Google Web Font](http://www.google.com/webfonts#HomePlace:home) that give you the code in a snippet to include on your site. I like to see how things work for myself, though. Let’s take a look at writing the CSS manually.

For this demonstration, I’m going to choose a font called “[Open Sans](http://www.google.com/webfonts/specimen/Open+Sans)”. I’m also going to use the [.WOFF](https://developer.mozilla.org/en/WOFF) filetype, since it is supported by Chrome, Firefox, and Internet Explorer’s latest versions. In a production environment, you’d probably want to provide more filetypes to fall back on.

First, [download a copy of Open Sans](http://themes.googleusercontent.com/static/fonts/opensans/v6/cJZKeOuBrn4kERxqtaUH3T8E0i7KZn-EPnyo3HZu7kw.woff). Place that file in the same folder as your CSS stylesheet. I even renamed mine “OpenSans.woff”. Next, let’s write some simple CSS to get us going.

```
@font-face {
  font-family: 'Open Sans';
  font-style: normal;
  font-weight: 400;
  src: local('Open Sans'), local('OpenSans'), url('OpenSans.woff') format('woff');
}
```


In the src definition, the first two items point to the user’s local machine. If they already have the font installed, we don’t want to waste time and bandwidth downloading it again. However, if the browser cannot find the font installed locally, it will move on the third item, which is a url to our copy of OpenSans.woff on the server. If the browser doesn’t support .WOFF, Open Sans won’t be available, but if you have more format for the same font, you could keep on adding urls.

Once @font-face is in place, simply call the web font like any other font in your stylesheet.

```
body {
  font-family: 'Open Sans', Arial, Helvetica, sans-serif;
}
```


It couldn’t be easier than that. [Six Revisions has a comprehensive, but a little dated guide on using @font-face.](http://sixrevisions.com/css/font-face-guide/) I recommend checking it out.

*This post first appeared on my old blog on April 20, 2012.*