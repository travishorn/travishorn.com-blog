---
title: "Setting up ESLint on VS Code with Airbnb JavaScript Style Guide"
datePublished: Thu Dec 07 2017 22:01:02 GMT+0000 (Coordinated Universal Time)
cuid: ckroy0vi30oyoems15odz3vyx
slug: setting-up-eslint-on-vs-code-with-airbnb-javascript-style-guide-6eb78a535ba6
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1627409464823/J_U0Oo_2h.jpeg
tags: airbnb, javascript, web-development, styleguide, eslint

---


Airbnb maintains a very popular JavaScript Style Guide that is used by many JavaScript developers worldwide. Following this style guide will ensure your code has a level of clarity that makes reading and maintaining your code easier for anyone who has to work on it.

[ESLint](https://eslint.org/) is a tool for “[linting](https://stackoverflow.com/questions/8503559/what-is-linting)” your code. It can analyze your code and warn you of potential errors. In order for it to work, you need to configure it with specific rules. Luckily, Airbnb — as part of their style guide — provides an ESLint configuration that anyone can use.

[VS Code](https://code.visualstudio.com/) is a popular code editor created by Microsoft. One of the nice features is that you can enable extensions that make your life as a developer easier. There is a VS Code extension for ESLint which will integrate it’s linting features right in your editor. You will be warned of potential problems on the fly as you write code.

Many developers consider setting up linting in this way essential. In this post, I’ll show you how to set it all up.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627409460388/NDLIAvjDI.jpeg)

### The concise version

1. `cd coding-directory`

1. `npm init -y`

1. `npm i -D eslint eslint-config-airbnb-base eslint-plugin-import`

1. Create `.eslintrc.js`: `module.exports = { extends: 'airbnb-base' };`

1. In VS Code, `Ctrl + Shift + X`

1. Search `ESLint`

1. Install ESLint

1. Restart VS Code

Follow along with the detailed version below if you are new to any of the concepts above or just want to get a better understanding of what these steps do.

### The detailed version

First, download and install [Node.js](https://nodejs.org/en/). Its package manager, npm, is included with the installation.

Once Node and npm are installed, open up a terminal (or command prompt). Change directories into the root directory for any coding you do. I call my directory “code”.

```
> cd C:\users\travis\code
```


This is the directory in which I want to configure ESLint. Any subdirectories within this one will also use the configuration we are about to set up. This works because, when you run ESLint, it looks in the current directory for a configuration; If it can’t find one, it moves up to the parent directory, checks there, and the process repeats until a configuration is found.

Now initialize a new npm configuration.

```
> npm init -y

```


Install and save the necessary packages for ESLint and the Airbnb configuration.

```
> npm i -D eslint eslint-config-airbnb-base eslint-plugin-import
```


In case you are wondering, `[eslint-plugin-import`](https://github.com/benmosher/eslint-plugin-import) is a peer dependency for `eslint-config-airbnb-base`. It enables support for linting the new import/export syntax for modules.

Now create `.eslintrc.js` with the following contents:

```
module.exports = {
  extends: 'airbnb-base',
};
```


ESLint is installed and configured for Airbnb’s style guide. Now we need to get it working in VS Code.

Download and install [VS Code](https://code.visualstudio.com/).

Open it and press `Ctrl + Shift + X` to open the **Extensions** panel.

Type `ESLint` in the search bar. Find the ESLint extension in the search results and click the green **Install** button next to it.

Go ahead and close VS Code and then re-open it.

Congratulations! ESLint is enabled in your [User Settings](https://code.visualstudio.com/docs/getstarted/settings) by default. If you open up any `.js` file in any sub-directory of `C:\users\travis\code` (or wherever **you** installed the configuration), ESLint will check your code against the Airbnb JavaScript Style Guide and warn you of any conflicts.

![ESLint at work in VS Code](https://cdn.hashnode.com/res/hashnode/image/upload/v1627409462597/v52djsCm4.gif)*ESLint at work in VS Code*