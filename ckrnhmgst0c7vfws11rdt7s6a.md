---
title: "Windows XP Startup and Shutdown Notifications"
datePublished: Tue Jul 26 2016 22:08:17 GMT+0000 (Coordinated Universal Time)
cuid: ckrnhmgst0c7vfws11rdt7s6a
slug: windows-xp-startup-and-shutdown-notifications-39f4f3190c6b
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1627409989462/C8n_NgRrs.jpeg
tags: desktop, microsoft, windows

---


This post is part of a project to move my old reference material to my blog. Before 2012, when I accessed the same pieces of code or general information multiple times, I would write a quick HTML page for my own reference and put it on a personal site. Later, I published these pages online. Some of the pages still get used and now I want to make them available on my blog.

![Photo by [Tim Gouw](https://cdn.hashnode.com/res/hashnode/image/upload/v1627409987450/Hj0V0wo1b.html)](https://cdn-images-1.medium.com/max/5984/1*_qLh-FsKPoPwvGfXpl4scA.jpeg)*Photo by [Tim Gouw](https://unsplash.com/@punttim)*

This step-by-step guide will show you how to create startup and shutdown scripts that will notify another PC when the target PC is turned on or off.

### Create the Scripts

The first step is to create the scripts that will run when the computer is turned on or off.

1. Create a folder on the **C:\** drive named **Scripts**

1. Open **Notepad**

1. Type @net send [IP Address] Machine started up!

1. Make sure to change [IP Address] to the IP address of the machine receiving the messages.

1. Save as **C:\Scripts\startup.bat**

1. Create a new document in **Notepad**

1. Type @net send [IP Address] Machine shut down!

1. Make sure to change [IP Address] to the IP address of the machine receiving the messages.

1. Save as **C:\Scripts\shutdown.bat**

1. You can close **Notepad** now

### Set the Scripts to Run at Startup/Shutdown

The next step is to set those scripts to run.

1. Click **Start** > **Run**

1. Open **Group Policy** by typing **gpedit.msc** and pressing **Enter**

1. In the left pane, expand **Computer Configuration** > **Windows Settings**

1. Click on **Scripts (Startup/Shutdown)**

1. In the right pane, double-click on **Startup**

1. Click **Add…**

1. Click **Browse…**, find the file **C:\Scripts\startup.bat** and click **Open**

1. Click **OK**

1. Click **OK** once more

1. Now double-click **Shutdown**

1. Click **Add…**

1. Click **Browse**, find the file **C:\Scripts\shutdown.bat** and click **Open**

1. Click **OK**

1. Click **OK** once more

1. You can close **Group Policy** now

### Turn Messenger Service On

Finally, to ensure the messages go through, the Messenger service must be running.

1. Click **Start** > **Run**

1. Type **services.msc** and press **Enter**

1. Find the service labeled **Messenger** and double-click on it

1. Change the **Startup type** to **Automatic** if it isn’t already

1. Click **OK**

1. You can close **Services** now