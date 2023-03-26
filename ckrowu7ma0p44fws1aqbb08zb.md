---
title: "Fireflies with canvas and requestAnimationFrame"
datePublished: Tue Jun 30 2015 01:35:00 GMT+0000 (Coordinated Universal Time)
cuid: ckrowu7ma0p44fws1aqbb08zb
slug: fireflies-with-canvas-and-requestanimationframe-e212e9589679
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1627409824731/foKxFH-uS.png
tags: javascript, web-design, animation, css-animation

---


I’ve added a new public pen to my CodePen account called [Fireflies](http://codepen.io/travishorn/pen/OVzeqg/). It’s a simulation of fireflies (lighting bugs) floating and lighting up in the night. This animation uses canvas and requestAnimationFrame. You can set various options in the JavaScript code. Look for the “constant” variables (ALL_CAPS).

[Go to the Pen](http://codepen.io/travishorn/pen/OVzeqg/)

You can see and edit the source code on CodePen, but I’ll write a little bit about how it’s made here.

The only required HTML is a &lt;canvas&gt; element with an id attribute.

```
<canvas id="canvas"></canvas>
```


The only required CSS is to make the body and canvas elements have a black background.

```
body, canvas {
  background-color: #000;
}
```


The magic happens in JavaScript, of course. First, we get a 2D context of our canvas element.

```
var canvas = document.getElementById('canvas');
var ctx = canvas.getContext('2d');
```


Then, we set up some constants. You can play with these to get different firefly effects.

```
var MAX_FLIES = 15;
var FLY_XSPEED_RANGE = [-1, 1];
var FLY_YSPEED_RANGE = [-0.5, 0.5];
var FLY_SIZE_RANGE = [1, 5];
var FLY_LIFESPAN_RANGE = [75, 150];
```


We’ll need an array to keep track of the currently displayed flies.

```
var flies = [];
```


Throughout the code, there will be many times we’ll need to get a random number within a certain range. I use a simple function for this.

```
function randomRange(min, max) {
  return Math.random() * (max - min) + min;
}
```


Now we’ll need a Fly object from which all instantiated flies will get their prototype. This object contains properties including the position of the fly, the velocity (xSpeed and ySpeed), size, lifespan, current age, and color.

```
function Fly(options) {
  if (!options) { options = {}; }

  this.x = options.x || randomRange(0, canvas.width);
  this.y = options.y || randomRange(0, canvas.height);
   this.xSpeed = options.xSpeed || randomRange(FLY_XSPEED_RANGE[0], FLY_XSPEED_RANGE[1]);
  this.ySpeed = options.ySpeed || randomRange(FLY_YSPEED_RANGE[0], FLY_YSPEED_RANGE[1]);
  this.size = options.size || randomRange(FLY_SIZE_RANGE[0], FLY_SIZE_RANGE[1]);
  this.lifeSpan = options.lifeSpan || randomRange(FLY_LIFESPAN_RANGE[0], FLY_LIFESPAN_RANGE[1]);
  this.age = 0;
  this.colors = options.colors || { red: 207, green: 255, blue: 4, alpha: 0 };
}
```


The next function is a little funky, and I’ve had to modify it to fit specific browsers (or the display frame on CodePen). It basically just makes an element fit to the full width and height of the screen.

```
function fitToScreen(element) {
  element.width = window.innerWidth - 8;
  element.height = window.innerHeight - 37;
}
```


Next, we’ll need a function for clearing the canvas. It simply draws a black rectangle over the entire canvas. You’ll see later that we actually call this every single frame, and re-draw our flies on a later step.

```
function clearScreen() {
  ctx.beginPath();
  ctx.fillStyle = 'rgb(0, 0, 0)';
  ctx.rect(0, 0, canvas.width, canvas.height);
  ctx.fill();
}
```


This function will create a fly as long as we haven’t hit our max (set earlier).

```
function createFlies() {
  if (flies.length !== MAX_FLIES) {
    flies.push(new Fly());
  }
}
```


This next step is pretty important. It loops through all our flies, and….

* updates their position on the canvas

* increments their age

* sets their opacity based on their age and lifespan (in this case, we’re making them start out invisible, fade in as they near half their “life”, and fade back out until they’re invisible again)

Code:

```
function moveFlies() {
  flies.forEach(function(fly) {
    fly.x += fly.xSpeed;
    fly.y += fly.ySpeed;
    fly.age++;

    if (fly.age < fly.lifeSpan / 2) {
      fly.colors.alpha += 1 / (fly.lifeSpan / 2);

      if (fly.colors.alpha > 1) { fly.colors.alpha = 1; }
    } else {
      fly.colors.alpha -= 1 / (fly.lifeSpan / 2);

      if (fly.colors.alpha < 0) { fly.colors.alpha = 0; }
    }
  });
}
```


Now we’ll loop through the flies and see if any individual fly’s age is equal or greater than their lifespan. If so, we’ll remove them from the array (and consequently, from the canvas). Don’t worry, on the next tick our script will see that there is room for another fly and instantiate a new one.

```
function removeFlies() {
  var i = flies.length;

   while (i--) {
     var fly = flies[i];

     if (fly.age >= fly.lifeSpan) {
       flies.splice(i, 1);
     }
  }
}
```


Here’s the function that actually draws the flies on the canvas. We loop through the flies array and add a new full arc (circle) for each one, with the attributes described by the fly’s object.

```
function drawFlies() {
  flies.forEach(function(fly) {
    ctx.beginPath();
    ctx.fillStyle = 'rgba(' + fly.colors.red + ', ' + fly.colors.green + ', ' + fly.colors.blue + ', ' + fly.colors.alpha + ')';
    ctx.arc( fly.x, fly.y, fly.size, 0, Math.PI * 2, false );
    ctx.fill();
  });
}
```


Almost done. We’ll set up a function to call each of our other functions in the correct order:

1. Clear the screen

1. Create new flies if we haven’t hit the max

1. Move the existing flies

1. Remove old flies that are past their lifespan

1. Draw flies on the canvas

Code:

```
function render() {
  clearScreen();
  createFlies();
  moveFlies();
  removeFlies();
  drawFlies();
}
```


I’m going to go ahead and fit the canvas to the screen, and watch for a window re-size. If the window is re-sized, we’ll re-fit the canvas.

```
window.addEventListener('resize', function() {
  fitToScreen(canvas);
});

fitToScreen(canvas);
```


Finally, we’ll set up an animation loop by calling requestAnimationFrame, rendering our flies, and repeating.

```
(function animationLoop() {
  window.requestAnimationFrame(animationLoop); render();
})();
```

%[https://codepen.io/travishorn/pen/OVzeqg]
