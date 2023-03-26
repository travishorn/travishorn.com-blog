---
title: "Netlify Lambda Functions from Scratch"
datePublished: Wed Sep 12 2018 22:01:01 GMT+0000 (Coordinated Universal Time)
cuid: ckrmffmig03o5ems1d2uvd9gf
slug: netlify-lambda-functions-from-scratch-1186f61c659e
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1627410364514/a_q7KAyjlo.png
tags: javascript, web-development, serverless, aws-lambda, netlify

---


When learning a new concept, I always like to try building things from as low a level as I can. Rather than using a pre-built template, I want to see if I can wire everything up using a only few bare-bones libraries and utilities, AKA “from scratch.”

[AWS Lambda functions](https://docs.aws.amazon.com/lambda/latest/dg/welcome.html) are a great way to run on-demand server-side code without having to manage a dedicated server. An extremely simple example would be a function that takes a user’s text input, reverses it, and returns the reversed string to the user. These are functions that are easy to run on your local machine, but without AWS lambda, are harder to deploy out to the public.

[Netlify](https://www.netlify.com/) is a service that offers static website hosting with [added features](https://www.netlify.com/features/) such as [authentication management](https://www.netlify.com/docs/identity/), [form handling](https://www.netlify.com/docs/form-handling/), and most importantly: [continuous deployment](https://www.netlify.com/docs/continuous-deployment/) and AWS lambda [functions](https://www.netlify.com/docs/functions/). They offer a free tier with limited function requests and run time.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627410362460/A8QQkTl0O.png)

In this article, I’m using [GitLab](https://gitlab.com), but Netlify also supports [GitHub](https://github.com/) and [Bitbucket](https://bitbucket.org/). Feel free to use whichever one you like, but you do need to use one of them.

### Set up the project

Go to GitLab, click **New project**, enter any name you like, and click **Create project**.

Clone the repository to your local computer. If you don’t have git, you will need to [install it](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git).

```
> git clone https://gitlab.com/[your username]/[your project name].git
```


Move into your project directory, [initialize an npm package](https://docs.npmjs.com/cli/init), and install [netlify-lambda](https://www.npmjs.com/package/netlify-lambda).

```
> cd [your project name]
> npm init -y
> npm i netlify-lambda
```


netlify-lambda is a tool for developing and building lambda functions compatible with Netlify. It has two helpful commands that we’ll set up in a minute.

But first, since netlify-lambda is a Node module, npm automatically created a `node_modules` directory full of third-party code that our application will use. [We don’t want git to track these files](https://julie.io/writing/javascript-node-modules/), so create a new file called `.gitignore` and type the following line.

`node_modules/`

Now we can set up the netlify-lambda commands. Open `package.json` and add two scripts, shown in bold below.

```
{
  "name": "netlify-lambda-functions",
  "version": "1.0.0",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    **"start:lambda": "netlify-lambda serve src/lambda",
    "build:lambda": "netlify-lambda build src/lambda"**
  },
  "license": "MIT",
  "dependencies": {
    "netlify-lambda": "^0.4.0"
  }
}
```


*Don’t worry if your `*package.json`* looks slightly different. I’ve cut out some non-essential lines in mine.*

Both of these commands reference a directory that that doesn’t exist yet: `src/lambda`. This is the directory where we’ll actually write our functions.

### Write a function

Create a directory named `src`. Then, create another directory inside it called `lambda`. Finally, create a file called `hello.js`.

`hello.js` will be our first lambda function. Netlify expects a [specific handler function format](https://www.netlify.com/docs/functions/).

```
exports.handler = (event, context, callback) => {
  // Function code goes here
};
```


The `event` object contains useful information from the client like the `headers` and `body` of the request. The `context` object contains information from Netlify which could include things like `user` information if you’re using their [Identity](https://www.netlify.com/docs/identity/) API.

The `callback` is a function that tells Netlify how to respond to the request. Let’s fill in our handler with a callback.

```
exports.handler = (event, context, callback) => {
  callback(null, {
    statusCode: 200,
    body: 'Hello, world!',
  });
};
```


*Why is the first argument `*null`* in the callback?* Because Netlify uses that argument to determine if there was an error with the function. If you wanted to return an error, you could write something like this.

```
exports.handler = (event, context, callback) => {
  callback('Oh no! An error.');
};
```


But in our case, we’re just responding with a simple text string, so no error occurs, and we can safely return `null` as the first argument.

### Run functions locally

We’re almost ready to test our function locally. Before we can do that, however, we’ll have to tell netlify-lambda where it should ultimately build our functions.

It’s important to understand that we are developing our functions inside the `src` directory, but the final product will be built by netlify-lambda and placed in some sort of build directory. Let’s create that directory now.

In the root of your project, create a directory called `lambda`.

Now, also in the root (alongside the new `lambda` folder, **not** inside it), create `netlify.toml`. This file is used to configure Netlify.

```
[build]
  Functions = "lambda"
```


In this file, we’re simply telling Netlify where to build or find our lambda functions.

Finally, we can test our function locally. Remember those npm scripts we set up earlier? It’s time to run the first one.

```
> npm run start:lambda
```


This command looks at the `src/lambda` directory. Any `.js` files it finds, it will build into the `/lambda` directory. It then serves the built functions on `localhost`. Finally, it watches the `src/lambda` directory, and re-builds everything when any change is made.

You can now go to [http://localhost:9000/hello](http://localhost:9000/hello). You should see “Hello, world!” displayed.

Note that the path `/hello` is derived from the filename `hello.js`. Any files you place in the `src/lambda` directory will be built and served on their own path based on the filename.

### A function that uses third-party libraries

In fact, let’s create a second, slightly more complex function. For this function, we’re going to be using [Axios](https://www.npmjs.com/package/axios) to pull data from [an API](https://jsonplaceholder.typicode.com/).

First, install Axios.

```
> npm i axios
```


Next, create a new file called `src/lambda/todo.js`. In this file, we’ll [require](https://medium.freecodecamp.org/requiring-modules-in-node-js-everything-you-need-to-know-e7fbd119be8) axios, and then write a handler just like before.

```
const axios = require('axios');

exports.handler = (event, context, callback) => {
  // Here we'll use Axios to get a remote resource
};
```


Inside the handler (where the comment is above), use Axios to get JSON from [https://jsonplaceholder.typicode.com](https://jsonplaceholder.typicode.com)

```
axios.get('https://jsonplaceholder.typicode.com/todos/1')
  .then((res) => {
    // Do something with successful response
  })
  .catch((err) => {
    // Do something with the error
  });
```


In this case, it is possible the GET request from Axios could fail. So we have two functions, one for success and one for failure.

If the request was a success, let’s tell Netlify to respond with the title of the todo.

```
callback(null, {
  statusCode: 200,
  body: res.data.title,
});
```


If the request fails, just pass the error along.

```
callback(err);
```


The full `todo.js` function looks like this:

```
const axios = require('axios');

exports.handler = (event, context, callback) => {
  axios.get('https://jsonplaceholder.typicode.com/todos/1')
    .then((res) => {
      callback(null, {
        statusCode: 200,
        body: res.data.title,
      });
    })
    .catch((err) => {
      callback(err);
    });
};
```


If you stopped it earlier, re-run the script to run netlify-lambda locally.

```
> npm run start:lambda
```


Otherwise, if you still had it running, you can skip the command above; netlify-lambda will have noticed the new file and re-build the functions.

Now, in addition to being able to access the `hello` function, you can now also access [http://localhost:9000/todo](http://localhost:9000/todo)

When you hit that URL, Axios pulls the todo from jsonplaceholder.typicode.com and returns the title, which is “delectus aut autem”

### Set up for deployment

Ready for deployment to Netlify? The process is simple, but we’ll need to do a few pieces of boilerplate first.

We don’t need git to track our built lambda functions because those will be build on the target machine with each deployment. So modify `.gitignore` to include this additional line:

```
/lambda
```


*The slash is important.* It tells git to ignore only the `lambda` directory at the root level; we still want our `src/lambda` to be tracked.

And finally, we need to tell Netlify to run our build command when deploying. Edit `netlify.toml` so that it includes the build command to run (in bold below).

```
[build]
  Functions = "lambda"
  **Command = "npm run build:lambda"**
```


### Deploy

We’re ready to deploy. Stage all changes, commit them, and push to GitLab.

```
> git add .
> git commit -m "Add hello and todo functions"
> git push origin master
```


GitLab now has our latest changes. We can link a new Netlify site to this repository.

On [Netlify](https://app.netlify.com/), click **New site from Git** and click **GitLab** (or GitHub/Bitbucket if you’re using either of those).

Find the repository that contains our project and click it.

Since our project has a `netlify.toml`, we don’t need to specify any options such as **Build command**. Instead, we can simply click **Deploy site**.

After a few moments, the site will be live. You can watch the progress of the build by clicking **Deploys** and then clicking the build that’s in progress.

Once it’s live, you can click **Functions** and see our two functions listed. Click either of them to see the endpoint URL. Copy & paste that URL into your browser and you should see your function respond!

### Continuous deployment

From now on, whenever you want to add a new function or modify an existing one, you can do that locally and then test with `npm run start:lambda`. When you’re ready to deploy, just commit your changes and push to GitLab. Netlify will automatically pick up the new code and re-deploy.