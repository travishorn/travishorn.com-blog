---
title: "Build & Deploy Serverless Functions in 5 Minutes"
datePublished: Tue Aug 13 2019 22:01:01 GMT+0000 (Coordinated Universal Time)
cuid: ckroxw7t80pfsfws15sl7gsrq
slug: build-deploy-serverless-functions-in-5-minutes-7a90aca05f1f
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1627409478680/eghriHCO-.png
tags: github, javascript, web-development, serverless, netlify

---


The serverless method promises to take away the hassle of managing servers so you can focus on development. But what does it take to actually get started?

While this article is not sponsored by Netlify, I’ll be using their platform heavily because I find it so much easier to get started versus some of the other available options.

I created a [similar tutorial](https://travishorn.com/netlify-lambda-functions-from-scratch-1186f61c659e) a while back. Since then, Netlify has released some tools that make it even easier to start building serverless lambda functions.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627409476580/KSpD0r29s.png)

To begin, [create a new repository on GitHub](https://github.com/new).

Clone the repository to your development machine, change into the directory, and initialize a new npm package.

```
git clone [https://github.com/[your](https://github.com/[your) name]/[your repo].git
cd [your repo]
npm init -y
```


We’re getting ready to install some Node modules, which don’t need to be tracked under version control. Tell git to ignore them by adding a file called `.gitignore` with this single line:

```
node_modules/
```


Now install the Netlify CLI.

```
npm install --development netlify-cli
```


Create a new directory called `functions/` and create a file called `hello-world.js` inside.

```
exports.handler = (event, context, callback) => {
  callback(null, {
    statusCode: 200,
    body: JSON.stringify({
      "message": "Hello, World!"
    })
  );
```


Back out in the project root directory, the last file we’ll add is `netlify.toml` which we can use to tell Netlify where our serverless functions are located.

```
[build]
  functions = "functions/"
```


We’re ready to run it locally.

```
npx netlify dev
```


Our function can be triggered by going to [http://localhost:8888/.netlify/functions/hello-world](http://localhost:8888/.netlify/functions/hello-world)

The server responds with a status of `200` and body:

```
{"message":"Hello, World!"}
```


### Ready to deploy?

Stage, commit, and push the code changes.

```
git add .
git commit -m "Add serverless function"
git push
```


Connect your repository to Netlify by going to [https://app.netlify.com](https://app.netlify.com) and clicking the **New site from git** button. Select **GitHub**, then select your repository. Finally, press the **Deploy site** button.

Your serverless function is online at https://[sitename].netlify.com/.netlify/functions/hello-world

### More!

When you have `npx netlify dev` running and you modify the function’s code, it will automatically re-build and be available the next time you hit the function’s URL.

When you `git push` to GitHub, Netlify automatically picks up the changes and re-deploys your live site.

To add another function, simply add another file in the `functions/` directory.

To access query string parameters in your function, look at `event.queryStringParameters`. For example if you hit /hello-world?id=1, then `event.queryStringParameters.id` will equal `1`.

To access the request body, look at `event.body`.

There’s so much more to cover on this topic. But hopefully you learned a quick way to get started easily.