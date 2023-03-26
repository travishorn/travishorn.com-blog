---
title: "NoSQL & MongoDB"
datePublished: Fri Aug 08 2014 05:00:00 GMT+0000 (Coordinated Universal Time)
cuid: ckrnhsoka0c9xfws19g4cahgu
slug: nosql-mongodb-54522710ed4d
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1627409954661/k3otgPg_k.png
tags: programming, mongodb, databases

---


The latest trend in database technology is away from relational tables. It seems that everybody is trying to find a way to fit a NoSQL database into their project… whether they need one or not. As with every programming language, framework, and library, you will need to take a good look to see whether a NoSQL database is a good fit for your project.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627409953406/0x_BaOfss.png)

[MongoDB](http://mongodb.org/) is the prevailing NoSQL database platform, so I’ll be using it to compare to relational databases like [Oracle](http://www.oracle.com/technetwork/database/index.html) and [PostgreSQL](http://www.postgresql.org/).

## Differences

Instead of storing data in tables with strict rows and columns, MongoDB is usually used in a “schema-less” format. What you would consider a “row” in a relational database, is called a “document” in MongoDB. Documents can can have any number of values, and can even include “sub-documents.” The nice thing is that no two documents in a “collection” (which is similar to a “table” in a relational database) are required to have the same values.

For example, a Users table in a relational database:

```
FirstName     LastName     FavoriteColor     FavoriteFood
Olivia        Hall         Green             *NULL*
Owen          Reed         *NULL*            Pizza
```


And the same data in a users collection in MongoDB:

```
{
  "firstName" : "Olivia",
  "lastName" : "Hall",
  "favoriteColor" : "Green"
}

{
  "firstName" : "Owen",
  "lastName" : "Reed",
  "favoriteFood" : "Pizza"
}
```


You can see that documents in MongoDB don’t have to follow the same schema; The first document has a favoriteColor key, but not a favoriteFood key. And the opposite is true for the second document. Some people make the argument that this type of data modeling is better for how modern software development works.

## How do you store relational data then?

This is where most people get tripped up. In a NoSQL database, there are no relationships between tables. Many people are used to the having keys in one table that relate to another. For example, a PurchaseOrders table that contains a CustomerId field. The CustomerId field would be linked via foreign key to a Customers table.

There are many ways to model this in NoSQL. The most common would probably be to embed the customer data in the purchase order data or vice-versa, like so:

```
> db.purchaseOrders.findOne()

{
  "_id" : 32543,
  "total" : 210.34,
  "date" : "2014-08-05 18:34:22",
  "customer" : {
    "name" : "Madison Brown",
    "address" : "621 Market St.",
    "city" : "Pleasant Hill",
    "state" : "IA"
  }
}
```


You can still get all distinct customer information without referring to another table. In this case, just a query like this will do:

```
> db.purchaseOrders.distinct('customer')

{
  "0" : {
    "name" : "Madison Brown",
    "address" : "621 Market St.",
    "city" : "Pleasant Hill",
    "state" : "IA"
  },
  "1" : {
    "name" : "Audrey Reed",
    "address" : "1965 Lincoln St.",
    "city" : "Greenville",
    "state" : "MA"
  }
}
```


If you do happen to need to “join” a table in MongoDB, you can do it on the application side by querying the first collection, and then querying the second collection based on the returned document(s). This is generally slower, and if you find yourself doing it a lot, you may consider remodeling your data structure into a more non-relational format.

## Advantages

My favorite advantage is the fact that data in MongoDB is stored in a JSON-like structure. If you’re working with JavaScript on the front-end, Node.js on the back-end, and MongoDB for your database, you’re using basically using JavaScript during the entire development process. In fact, designing an API is so easy, in most cases you can just return data exactly from MongoDB to the client as JSON without having to modify it server-side (as long as your write your queries the right way).

Take a look at MongoDB or other NoSQL databases and decide whether your project could benefit from their design vs a relational database. Just please don’t try to fit one in where it doesn’t belong. It will only lead to headaches later. NoSQL databases were never intended to replace relational databases. But there are many overlaps, and some developers have found ways to hammer a NoSQL database into working in a relational way, only to have headaches later when they realize it doesn’t work the way they expected.

*Originally published on my old blog on August 8, 2014.*