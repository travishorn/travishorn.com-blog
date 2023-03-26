---
title: "Boilerplate Classic ASP Page"
datePublished: Tue Jul 19 2016 22:01:02 GMT+0000 (Coordinated Universal Time)
cuid: ckrox6iw40p75fws1756fgrmq
slug: boilerplate-classic-asp-page-77406e8ec66b
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1627409770490/vaA0-ozCe.jpeg
tags: asp, web-development

---


This post is part of a project to move my old reference material to my blog. Before 2012, when I accessed the same pieces of code or general information multiple times, I would write a quick HTML page for my own reference and put it on a personal site. Later, I published these pages online. Some of the pages still get used and now I want to make them available on my blog.

![Photo by [Dakota Roos](https://cdn.hashnode.com/res/hashnode/image/upload/v1627409768635/2L8AlzW59.html)](https://cdn-images-1.medium.com/max/12000/1*Y6p9QEVetat_WANWdu5wCA.jpeg)*Photo by [Dakota Roos](https://unsplash.com/@dakotaroosphotography)*

Instead of starting a new page from scratch, use this boilerplate to get started. All of the groundwork is laid out for a simple ASP page that uses a SQL server database. You will see references to bootstrap.css and jquery.js. You can either download these from [http://getbootstrap.com](http://getbootstrap.com) and [http://jquery.com](http://jquery.com) or you can use a CDN.

### HTML/ASP

**boilerplate.asp**

```
<%@LANGUAGE="VBSCRIPT" CODEPAGE="65001"%>
<% Option Explicit %>
<!doctype html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <meta http-equiv="x-ua-compatible" content="ie=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1">

    <title>[TITLE]</title>

<link rel="stylesheet" href="css/bootstrap.css">
  </head>
  
  <!-- #include file="db-connect.asp" -->
  
  <body>
    <div class="container">
      [CONTENT]
    </div>

    <script src="js/jquery.js"></script>
    <script src="js/main.js"></script>
  </body>

  <% objConn.Close %>

</html>
```


### Database Connection

**db-connect.asp**

```
<%
  Dim objConn
  Set objConn = Server.CreateObject("ADODB.Connection")
  objConn.ConnectionString = "Provider=SQLOLEDB; Data Source=(local); Database=[DATABASE]; User ID=[USER ID]; Password=[PASSWORD]"
  objConn.Open
%>
```
