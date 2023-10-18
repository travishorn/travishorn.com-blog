---
title: "SvelteKit Magic: Server-Side Rendering for Observable Plot Explained"
datePublished: Wed Oct 18 2023 11:00:10 GMT+0000 (Coordinated Universal Time)
cuid: clnvn65xt000109im5wbibqxd
slug: sveltekit-magic-server-side-rendering-for-observable-plot-explained
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1697034733264/546f0c66-fe8d-4d9b-a684-e3b67c645fe9.png
tags: observable, svelte, data-visualization, sveltekit, plot

---

Using a web application framework like [SvelteKit](https://kit.svelte.dev/) opens up possibilities to bring your data visualizations to life on the server side before sending them to the client. In this guide, we'll walk through some of the secrets of server-side rendering (SSR) for plots created with [Observable Plot](https://observablehq.com/plot/) inside the SvelteKit framework. If you're looking to enhance the performance and user experience of your web app by seamlessly rendering charts on the server, you're in the right place.

Say your web app requires dynamic and interactive data visualizations, but your users are being bogged down by slow load times and poor accessibility. Server-side rendering (SSR) provides a solution to this challenge. By generating plots on the server and sending them as pre-rendered HTML to the client, we can significantly improve the initial page load performance. This not only boosts the speed of your application but also ensures that users with varying device capabilities can access your visualizations easily. Read on to understand the process of integrating Observable Plot into SvelteKit, diving into the world of performant server-side rendering to deliver an enhanced user experience.

## Create a new SvelteKit project

You may be able to skip this section if you already have a SvelteKit project that you're working on. Otherwise, continue reading to create a new project.

Make sure you have [Node.js](https://nodejs.org/) installed. Then, open up a terminal and create a new SvelteKit project.

```bash
npm create svelte@latest plot-ssr
```

I called the project **plot-ssr**, but feel free to change the name to anything you wish. The above command will guide you through some prompts to create your new project.

```bash
Need to install the following packages:
  create-svelte@5.1.1
Ok to proceed? (y) y

create-svelte version 5.1.1

┌  Welcome to SvelteKit!
│
◇  Which Svelte app template?
│  Skeleton project
│
◇  Add type checking with TypeScript?
│  Yes, using JavaScript with JSDoc comments
│
◇  Select additional options (use arrow keys/space bar)
│  none
│
└  Your project is ready!
```

Once created, change into the project directory.

```bash
cd plot-ssr
```

Install the dependencies.

```bash
npm install
```

Initialize a new git repository (optional)

```bash
git init && git add -A && git commit -m "Initial commit"
```

Start the development server

```bash
npm run dev -- --open
```

That last command should open the site in your web browser. If not, just go to http://localhost:5173.

## Install the libraries

Keep the terminal with the development server running. Open up a new terminal and change into the project directory.

```bash
cd plot-ssr
```

Install Observable Plot and [JSDOM](https://github.com/jsdom/jsdom) (both are necessary for this solution).

```bash
npm install @observablehq/plot jsdom
```

## Write the server-side code

Create a new file called `src/routes/+page.server.js`. Note that this file should sit right next to the existing `+page.svelte` in the same directory.

```javascript
// Import the necessary libraries
import * as Plot from '@observablehq/plot';
import { JSDOM } from 'jsdom';

// Plot will need a reference to a DOM window document later in the code
const { document } = new JSDOM().window;

/** @type {import('./$types').PageServerLoad} */
export async function load() {
  // Create your plot. Make sure the `document` property points to a DOM
  // window document. Also make sure you are getting the `outerHTML`
  // property (the line at the end).
  const plot = Plot.plot({
    marks: [
      Plot.rectY(
        { length: 10000 },
        Plot.binX({ y: "count" }, { x: Math.random })
      )
    ],
    document
  }).outerHTML;

  // At this point, `plot` contains the HTML string representing the
  // plotted chart. Return it.
  return {
    plot
  };
}
```

This file uses [SvelteKit's load function](https://kit.svelte.dev/docs/load) to load data on the server, and then send it to the page for rendering.

Now, in `+page.svelte`, you can pick up the passed `plot` variable which contains the HTML string for the plot.

```svelte
<script>
  /** @type {import('./$types').PageData} */
  export let data;
</script>

{@html data.plot}
```

This file uses `export let data` to get a reference to the data passed in from `+page.server.js`. The `data` object will contain the `plot` variable we generated earlier.

Use `data.plot` with [Svelte's {@html ...} tag](https://svelte.dev/docs/special-tags#html) to write HTML code directly to the page.

## Result

If you look at the page again (http://localhost:5173), you'll see the plot.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1697030649773/c85207a0-6199-454e-b7d1-5ead21b281d4.png align="center")

You can verify the HTML for the plot is sent directly from the server, and not just generated when the component is mounted by looking at the **Network** tab in Chrome's DevTools:

1. Press **Ctrl** + **Shift** + **I**
    
2. Click on **Network**
    
3. Refresh the page
    
4. Scroll up and click on the row with the name of **localhost**, status of **200**, and type of **document**
    
5. Look at the **Response** tab. This is the response from the server. In it, you can see the `<svg>` element that makes up the plot.
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1697031054758/24764737-527e-405a-bce9-f181dd6aadf1.png align="center")

## Conclusion

You can use the knowledge gained here to open up possibilities for performant and dynamic data visualizations. When you adopt SSR, you optimize load times and create easier user experiences. Use this tool to ensure your visualizations shine with both functionality and speed.

All of the code for this article can be found on GitHub.

%[https://github.com/travishorn/sveltekit-plot-ssr]