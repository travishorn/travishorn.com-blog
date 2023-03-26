---
title: "Getting Started with Vue Single File Components"
datePublished: Thu Apr 11 2019 11:01:01 GMT+0000 (Coordinated Universal Time)
cuid: ckrnh8ign0bocems193o03zpv
slug: getting-started-with-vue-single-file-components-f29765a771a3
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1627410127693/TyLD1va8B.png
tags: javascript, web-development, vuejs

---


If you’re just starting out with Vue and you’ve followed along with the official guide, it begins by detailing how to use Vue in the easiest way possible: including the library directly using the <script> tag. This is a very fast way to get started. It’s effective for new users.

Eventually, you’ll discover a need for components. While you *can* still use the direct`&lt;script&gt;` method with components, it would be wise to start thinking about switching over to [Single File Components](https://vuejs.org/v2/guide/single-file-components.html) — or SFCs.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627410122657/lWm7EFU5z.png)

### First, what is a component?

Components are a powerful way to write re-usable code with Vue. If you’re adding Vue to your page via direct `&lt;script&gt;` include, you define a component like this:

```
Vue.component('person-greeter', {
  props: ['personName'],
  template: '<h2>Hello, {{ personName }}!</h2>'
})
```


And use it inside a Vue instance like this:

```
<div id="app">
  <h1>My Vue App</h1>
  <person-greeter personName="Travis" />
</div>

new Vue({
  el: '#app'
});
```


Components are very powerful because you can lay out your app into logical *components* and re-use them when it makes sense to do so. If you’re not familiar with them, I recommend checking out [the official documentation section on components](https://vuejs.org/v2/guide/components.html).

### Now, what is a *Single File* Component?

SFCs are components (just like above) except they are self-contained in their own files.

### Why should I use SFCs over the method above?

* **More naming flexibility **— As you can see above, components built using direct `&lt;script&gt;` include must be registered using `Vue.component`. They are all *globally* registered. This forces all component names to be unique. Single file components are registered using a different method that allows you can name them however you wish. For example: If you are using two libraries that share component names, you have a way to handle that with SFCs.

* **Prettier templates **— Components registered using `Vue.component` must have a `template` property which is a string that describes the template. With SFCs, you can code the template more naturally in your editor instead of using a single string. Most modern editors can even syntax highlight templates just as if you were writing regular HTML.

* **Component styling** — Take another look at the component above. You can see the HTML and JavaScript that make up the component. But CSS, if you wanted to add any, would have to be included separately. SFCs provide a method to code the component’s CSS with the component itself.

* **Pre-processor support** —It’s not so easy to use pre-processors like Babel, Pug, or SASS with the direct `&lt;script&gt;` method. This problem is solved with SFCs.

### What does an SFC look like?

The example above can be written in a single file component called `PersonGreeter.vue` (or any other name, really).

```
<template>
  <h2>Hello, {{ personName }}!</h2>
</template>

<script>
module.exports = {
  props: ['personName']
}
</script>

<style scoped>
  h2 {
    font-size: 40px;
    color: mediumseagreen;
  }
</style>
```


Note that I included some CSS this time which was impossible before without a global stylesheet and some cleverly named `class`es.
> Some developers may be thinking “What about separation of concerns?” The official Vue documentation has [an answer for you](https://vuejs.org/v2/guide/single-file-components.html#What-About-Separation-of-Concerns).

### Prerequisites

Before we begin development with SFCs, you will need to make sure you have [Node.js](https://nodejs.org/en/) installed. A basic understanding of how Node.js works would also be helpful.

### Let’s start using SFCs

This may be a big jump for many developers because it involves setting up a build environment. Let’s walk through it step-by-step.

Create a new directory to house our new project and move into it.

```
> mkdir myvueapp
> cd myvueapp
```


Initialize a new npm project.

```
> npm init -y
```


In this basic example, the only production dependency is Vue.

```
> npm i vue
```


There are a few development dependencies to install, as well.

```
> npm i -D @vue/cli-service vue-template-compiler
```

> The **Vue CLI service** is a tool that will allow us to A) start a dev server with hot-module-replacement and B) build a production-ready bundle.
> The **Vue template compiler** pre-compiles Vue templates before production. This reduces the overhead on clients when using our app.

Once all our dependencies are installed, we need to define some npm scripts. We’ll want one for *serving* during development and one for *building* for production. Open `package.json` and add the scripts (in bold below).

```
"scripts": {
  "test": "echo \"Error: no test specified\" && exit 1"**,
  "serve": "vue-cli-service serve",
  "build": "vue-cli-service build"**
},
```


When serving and building our site, the Vue CLI service will look for an HTML file in which to auto-inject built files. Let’s create it now. Create a new directory inside `myvueapp` called `public`. Inside this directory, create a file called `index.html`. This is a very bare-bones HTML file that will automatically be injected with code from our Vue app.

```
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width,initial-scale=1.0">

    <title>My Vue App</title>
  </head>
  <body>
    <div id="app"></div>
  </body>
</html>
```


Take note of the `&lt;div id="app"&gt;`. This will be our root element.

Now for the Vue app itself. Create a new directory called `src` alongside `public` (**not** inside it). The first file in this directory will be `main.js`. Create it now.

```
import Vue from 'vue'
import App from './App.vue'

new Vue({
  render: h => h(App)
}).$mount('#app')
```


When building our app, the build tool will look at this file first to figure out how to build the rest of the app. You can see here that we’re pulling in Vue and another file (`App.vue` — which we’ll create in a moment). Then, we’re instantiating a new Vue instance which will render our app and place it in the root `&lt;div id="app"&gt;` element we wrote earlier in HTML.

Since `main.js` is looking for `App.vue`, let’s create that.

```
<template>
  <div id="app">
    <h1>My Vue App</h1>
  </div>
</template>

<script>
export default {
  name: 'App'
}
</script>

<style>
#app {
  font-family: Helvetica, Arial, sans-serif;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
```


We’re now ready to see what our app looks like. Go ahead and run the dev server.

```
> npm run serve

App running at:
- Local:   http://localhost:8080/
- Network: http://XX.XX.XX.XX:8080/
```


If you visit [http://localhost:8080/](http://localhost:8080/), you should see the app.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627410124402/tD2Gri955.png)

It’s incredibly basic, but it’s a running Vue app! Keep this dev server running while we stick another component inside this main one. Create a new directory inside `src` called `components`. Inside that, create `PersonGreeter.vue`.

```
<template>
  <h2>Hello, {{ personName }}!</h2>
</template>

<script>
module.exports = {
  name: 'PersonGreeter',
  props: ['personName']
}
</script>

<style scoped>
  h2 {
    font-size: 40px;
    color: mediumseagreen;
  }
</style>
```


We can use this component inside `App.vue` like so. Note the new lines in bold below.

```
<template>
  <div id="app">
    <h1>My Vue App</h1>
**    <PersonGreeter personName="Travis" />**
  </div>
</template>

<script>
import PersonGreeter from './components/PersonGreeter.vue'

export default {
  name: 'app',
  components: {
    PersonGreeter
  }
}
</script>
```


As soon as you save the file, the dev server hot-replaces it. Our component is now visible.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627410126179/PkHLUiGGh.png)

### Ready for deployment

The dev server works well for development. But when we’re ready to deploy, we want a pre-compiled, minified, and optimized app without any of the development tools like hot-module-replacement. This is as easy as running a single build command:

```
> npm run build
```


A new directory — `dist` — is created. Push the files in this directory to your production server, and your site will be live. The official Vue documentation has a [guide on deployment](https://cli.vuejs.org/guide/deployment.html), including instructions for various popular platforms such as Amazon S3 and Firebase.

The source code I wrote for this post is open and available on GitHub.
[**travishorn/getting-started-vue-sfcs**
*The bare minimum for using Vue single file components. - travishorn/getting-started-vue-sfcs*github.com](https://github.com/travishorn/getting-started-vue-sfcs)

You can import any component into any other component, nested as many times as necessary for your app. Before getting too deep into it, you would be wise to do some planning and research into how best to structure Vue apps using components. Some good resources are [ITNEXT’s “How to Structure a Vue.js Project”](https://itnext.io/how-to-structure-a-vue-js-project-29e4ddc1aeeb) and [VueSchool’s “Structuring Vue Components”](https://vueschool.io/articles/vuejs-tutorials/structuring-vue-components/).