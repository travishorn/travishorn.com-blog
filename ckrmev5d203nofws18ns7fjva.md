---
title: "Create a Bouncing Message Notification Icon from Scratch with SVG & CSS"
datePublished: Thu Oct 19 2017 22:01:00 GMT+0000 (Coordinated Universal Time)
cuid: ckrmev5d203nofws18ns7fjva
slug: create-a-bouncing-message-notification-icon-from-scratch-with-svg-css-2d9c446ea0ab
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1627410504223/t57dPlEtW.png
tags: css, web-development, animation, svg, css-animation

---


Let’s see how easy it is to create a simple icon with SVG and animate it with CSS.

![The end result will look similar to this. Just imagine it bouncing up and down.](https://cdn.hashnode.com/res/hashnode/image/upload/v1627410499899/n8t69myM-j.png)*The end result will look similar to this. Just imagine it bouncing up and down.*

Start off with a new `[&lt;svg&gt;`](https://developer.mozilla.org/en-US/docs/Web/SVG/Element/svg) element in your HTML.

```
<svg version="1.1"
     baseProfile="full"
     width="800"
     height="600"
     viewBox="0 0 800 600"
     xmlns="[http://www.w3.org/2000/svg](http://www.w3.org/2000/svg)">

</svg>
```


It is 800 pixels wide by 600 pixels tall, which is a huge area for a notification icon. However, it also has a `[viewBox`](https://developer.mozilla.org/en-US/docs/Web/SVG/Attribute/viewBox) attribute with those same dimensions. This will allow you to resize the `&lt;svg&gt;` element to any size you need without cutting off the contents.

Using CSS, set some alignment and color styles.

```
body,
svg { background-color: black; }

body { text-align: center; }

.icon{
  fill: none;
  stroke: white;
}
```


For the sake of example, my page has a black background, so I want my icon to have a black background, too. I also want it centered on the page. The icon itself will be transparent with a white outline. Feel free to modify the colors to your liking.

The icon is created using a `[&lt;path&gt;`](https://developer.mozilla.org/en-US/docs/Web/SVG/Element/path). Add it inside your `&lt;svg&gt;` element.

```
<path class="icon"
      d="" />
```


The `[d` attribute](https://developer.mozilla.org/en-US/docs/Web/SVG/Attribute/d) defines the path this element will follow.

Start out by **moving** the cursor to the 0, 0 position (top left) and creating a **line** to the 800, 0 position (top right).

```
d="M   0   0
   L 800   0"
```


If you’ve done everything correctly so far, you should see a black page with a thin white horizontal line at the top.

Now for the right edge, create a **line** down to 800, 500. The reason we stop at 500 instead of going all the way to the bottom (600) is because we want our notification icon to have a “callout” style tail on the bottom, so we leave room for that.

```
d="M   0   0
   L 800   0 
   L 800 500"
```


Normally, to complete a rectangle, you would **line** to the bottom left corner. But we’re not creating a perfect rectangle. We want the middle third to come down to a point. Dividing 800 / 3 gives us 267 (I rounded). So what you really want at this point is to draw a horizontal line that stops at two thirds (2 * 267 = 534) along the bottom.

```
d="M   0   0
   L 800   0
   L 800 500
   L 534 500"
```


Now a line that comes down to the very bottom (600) at exactly the middle of the icon (800 / 2 = 400).

```
d="M   0   0
   L 800   0
   L 800 500
   L 534 500
   L 400 600"
```


And back up to one third, and then to the bottom left corner, and finally to the top right corner again.

```
d="M   0   0
   L 800   0
   L 800 500
   L 534 500
   L 400 600
   L 267 500
   L   0 500
   L   0   0"
```


At this point, we have a nice notification icon!

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627410501381/EvE3Nr48P.png)

But it looks a little empty. To give it some substance, add three stacked horizontal lines. I’ll skip the math required to place them exactly, but the basic idea is to **move** the cursor to the start of the line, then **line** to the end of the line. Repeat for the other two lines. The final `&lt;path&gt;` looks like this:

```
d="M   0   0
   L 800   0 
   L 800 500
   L 534 500
   L 400 600
   L 267 500
   L   0 500
   L   0   0
   
   M 100 125
   L 700 125
   M 100 250
   L 700 250
   M 100 375
   L 700 375"
```


![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627410502727/-g9Mgdpq5.png)

Making it bounce is done through CSS animations.

Wrap the `&lt;path&gt;` in a `[&lt;g&gt;` element](https://developer.mozilla.org/en-US/docs/Web/SVG/Element/g) with the `bounce` class.

```
<g class="bounce">
  <path class="icon"
        d="..snip.." />
</g>
```


In CSS, apply a `bounce` animation to that class.

```
.bounce { animation: 2s bounce infinite; }
```


The animation plays out over 2 seconds repeatedly forever.

The `bounce` animation will only have a single keyframe. Again, in your CSS:

```
@keyframes bounce {
  50% { transform: translateY(25px); }
}
```


The code above basically says that…

* at the start of the animation, the icon appears in its default position

* at 50% through the animation, the icon appears translated along the Y axis (vertically moved) 25 pixels

* at the end of the animation, the icon appears back in its default position

This gives the `&lt;g&gt;` a bouncing effect. The icon bounces now!

But there’s a problem. When it moves downward, the tail on the bottom is cut off. An easy fix is to wrap everything in another `&lt;g&gt;` that will scale down the icon.

```
<g transform="scale(.96)>
  <g class="bounce">
    <path class="icon" ..snip..>
  </g>
</g>
```


The `[transform` attribute](https://developer.mozilla.org/en-US/docs/Web/SVG/Attribute/transform) `scale`s everything in the `&lt;g&gt;` to 96% it’s original size, leaving room for the icon to bounce without cutting it off.

Now you have a great looking bouncing notification icon made purely in SVG & CSS.

### Bonus!

You can create the illusion of the icon being “drawn” on the screen in front of you by using a trick with the `[stroke-dasharray`](https://developer.mozilla.org/en-US/docs/Web/SVG/Attribute/stroke-dasharray) and `[stroke-dashoffset`](https://developer.mozilla.org/en-US/docs/Web/SVG/Attribute/stroke-dashoffset) attributes.

For the first part of the trick, you’ll need to know the total length of the path. Use JavaScript to find out:

```
> document.querySelector('.icon').getTotalLength();
4466.6005859375
```


`stroke-dasharray` controls the pattern of dashes and gaps on the line. In CSS, set the path’s `stroke-dasharray` to the total length of itself.

```
.icon {
  fill: none;
  stroke: white;
  **stroke-dasharray: 4467;**
}
```


This seems unnecessary at first. But now use `stroke-dashoffset` to force the dash to start after the end of the path.

```
.icon {
  fill: none;
  stroke: white;
  stroke-dasharray: 4467;
  **stroke-dashoffset: 4467;**
}
```


The icon disappears completely. We went from unnecessary to broken!

For the final part of the trick, add a `draw` animation to the path that takes 2 seconds to complete and moves forward linearly.

```
.icon {
  fill: none;
  stroke: white;
  stroke-dasharray: 4467;
  stroke-dashoffset: 4467;
  animation: draw 2s linear forwards;
}
```


This animation will have one keyframe.

```
@keyframes draw {
  to { stroke-dashoffset: 0; }
}
```


It moves the `stroke-dashoffset` from it’s default value of 4467 all the way to 0, which reveals the icon. Pretty tricky!

The end result looks like this:
