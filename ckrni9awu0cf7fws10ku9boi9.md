---
title: "Increase Concurrent Connections in Windows Server 2000"
datePublished: Sun Aug 07 2016 22:01:02 GMT+0000 (Coordinated Universal Time)
cuid: ckrni9awu0cf7fws10ku9boi9
slug: increase-concurrent-connections-in-windows-server-2000-7490fa937bde
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1627409889291/364ryyylX.jpeg
tags: sysadmin, microsoft, server, windows, windows-server

---


This post is part of a project to move my old reference material to my blog. Before 2012, when I accessed the same pieces of code or general information multiple times, I would write a quick HTML page for my own reference and put it on a personal site. Later, I published these pages online. Some of the pages still get used and now I want to make them available on my blog.

![Photo by [Patryk Sobczak](https://cdn.hashnode.com/res/hashnode/image/upload/v1627409887609/dq97G5Ui_.html)](https://cdn-images-1.medium.com/max/3840/1*77O7P_6HX2aCffwA_FRfqA.jpeg)*Photo by [Patryk Sobczak](https://unsplash.com/@patryksobczak)*

By default, Windows Server 2000 only allows a certain number of concurrent connections. If users try to access your site while the connections are maxed out, they will be given an error. You can remedy the situation by buying more CALs from Microsoft, then changing your server’s settings to allow more connections.

### Increase Maximum Supported Connections

1. Buy **Client Access Licenses** (CALs) from Microsoft

1. Click **Start**, **Control Panel**, **Licenses**

1. Click **Add Licenses**

1. Type the number of *new* licenses

1. Click **OK**

1. Check **I agree…**

1. Click **OK**

### Check Live Concurrent Connections

It may be useful to see the number of live connections to your server.

1. Click **Start**, **Settings**, **Control Panel**, **Administrative Tools**, **Performance**

1. In the right pane, right-click the blank gray space

1. Choose **Add Counters…**

1. In **Performance object**, select **Web Service**

1. In **Select counters from list**, select **Current Connections**

1. Select **All instances**