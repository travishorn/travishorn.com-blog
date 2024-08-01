---
title: "Setting up a Project with ESLint and Prettier"
datePublished: Thu Aug 01 2024 12:48:55 GMT+0000 (Coordinated Universal Time)
cuid: clzb9vcxe000009jx6woscvye
slug: setting-up-a-project-with-eslint-and-prettier
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1722516453600/c2fdae49-e08c-448c-ba17-1921afe52d74.png
tags: javascript, best-practices, eslint, prettier, formatting

---

[ESLint](https://eslint.org/) is a tool for “[linting](https://stackoverflow.com/questions/8503559/what-is-linting)” your code. It can analyze your code and warn you of potential errors. In order for it to work, you need to configure it with specific rules. Fortunately, the ESLint team provides a recommended configuration that anyone can use. You can also customize the configuration to suit yourself or your team.

[Prettier](https://prettier.io/) is an opinionated code formatter. It is fundamentally different than a linter like ESLint. Prettier doesn’t care how you write your JavaScript. It simply comes in after you’re done and formats all your code in a standard way.

The combination of ESLint and Prettier provides great advantages for maintaining code quality and consistency. Many developers consider setting up linting in this way essential. Follow along and I'll show you how to set up your JavaScript project with these tools.

## Prerequisites

The steps in this guide will walk you through how to use these tools with a JavaScript project using Node.js and npm. [Download and install Node.js](https://nodejs.org/e) if you don't already have it. By default, it comes with the package manager npm.

While optional, I'll also show you how to install extensions for the code editor Visual Studio Code. [Download and install VS Code](https://code.visualstudio.com/) if desired.

## The Steps

First, make a new directory for your project.

```bash
mkdir my-project
```

Change into that directory.

```bash
cd my-project
```

Initialize a new npm package inside the project directory.

```bash
npm init -y
```

Update `package.json` to enable [ES Modules](https://nodejs.org/api/esm.html) and set up some scripts for linting and formatting.

```json
{
  ...snip...
  "type": "module",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "lint": "eslint .",
    "format": "prettier --write ."
  },
  ...snip...
}
```

Install the necessary dependencies.

```bash
npm i -D @eslint/js eslint eslint-config-prettier globals prettier
```

Create a `.prettierrc` file.

```json
{}
```

Create an `eslint.config.js` file.

```javascript
import globals from "globals";
import pluginJs from "@eslint/js";
import eslintConfigPrettier from "eslint-config-prettier";

export default [
  { languageOptions: { globals: globals.node } },
  pluginJs.configs.recommended,
  eslintConfigPrettier,
];
```

ESLint and Prettier and now installed and configured.

You can lint and format your code from the terminal at any time.

```bash
npm run lint
npm run format
```

## VS Code Integration

Both of these tools have VS Code extensions available for additional benefits, such as real-time feedback as you edit your code and automatic formatting when you save.

Open VS Code, go to the Extensions options (Ctrl + Shift + X), search for **ESLint**, and click the blue **Install** button.

Do the same for **Prettier**.

Congratulations! You've done it.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1722367180896/02cadfdd-9c7e-4606-853f-979ad0c72dd5.gif align="center")

Notice the real-time errors from ESLint and the automatic formatting-on-save from Prettier.