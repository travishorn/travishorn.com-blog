---
title: "What did I learn this week? Knex.js & Bookshelf.js"
datePublished: Wed Mar 09 2016 17:30:04 GMT+0000 (Coordinated Universal Time)
cuid: ckrmf2df803lfems15fl52nhm
slug: what-did-i-learn-this-week-knex-js-bookshelf-js-95d3490e3a6f
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1627410425923/OX2zbbKRs.jpeg
tags: javascript, nodejs, databases, data-structures, sql

---


A while back, I found an awesome open-source project by h5bp called Front-end Job Interview Questions. The project is…
> A list of helpful front-end related questions you can use to interview potential candidates, test yourself or completely ignore.

I am not necessarily a full-time front-end developer, but I think I have a strong grasp on the subject. I took on the task of answering the large set of questions as a personal project. I learned a lot right away just by reading the questions. And I am still learning a lot as I go through and answer them. I’m having a blast.

I will be posting my answers to the questions on this blog periodically. Which brings me to the first question:

### What did you learn yesterday/this week?

My personal answer is the (both incredible) [Knex.js](http://knexjs.org/) SQL query builder and [Bookshelf.js](http://bookshelfjs.org/) ORM.

![Photo by [kazuend](https://cdn.hashnode.com/res/hashnode/image/upload/v1627410423778/qmhqEYnhJ.html)](https://cdn-images-1.medium.com/max/12000/1*pgRgT8aZHI7RaB0N8uSHAw.jpeg)*Photo by [kazuend](https://unsplash.com/kazuend)*

I have used [MongoDB](https://www.mongodb.org/) in many of my side projects and I really started to take a liking to [Mongoose](http://mongoosejs.com/) (an ODM for MongoDB). However, I kept on noticing that I needed relationships in my data, which would hint that I should be using a relational database, such as the many SQL database systems out there. In addition, I started questioning why I was starting with a schema-less database, and then immediately adding a ODM to create schemas.

I still love MongoDB and Mongoose for their syntax and innate compatibility with JavaScript and JSON. But lately I’ve been using the SQL database systems that I grew up on more and more in my side projects.

In the past, interfacing with a SQL server in Node.js was a chore. I have tried many modules, but none of them were easy enough to use or “finished” enough. However, with my re-sparked interest in relational databases, I did another search and — wow — the market has changed.

The first module I tried using was [Sequelize](http://docs.sequelizejs.com/en/latest/). I really like it and I’m still using it in some ongoing projects. However, there is something about it that just doesn’t fit my style.

Somewhere along the way I stumbled across Bookshelf.js. This is a library I *really* get along with. In the process of setting it up, I realized it relies heavily on Knex.js, which I also find interesting. Here, I’ll walk you through my first encounter with these modules.

### Building a CRUD API using Knex.js and Bookshelf.js with [Express](http://expressjs.com/)

My first project was just a simple test of how everything works. A [CRUD](https://en.wikipedia.org/wiki/Create,_read,_update_and_delete) API for managing contacts’ email addresses.

First, create a new directory to house the project…

```
mkdir contacts
cd contacts
```


Initialize npm and answer all its questions…

```
npm init

name: contacts
version: (1.0.0)
description: A contacts manager using Bookshelf.js
entry point: index.js
test command:
git repository:
keywords: contacts manager store api
author: Travis Horn
license: MIT


Is this ok? yes
```


Install the necessary modules…

```
npm install --save express body-parser knex bookshelf sqlite3
```


Let’s start with a simple index.js…

```
'use strict';

var express = require('express');
var bodyParser = require('body-parser');

var contactRoute = require('./routes/contact');

var app = express();

var PORT = process.env.PORT || 3000;

app.use(bodyParser.urlencoded({ extended: true });
app.use('/contact', contactRoute);

app.listen(PORT, function() {
  console.log('Contacts server listening on port %s.', PORT);
});
```


This will get the server up and running, but you may have noticed we’ve required a contactRoute module that we still need to set up to handle all HTTP requests to the /contact route.

Let’s create routes/contact.js…

```
'use strict';

var express = require('express');

var Contact = require('../models/contact');

var router = express.Router();

router.route('/')
  .get(function(req, res) {
    Contact
      .fetchAll()
      .then(function(contacts) {
        res.json({ contacts });
      });
  });

module.exports = router;
```


Now, our route will respond to HTTP GET requests to /contact. The Contact.fetchAll().then(); call is our first dive into Bookshelf.js.

But before we can use this route, we need to define the Contact model.

Create models/contact.js…

```
'use strict';

var bookshelf = require('../bookshelf');

var Contact = bookshelf.Model.extend({
  tableName: 'contacts',
});

module.exports = Contact;
```


That’s it for the model, but please take note of two things:

1. We are not requiring the bookshelf module, but rather a bookshelf.js *file* that we will soon create in our project’s root directory. This will be our connection to the database. All models will use this file.

1. We didn’t define any columns for our table. Why? Because Bookshelf actually looks at the table you defined in the tableName property and gets the columns from there. You can create this table any way you want, but we will look at an easy way to do this with Knex.js migrations later.

Since we are requiring the ../bookshelf.js file in our model, let’s create that…

```
'use strict';

var knex = require('knex')(require('./knexfile'));

var bookshelf = require('bookshelf')(knex);

module.exports = bookshelf;
```


We are finally requiring the bookshelf module here. When we do, we are passing along an instance of knex. And when we require knex, we are passing along a configuration object called knexfile.js.

Let’s make knexfile.js so we can tell Knex how to connect to our database server (or in this case, the SQLite file)…

```
'use strict';

module.exports = {
  client: 'sqlite3',
  connection: {
    filename: 'database.sqlite3',
  },
  useNullAsDefault: true,
};
```


We’re almost done, but remember that the Bookshelf model will attempt to read our table and create a model based on it. We need to create the contacts table. Fortunately, we can define our table in JavaScript and use the Knex CLI to create it. This is called a “migration.”

First, create migrations/contacts.js…

```
'use strict';

exports.up = function(knex) {
  return knex.schema
    .createTable('contacts', function(table) {
      table.increments('id').primary();
      table.string('firstName');
      table.string('lastName');
      table.string('emailAddress');
    });
};

exports.down = function(knex) {
  return knex.schema
    .dropTable('contacts');
};
```


We can run this migration with the Knex CLI. One option is to install Knex globally and run it.

```
npm install -g knex
knex migrate:latest
```


Another option, since we already installed Knex locally, is to run

```
node_modules/.bin/knex migrate:latest
```


My favorite option, though, is to use an npm script. npm automatically looks in /node_modules/.bin for executable commands. To set this up, just create a new script in package.json…

```
...

"scripts": {
  "test": "echo \"Error: no test specified\" && exit 1",
  "migrate": "knex migrate:latest"
},

...
```


Now that this is set up, simply run

```
npm run migrate
```


To test out the server, run

```
node index.js
```


After a moment, you should see “Contacts server listening on port 3000.” If you GET [http://127.0.0.1:3000/contact](http://127.0.0.1:3000/contact), you will get a response:

```
{"contacts":[]}
```


Since there are no contacts in the database at the moment, the array is empty.

Of course, we want to create routes for creating (POST), updating (PUT), and deleting (DELETE) contacts. Go back to routes/contact.js.

For creating (POST) a contact, write…

```
...

router.route('/')
  .get(function(req, res) {
    ...
  })
  .post(function(req, res) {
    new Contact({
      firstName: req.body.firstName,
      lastName: req.body.lastName,
      emailAddress: req.body.emailAddress,
    })
      .save()
      .then(function(saved) {
        res.json({ saved });
      });
  });

...
```


For updating (PUT) and deleting (DELETE), we will need a separate route that includes the id we want to modify.

Let’s do updating first. Again, in routes/contact.js…

```
...

router.route('/:id')
  .put(function(req, res) {
    Contact
      .where('id', req.params.id)
      .fetch()
      .then(function(contact) {
        contact
          .save({
            firstName: req.body.firstName,
            lastName: req.body.lastName,
            emailAddress: req.body.emailAddress,
          })
          .then(function(saved) {
            res.json({ saved });
          });
      });
  });

module.exports = router;
```


And deleting…

```
...

router.route('/:id')
  .put(function(req, res) {
    ...
  })
  .delete(function(req, res) {
    Contact
      .where('id', req.params.id)
      .destroy()
      .then(function(destroyed) {
        res.json({ destroyed });
      });
  });
```


If you restart your server…

```
node index.js
```


and POST [http://127.0.0.1:3000/contact](http://127.0.0.1:3000/contact) with a urlencoded body of firstName=Travis&lastName=Horn&emailAddress=travis%40travishorn.com, you will get a response:

```
{
  “saved”: {
    “firstName”: “Travis”,
    “lastName”: “Horn”,
    “emailAddress”: “travis@travishorn.com”,
    “id”: 1
 }
}
```


and if you GET [http://127.0.0.1:3000/contact,](http://127.0.0.1:3000/contact,) you will get the response:

```
{
  “contacts”: [
    {
      “id”: 1,
      “firstName”: “Travis”,
      “lastName”: “Horn”,
      “emailAddress”: “travis@travishorn.com”
     }
   ]
}
```


We can update the record by sending a PUT request to [http://127.0.0.1:3000/contact/1](http://127.0.0.1:3000/contact/1) with a urlencoded body of firstName=Travis&lastName=Horn&emailAddress=me%40travishorn.com, you will get the response:

```
{
  “saved”: {
    “firstName”: “Travis”,
    “lastName”: “Horn”,
    “emailAddress”: “me@travishorn.com”,
    “id”: 1
 }
}
```


and finally, if you send a DELETE request to [http://127.0.0.1:3000/contact/1,](http://127.0.0.1:3000/contact/1,) you will get the response:

```
{
  “destroyed”: {}
}
```


The record no longer exists and you get an empty object back.

If you stuck around this long, it might be worth mentioning that you can implement a search function pretty easily. Modify routes/contact.js…

```
...

router.route('/')
  .get(function(req, res) {
    Contact
      .where(req.query)    // Add this one line
      .fetchAll()
      .then(function(contacts) {
        res.json({ contacts });
      });
  })
  ...

...
```


With the simple addition of a where() function/clause we can now send GET requests like [http://127.0.0.1:3000/contact?firstName=Travis](http://127.0.0.1:3000/contact?firstName=Travis) and it will only return records that match the query. Pretty cool!

The code for this post is open source and located at [https://github.com/travishorn/express-contacts-crud](https://github.com/travishorn/express-contacts-crud). Feel free to fork and build upon it.

I plan on continuing to post answers to the Front-end questions project on a semi-regular basis. I will try to answer all the questions, but I may not post about them all. Some are too short or don’t have much interesting discussion involved. Be on the lookout for the other posts.