---
title: "960 Grid System"
datePublished: Fri Mar 16 2012 05:00:00 GMT+0000 (Coordinated Universal Time)
cuid: ckrni18m70by6ems1cpp39jcp
slug: from-the-archive-960-grid-system-f2265f7239a0
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1627409926528/GU1-5ODei.png
tags: css, web-design, web-development

---


One amazingly helpful design tool I’ve used recently is the grid system. Specifically on this site, I’m using the 960 Grid System. Essentially, it allows a designer to organize a page into 12 or 16 columns. The standard grid uses 60-pixel columns with 20-pixel gutters in-between, which equals 960 pixels (The project’s namesake). By giving your page elements specific classes defined by 960.gs, you can set how many columns that element will cover.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627409925113/8fGZIcLo6.png)

For example, the following code would create two equal columns of text, each taking up half the available page width (6 out of 12 columns each):

```
<div class="container_12"> <div class="grid_6"> Lorem ipsum dolor sit amet, consectetur adipiscing elit. Curabitur consectetur facilisis laoreet. Quisque accumsan sollicitudin gravida. </div> <div class="grid_6"> Morbi elit lorem, tristique a pharetra et, egestas nec enim. Sed id velit nibh, vitae ullamcorper nunc. Aenean quis faucibus risus. </div> </div>
```


That’s the basic markup you need to get started. However, there are other features of the system I won’t go into detail with here such as pushes, pulls, prefixes, and suffixes. The numerous variants of 960.gs and plugins also add to the capabilities of using a grid.

Grids make prototyping a page easy. Section off portions of your page into columns, then fill them in with content. This is much simpler than calculating pixel widths by hand and designing by trial-and-error. Another benefit I immediately saw was how beautiful a page can look when everything is aligned for you. Logical sections of information just look good to me, and using the 960 Grid System makes that easy.

I don’t necessarily like how most grid systems have designers putting design into the markup. I believe code is easier to maintain when it’s logically separated into design (CSS) and content (HTML). The semantics are arguable but, overall, the advantages gained by using grids can really speed up and simplify development.

You can get more information and see live demonstrations from all around the web at [960.gs](http://960.gs/). There are an amazing amount of sites using 960.gs right now. You can find many tutorials online, as well. I like [this tutorial from Six Revisions](http://sixrevisions.com/web_design/the-960-grid-system-made-easy/).

*This post first appeared on my old blog on March 16, 2012.*