---
title: "Enable File and Printer Sharing Across Subnets in Windows Server 2003"
datePublished: Thu Jul 21 2016 22:01:02 GMT+0000 (Coordinated Universal Time)
cuid: ckrni8b5k0c0cems1auk60fud
slug: enable-file-and-printer-sharing-across-subnets-28af49535313
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1627409894121/zX5jWhOlm.jpeg
tags: desktop, microsoft, windows, windows-server

---


This post is part of a project to move my old reference material to my blog. Before 2012, when I accessed the same pieces of code or general information multiple times, I would write a quick HTML page for my own reference and put it on a personal site. Later, I published these pages online. Some of the pages still get used and now I want to make them available on my blog.

![Photo by [Blake Parkinson](https://cdn.hashnode.com/res/hashnode/image/upload/v1627409892416/f73yfs_9i.html)](https://cdn-images-1.medium.com/max/6528/1*z5y_JoCKhE_iy_kOIGzL0A.jpeg)*Photo by [Blake Parkinson](https://unsplash.com/@blakeparkinson)*

By default, Windows Firewall in Windows Server 2003 only allows File and Printer Sharing connections to the subnet the machine is currently on. To allow connections from any subnet, follow the instructions below.

1. Click **Start**, **Control Panel**, **Windows Firewall**

1. Click the **Exceptions** tab

1. Highlight **File and Printer Sharing** in the **Programs and Services** list box

1. Click the **Edit…** button

1. Highlight the first port (the first item in the list box)

1. Click the **Change scope…** button

1. Tick the radio button for **Any computer (including those on the Internet)**

1. Click the **OK** button

1. Repeat steps 6–8 for each port (each item in the list box)

1. Click the **OK** button on the **Edit a Service** dialog box

1. Click the **OK** button on the **Windows Firewall** dialog box