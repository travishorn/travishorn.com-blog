---
title: "Browser standards implementation"
datePublished: Sat Aug 23 2014 21:37:20 GMT+0000 (Coordinated Universal Time)
cuid: ckrni4me60bz9ems1f1v21axt
slug: browser-standards-implementation-75c7dcbdfbad
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1627409906973/x4h0p4G9D.png
tags: css, web-design, web-development

---


Every browser renders pages and behaves differently. Even the ones that try to stick to standards implement them inconsistently. Make sure your site is usable for your target audience. If you are developing for the general public, the audience may be using any browser they choose. At a minimum test against…

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627409905647/fpqApfwkP.png)

Web Browser Market Share for July 2014 from [W3Counter](http://www.w3counter.com/globalstats.php)

Also consider how browsers render your site in different operating systems. The major ones being…

* [Apple OSX](http://www.apple.com/osx/)

* Linux (such as [Ubuntu](http://www.ubuntu.com/))

* [Microsoft Windows](http://windows.microsoft.com/en-us/windows/home)

and mobile platforms such as…

* [Android](http://www.android.com/)

* [BlackBerry OS](http://us.blackberry.com/)

* [iOS](http://www.apple.com/ios/)

A good starting point is using a normalizing stylesheet.

Consider using a [reset stylesheet](http://meyerweb.com/eric/tools/css/reset/) or [normalize.css](http://necolas.github.io/normalize.css/).
> *The goal of a reset stylesheet is to reduce browser inconsistencies in things like default line heights, margins and font sizes of headings, and so on. … Reset styles quite often appear in CSS frameworks, and the original “meyerweb reset” found its way into [Blueprint](http://blueprintcss.org/), among others.*

and
> *Normalize.css makes browsers render all elements more consistently and in line with modern standards. It precisely targets only the styles that need normalizing.*

Keep in mind that you should always be catering to your users. If you are developing an internal-only site for an organization that requires its employees to use Internet Explorer, make sure the site works in Internet Explorer first.

For example, take a look at the following code.

```
<style>
  .intro {
    width: 100px;
    text-align: center;
    border: 1px solid #000;
    border-radius: 10px;
  }
</style>

<div class="intro">
  I love cats.
</div>
```


This will correctly show rounded corners in Chrome and Firefox, but squared corners in Internet Explorer 8 and older. This is a fairly harmless example, but worse inconsistencies can completely break a site.

Check out [“Can I use”](http://caniuse.com/) for up-to-date information on browser feature support.
> *“Can I use” provides up-to-date browser support tables for support of front-end web technologies on desktop and mobile web browsers.*

Lately, there have been a number of all-in-one HTML, CSS, and JS frameworks; the most popular of which is Twitter’s [Bootstrap](http://getbootstrap.com/). Including Bootstrap in your project will make sure your site displays consistently across the most popular browsers and devices.

Best of all, it’s easy to set up. Just include the CSS & JavaScript, and code your site according to [its’ structure and classes](http://getbootstrap.com/css/).

*Originally published on my old blog on August 23, 2014.*