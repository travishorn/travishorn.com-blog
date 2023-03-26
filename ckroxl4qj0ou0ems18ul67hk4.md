---
title: "Setting up Prettier on VS Code"
datePublished: Thu Oct 11 2018 11:01:02 GMT+0000 (Coordinated Universal Time)
cuid: ckroxl4qj0ou0ems18ul67hk4
slug: setting-up-prettier-on-vs-code-1fd5e5a43523
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1627409626082/sl2ljB3Q0.png
tags: javascript, web-development, styleguide, eslint, prettier

---

Prettier is an opinionated code formatter. It is fundamentally different than a style linter. Prettier doesn’t care how you write your JavaScript. It simply comes in after you’re done and formats all your code in a standard way.

I posted a very similar tutorial on [how to set up ESLint on VS Code with Airbnb JavaScript Style Guide](https://travishorn.com/setting-up-eslint-on-vs-code-with-airbnb-javascript-style-guide-6eb78a535ba6). Many people found it useful. I figured I’d do something similar with Prettier.

Some may find it useful to install a linting configuration that obeys Prettier’s recommended rules so they can write code in the style that Prettier already expects. Following these rules will ensure your code has a level of clarity that makes reading and maintaining your code easier for anyone who has to work on it. For the linting step, you can use [ESLint](https://eslint.org/), which is a tool for “[linting](https://stackoverflow.com/questions/8503559/what-is-linting)” your code. It can analyze your code and warn you of potential errors. For it to work, you need to configure it with specific rules. Luckily, Prettier provides an ESLint configuration that anyone can use.

[VS Code](https://code.visualstudio.com/) is a popular code editor created by Microsoft. One of the nice features is that you can enable extensions that make your life as a developer easier. There is an extension for ESLint which will integrate its formatting and linting features right in your editor. You will be warned of potential problems on the fly as you write code and you can auto-format your code on save.

Many developers consider setting up linting in this way essential. In this post, I’ll show you how to set it all up.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627409622037/8HJ4P5Zp9.png align="left")

### The concise version

1. `cd project-directory`
    
2. `npm init -y`
    
3. `npm i -D eslint eslint-config-prettier eslint-plugin-prettier prettier`
    
4. Create `.eslintrc.cjs`: `module.exports = { "extends": "plugin:prettier/recommended" };`
    
5. In VS Code, `Ctrl + Shift + X`
    
6. Search and install `ESLint`
    

Follow along with the detailed version below if you are new to any of the concepts above or just want to get a better understanding of what these steps do.

### The detailed version

First, download and install [Node.js](https://nodejs.org/en/). Its package manager, npm, is included with the installation.

Once Node and npm are installed, open up a terminal (or command prompt). Change into the directory for your project. Here's my example:

```bash
> cd C:\Users\Travis\Development\my-project
```

Now initialize a new npm configuration.

```bash
> npm init -y
```

Install and save the necessary packages.

```bash
> npm i -D eslint eslint-config-prettier eslint-plugin-prettier prettier
```

Now create `.eslintrc.cjs` with the following contents:

```javascript
module.exports = {
  "extends": ["plugin:prettier/recommended"],
};
```

Prettier and ESLint are now installed and configured. At this point, you can lint your code in the terminal.

```bash
npx eslint .
```

But we want to get any linting errors to show up in VS Code.

Open VS Code and press `Ctrl + Shift + X` to open the **Extensions** panel.

Type `ESLint` in the search bar. Find the ESLint extension in the search results and click the blue **Install** button next to it.

Congratulations! ESLint and Prettier are enabled. If you open up any `.js` file in your project, ESLint will check your code against Prettier’s recommended rules and warn you of any conflicts.

To automatically fix the problems it finds, open the command pallet with Ctrl + Shift + P and search for "ESLint: Fix all auto-fixable Problems". You can also automatically format your code every time you save with the `editor.codeActionsOnSave` option in your `settings.json`.

```json
{
  "editor.codeActionsOnSave": {
    "source.fixAll": true
  }
}
```

![ESLint warns on hover. Prettier fixes on Shift+Alt+F](https://cdn.hashnode.com/res/hashnode/image/upload/v1627409624027/KnR1Wi_Mw.gif align="left")

*ESLint warns on hover and auto-fixes on save.*