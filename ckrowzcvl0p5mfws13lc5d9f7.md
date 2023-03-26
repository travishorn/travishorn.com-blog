---
title: "Visualizing SVG Elements"
datePublished: Thu Jan 04 2018 23:01:06 GMT+0000 (Coordinated Universal Time)
cuid: ckrowzcvl0p5mfws13lc5d9f7
slug: visualizing-svg-elements-debd4e1274fb
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1627409810665/5harach0s.jpeg
tags: web-development, svg, data-visualisation-1

---


Scalable Vector Graphics, or SVG, is a language for creating vector graphics. Vector graphics are awesome because they have excellent browser support and can be used to show illustrations and data visualizations.

There are a number of elements that SVG provides to display shapes on a screen. Some of these include

* Circles

* Rectangles

* Lines

* Paths

* and many more

Keep reading to get a better understanding of how these elements work.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627409808343/8gDns4X16.jpeg)

### &lt;circle&gt;

The simplest `&lt;circle&gt;` includes three attributes:

* cx — The position of the center of the circle along the X axis

* cy — The position of the center of the circle along the Y axis

* r — The radius of the circle

Run the pen below to visualize a `&lt;circle&gt;`. You can adjust the range input sliders to change the values of the attributes described above.

%[https://codepen.io/travishorn/pen/LOjGwB]

### &lt;rect&gt;

The simplest `&lt;rect&gt;` includes four attributes:

* x — The position of the upper-left corner of the rectangle along the X axis

* y — The position of the upper-left corner of the rectangle along the Y axis

* width — The width of the rectangle

* height — The height of the rectangle

Run the pen below to visualize a `&lt;rect&gt;`. You can adjust the range input sliders to change the values of the attributes described above.

%[https://codepen.io/travishorn/pen/zPdvXv]

### &lt;path&gt;

Paths are like lines that you direct around the SVG canvas.

The simplest `&lt;path&gt;` includes one attribute:

* d — Defines a path to follow

Within the `d` attribute, you can direct the path to…

* move (M)— “Jump” to a point without drawing a line

* line (L)— Draw a line to the point described

* curve (C or Q)— Draw a curved line to the point described

* arc (A)— Another method of drawing a curved line to the point described

* close (Z) — Draw a line from the current point to the first point

Run the pen below to visualize a `&lt;path&gt;`. You can adjust the range input sliders to change the `d` attribute.

%[https://codepen.io/travishorn/pen/LOjoOX]

### Bezier curves in &lt;path&gt;

You can describe a curve in a path using the bezier method. Inside the `d` attribute, after giving the `C` command, you write numbers describing the curve:

* The X value of the first control point

* The Y value of the first control point

* The X value of the second control point

* The Y value of the second control point

* The X value of the curve destination

* The Y value of the curve destination

Run the pen below to visualize a `&lt;path&gt;` with a bezier curve. You can adjust the range input sliders to change the values of the curve described above.

%[https://codepen.io/travishorn/pen/xPrrPV]

### Quadratic curves in &lt;path&gt;

Another way you can describe a bezier curve in a path is by using the quadratic method. Inside the `d` attribute, after giving the `Q` command, you write numbers describing the curve:

* The X value of the control point

* The Y value of the control point

* The X value of the curve destination

* The Y value of the curve destination

Run the pen below to visualize a `&lt;path&gt;` with a quadratic curve. You can adjust the range input sliders to change the values of the curve described above.

%[https://codepen.io/travishorn/pen/bYRRax]

You can combine all of these elements to create an illustration or data visualization on the web. Additionally, there are many more elements and features SVG provides to facilitate drawings like this. You can learn more in [MDN’s SVG Tutorial](https://developer.mozilla.org/en-US/docs/Web/SVG/Tutorial).