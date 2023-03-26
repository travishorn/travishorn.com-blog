---
title: "The Basics of Modular JavaScript & npm Packages"
datePublished: Wed Aug 10 2016 22:01:01 GMT+0000 (Coordinated Universal Time)
cuid: ckrmeug1p03k9ems1hadq17qe
slug: the-basics-of-modular-javascript-npm-packages-ecdc8b8388a4
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1627410510352/rwJM_a7gC.jpeg
tags: javascript, npm, javascript-modules, programming-tips

---


One of the biggest jumps a new JavaScript developer can make is going from writing collections of spaghetti code to using a system of loosely coupled modules, where each module has a particular task and can be included in any project where it is needed.

I will break down the process of using and writing JavaScript modules into these five steps. Feel free to skip some if you are already comfortable with them.

1. Installing Git (on Windows)

1. Installing Node.js (on Windows)

1. Using npm packages in Node

1. Using npm Packages in the browser

1. Creating your own modules

![Photo by [Erwan Hesry](https://cdn.hashnode.com/res/hashnode/image/upload/v1627410508402/MSDoSaUAD.html)](https://cdn-images-1.medium.com/max/11954/1*UHCgPFM7FyNVvVx4CY6yWw.jpeg)*Photo by [Erwan Hesry](https://unsplash.com/@erwanhesry)*

### 1. Installing Git on Windows

There are many options for installing and using [Git](https://git-scm.com/). On Windows, I recommend installing the GitHub desktop app, which has Git bundled with it. With the GitHub desktop app, you will be able to easily publish any modules you make to the open source community and allow others to access, modify, and collaborate on your modules.

**Creating a GitHub account**

1. Go to [https://github.com/](https://github.com/)

1. Enter a username, your email address, and a password of your choosing

1. Click the **Sign up for GitHub** button

**Installing the software**

1. Go to [https://desktop.github.com/](https://desktop.github.com/)

1. Click **Download GitHub Desktop**

1. When the .exe finishes downloading, double-click it

1. Follow the guided installation instructions

1. Open the application and sign in with your GitHub account

### 2. Installing Node.js on Windows

There are also many ways to install [Node.js](https://nodejs.org/en/). Each have their benefits. The easiest way to get up and running is the official Node.js binary for Windows.

1. Go to [https://nodejs.org](https://nodejs.org)

1. Click the green download button for the latest or LTS version. While either one is honestly fine, I usually stick with the LTS version at the cost of not being on the bleeding edge.

1. Once the .msi file has been downloaded, double-click it and follow the installation instructions

1. Open a **Command Prompt** and run `node -v` to make sure it works

```
> node -v
v4.4.7
```


### 3. Using npm packages in Node

Once you start breaking your apps into smaller modular packages, you will find that, in so many cases, someone has already had the same need as you and has already written a package for it. In the JavaScript world, the main repository for all of these packages is [npm](https://npmjs.com/). First, let’s see how to use these packages in Node, then we’ll see how you can also use them in the browser.

**Setup**

First, make sure you have Node installed.

Then, when creating a new project…

1. Open a **Command Prompt** and `cd` into the folder you are working in

1. Run `npm init` and answer each question about your project

```
> mkdir mypackage
> cd mypackage
> npm init

name: (mypackage)
version: (1.0.0)
description: My first npm package
entry point: (index.js)
test command: 
git repository:
keywords:
author: Travis Horn
license: (ISC) MIT

About to write to C:\code\mypackage\package.json:

{
  "name": "mypackage",
  "version": "1.0.0",
  "description": "My first npm package",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "Travis Horn",
  "license": "MIT"
}

Is this ok? (yes)
```


This will create a **package.json** file that contains metadata about your project.

**Installing a package**

When you find a package that you want to use, just run

```
npm install [package name] --save
```


This installs the package and saves it as a dependency in **package.json**.

```
npm install lodash --save
```


package.json:

```
{
  "name": "mypackage",
  "version": "1.0.0",
  ...
  "dependencies": {
    "lodash": "^4.14.1"
  }
}
```


**Using a package**

Once it is installed, just require it at the top of any script that uses it, like so:

```
var _ = require('lodash');
```


You now have access to the `_` object, which contains all the code downloaded and installed with the package.

```
var myObj = _.assign({ 'a': 1 }, { 'b': 2 }, { 'c': 3 });

// myObj = { 'a': 1, 'b': 2, 'c': 3 }
```


### 4. Using npm packages in the browser

Using packaged modules like that in Node is great. But what about the browser? Like everything else I’ve mentioned, there are *many* ways to get this done. One simple option is **browserify**.

**Setup**

Open a **Command Prompt** and run

```
npm install -g browserify
```


This will install browserify globally. Meaning that it’s not tied to a specific project, but rather a utility that you can use anywhere.

**Installing a package**

When you find a package you want to use, just run

```
npm install [package name]
```


**Using a package**

First, require it at the top of any script that uses it, like so:

```
var _ = require('lodash');
```


You now have access to the `_` object, which contains all the code downloaded and installed with the package. Before running in the browser, you need to bundle your code with **browserify**…

1. Run `browserify [filename].js -o bundle.js`. Remember to replace [filename] with the filename of the script you wrote.

1. Include the bundle in your HTML with `<script src=”bundle.js”></script>

### 5. Creating your own modules (npm packages)

There may not be a module out there that does what you want to do. In this case, it’s a good idea to write your own module that you can re-use any time you need it. You can even publish it to the world (using npm) and help other people if you wish.

A good module is well tested, so I’ll show you how to write a module with tests.

1. Open the **GitHub** desktop app and create a new repository by clicking the **plus icon**, entering a project **Name** and **Local path**, choosing **Node** from the **Git ignore** dropdown box, and clicking **Create repository**

1. Open a **Command Prompt** and `cd` into the repository’s directory

1. Run `npm init` and answer each question about your project

1. Run `npm install mocha chai — save-dev`

1. Create test.js and write testing like **Fig 1**

1. Modify package.json scripts to include a **test** script like **Fig 2**

1. Write your module in app.js. A single object will need to be exported with `module.exports` like **Fig 3**

1. Run `npm test` and make sure everything is passing. If not, modify code to pass tests

1. Write **README.md** with a description, installation instructions, usage, how to run tests, and a software license

1. Commit changes in the **GitHub** desktop app by clicking the **Changes** tab, entering a summary of changes, and then clicking **Commit to master**

1. Optional — push to GitHub (online site) by clicking **Publish** and then clicking **Publish [name]**

1. Optional — publish on npm by running `npm publish` in the command prompt

**Fig 1**

```
require('chai').should();
var pkg = require('./app');

describe('pkg', function() {
  it('adds two numbers', function() {
    pkg.add(1, 1).should.equal(2);
  });
});
```


**Fig 2**

```
"scripts": { "test": "mocha --reporter spec tests" }
```


**Fig 3**

```
module.exports = {
  add: function(num1, num2) {
    return num1 + num2;
  }
};
```


The methods I used in these steps are only one way of doing it. You will undoubtedly develop your own style and figure out what tools work best for you. Hope fully this post will give you a head start.