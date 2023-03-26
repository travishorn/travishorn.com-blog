---
title: "Using MSSQL with Classic ASP"
datePublished: Wed Jul 20 2016 22:01:01 GMT+0000 (Coordinated Universal Time)
cuid: ckrowsvok0okkems18xed0cw1
slug: using-mssql-with-classic-asp-caeb96bef7e8
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1627409829087/K_gHQ3RkS.jpeg
tags: asp, web-development, databases, sql

---


This post is part of a project to move my old reference material to my blog. Before 2012, when I accessed the same pieces of code or general information multiple times, I would write a quick HTML page for my own reference and put it on a personal site. Later, I published these pages online. Some of the pages still get used and now I want to make them available on my blog.

![Photo by [Steinar La Engeland](https://cdn.hashnode.com/res/hashnode/image/upload/v1627409827214/tCAzRgkRV.html)](https://cdn-images-1.medium.com/max/9792/1*KJXAEkFYquub0wB_IPhzOw.jpeg)*Photo by [Steinar La Engeland](https://unsplash.com/@steinart)*

Below you will find four samples of ASP code: connecting to a database, inserting data, retrieving data, and closing the connection. These four pieces are the framework of a database-driven site.

### Connecting to a Database

```
Dim objConn
Set objConn = Server.CreateObject("ADODB.Connection")
objConn.ConnectionString = "Provider=SQLOLEDB; Data Source=(local); Database=[DATABASE]; User ID=[USER ID]; Password=[PASSWORD]"
objConn.Open
```


The above lines create a ADO object, set the connection string (which includes the provider, data source, database, user ID, and password), and opens the connection.

### Inserting Data into a Table

```
Dim SQL
SQL = "INSERT INTO [TABLE NAME] ([COLUMNS]) VALUES ('[VALUE]')"
objConn.Execute SQL
```


Probably the simplest example here. These lines simply define a SQL query and then execute it. This SQL query happens to be an insert statement.

### Retrieving Data

```
Dim SQL, objRec
SQL = "SELECT [COLUMNS] FROM [TABLE NAME]"
Set objRec = objConn.Execute(SQL)
While Not objRec.EOF
  Response.Write(objRec("Column_Name"))
  objRec.MoveNext
Wend
objRec.Close
Set objRec = Nothing
```


The first part of this block of code is the same as inserting data. You define a query and execute it. However, this time we’re also storing the data in an object called objRec. From there, we loop through objRec, performing actions on each record (in this case, writing the data).

### Closing the Connection

```
objConn.Close
```


It’s important to close the connection when you’re done interacting with the database. This frees up resources on your database server, allowing for faster performance.