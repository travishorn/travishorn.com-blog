---
title: "Integrating SQL Data into your Next.js App"
datePublished: Wed Jan 17 2024 12:00:19 GMT+0000 (Coordinated Universal Time)
cuid: clrhqe1pn000009jnakq9bvrp
slug: integrating-sql-data-into-your-nextjs-app
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1689350712695/ce36ca53-4c03-4a74-b176-79d68ca96b4d.png
tags: mysql, web-development, ssr, nextjs, database-integration

---

In the world of web development, building dynamic applications often involves connecting to a database to store and retrieve data. Next.js, a popular React framework, provides an excellent platform for creating server-side rendered and statically generated applications. In this article, I'll walk you through the process of seamlessly integrating MySQL, a powerful open-source database management system, with your Next.js app. From installing dependencies to executing raw queries, this comprehensive guide will equip you with the knowledge and skills needed to connect your Next.js app to MySQL and harness the full potential of database-driven applications.

I'll be using Knex.js. At the time of this writing, Prisma is the hot ORM for JavaScript. While Prisma does have some mind-blowing advantages, I still prefer Knex.js. Mainly for three reasons:

* I can write my queries in raw SQL, with parameter binding. Whenever I'm writing code to interact with a database, I tend to write the SQL first anyway (in something like DBeaver or MS Management Studio). From there I can use that raw SQL instead of porting it over into ORM-specific code.
    
* While I could couple my database-accessing code to a library like `mysql2`, but Knex.js provides nice abstractions so I can more easily swap between the RDBMSs I typically use (SQLite, MariaDB, and MSSQL).
    
* I am familiar with it. It's tried and true. I can develop faster with it.
    

## Prerequisites

* A working Next.js app
    
* A database set up in MySQL or MariaDB. [See my Getting Started with MariaDB article.](https://travishorn.com/getting-started-with-mariadb)
    

## **Install Dependencies**

This solution uses the `knex` and `mysql2` packages. Install them as dependencies.

```bash
npm install knex mysql2
```

## **Set Environment Variables**

Set the following environment variables in whichever way makes sense for your situation. If you are connecting to a development database server from a development machine, you may create a file called `.env.local` in the root directory of your app.

```bash
DB_HOST= 127.0.0.1
DB_PORT= 3306
DB_DATABASE= [database name]
DB_USER= [database user name]
DB_PASSWORD= [database user password]
```

You may need to change the host or the port depending on your setup.

## **Create a Database Connection File**

In your Next.js app, create a new file called `lib/db.ts`.

```javascript
import knex from "knex";

export const db = knex({
  client: "mysql2",
  connection: {
    host: process.env.DB_HOST,
    port: process.env.DB_PORT,
    user: process.env.DB_USER,
    password: process.env.DB_PASSWORD,
    database: process.env.DB_DATABASE,
  },
});
```

We'll import this file into any file that needs to connect to the database.

## **Get Data from the Database Server-Side**

Inside a Next.js page like `pages/index.tsx`, import the `db` function from the file we just created.

```javascript
import { db } from "../lib/db";
```

Then create a `getServerSideProps()` function. Next.js will use this to get data server-side and pass them as props to the page.

```javascript
export async function getServerSideProps() {
  const message = await db("Message").first("text").where({ id: 1 });

  return {
    props: { message },
  };
};
```

> The code above assumes you have a table named `Message` with columns for `id` and `text`. Change the code to fit your data and what you're trying to get from it.

Now, you can use the prop in the page's render function.

```javascript
export default function Home({ message }) {
  return (
    <main>
      <h1>{message.text}</h1>
    </main>
  )
}
```

Simple!

## **Using a Raw Query**

Knex.js has a built in `raw()` function that allows you to execute raw SQL. Unfortunately, the format of the data returned by this function doesn't work well with Next.js.

However, we can write a simple wrapper around it to help us.

Add the following to `lib/db.ts`.

```javascript
export const raw = async (sql: string, bindings?: any) => {
  const result = await db.raw(sql, bindings);
  const data = result[0];
  const parsed = JSON.parse(JSON.stringify(data));
  return parsed.length === 1 ? parsed[0] : parsed;
};
```

Now on your page, you can import this function.

```javascript
import { db, raw } from "../lib/db";
```

And you can use it to execute raw queries in `getServerSideProps()`.

```javascript
const message = await raw("SELECT text FROM Message WHERE id = 1 LIMIT 1;");
```

By following this guide, you have learned how to establish a reliable connection between your Next.js app and a MySQL database. You now have the tools to seamlessly interact with the database server-side, retrieve data, and present it to your clients. Whether you are building a small blog or a large-scale application, the ability to integrate databases into your Next.js projects opens up a world of possibilities for data-driven functionality. With your newfound knowledge, you can confidently create feature-rich applications that leverage the power of Next.js and MySQL, taking your web development skills to the next level.

Cover photo by [Cai Fang](https://unsplash.com/es/@caipod?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/photos/a-close-up-of-a-building-with-a-clock-on-the-side-of-it-6g0MSzyjWs4?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText).