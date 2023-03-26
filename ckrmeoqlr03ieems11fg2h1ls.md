---
title: "API Server with JWT Authentication"
datePublished: Thu Apr 25 2019 11:01:01 GMT+0000 (Coordinated Universal Time)
cuid: ckrmeoqlr03ieems11fg2h1ls
slug: api-server-with-jwt-authentication-6bb4985c5253
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1627410629510/APaKBkZ0g.png
tags: koa, javascript, web-development, jwt, sqlite

---


In this article, I’ll go over how to create an API server that signs and verifies JSON Web Tokens for authentication.

Some of the technologies this server uses include [Koa](https://koajs.com/), [JWTs](https://jwt.io/), [Knex](https://knexjs.org/), [SQLite](https://www.sqlite.org/), and [bcrypt](https://www.npmjs.com/package/bcrypt-nodejs).

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627410609104/aGuzqCsXb.png)

By the end of this article, we’ll have a fully functional server that can give out JWTs when correct credentials are provided. It will also be able to serve protected information when a valid signed JWT is provided.

A server like this can be used with a de-coupled JavaScript front-end — typically built with some framework like Angular, React, or Vue.

## Setup

Create a new directory to house our server code, and `cd` into it.

```
> mkdir jwt-server
> cd jwt-server
```


Initialize a new npm package in this directory.

```
> npm init -y
```


Now we can start adding modules. Start with [Koa.js](https://koajs.com/). It is a web framework which we’ll use to serve our API.

```
> npm i koa
```


### The main module

Create a new file called `index.js`. This will be the main module that we launch to start our server. Inside, set up a basic Koa server.

```
const Koa = require('koa');

const app = new Koa();

app.use((ctx) => {
  ctx.body = 'Hello World';
});

app.listen(process.env.PORT || 3000);
```


Run this code with Node.

```
> node index
```


If you visit [http://localhost:3000/](http://localhost:3000/) you’ll see “Hello World” displayed in your browser.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627410611444/JgCiQWgS1.png)

Press Ctrl+C to stop the server. Throughout this article, we’ll be making many changes to the code. To test out the changes, you can either stop and start the server each time or use something like [nodemon](https://nodemon.io/), which will do it for you automatically.

### Error handling

Before we start adding more features to the server, let’s set up some basic error handling. We can achieve this with a custom middleware handler. Create a new directory named `middleware`. Inside it, create `errorHandler.js`

```
module.exports = async (ctx, next) => {
  try {
    await next();
  } catch (err) {
    ctx.status = err.status || 500;
    ctx.body = err.message;
    ctx.app.emit('error', err, ctx);
  }
};
```


This small file will simply try and pass execution to the next middleware. If an error occurs, it is caught and handled. Specifically, the server responds with a 500 error, passes the error text to the client, and emits the error so you can see it in your terminal.

Using this error handler is simple enough. In `index.js` just `require` it and then call it with `app.use()` (before any other middleware are called). Note the changed lines in bold below.

```
const Koa = require('koa');

const errorHandler = require('./middleware/errorHandler');

const app = new Koa();

app.use(errorHandler);

app.use((ctx) => {
  ctx.body = 'Hello World';
});

app.listen(process.env.PORT || 3000);
```


Good. Basic error handling is set up.

### Routing

Right now, the server responds with “Hello World” no matter what URL you try to access. Let’s add support for multiple routes at multiple URLs.

For that we need to add [koa-router](https://github.com/alexmingoia/koa-router#readme). This package let’s us define “routes” that get executed when specific URLs are accessed.

```
> npm i koa-router
```


Add it to `index.js` like so. Notice the four new lines in bold below.

```
const Koa = require('koa');
**const Router = require('koa-router');**

const errorHandler = require('./middleware/errorHandler');

const app = new Koa();
const router = new Router();

app.use(errorHandler);

app.use((ctx) => {
  ctx.body = 'Hello World';
});

app
  .use(router.routes())
  .use(router.allowedMethods())
  .listen(process.env.PORT || 3000);
```


We `require` the router, create a new instance, and tell our app to use it’s routes and allowed methods. We’ll define routes in a minute. But what is `allowedMethods()`? It tells our server how to respond when someone tries to access a route with the incorrect method (`GET`, `POST`, etc).

Let’s add our first route; the `auth` route. Remove this code…

```
app.use((ctx) => {
  ctx.body = 'Hello World';
});
```


.. and replace it with this:

```
router.post('/auth', authRoute);
```


When someone tries to `POST` to /auth, the `authRoute` route is called. We haven’t created it yet, so let’s do that now.

First, `require` it near the top of `index.js`.

```
const authRoute = require('./routes/auth');
```


This `require` is looking for `./routes/auth`. Create that file now by creating a new directory called `routes` and a new file inside it called `auth.js`.

```
module.exports = (ctx) => {
  ctx.body = 'I am the auth route.';
};
```


If you `POST` to [http://localhost:3000/auth,](http://localhost:3000/auth,) you’ll see the correct response from the server: “I am the auth route.”

`POST`ing isn’t as easy as visiting the URL in your browser; that would issue a `GET` request. Instead, you’ll need to use a tool like [curl](https://curl.haxx.se/) (command line interface) or [Postman](https://www.getpostman.com/) (graphical user interface).

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627410613121/AzQlqpdLq.png)

### Signing JWTs

We want the `auth` route to respond with a [JSON Web Token](https://jwt.io/) (JWT). For that, we’ll need to bring in the [jsonwebtoken](https://github.com/auth0/node-jsonwebtoken#readme) package.

```
> npm i jsonwebtoken
```


`require` it at the top of `routes/auth.js`.

```
const jwt = require('jsonwebtoken');
```


And replace `ctx.body = 'I am the auth route.';` with a JWT token.

```
const token = jwt.sign(payload, secret);
ctx.body = token;
```


The `jwt.sign()` function takes two parameters:

* `payload`: the actual data we want to store in the token

* `secret`: a secret key that we sign the token with. Only our server will know the secret, so we can verify that the token came from it in the future.

Just before calling `jwt.sign()`, create the payload object.

```
const payload = { sub: 1 };
```


This code says that the `sub` claim (short for [subject](https://tools.ietf.org/html/rfc7519#section-4.1.2)) should be `1`. This would normally be a username or a user ID or something that identifies a unique user on your system. For now, we’ll just mock it up with a simple number.

Now for the `secret`. This will be the same for every token we sign. So place it at the top right under any `require`s.

```
const secret = process.env.JWT_SECRET || 'secret';
```


This code means that our tokens will be signed with the [environment variable](https://en.wikipedia.org/wiki/Environment_variable) `JWT_SECRET`. If none exists, the string “secret” will be used. Obviously, you’ll want to pick a strong secret only you know.

Now, when `POST`ing to [http://localhost:3000/auth,](http://localhost:3000/auth,) it responds with a JWT.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627410614844/Ic23_-wTW.png)

You can even paste this token into [https://jwt.io](https://jwt.io) and view it’s contents. This is an important concept: tokens shouldn’t contain sensitive information. Anyone can view them. The beauty of them becomes apparent when you realize that *only your server*, with the secret key, can verify it’s signature. When verified, you can be sure that your server was the one who issued the token.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627410616446/_EDM0Vb4F.png)

Take special note of the payload. Our `sub` key is there!

### Accepting form data

Right now, our server gives everyone who accesses the `auth` route a valid JWT. We really only want to give the token to those who supply the correct username and password.

First, we must correctly parse form data so the client can pass our server usernames and passwords. There is a great library for parsing form data.

```
> npm i koa-bodyparser
```


Use it in `index.js` like so:

```
const bodyParser = require('koa-bodyparser');

/* snip */

router.post('/auth', bodyParser(), authRoute);
```


Above, we `require` [koa-bodyparser](https://github.com/koajs/bodyparser). Then, we add it to the route.

Our server can now parse supplied form data in the [x-www-form-urlencoded](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/POST) format.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627410618190/gj40HBUTs.png)

We can access this information in the `auth` route.

```
module.exports = (ctx) => {
  const { username, password } = ctx.request.body;

  if (!username) ctx.throw(422, 'Username required.');
  if (!password) ctx.throw(422, 'Password required.');

  /* snip */
};
```


The code above will pull the supplied username and password. If either of them doesn’t exist, it throws an error and responds with a description of the problem.

But as it stands, our server doesn’t care what username or password you give it. We need a system to verify submitted credentials.

### Verifying credentials

A naive approach to verifying this information would be a system of `if` statements such as…

```
if (username === 'travis' && password === 'mypassword) { /*...*/ }
```


A better — and more standard — approach is looking up the supplied username and password in a database of users.

Let’s set up a basic [SQLite](https://www.sqlite.org/index.html) database. You can do this with any tool you like. I’m going to be using [DB Browser for SQLite](https://sqlitebrowser.org/).

### SQLite database

First, I’m going to create a new directory to house the database and database-accessing code. Under our jwt-server’s root directory, create a directory called `db`. Using DB Browser (or whatever tool you like), create `db.sqlite` in this directory.

The first table in this database will be called`users`.

```
CREATE TABLE `users` (
 `id` INTEGER NOT NULL PRIMARY KEY AUTOINCREMENT UNIQUE,
 `username` TEXT NOT NULL UNIQUE,
 `password` TEXT NOT NULL
);
```


With the table created insert the first row.

```
INSERT INTO `users` (
  `username`,
  `password`
) VALUES (
  'travis',
  'mypassword'
)
```


Choose any username and password you like.

If you’re using DB Browser like me, you can save this database with the table and row by clicking **Write Changes**.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627410620609/J9wxCz3Oq.png)
> Note: If you know anything about user information or database security, alarm bells might be going off in your head right now. We’re storing passwords in plaintext. Don’t worry, we’ll solve that in a moment.

The database is set up. We need to hook into it with our JWT server so we can look through it and verify usernames and passwords. We’ll need a couple new packages.

```
> npm i knex sqlite3
```


Inside the `db` folder, create `index.js`. This will be our main database instance.

```
const path = require('path');
const knex = require('knex');

module.exports = knex({
  client: 'sqlite3',
  connection: { filename: path.join(__dirname, 'db.sqlite') },
  useNullAsDefault: true,
});
```


[Knex](https://knexjs.org/) is a SQL query builder. The code above tells it that we have a SQLite database and it’s in this same directory with the filename `db.sqlite`. It also says to use `null` as default. We have to do that last part because SQLite doesn’t support inserting default values.

Back to our `auth` route. Pull in the database configuration.

```
const db = require('../db');
```


Database calls in Knex return [Promises](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise). We’re going to take advantage of that fact by using [JavaScript’s async/await features](https://javascript.info/async-await).

After the checks that `username` and `password` exist, find the database record.

```
const dbUser = await db.first(['id', 'password'])
  .from('users')
  .where({ username });

if (!dbUser) ctx.throw(401, 'No such user.');

if (password === dbUser.password) {

  /* Sign and return the token just like before
   * except this time, sub is the actual database
   * user ID. */

  const payload = { sub: dbUser.id };
  const token = jwt.sign(payload, secret);
  ctx.body = token;
} else {
  ctx.throw(401, 'Incorrect password.');
}
```


You may notice the use of `await` above. Since we are using `await`, we need to change how we define our parent function. Specifically, we need to add the `async` keyword.

```
module.exports = **async** (ctx) => {
  /* snip */
};
```


Perfect! The `auth` route only returns a signed token if the username and password are correct. Otherwise an error is given.

![Correct username and password = JWT](https://cdn.hashnode.com/res/hashnode/image/upload/v1627410622373/uZuAXGUrKi.png)*Correct username and password = JWT*

![Incorrect password = error](https://cdn.hashnode.com/res/hashnode/image/upload/v1627410624350/5iClaHI30.png)*Incorrect password = error*

… except there are a couple glaring issues.

### Issue #1: Incorrect credential messages

It is generally considered best practice to display the same error regardless of whether the username or password (or both) are incorrect. We can fix this easily. Create an error message constant.

```
const wrongUserPassMsg = 'Incorrect username and/or password.';
```


And replace all 401 error throws with this:

```
ctx.throw(401, wrongUserPassMsg);
```


### Issue #2: Plaintext passwords

We are storing our passwords in plaintext. [This is a very, very bad idea](https://security.stackexchange.com/questions/120540/why-shouldnt-i-store-passwords-in-plaintext). Avoid it at all costs. Instead, hash your passwords with something like [bcrypt](http://bcrypt.sourceforge.net/).

```
> npm i bcrypt-nodejs
```


This library uses callbacks, which just don’t play nice with Koa. Since we’re only going to need three functions from this library, we can pretty quickly wrap these functions with Promises, which *do* play nice with Koa.

Create a directory called `utilities`. Inside, create `bcrypt.js`. I’m just going to show the whole file here. We are basically wrapping `getSalt`, `hash`, and `compare` in Promises. It’s not too exciting, but necessary to use this library with Koa.

```
const bcrypt = require('bcrypt-nodejs');

const saltRounds = process.env.SALT_ROUNDS || 20000;

const genSalt = rounds => new Promise((resolve, reject) => {
  bcrypt.genSalt(rounds, (err, result) => {
    if (err) {
      reject(err);
    } else {
      resolve(result);
    }
  });
});

const hash = data => new Promise(async (resolve, reject) => {
  const salt = await genSalt(saltRounds);

bcrypt.hash(data, salt, null, (err, result) => {
    if (err) {
      reject(err);
    } else {
      resolve(result);
    }
  });
});

const compare = (data, encrypted) => new Promise((resolve, reject) => {
  bcrypt.compare(data, encrypted, (err, result) => {
    if (err) {
      reject(err);
    } else {
      resolve(result);
    }
  });
});

module.exports = {
  hash,
  compare,
};
```


You may notice that, at the end, we’re only exporting `hash` and `compare`. We don’t need to export `genSalt` because it’s only useful *inside* `hash`, which we *are* exporting.

The only other interesting thing to note is that we’re getting the number of [salt rounds](https://stackoverflow.com/questions/46693430/what-are-salt-rounds-and-how-are-salts-stored-in-bcrypt) from an environment variable called`SALT_ROUNDS`. If it doesn’t exist, we use 20,000.

For the purposes of this demo, I simply want to get a hash of the password I’m already using. In this case, I’ll simply use the [Node REPL](https://hackernoon.com/know-node-repl-better-dbd15bca0af6) to access my bcrypt utility library and hash the password directly.

```
> node
> const bcrypt = require('./utilities/bcrypt');
> bcrypt.hash('mypassword').then(hash => console.log(hash));

$2a$10$3pwJNVCM6NKzBOD1HPZHjuFdjX2Dg771aPZNbbBw9jTUbEMS919Te
```


The output there is what we really want to store in our database.

Let’s modify the `users` table in `db.sqlite`. The new table has this structure:

```
CREATE TABLE `users` (
 `id` INTEGER NOT NULL PRIMARY KEY AUTOINCREMENT UNIQUE,
 `username` TEXT NOT NULL UNIQUE,
 `passwordHash` TEXT NOT NULL
);
```


Notice `password` is gone and `passwordHash` is in it’s place.

Now I’ll simply copy & paste the generated hash into the `passwordHash` field and write changes.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627410626038/WGmKr-BiG.png)

### Comparing hashes

With these changes in place in the database, we need to modify the `auth` route.

First, `require` the bcrypt utility library.

```
const bcrypt = require('../utilities/bcrypt');
```


Second, instead of grabbing the password, we want to grab the password *hash*.

```
const dbUser = await db.first(['id', **'passwordHash'**])
  .from('users')
  .where({ username });
```


Third, instead of manually checking to see if the entered password equals the database password, we’ll use the `bcrypt.compare()` function.

```
if (await bcrypt.compare(password, dbUser.passwordHash)) {
  /* Snip.. return token like before */
} else {
  ctx.throw(401, wrongUserPassMsg);
}
```


We `await bcrypt.compare()` and pass it two arguments: the entered password and the password hash from the database. This function hashes the entered password and checks that it matches the hash in the database. If it does, it returns `true` and we return the token like before. Otherwise, it returns `false` and we return the same error as before.

### Using JWTs for protected data

Now we have a way to authenticate users and give them a signed JWT. How do you use the JWT?

Let’s say our database contained a list of pets for each user. Authenticated users should be able to see the pets *they* own.

Create a table called `pets` in the database.

```
CREATE TABLE `pets` (
 `id` INTEGER NOT NULL PRIMARY KEY AUTOINCREMENT UNIQUE,
 `user_id` INTEGER NOT NULL,
 `name` TEXT NOT NULL,
 `species` TEXT
);
```


I’m just going to insert a couple rows into this table.

```
INSERT INTO `pets` (
  `user_id`,
  `name`,
  `species`
) VALUES (
  1,
  'Bunny',
  'dog'
)

INSERT INTO `pets` (
  `user_id`,
  `name`,
  `species`
) VALUES (
  1,
  'Ru',
  'cat'
)
```


Remember that our user “travis” has an id of `1`. So these two pets are associated with him because they have `1` as their `user_id`.

### Authentication middleware

For each route that needs to be protected behind authentication, we will protect it with the same custom middleware. Let’s create it now. Inside the `middleware` directory, create `authenticated.js`.

The main function of this — and most other middlewares — looks like this.

```
module.exports = async (ctx, next) => {
  await next();
};
```


Right now, the middleware effectively does nothing. It simply moves on to the next middleware.

In our case, we want to check for the existence of a JWT here.

```
module.exports = async (ctx, next) => {
  if (!ctx.headers.authorization) ctx.throw(403, 'No token.');
  await next();
};
```


If no token is present, we throw a 403 error. Otherwise, continue on to the next middleware.

But it’s not actually enough to check for the existence of the token. We need to verify our server signed it. We need to use `jwt.verify()`. That means we’ll have to `require` the package first. At the top…

```
const jwt = require('jsonwebtoken');
```


Then get another reference to our secret.

```
const secret = process.env.JWT_SECRET || 'secret';
```


This secret has to match the one that the token was signed with. Again, only your server should have knowledge of this secret.

The way JWTs are passed from frontend to backend is via an authorization header. Typically, it will look something like this:

```
Bearer eyJhbGcizI1NInRVCJ9.eyJz..SNIP..OjEsImzOKnGk-edQsP9QV1INJmZA
```


Notice the value of the header starts with `Bearer`, then there’s a space, then the actual token. So after we check for the existence of the token, we capture the actual token by splitting the string into two and getting the second part, as indicated in bold below.

```
const jwt = require('jsonwebtoken');

const secret = process.env.JWT_SECRET || 'secret';

module.exports = async (ctx, next) => {
  if (!ctx.headers.authorization) ctx.throw(403, 'No token.');

  const token = ctx.headers.authorization.split(' ')[1];

  await next();
};
```


Remember, arrays start at 0, so 1 is the *second* value.

After acquiring the actual token, we can verify its signature.

```
try {
  ctx.request.jwtPayload = jwt.verify(token, secret);
} catch (err) {
  ctx.throw(err.status || 403, err.text);
}
```


The function `jwt.verify` returns the token’s payload (which includes our user id in the `sub` property). We set the `ctx.request.jwtPayload` variable to this value. That way it can be picked up further down the middleware chain.

If the token isn’t valid, however, it throws an error. The error is immediately caught by the [try…catch](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/try...catch) statement. Then it’s thrown again to the `errorHandler` we wrote before.

Now it’s time to see how our `authenticated` middleware works.

Back in `index.js`, `require` it after the `errorHandler` middleware.

```
const authenticated = require('./middleware/authenticated');
```


Now define a new route for getting the pets.

```
router.get('/my-pets', authenticated, petsRoute);
```


When a user accesses [http://localhost:3000/my-pets,](http://localhost:3000/my-pets,) their request first goes through the `authenticated` middleware that we just wrote, then to the `petsRoute`.

The `petsRoute` route doesn’t exist on our server yet. We have to `require` it first — underneath the `require` for `authRoute`.

```
const petsRoute = require('./routes/pets');
```


Now create `pets.js` in the `routes` directory.

```
const db = require('../db');

module.exports = async (ctx) => {
  const userId = ctx.request.jwtPayload.sub;

  const pets = await db.select(['name', 'species'])
    .from('pets')
    .where({ user_id: userId });

  ctx.body = pets;
};
```


The first thing we do in this route is `require` the database.

Then we get a reference to the authenticated user’s id. Remember in `authenticated.js`, we set `ctx.request.jwtPayload`? The payload contains the user’s id in the `sub` property.

We then select the name and species of every pet whose `user_id` is equal to the jwt payload’s `sub`. This way, we know we’re only grabbing pets for a user that’s been authenticated.

Finally, we send those pet objects to the client.

### Basic summary of the API server

1. Client `POST`s a `username` and `password` to [http://localhost:3000/auth.](http://localhost:3000/auth.)

1. If the credentials are correct, server returns a signed JWT

1. Client makes `GET` requests with the JWT in the `Authorization` header

1. If JWT exists and can be verified, server returns protected information

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627410627751/YpmvLu7m_.png)

There is one last thing we need to take care of.

By default, most clients will block JavaScript network requests across different domains. We need to enable our server to accept requests from anywhere. This is done with [CORS](https://enable-cors.org/) and the [@koa/cors](https://github.com/koajs/cors) middleware. Install the package.

```
> npm i @koa/cors@2
```


`require` it in `index.js`

```
const cors = require('@koa/cors');
```


And tell the server to use it. I recommend calling it *just* below the `errorHandler` middleware.

```
app.use(cors());
```


With that in place, our server will serve requests from any domain.

Source code for this article can be found on GitHub. Feel free to fork it and modify for your needs.
[**travishorn/koa-sqlite-jwt-server**
*An API server with JWT-authenticated routes. Uses Koa and Knex. - travishorn/koa-sqlite-jwt-server*github.com](https://github.com/travishorn/koa-sqlite-jwt-server)

A server like this can be used with a de-coupled JavaScript front-end — typically built with some framework like Angular, React, or Vue.

To enter new users into the database, you can create an unauthenticated “sign up” route and/or an administration panel which requires authentication along with something like an “admin” flag in the JWT payload.