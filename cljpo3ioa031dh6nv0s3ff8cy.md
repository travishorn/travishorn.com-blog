---
title: "Introducing the Knex Adapter for Auth.js"
datePublished: Wed Jul 05 2023 12:00:39 GMT+0000 (Coordinated Universal Time)
cuid: cljpo3ioa031dh6nv0s3ff8cy
slug: introducing-the-knex-adapter-for-authjs
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1686940098799/9dc813ea-2f9e-4c41-ad26-ac11ad278fcb.png
tags: authentication, opensource, knexjs, nextauthjs, authjs

---

I am excited to share with you my latest open-source project, an unofficial adapter for [Auth.js/NextAuth.js](https://authjs.dev/) that allows seamless connectivity to any database service using [Knex.js](https://knexjs.org/). In this article, I delve into the necessary steps before installation, guide you through the installation process, and demonstrate how to integrate it effortlessly with [Next.js](https://nextjs.org/) or [SvelteKit](https://kit.svelte.dev/). Furthermore, I provide insights into the motivation behind creating this adapter - an essential tool I needed for the major rewrite of apps and services at my workplace. Although Auth.js proves to be an excellent authentication library, it lacks an official Knex.js adapter. Hence, I took it upon myself to fill this gap. In this article, we'll also take a deep dive into the source code, unraveling the inner workings so that you can understand how Auth.js interfaces with your database.

## Installation

I highly recommend getting NextAuth.js (aka Auth.js) up and running with your OAuth provider(s) first. Once all of that is working, then work on connecting it to your database using this adapter.

Assuming...

1. you already have a Next.js, SvelteKit, or SolidStart app, and
    
2. NextAuth.js/Auth.js is already working with it, and
    
3. you have Knex.js installed and configured to work with your database,
    

all you need to do is install this adapter:

```bash
npm install authjs-knexjs-adapter
```

Then, configure it according to your framework.

### Using it with Next.js

Import the adapter into `pages/api/auth/[...nextauth].ts`, give it a Knex.js database connection, and set it as the `adapter` in your `NextAuth()` configuration.

```typescript
import { KnexAdapter } from "authjs-knexjs-adapter";
import knex from "knex";
import NextAuth from "next-auth";

const db = knex({
  client: "mysql2",
  connection: {
    host: process.env.DB_HOST,
    port: parseInt(process.env.DB_PORT ?? "3306", 10),
    user: process.env.DB_USER,
    password: process.env.DB_PASSWORD,
    database: process.env.DB_DATABASE,
  },
});

export default NextAuth({
  adapter: KnexAdapter(db),
  providers: [ /* your providers */ ],
});
```

### Using it with SvelteKit

Import the adapter into `src/hooks.server.js`, give it a Knex.js database connection, and set it as the `adapter` in your `SvelteKitAuth` configuration.

```javascript
import { DB_HOST, DB_PORT, DB_USER, DB_PASSWORD, DB_DATABASE } from '$env/static/private';
import { KnexAdapter } from 'authjs-knexjs-adapter';
import { SvelteKitAuth } from '@auth/sveltekit';
import knex from "knex";

const db = knex({
  client: 'mysql2',
  connection: {
    host: DB_HOST,
    port: parseInt(DB_PORT ?? '3306', 10),
    user: DB_USER,
    password: DB_PASSWORD,
    database: DB_DATABASE
  }
});

export const handle = SvelteKitAuth({
  adapter: KnexAdapter(db),
  providers: [ /* your providers */ ],
});
```

### Using it with SolidStart

I don't have any experience with SolidJS or SolidStart. However, this adapter should work out of the box with your existing Auth.js configuration. Just set it as the adapter. Have you used this successfully in your SolidStart app? Let me know!

## Database Schema

Just configuring your app is not enough. Auth.js expects a specific database schema. If you're using a SQL database, try these commands:

```sql
CREATE TABLE `User` (
  `id` char(36) NOT NULL DEFAULT uuid(),
  `name` text DEFAULT NULL,
  `email` text DEFAULT NULL,
  `emailVerified` timestamp DEFAULT NULL,
  `image` text DEFAULT NULL,
  PRIMARY KEY (`id`),
  UNIQUE KEY `user_email_unique` (`email`) USING HASH
);

CREATE TABLE `Account` (
  `id` char(36) NOT NULL DEFAULT uuid(),
  `userId` char(36) DEFAULT NULL,
  `type` text NOT NULL,
  `provider` text NOT NULL,
  `providerAccountId` text NOT NULL,
  `refresh_token` text DEFAULT NULL,
  `access_token` text DEFAULT NULL,
  `expires_at` bigint(20) DEFAULT NULL,
  `expires_in` bigint(20) DEFAULT NULL,
  `token_type` text DEFAULT NULL,
  `scope` text DEFAULT NULL,
  `id_token` text DEFAULT NULL,
  `session_state` text DEFAULT NULL,
  PRIMARY KEY (`id`),
  UNIQUE KEY `account_provider_provideraccountid_unique` (`provider`,`providerAccountId`) USING HASH,
  KEY `account_userid_foreign` (`userId`),
  CONSTRAINT `account_userid_foreign` FOREIGN KEY (`userId`) REFERENCES `User` (`id`)
);

CREATE TABLE `Session` (
  `id` char(36) NOT NULL DEFAULT uuid(),
  `expires` timestamp NOT NULL,
  `sessionToken` text NOT NULL,
  `userId` char(36) DEFAULT NULL,
  PRIMARY KEY (`id`),
  UNIQUE KEY `session_sessiontoken_unique` (`sessionToken`) USING HASH,
  KEY `session_userid_foreign` (`userId`),
  CONSTRAINT `session_userid_foreign` FOREIGN KEY (`userId`) REFERENCES `User` (`id`)
);

CREATE TABLE `VerificationToken` (
  `identifier` text DEFAULT NULL,
  `token` varchar(255) NOT NULL,
  `expires` timestamp NOT NULL,
  PRIMARY KEY (`token`),
  UNIQUE KEY `verificationtoken_token_identifier_unique` (`token`,`identifier`) USING HASH
);
```

If you would rather use a Knex.js migration, use the following. Note that you may need to modify some lines. I successfully used this migration with MariaDB, but your database engine might require some customization.

```javascript
export const up = (knex) => {
  return knex.schema
    .createTable("User", (table) => {
      table.uuid("id").defaultTo(knex.raw("(UUID())"));
      table.text("name");
      table.text("email").unique();
      table.timestamp("emailVerified");
      table.text("image");
      table.primary("id");
    })
    .createTable("Account", (table) => {
      table.uuid("id").defaultTo(knex.raw("(UUID())"));
      table.uuid("userId");
      table.text("type").notNullable();
      table.text("provider").notNullable();
      table.text("providerAccountId").notNullable();
      table.text("refresh_token");
      table.text("access_token");
      table.bigInteger("expires_at");
      table.bigInteger("expires_in");
      table.text("token_type");
      table.text("scope");
      table.text("id_token");
      table.text("session_state");
      table.primary("id");
      table.unique(["provider", "providerAccountId"]);
      table.foreign("userId").references("id").on("User");
    })
    .createTable("Session", (table) => {
      table.uuid("id").defaultTo(knex.raw("(UUID())"));
      table.timestamp("expires").notNullable();
      table.text("sessionToken").notNullable().unique();
      table.uuid("userId");
      table.primary("id");
      table.foreign("userId").references("id").on("User");
    })
    .createTable("VerificationToken", (table) => {
      table.text("identifier");
      table.string("token", 255);
      table.timestamp("expires").notNullable();
      table.primary("token");
      table.unique(["token", "identifier"]);
    });
};

export const down = (knex) => {
  return knex.schema
    .dropTable("VerificationToken")
    .dropTable("Session")
    .dropTable("Account")
    .dropTable("User");
};
```

With the adapter configured and the database schema created, launch your app (via `npm run dev` or similar) and enjoy!

## Why did I create this adapter?

I recently started a major rewrite of several apps and services at work. My primary goal was to write a single app that was flexible enough to cover all the existing use cases while also being modular enough to hook into any information system we may use now or in the future.

As part of this rewrite, I needed a way to authenticate users inside Next.js. Whatever solution I came up with had to support [Google as an OAuth provider](https://developers.google.com/identity/protocols/oauth2), but also be able to support other providers and [custom credential authentication](https://authjs.dev/getting-started/credentials-tutorial) if necessary in the future.

I found NextAuth.js (currently going through a name change to **Auth.js**), which is a great solution.

You just install it.

```bash
npm install next-auth
```

And create the server configuration at `pages/api/auth/[...nextauth].ts`.

```typescript
import NextAuth from "next-auth"
import GoogleProvider from "next-auth/providers/google"

export default NextAuth({
  providers: [
    GoogleProvider({
      clientId: process.env.OAUTH_GOOGLE_CLIENT_ID,
      clientSecret: process.env.OAUTH_GOOGLE_CLIENT_SECRET,
    }),
  ],
})
```

Note: Auth.js is adding official support for other frameworks as we speak. I'm using it successfully with SvelteKit right now.

This works great, except I needed the app to persist user information and support fine-grained authorization. For those things, I needed Auth.js to connect to a database.

Thankfully, there are official adapters for database connectivity. There are adapters for Firebase, MongoDB, Prisma, Sequelize, Supabase, and more.

I was already using Knex.js in my app so I went looking for that adapter. Unfortunately, there was no official adapter for Knex.js. On the plus side, however, Auth.js provides handy [documentation for writing an adapter yourself](https://authjs.dev/guides/adapters/creating-a-database-adapter).

I got to work creating the database schema and the Knex.js adapter. What resulted is this unofficial Knex adapter for Auth.js. It is available for installation via npm (`npm install authjs-knexjs-adapter`) and the source is available on GitHub.

%[https://github.com/travishorn/authjs-knexjs-adapter] 

## A Dive into the Source Code

If you're interested in some of the more technical details, continue reading!

At the most basic level, an adapter is a single function that takes a database connection and returns an Auth.js "adapter." So the outline of the function looks like this:

```typescript
import type { Adapter } from "next-auth/adapters";
import type { Knex } from "knex";

export { Adapter, Knex };

/**
 * An adapter for Auth.js/NextAuth.js to allow you to connect to any database
 * service via Knex.js.
 *
 * @param {Knex} knex - The Knex.js connection
 * @param options - Auth.js options
 * @returns {Adapter} - An Auth.js adapter for Knex.js
 */
export function KnexAdapter(knex: Knex): Adapter {
  return {};
}
```

It is written in TypeScript.

First, it imports the `Adapter` type from `next-auth/adapters` and the `Knex` type from `knex`. This gives us type safety for the rest of the code. Next, it exports those types back out so consumers of this adapter can enjoy type safety as they develop their app.

The next block is just a JSDoc comment to help developers understand the purpose and signature of the function.

Finally, the actual adapter function itself is exported. Notice it takes a Knex connection and returns an Auth.js adapter. The return value is empty in the code above. Let's take a look at what goes inside there.

```typescript
return {
  async createUser(user) {},
  async getUser(id) {},
  async getUserByEmail(email) {},
  async getUserByAccount({ provider, providerAccountId }) {},
  async updateUser(user) {},
  async deleteUser(userId) {},
  async linkAccount(account) {},
  async unlinkAccount({ provider, providerAccountId }) {},
  async createSession(session) {},
  async getSessionAndUser(sessionToken) {},
  async updateSession(session) {},
  async deleteSession(sessionToken) {},
  async createVerificationToken(verificationToken) {},
  async useVerificationToken({ identifier, token }) {},
};
```

You can see that an adapter is an object that contains a series of functions that interact with a database. Auth.js calls on all of these functions at some point or another (depending on the flow of the app into which it's integrated).

Let's go through them one by one.

Starting with `createUser()`.

```typescript
async createUser(user) {
  // Insert user data into `User` table
  await knex("User").insert(user);

  // Get the full row that was just inserted
  const dbUsers = await knex("User")
    .select("*")
    .where({ email: user.email })
    .limit(1);

  // Return the newly inserted user data
  return dbUsers[0];
}
```

It takes a `user` object and inserts it into the `User` table. Then it selects the row that was just inserted, using the email to identify it (the `email` column is unique in the database schema). Finally, it returns that selected user object.

*Where does that* `knex()` *function come from?* Remember that it is given to the `KnexAdapter` function. It will be provided in the consuming app. Such as in `pages/api/auth/[...nextauth].ts` like so:

```typescript
const db = knex({ /* database connection properties */ });

export default NextAuth({
  adapter: KnexAdapter(db),
});
```

*Why do we have to select the user immediately after inserting it? Can't we use Knex.js's* `returning()` *method?* Not really. Specifying which columns should be returned during insert, update, and delete operations isn't supported by all database engines. Selecting the data as a separate transaction after inserting it ensures this adapter works with all engines. If you know you will be using a specific engine that supports `returning()` and you want to make this adapter more efficient, I encourage you to fork the code and make those changes. Maybe you could even open-source your fork to allow others to use it and collaborate with you!

*Why do we even have to select anything from the database at all? Aren't we just selecting the same data we just inserted?* We are selecting the data as it sits in the database, including the newly created `id` the database assigned. Just returning the original `user` object is not enough.

Now let's look at `getUser()`.

```typescript
async getUser(id) {
  // Get a user row based on the id
  const dbUsers = await knex("User").select("*").where({ id }).limit(1);

  // If no user was found, return null
  if (dbUsers.length === 0) return null;

  // Return the user data
  return dbUsers[0];
}
```

It takes a user's `id` and selects the row from the database that matches that `id`. If no record was found, the `id` does not belong to any user in the database and it returns `null`. Otherwise, it returns the user object.

Next is `getUserByEmail()`.

```typescript
async getUserByEmail(email) {
  // Get a user row based on the email
  const dbUsers = await knex("User").select("*").where({ email }).limit(1);

  // If no user was found, return null;
  if (dbUsers.length === 0) return null;

  // Return the user data
  return dbUsers[0];
}
```

This function does the same thing as the last one, except it looks up a user by their `email` instead of their `id`.

How about `getUserByAccount()`? This one is a little more complicated because it involves a database join.

```typescript
async getUserByAccount({ provider, providerAccountId }) {
  // Get a user row based on the associated account given the unique
  // provider account id and provider
  const dbUsers = await knex("User")
    .select("User.*")
    .join("Account", "Account.userId", "=", "User.id")
    .where({
      "Account.provider": provider,
      "Account.providerAccountId": providerAccountId,
    })
    .limit(1);

  // If no user was found, return null
  if (dbUsers.length === 0) return null;

  // Return the user data
  return dbUsers[0];
}
```

This function also looks up a user, but not by any single property of the user. Instead, it takes a unique `provider` and `providerAccountId` pair to find the account and then finds the `userId` associated with that account.

It joins the `User` and `Account` tables together based on the `userId` to get the user data which it ultimately returns.

Next, let's look at `updateUser()`.

```typescript
async updateUser(user) {
  // Update a user row based on id
  await knex("User").where({ id: user.id }).update(user);

  // Get the row that was just updated
  const dbUsers = await knex("User")
    .select("*")
    .where({ id: user.id })
    .limit(1);

  // Return the user data
  return dbUsers[0];
}
```

It takes a user object and updates `User` table with its contents, based on the value of its `id`.

Again, it necessarily uses a separate transaction to select the user object that was just updated and then returns it. This ensures it returns the exact data that is sitting in the database.

Now for `deleteUser()`.

```typescript
async deleteUser(userId) {
  // Delete session data for the given user
  await knex("Session").where({ userId }).del();

  // Delete account data for the given user
  await knex("Account").where({ userId }).del();

  // Delete user data for the given user
  await knex("User").where({ id: userId }).del();
}
```

This one is pretty simple. It takes a `userId` and deletes all data related to it. It executes three transactions to delete data from three tables (`Session`, `Account`, and `User`). Another technique to do this would have been to just delete the row in the `User` table and let the database cascade those changes to the other tables. However, that method relies on the database engine supporting foreign constraints, supporting cascading deletes, and being configured correctly. Executing three separate transactions is the safer option because it covers more cases.

Moving on to `linkAccount()`.

```typescript
async linkAccount(account) {
  // Insert account data into `Account` table
  await knex("Account").insert(account);

  // Get the row that was just inserted
  const dbAccounts = await knex("Account")
    .select("*")
    .where({
      provider: account.provider,
      providerAccountId: account.providerAccountId,
    })
    .limit(1);

  // Return the account data
  return dbAccounts[0];
}
```

This function is called when Auth.js wants to link an account with a user, which is particularly used with OAuth flows. Functionally, however, "linking" an account is just inserting a new record into the `Account` table. The code inserts the account, selects the data that was just inserted, and returns it. A very familiar pattern at this point.

How about `unlinkAccount()`?

```typescript
async unlinkAccount({ provider, providerAccountId }) {
  // Delete an account row based on provider information
  await knex("Account").where({ provider, providerAccountId }).del();
}
```

This function is used when Auth.js wants to "unlink" an OAuth account from a user. Functionally, it deletes the row that matches the given provider and provider account id.

We've already looked at creating *users* and *accounts*. Now let's look at creating a *session* with `createSession()`.

```typescript
async createSession(session) {
  // Insert a session row into the `Session` table
  await knex("Session").insert(session);

  // Get the row that was just inserted
  const dbSessions = await knex("Session")
    .select("*")
    .where({ sessionToken: session.sessionToken })
    .limit(1);

  // Return the session data
  return dbSessions[0];
}
```

This is another function with the same pattern of insert, select, and return. Easy!

The next one, `getSessionAndUser()` is a little unique.

```typescript
async getSessionAndUser(sessionToken) {
  // Get a session row based on the given token
  const dbSessions = await knex("Session")
    .select("*")
    .where({ sessionToken })
    .limit(1);

  // If no session was found, return null
  if (dbSessions.length === 0) return null;

  // If session exists, get the user data for that session
  const dbUsers = await knex("User")
    .select("*")
    .where({ id: dbSessions[0].userId })
    .limit(1);

  // If no user was found, return null
  if (dbUsers.length === 0) return null;

  // Return the session and the user data
  return { session: dbSessions[0], user: dbUsers[0] };
}
```

Given a session token, this function finds both a session and the associated user.

It's done in two steps:

1. The session is found by querying the `Session` table. If no matching session was found, it returns `null` early.
    
2. The user is found by querying the `User` table using the `userId` found in the matching session. If no matching user was found, it returns `null`, as well.
    

Finally, both objects are nested into a parent object and returned.

Next is `updateSession()`.

```typescript
async updateSession(session) {
  // Update a session row based on the given token
  await knex("Session")
    .where({ sessionToken: session.sessionToken })
    .update(session);

  // Get the session row that was just updated
  const dbSessions = await knex("Session")
    .select("*")
    .where({ sessionToken: session.sessionToken })
    .limit(1);

  // Return the session data
  return dbSessions[0];
}
```

It takes a session object which contains a session token along with updated data. It then updates the appropriate database row based on the `sessionToken`. Again, it selects the updated data and returns it.

With session creation and updating out of the way, we turn to `deleteSession()`.

```typescript
async deleteSession(sessionToken) {
  // Get a session row based on the given token
  const dbSessions = await knex("Session")
    .select("*")
    .where({ sessionToken })
    .limit(1);

  // Delete a session row based on the given token
  await knex("Session").where({ sessionToken }).del();

  // Return the session data
  return dbSessions[0];
}
```

Theoretically, this function only needs to be one line long. It only needs to delete a session matching the given `sessionToken`. However, I believe some Auth.js functionality might depend (now or in the future) on this function returning the deleted session data. So the session is first selected before being deleted from the `Session` table. The data is then returned at the end of the function.

We're down to the final two functions. They both deal with verification tokens, which are used in email authentication methods. First, `createVerificationToken()`.

```typescript
async createVerificationToken(verificationToken) {
  // Insert a new verification token row into the `VerificationToken` table
  await knex("VerificationToken").insert(verificationToken);

  // Get the verification token that was just inserted
  const dbVerificationTokens = await knex("VerificationToken")
    .select("*")
    .where({ token: verificationToken.token })
    .limit(1);

  // Return the verification token data
  return dbVerificationTokens[0];
}
```

It's the same pattern of inserting the given data, selecting what was saved in the database, and returning it.

Finally, we have `useVerificationToken()`.

```typescript
async useVerificationToken({ identifier, token }) {
  // Get a verification token row based on id and token
  const dbVerificationTokens = await knex("VerificationToken")
    .select("*")
    .where({ identifier, token })
    .limit(1);

  // Delete that row
  await knex("VerificationToken").where({ identifier, token }).del();

  // Return the verification token data
  return dbVerificationTokens[0];
}
```

"Using" a verification token means that it's been consumed and should be deleted from the database. First, the function selects the token data based on the given `identifier` and `token`, then it deletes it from the database and finally returns the deleted data.

That's all for the functional part of the adapter. However, there are still some other important files necessary for the adapter to work properly with Auth.js and be publishable.

`package.json` contains some configuration to facilitate its use. I won't include the full file here, but here are some highlights:

```json
{
  "name": "authjs-knexjs-adapter",
  "type": "module",
  "engines": { "node": "^12.20.0 || ^14.13.1 || >=16.0.0" },
  "exports": "./index.js",
  "types": "./index.d.ts",
  "files": ["*.js", "*.d.ts*", "src"],
  "devDependencies": {
    "knex": "^2.4.2",
    "next-auth": "^4.22.1",
    "typescript": "^5.0.4"
  }
}
```

It uses `"type": "module"` since it takes advantage of ESM `import`s and `export`s.

The `"engines"` are set to the Node.js engines that support ESM, as well.

`"exports"` points to where the transpiled JavaScript file will be written when the project is built (more on that below).

`"types"` points to where the TypeScript types will be written.

`"files"` indicates which files should be packaged for distribution. That includes all of the built JavaScript files, all of the types & sourcemaps, and anything in `src` (which includes the TypeScript source for use with the sourcemaps).

Finally, the `"devDependencies"` include `knex` and `next-auth` so it can import their types, as well as `typescript` for transpilation.

Speaking of TypeScript, the last bit of source code I'll go into here is the TypeScript configuration in `tsconfig.json`.

```json
{
  "compilerOptions": {
    "allowJs": true,
    "baseUrl": ".",
    "declaration": true,
    "declarationMap": true,
    "esModuleInterop": true,
    "forceConsistentCasingInFileNames": true,
    "isolatedModules": true,
    "module": "ESNext",
    "moduleResolution": "node",
    "outDir": ".",
    "rootDir": "src",
    "skipDefaultLibCheck": true,
    "skipLibCheck": true,
    "strict": true,
    "strictNullChecks": true,
    "stripInternal": true,
    "target": "ES2019"
  },
  "include": ["src/**/*"],
  "exclude": ["*.js", "*.d.ts"]
}
```

Most of the configuration comes from the [Auth.js adapter base TypeScript configuration](https://github.com/nextauthjs/next-auth/tree/a79774f6e890b492ae30201f24b3f7024d0d7c9d/packages/tsconfig) that the official adapters use. Some important highlights are:

* `"outDir": "."` tells TypeScript to place the built files right in the root directory. This is important so the `exports` and `types` properties in `package.json` point to the right location of these important files.
    
* This package uses `"target": "ES2019"` as it is the latest target that is 100% compatible with the earliest engine defined in `package.json` (Node.js 12.20.0).
    

This configuration means you only need to run `npx tsc` to transpile everything to JavaScript and then `npm publish` to publish it for anyone to use.

The full source code can be found on GitHub. I hope this project can save you some time and headaches in your own projects!

%[https://github.com/travishorn/authjs-knexjs-adapter]