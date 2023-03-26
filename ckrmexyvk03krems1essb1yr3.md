---
title: "Google-style gauge charts using D3"
datePublished: Thu Jan 11 2018 23:01:02 GMT+0000 (Coordinated Universal Time)
cuid: ckrmexyvk03krems1essb1yr3
slug: google-style-gauge-charts-using-d3-588a3c1677af
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1627410480732/0dzo-giMt.jpeg
tags: d3js, web-development, svg, data-visualisation-1

---


Google provides a free charting library to display live charts on your site. It works pretty well but has some disadvantages:

* The library must be loaded from Google’s servers

* Loading can be slow at times

* Limited customization

All of their charts can be re-created using an open-source library like [D3.js](https://d3js.org/) but some of them are easier to re-create than others. Recently, I wanted to switch over a site that was using Google Charts’s gauge chart. I didn’t want to rely on Google anymore.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627410477489/GPP7cZD01.jpeg)

I was successful in re-creating the gauge chart with basic functionality using D3.js. Here’s how to do it from start to finish.

First, let’s add some styles. These will mostly be colors, as sizes will be defined in JavaScript based on the container element’s dimensions.

```
<style>
  .gaugeChart-bezel-outer {
    fill: none;
    stroke: black;
  }

  .gaugeChart-bezel-inner {
    fill: none;
    stroke: #CCCCCC;
  }

  .gaugeChart-tick {
    fill: none;
    stroke: black;
  }

  .gaugeChart-face,
  .gaugeChart-tickHider {
    stroke-width: 0;
    fill: white;
  }

  .gaugeChart-needle {
    stroke: #CD4120;
    fill: #E57358;
    opacity: 0.8;
  }

  .gaugeChart-needle-cap {
    stroke: #666666;
    fill: #4684EE;
  }
</style>
```


These colors resemble the Google chart, but feel free to modify them any way you’d like.

With styles out of the way, we can work on the JavaScript. The first order of business is to include D3.js.

```
<script src="https://cdnjs.cloudflare.com/ajax/libs/d3/4.11.0/d3.min.js"></script>
```


Then, write an `&lt;svg&gt;` element that our chart will bind to. This element must have a `width` and `height`. For best results, make it square.

```
<svg id="memChart" width="110" height="110"></svg>
```


Everything will be contained in a single function that accepts an options object.

```
const gaugeChart = (o) => {
  /* Build the chart here */
};
```


This is how the function is called:

```
gaugeChart({
  el: '#memChart',
  label: 'Memory',
  value: 73,
});
```


Inside the `gaugeChart()` function, use D3.js to select the `&lt;svg&gt;` element.

```
const chart = d3.select(o.el);
```


Now, get the width of this chart.

```
const width = chart.attr('width');
```


We will now set a plethora of heights, widths, and spacing relative to this width.

```
const center = width / 2;
const outerBezelWidth = width * 0.009;
const outerBezelRadius = center - outerBezelWidth;
const innerBezelWidth = width * 0.072;
const innerBezelRadius = outerBezelRadius - (innerBezelWidth / 2);
const tickHeight = outerBezelWidth + innerBezelWidth + (width * 0.027);
const tickWidth = width * 0.009;
const tickHiderRadius = width * 0.345;
const labelY = center / 1.3;
const valueLabelY = width * 0.75;
const labelFontSize = width * .13;
const needleWidth = width * 0.054;
const needleCapRadius = width * 0.059;
```


Feel free to customize these numbers to change the sizing on any of the chart’s components.

Before we move on, we will also set some values for the ticks.

```
const tickSpacing = 13.5;
const lastTickAngle = 135;
let angle = -135;
```


You may notice the last value is a variable rather than a constant. This is because we’ll use it later in a loop to build the ticks around the gauge.

Next, we need a linear scale.

The domain should be the min & max of the input data. By default, I made it 0 and 100, but this can also be set as an option when calling the function.

The range is going to be the angle of the first tick and the angle of the last tick.

```
const needleScale = d3.scaleLinear()
  .domain([o.min || 0, o.max || 100])
  .range([angle, lastTickAngle]);
```


Using this scale, we can get the angle the corresponds to the input value.

```
const needleAngle = needleScale(o.value);
```


With all of that out of the way, we can start adding elements to build the gauge.

First is the outer bezel (the thin black circle).

```
const outerBezel = chart.append('circle')
  .attr('class', 'gaugeChart-bezel-outer')
  .attr('cx', center)
  .attr('cy', center)
  .attr('stroke-width', outerBezelWidth)
  .attr('r', outerBezelRadius);
```


Next is the face of the gauge (a white circle) inside the bezel.

```
const face = chart.append('circle')
  .attr('class', 'gaugeChart-face')
  .attr('cx', center)
  .attr('cy', center)
  .attr('r', outerBezelRadius - 1);
```


Next is the inner bezel (the gray circle) inside the outer bezel.

```
const innerBezel = chart.append('circle')
  .attr('class', 'gaugeChart-bezel-inner')
  .attr('cx', center)
  .attr('cy', center)
  .attr('stroke-width', innerBezelWidth)
  .attr('r', innerBezelRadius);
```


Now, we can use a `while` loop to create the ticks. We loop until the angle is equal to or greater than the last tick angle, adding a tick mark during each iteration.

```
while (angle <= lastTickAngle) {
  chart.append('line')
    .attr('class', 'gaugeChart-tick')
    .attr('x1', center)
    .attr('y1', center)
    .attr('x2', center)
    .attr('y2', tickHeight)
    .attr('stroke-width', tickWidth)
    .attr('transform', `rotate(${angle} ${center} ${center})`);
  
  angle += tickSpacing;
}
```


You may notice that the tick marks are lines that extend from the center of the gauge to the outer edge.

We only want the tips of these lines showing, so add a new circle of the same color as the gauge face. This circle will be placed on top of the ticks to hide most of each of them. Only the outer tips will still be visible.

```
const tickHider = chart.append('circle')
  .attr('class', 'gaugeChart-tickHider')
  .attr('cx', center)
  .attr('cy', center)
  .attr('r', tickHiderRadius);
```


Now the label.

```
const label = chart.append('text')
  .attr('class', 'gaugeChart-label')
  .attr('x', center)
  .attr('y', labelY)
  .attr('text-anchor', 'middle')
  .attr('alignment-baseline', 'middle')
  .attr('font-size', labelFontSize)
  .text(o.label);
```


And the value label (the actual number that will appear near the bottom of the gauge).

```
const valueLabel = chart.append('text')
  .attr('class', 'gaugeChart-label-value')
  .attr('x', center)
  .attr('y', valueLabelY)
  .attr('text-anchor', 'middle')
  .attr('alignment-baseline', 'middle')
  .attr('font-size', labelFontSize)
  .text(o.value);
```


The needle itself is the most complex part of the drawing. It is a triangle made using a `&lt;path&gt;`. By default, it is pointing straight up (0 degrees). But then we rotate it based on the `needleAngle` set earlier.

```
const needle = chart.append('path')
  .attr('class', 'gaugeChart-needle')
  .attr('stroke-width', outerBezelWidth)
  .attr('d', `M ${center - needleWidth / 2} ${center}
              L ${center} ${tickHeight}
              L ${center + needleWidth / 2} ${center} Z`)
  .attr('transform', `rotate(${needleAngle} ${center} ${center})`);
```


Finally, we add the “cap” that covers the base of the needle.

```
const needleCap = chart.append('circle')
  .attr('class', 'gaugeChart-needle-cap')
  .attr('cx', center)
  .attr('cy', center)
  .attr('stroke-width', outerBezelWidth)
  .attr('r', needleCapRadius);
```


The entire function looks like this:

```
const gaugeChart = (o) => {
  const chart = d3.select(o.el);
  const width = chart.attr('width');
  const center = width / 2;
  const outerBezelWidth = width * 0.009;
  const outerBezelRadius = center - outerBezelWidth;
  const innerBezelWidth = width * 0.072;
  const innerBezelRadius = outerBezelRadius - (innerBezelWidth / 2);
  const tickHeight = outerBezelWidth + innerBezelWidth + (width * 0.027);
  const tickWidth = width * 0.009;
  const tickHiderRadius = width * 0.345;
  const labelY = center / 1.3;
  const valueLabelY = width * 0.75;
  const labelFontSize = width * .13;
  const needleWidth = width * 0.054;
  const needleCapRadius = width * 0.059;
  const tickSpacing = 13.5;
  const lastTickAngle = 135;
  let angle = -135;
  
  const needleScale = d3.scaleLinear()
    .domain([o.min || 0, o.max || 100])
    .range([angle, lastTickAngle]);
  
  const needleAngle = needleScale(o.value);
  
  const outerBezel = chart.append('circle')
    .attr('class', 'gaugeChart-bezel-outer')
    .attr('cx', center)
    .attr('cy', center)
    .attr('stroke-width', outerBezelWidth)
    .attr('r', outerBezelRadius);
  
  const face = chart.append('circle')
    .attr('class', 'gaugeChart-face')
    .attr('cx', center)
    .attr('cy', center)
    .attr('r', outerBezelRadius - 1);
  
  const innerBezel = chart.append('circle')
    .attr('class', 'gaugeChart-bezel-inner')
    .attr('cx', center)
    .attr('cy', center)
    .attr('stroke-width', innerBezelWidth)
    .attr('r', innerBezelRadius);
  
  while (angle <= lastTickAngle) {
    chart.append('line')
      .attr('class', 'gaugeChart-tick')
      .attr('x1', center)
      .attr('y1', center)
      .attr('x2', center)
      .attr('y2', tickHeight)
      .attr('stroke-width', tickWidth)
      .attr('transform', `rotate(${angle} ${center} ${center})`);
    
    angle += tickSpacing;
  }
  
  const tickHider = chart.append('circle')
    .attr('class', 'gaugeChart-tickHider')
    .attr('cx', center)
    .attr('cy', center)
    .attr('r', tickHiderRadius);
  
  const label = chart.append('text')
    .attr('class', 'gaugeChart-label')
    .attr('x', center)
    .attr('y', labelY)
    .attr('text-anchor', 'middle')
    .attr('alignment-baseline', 'middle')
    .attr('font-size', labelFontSize)
    .text(o.label);
  
  const valueLabel = chart.append('text')
    .attr('class', 'gaugeChart-label-value')
    .attr('x', center)
    .attr('y', valueLabelY)
    .attr('text-anchor', 'middle')
    .attr('alignment-baseline', 'middle')
    .attr('font-size', labelFontSize)
    .text(o.value);
  
  const needle = chart.append('path')
    .attr('class', 'gaugeChart-needle')
    .attr('stroke-width', outerBezelWidth)
    .attr('d', `M ${center - needleWidth / 2} ${center}
                L ${center} ${tickHeight}
                L ${center + needleWidth / 2} ${center} Z`)
    .attr('transform', `rotate(${needleAngle} ${center} ${center})`);
  
  const needleCap = chart.append('circle')
    .attr('class', 'gaugeChart-needle-cap')
    .attr('cx', center)
    .attr('cy', center)
    .attr('stroke-width', outerBezelWidth)
    .attr('r', needleCapRadius);
};
```


Once again, if we have an `&lt;svg&gt;` container like this:

```
<svg id="memChart" width="110" height="110"></svg>
```


We can call the function like this:

```
gaugeChart({
  el: '#memChart',
  label: 'Memory',
  value: 73,
});
```


The result is a nice gauge chart.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627410479292/thPwKzEKj.png)

Here is an example pen with all of the code contained in this post:
