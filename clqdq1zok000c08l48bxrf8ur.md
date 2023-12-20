---
title: "Getting Started with MariaDB"
datePublished: Wed Dec 20 2023 12:00:09 GMT+0000 (Coordinated Universal Time)
cuid: clqdq1zok000c08l48bxrf8ur
slug: getting-started-with-mariadb
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1689350162785/8613fca5-3c82-4477-9c60-9859edb02d3b.png
tags: mariadb, database-administration, data-security, databasemanagement

---

MariaDB is a great open-source relational database management system. This article will walk you through the process of installing and configuring MariaDB, creating database users, granting appropriate privileges, and enabling remote connections. With its robust features and performance optimizations, MariaDB is an excellent choice for managing your data efficiently.

I recently had to install and configure MariaDB on a Debian Linux server. The goal was to have a place to store data for a Next.js web application. Here's how I did it.

## **Install MariaDB**

```bash
sudo apt update
sudo apt install -y mariadb-server
```

## **Create a Database and a User**

Log in as the root system user.

```bash
sudo mariadb
```

Create a database for your web application.

```bash
CREATE DATABASE [mydatabase];
```

Replace `[mydatabase]` with the name you want to use for your database.

Create a user to represent your web application.

```bash
CREATE USER '[username]'@'%' IDENTIFIED BY '[password]';
```

Replace `[username]` and `[password]` with the actual credentials you want your web applications to use when they authenticate to the database.

> The `%` means that the user can connect from any IP address. Replace it with `localhost` to only allow local connections or replace it with an IP address to only allow connections from a specific IP address.
> 
> You may only want this user to connect locally if you're using it to provide access to a locally-running web application. You also may want to leave this user `localhost` only, and then create a separate user with fewer privileges for remote-access. The choice depends on how you want to use your database system.

Grant database privileges to this user.

```bash
GRANT ALL PRIVILEGES ON mydatabase.* TO '[username]'@'%';
```

> The same warning as above applies to this `%`.

Apply the privilege changes.

```bash
FLUSH PRIVILEGES;
```

Exit the MariaDB REPL.

```bash
EXIT;
```

## **Allow Remote Connections**

> This step is optional and depends on your use case. You don't have to allow remote connections. You may only want a web application running locally to access the database. However, enabling remote connections means you can administrate MariaDB and connect any apps in development from another machine.

Edit `/etc/mysql/my.cnf`.

Add the following lines at the end of the file.

```bash
[mysqld]
bind-address = 0.0.0.0
```

Save and close the file.

Restart the database service.

```bash
sudo systemctl restart mariadb
```

Setting up MariaDB establishes a reliable and scalable place to store data for your applications. By following the steps outlined in this guide, you have learned how to install MariaDB, create database users, grant privileges, and enable remote connections. Whether you're building a small-scale application or a large enterprise solution, MariaDB's flexibility and feature-rich capabilities make it a solid choice for your database needs. Hopefully, this article can help you get started so you can work toward unlocking the full potential of MariaDB and your applications.

Before you get too far, it would be wise to set up a backup and recovery plan. I will publish an article soon on that topic.

Cover photo by [Amritansh Dubey](https://unsplash.com/@amritanshdubey?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/photos/2XVKj6Ui1bw?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText).