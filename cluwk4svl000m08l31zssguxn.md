---
title: "Introducing libsql Migrate"
datePublished: Fri Apr 12 2024 11:00:50 GMT+0000 (Coordinated Universal Time)
cuid: cluwk4svl000m08l31zssguxn
slug: introducing-libsql-migrate
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1712849384666/5b5d2dce-db10-447f-9363-321db8d01246.png
tags: databases, migration, sqlite, libsql

---

**libsql Migrate** is a database migration and seed management tool for [libsql](https://github.com/tursodatabase/libsql) with configurable options, built with [Node.js](https://nodejs.org/) and [available as an npm package](https://www.npmjs.com/package/libsql-migrate).

%[https://github.com/travishorn/libsql-migrate] 

## Installation

The easiest method is a global installation via [npm](https://www.npmjs.com/).

```bash
npm install -g libsql-migrate
```

The `libsql-migrate` command is now available for your usage.

### Local Installation

Instead of installing globally, you can install locally at the project-level.

```bash
npm install --save-dev libsql-migrate
```

This prevents compatibility issues because it defines the version of `libsql-migrate` that the migrations/seeds in your repository were written for.

You can run `npx libsql-migrate <command>` and/or add migration commands to your `package.json`:

```json
{
  "name": "mypackage",
  "version": "0.1.0",
  "scripts": {
    "migrate": "libsql-migrate latest",
    "seed": "libsql-migrate seed:run"
  }
}
```

## Usage

First, navigate to your project's root directory

```bash
cd my/project/root
```

Then, create a fresh configuration file.

```bash
libsql-migrate init

# prefix with npx if you installed locally, like so:
npx libsql-migrate init
```

This adds a file called `libsqlrc.js` to your project with the following contents. Modify it to meet your project's configuration.

```javascript
export default {
  development: {
    connection: {
      url: "file:local.db",
    },
  },
  production: {
    connection: {
      url: "libsql://...",
      authToken: "...",
    },
  },
};
```

<div data-node-type="callout">
<div data-node-type="callout-emoji">💡</div>
<div data-node-type="callout-text">This file should be tracked alongside the rest of your source code.</div>
</div>

### Making a Migration

When you want to make changes to your database, create a new migration.

```bash
libsql-migrate make demo
```

<div data-node-type="callout">
<div data-node-type="callout-emoji">💡</div>
<div data-node-type="callout-text">Replace <code>demo</code> with whatever name you'd like to give the migration. Ideally, it would be a short name that describes the changes.</div>
</div>

A file with the current timestamp and the name you chose will be added to the migrations directory. This directory is `./migrations` by default, but can be configured in `libsqlrc.js` like so:

```javascript
export default {
  development: {
    connection: {
      url: "file:local.db",
    },
    migrations: {
      directory: "my_migrations_directory",
    },
  },
  // ...
};
```

Migration files look like this:

```javascript
export async function up(client) {}

export async function down(client) {}
```

In the migration file, write the code that brings your schema **up** toward the latest version in the `up()` function. For example:

```javascript
export async function up(client) {
  await client.execute(
    "CREATE TABLE users (id INTEGER PRIMARY KEY AUTOINCREMENT, name TEXT);",
  );
}
```

Now, write the code that brings your schema **down** (reverses those changes) in the `down()` function.

```javascript
export async function down(client) {
  await client.execute("DROP TABLE users;");
}
```

<div data-node-type="callout">
<div data-node-type="callout-emoji">💡</div>
<div data-node-type="callout-text">Track the migrations directory in source control. Ideally, it will be a living history of changes to your database schema.</div>
</div>

### Running the Next Migration

Once a migration is written, it needs to be run against the database to commit the changes it contains.

The `up` subcommand runs the next migration that has not yet been run.

```bash
libsql-migrate up
```

The `up()` function in the next migration file (alphabetically) in the migrations directory will be executed.

You can repeatedly run this command to keep migrating up. If you want to run all pending migrations to bring the database schema fully up-to-date, use the `latest` subcommand (detailed later).

### Rolling Back the Latest Migration

Sometimes you need to undo the changes that you previously made. Use the `down` subcommand for that.

```bash
libsql-migrate down
```

The `down()` function in the most recently executed migration file will be executed.

You can repeatedly run this command to roll back the database schema further and further back.

### Running All Pending Migrations

If you have multiple pending migrations and you simply want to migrate the database schema all the way up so it's fully up-to-date, use the `latest` subcommand.

```bash
libsql-migrate latest
```

The `up()` function for all pending migration files will be executed in series, alphabetically.

<div data-node-type="callout">
<div data-node-type="callout-emoji">💡</div>
<div data-node-type="callout-text">All migrations that were run during this command are considered part of the same "batch".</div>
</div>

### Rolling Back the Latest Batch

If you ran `libsql-migrate latest` and want to revert the changes it made, you can always roll back the latest batch.

```bash
libsql-migrate rollback
```

The `down()` function for all migrations in the last batch will be executed in series.

### Making a Seed

If you want to fill your database with some preset data, start by generating a new seed file.

```bash
libsql-migrate seed:make demo
```

<div data-node-type="callout">
<div data-node-type="callout-emoji">💡</div>
<div data-node-type="callout-text">Replace <code>demo</code> with whatever name you'd like to give the seed.</div>
</div>

A file with the name you chose will be added to the seeds directory. This directory is `./seeds` by default, but you can change that in `libsqlrc.js` like this if you wish:

```javascript
export default {
  development: {
    connection: {
      url: "file:local.db",
    },
    seeds: {
      directory: "my_seeds_directory",
    },
  },
  // ...
};
```

Seed files look like this:

```javascript
export async function seed(client) {}
```

Inside the file, write the code that seeds your database with preset data in the `seed()` function.

```javascript
export async function seed(client) {
  await client.execute("DELETE FROM users;");
  await client.execute("INSERT INTO users (name) VALUES ('admin')");
}
```

<div data-node-type="callout">
<div data-node-type="callout-emoji">💡</div>
<div data-node-type="callout-text">While not required, you'll probably want to delete existing data (like the example above shows) before seeding.</div>
</div>

### Run a Seed

Use the `seed:run` subcommand with the name of the seed to run it.

```bash
libsql-migrate seed:run demo
```

This will execute the `seed()` function inside `demo.js` in the seeds directory.

### Run Multiple Seeds

You can run multiple seeds by providing multiple names.

```bash
libsql-migrate seed:run demo demo2 demo3
```

This will run the `demo`, `demo2`, and `demo3` seed files (in alphabetical order, no matter what order you type them).

### Run All Seeds

You can run all seeds in alphabetical order by running the `seed:run` subcommand without any names.

```bash
libsql-migrate seed:run
```

---

That's all you need to get started with **libsql Migrate**. If you'd like to read more about how and why I created this tool, read on!

## Why Did I Create This?

With [PlanetScale eliminating their free tier](https://planetscale.com/blog/planetscale-forever), I was on the hunt for an alternative database hosting platform. Lately, I've been hearing some buzz about [Turso](https://turso.tech/). In addition to some other excellent features, their [free tier](https://turso.tech/pricing) allows for 9 GB of storage, 500 databases, 1 billion row reads per month, and 25 million row writes per month.

I was delighted to hear that Turso uses libsql under the hood, which is a fork of [SQLite](https://www.sqlite.org/index.html). I was already familiar with SQLite and used it with numerous small and medium size apps. Being able to develop locally with libsql and then seamlessly deploy to Turso (especially with the generous free tier) made trying it out a no-brainer.

I was eager to get started, so I created a new project and installed the `@libsql/client` npm package.

```bash
npm init -y
npm install @libsql/client
```

To start, I wanted to develop locally. So I created `index.js` and started coding.

```javascript
import { createClient } from "@libsql/client";

const client = createClient({
  url: "file:local.db"
});
```

Now at this point, I could have just executed some initial queries to create a table and insert some default data like this:

```javascript
await client.execute(`
  CREATE TABLE users (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    name TEXT
  );
`);

await client.execute(`INSERT INTO users (name) VALUES ('admin');`);
```

But I obviously didn't want to run this every time my `index.js` file ran.

My next thought was to move the creation and seed queries into a separate, standalone file that I could run just to initialize the database.

But I figured...

1. If I'm going through the effort of imitating a database migration system, and
    
2. I want the added safety of versioning my database schema
    

...I might as well go all-in on setting up proper database migration.

### No ORM?

You might be thinking, "wait! Aren't you going to use [Prisma](https://www.prisma.io/) or [Drizzle](https://orm.drizzle.team/) or some other ORM-like tool?" It's true, if I used a tool like that, it would reduce or eliminate the need for database migrations in the way **libsql Migrate** does them. See, I find myself eventually preferring to write raw SQL in almost every project.

Mainly because, in a big enough application, queries become complex. So complex that you must use an ORM's raw query feature anyway. And for the simpler queries, I find myself starting by writing raw SQL in database administration software until the results look how I want them and then trying to figure out how to convert it to whatever specialized format the ORM I'm using at the time uses.

I like the way [Knex.js](https://knexjs.org/) does migrations. Unfortunately, there is no libsql support in Knex.js. So if I wanted to write Knex.js-style migrations, I'd have to write extra code to run them.

So I started a new repository and refactored the migration-running code into it. That side-project grew in scope for a while until it was a complete database migration system with all the features I needed. Thus, **libsql Migrate** was born!

## Contributions Welcome

If you find a bug or would like to add a feature, do not hesitate to fork the project on GitHub and send a pull request! This project is MIT-licensed.

%[https://github.com/travishorn/libsql-migrate]