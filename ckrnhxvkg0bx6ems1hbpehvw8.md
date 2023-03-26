---
title: "“Shrinkwrapped” List with CSS"
datePublished: Tue Aug 02 2016 22:49:11 GMT+0000 (Coordinated Universal Time)
cuid: ckrnhxvkg0bx6ems1hbpehvw8
slug: shrinkwrapped-list-with-css-3bd3dfa88bfb
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1627409935059/ch_Cg-5yC.jpeg
tags: css, web-design, web-development

---


This post is part of a project to move my old reference material to my blog. Before 2012, when I accessed the same pieces of code or general information multiple times, I would write a quick HTML page for my own reference and put it on a personal site. Later, I published these pages online. Some of the pages still get used and now I want to make them available on my blog.

![Photo by [Annie Spratt](https://cdn.hashnode.com/res/hashnode/image/upload/v1627409933332/C0SuccDR2.html)](https://cdn-images-1.medium.com/max/8000/1*09ICYcC6EVhb5rbscfMN9w.jpeg)*Photo by [Annie Spratt](https://unsplash.com/@fableandfolk)*

The following code will create a list (&lt;ul&gt; in this case) that is only as wide as its longest item.

### HTML

The HTML is very simple. Just an unordered list:

```
<ul>
  <li>Lorem ipsum</li>
  <li>Dolor sit amet</li>
  <li>Consectetur</li>
  <li>Adipiscing</li>
  <li>Morbi odio</li>
  <li>Mi blandit vehicula</li>
</ul>
```


### CSS

These lines do all the work. The border declaration is optional but helps to visualize the result.

```
ul {
  border: solid 1px #555555;
  display: inline-block;  
}
```


### Examples


[The browser support for display: inline-block](http://caniuse.com/#feat=inline-block) is excellent. You can feel comfortable using it in today’s web environment.