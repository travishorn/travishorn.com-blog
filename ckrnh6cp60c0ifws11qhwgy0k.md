---
title: "Build a Sparkline Vue Component"
datePublished: Sat Aug 15 2020 19:36:58 GMT+0000 (Coordinated Universal Time)
cuid: ckrnh6cp60c0ifws11qhwgy0k
slug: build-a-sparkline-vue-component-d3fdec764145
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1627410177658/Z8PigsLA3.png
tags: javascript, d3js, web-development, vuejs, data-visualisation-1

---


Sparklines can be used to quickly visualize data variance. They are small and intuitive to understand.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627410170335/Oh7Gl0ney.png)

We’ll be using Vue and D3 to build a small [sparkline](https://en.wikipedia.org/wiki/Sparkline) component using [Vue.js](https://vuejs.org/). In the next article, I’ll show you [how to use this component to build a “dashboard”](https://travishorn.com/building-a-sparkline-dashboard-8bbc9c26c401) for easy visualization of many data points.

First, make sure you have the [Vue CLI](https://cli.vuejs.org/) installed. Then, create a new project.

```
vue create sparkboard
```


The CLI will ask you questions about how you’d like to set up your project. You can accept the defaults or manually select features you like. I manually selected…

* Babel

* Linter/Formatter

* ESLint + Prettier

* Lint on save

* In dedicated config files

Once the project has been created, start the dev server.

```
cd sparkboard
npm run serve
```


Create a new component in the `src/components` directory called `SparkLine.vue`

```
<template>
  <svg viewBox="0 0 200 40">
    <path d="M 0 20 H 200"
          fill="none"
          stroke-width="3"
          stroke-color="gold" />
  </svg>
</template>

<script>
export default {
  name: "SparkLine"
}
</script>
```


Notice the root element of the component is an `&lt;svg&gt;`. This is the element we’ll bind D3 to when creating the chart. You can also see that the `&lt;svg&gt;` has a `viewBox` of `0 0 200 40`. This gives it the dimensions of 200 pixels wide by 40 pixels tall. The actual width and height are determined by the element’s container, but the 200 x 40 `viewBox` gives it a good height to width ratio for a sparkline chart.

Finally, there is a `&lt;path&gt;` inside the `&lt;svg&gt;`. This is our actual line. For right now, it’s just a hard-coded, perfectly straight, horizontal line.

We can see our progress so far by putting our component in the main `App.vue`. In that file, replace everything in the template with…

```
<template>
  <div style="width: 100px">
    <SparkLine />
  </div>
</template>
```


Notice how we put the sparkline component inside a container element with a narrow width. This will give it the traditional *small* appearance.

Now, replace the script section with…

```
<script>
import SparkLine from "./components/SparkLine.vue";

export default {
  name: "App",
  components: {
    SparkLine
  }
};
</script>
```


You may notice how we just changed `HelloWorld` to `SparkLine`. Everything else is the same.

Since we cleared out the template, we no longer need the default styles. So feel free to remove everything from the `&lt;style&gt;` section.

Now run the development server.

```
npm run serve
```


With the server running, we can visit [http://localhost:8080/](http://localhost:8080/) and see our progress so far.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627410171856/thFpM5CGj.png)

It is literally just a horizontal yellow line, which is perfect because that’s exactly what we’ve coded so far.

Let’s code a way to pass data into this component so we can display it using this line.

Going back to `SparkLine.vue` and adding to the `script` section, define a prop called `data`. This prop will be an array. We’ll also tell it to simply use a basic array of `[0, 0]` if no data is passed in.

```
<script>
export default {
  name: "SparkLine",
  props: {
    data: {
      type: Array,
      default() {
        return [0, 0];
      }
    }
  }
};
</script>
```


We can pass data into the component back in `App.vue` like so:

```
<SparkLine :data="[808, 1475, 1426, 1884, 1396]" />
```


Note that the line above replaces the existing `&lt;SparkLine /&gt;`. Also note that these are just random values I made up. Feel free to use whatever you like.

Now it’s time to use [D3.js](https://d3js.org/) to bind data to the line. First, install D3.

```
npm i d3
```


Then, in `SparkLine.vue`, import it at the top of the `&lt;script&gt;` section.

```
import * as d3 from "d3";
```


When the component is mounted, we need to tell D3 to set up the chart. So below the `props` definition, add a `mounted()` function.

```
export default {
  name: "SparkLine",
  props: { /* ... snip ... */ },
  mounted() {
    // We will set up the chart here
  }
};
```


Inside the `mounted()` function, tell D3 where our chart will go.

```
this.chart = d3.select("svg"); // Targets the (only) SVG element
```


Now we need to set up x and y scales. The x axis will start at the very start (0) of our chart and end at the very end (right) of our chart (200).

```
this.x = d3.scaleLinear().range([0, 200]);
```


And the y axis will start at the bottom (40) and end at the top (0) of our chart.

```
this.y = d3.scaleLinear().range([40, 0]);
```


If you’ve used D3 scales before, you might be wondering why we’re not setting the domain here. That’s because we’re going to put that logic in a separate function that can get called any time our data changes.

Now we must define the line function.

```
this.line = d3
  .line()
  .x((d, i) => this.x(i))
  .y(d => this.y(d));
```


What the code above does, is defines a D3 line where the x value is determined by the position of the data in the array and the y value is determined by the data point itself.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627410173302/h_fNqdcvQ.png)

Finally, let’s call a method to actually plot the data.

```
this.plot();
```


The function called in the line above doesn’t exist, yet. Let’s create it now.

This will go in our components `methods`.

```
export default {
  name: "SparkLine",
  props: { /* ... snip ... */ },
  methods: {
    plot() {
      // Plotting code goes here
    }
  },
  mounted() { /* ... snip ... /* }
};
```


The first thing to do inside of `plot()` is to set the domain of our scales according to the data.

```
this.x.domain([0, this.data.length - 1]);
this.y.domain(d3.extent(this.data));
```


As you can see above, the x scale’s domain starts at 0 and ends at the end of the array. The y scale’s domain starts with the smallest data value and ends with the largest.

Now to actually adjust the chart line. Also in `plot()`:

```
this.chart.select("path").attr("d", this.line(this.data));
```


If you save `SparkLine.vue` and look again at [http://localhost:8080/](http://localhost:8080/) (assuming the development server is still running), you can see the sparkline!

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627410174651/-fzfpugMt.png)

What happens if our data changes? All we need to do is call the `plot()` method again. Set up a watcher to do that. This watcher will “watch” the `data` property and call `plot()` any time it changes.

```
export default {
  name: "SparkLine",
  props: { /* ... snip ... */ },
  watch: {
    data() {
      this.plot();
    }
  },
  methods: { /* ... snip ... /* },
  mounted() { /* ... snip ... /* }
};
```


And now our component can react to data changes!

While all of this works as a single component, there’s one important bug that we have to fix if we want to re-use this component more than once on the same page. Right now, we have D3 selecting the sole `&lt;svg&gt;` element on the page. If you have more than one `&lt;svg&gt;` on the same page, D3 is just going to select the first one it finds. This is no good. We want to select the `&lt;svg&gt;` in *this* component.

To do that, we can assign our `&lt;svg&gt;` a unique ID.

First, install [nanoid](https://github.com/ai/nanoid).

```
npm i nanoid
```


Import it into our component at the top of the `&lt;script&gt;` section.

```
import { nanoid } from "nanoid";
```


Create a new `id` data property and use nanoid to make it random.

```
export default {
  name: "SparkLine",
  props: { /* ... snip ... /* },
  data() {
    return {
      id: `chart-${nanoid()}`
    };
  },
  methods: { /* ... snip ... /*},
  mounted() { /* ... snip ... /*}
};
```


We use the template string ``chart-${nanoid()}`` so that our random id is in the format `chart-kHM_8K1yz8GGq6MVBwfoG` or `chart-VG5J13fYZIdMBrSDRuEBX` for example. It’s always random.

In the template, set the `&lt;svg&gt;`'s `id` to this new id.

```
<svg :id="id" viewBox="0 0 200 40">
```


Finally, tell D3 to select the element based on this ID.

Replace

```
this.chart = d3.select("svg");
```


with

```
this.chart = d3.select(`#${this.id}`);
```


That’s it Our sparkline component is done! You can add multiple sparkline components on the same page and they’ll all show up perfectly.

```
<template>
  <div style="width:100px">
    <SparkLine :data="[808, 1475, 1426, 1884, 1396]" />
    <SparkLine :data="[3246, 1941, 2649, 1633, 1262]" />
    <SparkLine :data="[190, 128, 209, 208, 116]" />
  </div>
</template>
```


![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627410176055/zCGy7mRfo.png)

All code for this article is open source and published on GitHub.
[**travishorn/vue-sparkline-component**
*Basic sparkline component for Vue.js using D3.js npm install npm run serve npm run build npm run lint See Configuration…*github.com](https://github.com/travishorn/vue-sparkline-component)

In the next article, I’ll show you [how to use this component to build a “dashboard”](https://travishorn.com/building-a-sparkline-dashboard-8bbc9c26c401) for easy visualization of many data points.

### Exercise to the reader

The component is hard-coded to have a yellow line. What if you wanted to be able to set the color individually per component? Hint: use a `prop` (and don’t forget to set a default value!).