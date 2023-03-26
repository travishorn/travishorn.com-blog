---
title: "Self-contained D3 Pie Chart Function"
datePublished: Mon Oct 07 2019 22:01:03 GMT+0000 (Coordinated Universal Time)
cuid: ckrnh3i950bmrems1d4ue5543
slug: self-contained-d3-pie-chart-function-e5b7422be676
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1627410205962/UvYnTKwKU.png
tags: javascript, d3js, web-development, data-visualisation-1

---


D3 is a great data visualization library. I often find myself coding the same types of charts over and over again. I decided that it would be wise to build each basic chart type I use into a single self-contained function that can be included and called on any page. These functions should create a static chart from a set of data and insert it into any element.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627410196869/8Iw0O6_eg.png)

This post is the second in a series where I write a function for each chart type. In this post, we’ll be writing a pie chart function. [In the last one, we wrote a bar chart function.](https://travishorn.com/self-contained-d3-bar-chart-function-a41417beda8a)

### The Goal

Say we have data in this format.

```
[
  { name: 'Option A', value: 107 },
  { name: 'Option B', value: 31 },
  { name: 'Option C', value: 635 },
]
```


And we want to visualize it like this.

![A simple pie chart with three data points.](https://cdn.hashnode.com/res/hashnode/image/upload/v1627410198510/CLopa2cZoQ.png)*A simple pie chart with three data points.*

### The Function

Just like the bar chart, let’s write a function that accepts two arguments.

```
const pieChart = (selector, data) => {
  // All the rest of the code goes here
};
```


The first parameter, `selector`, will be the DOM element in which to add the chart. The second parameter, `data`, will be the data to visualize.

At the top of the function, we need to define some constants.

```
const size = 500;
const fourth = size / 4;
const half = size / 2;
const labelOffset = fourth * 1.4;
const total = data.reduce((acc, cur) => acc + cur.value, 0);
const container = d3.select(selector);
```


This is very similar to the constants in the previous post. Although they’re not exactly the same, we set some sizing constants first. These are `size`, `fourth`, `half`, and `labelOffset`.

The `total` constant simply sums up the data values. We’ll reference this number later.

The `container` constant uses a D3 selector to select the passed in element.

Next we create the chart element itself inside the container.

```
const chart = container.append('svg')
  .style('width', '100%')
  .attr('viewBox', `0 0 ${size} ${size}`);
```


Now for the plot area. This is a container element that starts exactly in the middle of the chart. This is a good reference point for where to place the pie slices.

```
const plotArea = chart.append('g')
  .attr('transform', `translate(${half}, ${half})`);
```


### Scales

There’s only one scale this time: the color scale. We use this to color each slice based on the data names.

```
const color = d3.scaleOrdinal()
  .domain(data.map(d => d.name))
  .range(d3.schemeCategory10);
```


### Calculating the slices

We calculate the slices using three D3 methods.

```
const pie = d3.pie()
  .sort(null)
  .value(d => d.value);
  
const arcs = pie(data);
  
const arc = d3.arc()
  .innerRadius(0)
  .outerRadius(fourth);
```


`d3.pie` is the base of the calculations. By default, D3 sorts our data from largest to smallest. In my case, I want to keep data points in the order they’re defined, so I pass `.sort(null)`. Then I define where the data values come from.

Next we tie in our pie function with our data.

Finally we create an `arc` function which we’ll use to calculate the SVG [path](https://developer.mozilla.org/en-US/docs/Web/SVG/Element/path)s. The inner radius is 0 so our slices start in the center of the chart. The outer radius goes out to a fourth of the total chart size.

To understand the concept of the pie going out the a fourth the chart size, it may be helpful to visualize the chart broken into 4 pieces.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627410199956/X5fdripvT.png)

We’ll position the labels with arcs, too.

```
const arcLabel = d3.arc()
  .innerRadius(labelOffset)
  .outerRadius(labelOffset);
```


We defined `labelOffset` earlier as 1.4 times a fourth of the chart size. This spaces the labels away from the slices a bit. Increase this number for farther-away labels. Decrease it for closer or overlapping labels.

### Plotting the Slices

Now we can plot the slices by appending `path`s to our chart.

```
plotArea.selectAll('path')
  .data(arcs)
  .enter()
  .append('path')
  .attr('fill', d => color(d.data.name))
  .attr('stroke', 'white')
  .attr('d', arc);
```


![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627410201475/bg_41mM77m.png)

### Labels

It looks good but we still need some labels.

```
const labels = plotArea.selectAll('text')
  .data(arcs)
  .enter()
  .append('text')
  .style('text-anchor', 'middle')
  .style('alignment-baseline', 'middle')
  .style('font-size', '20px')
  .attr('transform', d => `translate(${arcLabel.centroid(d)})`)
```


Each label is positioned based on a D3 arc method called `centroid`. It places the label near the center of the arc.

While the code block above does add the `text` elements. They are empty at the moment.

First add the data names by appending a `tspan` to each `text` label.

```
labels.append('tspan')
  .attr('y', '-0.6em')
  .attr('x', 0)
  .style('font-weight', 'bold')
  .text(d => `${d.data.name}`);
```


![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627410202913/RrpXxuL6T.png)

Then add the data values and percentages by appending another `tspan`.

```
labels.append('tspan')
  .attr('y', '0.6em')
  .attr('x', 0)
  .text(d => `${d.data.value} (${Math.round(d.data.value / total * 100)}%)`);
```


The values appear first, then the percentage is calculated inside of parenthesis.

That’s it! Our pie chart is complete.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627410204453/qtsCSsbr1.png)

### Putting it all Together

The entire function looks like this.

```
const pieChart = (selector, data) => {
  const size = 500;
  const fourth = size / 4;
  const half = size / 2;
  const labelOffset = fourth * 1.4;
  const total = data.reduce((acc, cur) => acc + cur.value, 0);
  const container = d3.select(selector);
  
  const chart = container.append('svg')
    .style('width', '100%')
    .attr('viewBox', `0 0 ${size} ${size}`);
  
  const plotArea = chart.append('g')
    .attr('transform', `translate(${half}, ${half})`);
  
  const color = d3.scaleOrdinal()
    .domain(data.map(d => d.name))
    .range(d3.schemeCategory10);

const pie = d3.pie()
    .sort(null)
    .value(d => d.value);
  
  const arcs = pie(data);
  
  const arc = d3.arc()
    .innerRadius(0)
    .outerRadius(fourth);
  
  const arcLabel = d3.arc()
    .innerRadius(labelOffset)
    .outerRadius(labelOffset);
  
  plotArea.selectAll('path')
    .data(arcs)
    .enter()
    .append('path')
    .attr('fill', d => color(d.data.name))
    .attr('stroke', 'white')
    .attr('d', arc);
  
  const labels = plotArea.selectAll('text')
    .data(arcs)
    .enter()
    .append('text')
    .style('text-anchor', 'middle')
    .style('alignment-baseline', 'middle')
    .style('font-size', '20px')
    .attr('transform', d => `translate(${arcLabel.centroid(d)})`)
    
  labels.append('tspan')
    .attr('y', '-0.6em')
    .attr('x', 0)
    .style('font-weight', 'bold')
    .text(d => `${d.data.name}`);
  
  labels.append('tspan')
    .attr('y', '0.6em')
    .attr('x', 0)
    .text(d => `${d.data.value} (${Math.round(d.data.value / total * 100)}%)`);
};
```


This function has only one dependency: D3.

See the pen below for full code and a demonstration.
