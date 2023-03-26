---
title: "Removing Conficker/Downadup"
datePublished: Mon Jul 25 2016 22:01:01 GMT+0000 (Coordinated Universal Time)
cuid: ckrows7f00p2zfws16l311970
slug: removing-conficker-downadup-d8fa0118ee48
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1627409834181/beOhpFE1Z.jpeg
tags: sysadmin, microsoft, security, windows

---


This post is part of a project to move my old reference material to my blog. Before 2012, when I accessed the same pieces of code or general information multiple times, I would write a quick HTML page for my own reference and put it on a personal site. Later, I published these pages online. Some of the pages still get used and now I want to make them available on my blog.

![Photo by [Denys Nevozhai](https://cdn.hashnode.com/res/hashnode/image/upload/v1627409832466/sBVqiuFrb.html)](https://cdn-images-1.medium.com/max/6140/1*3ViYuNUJbq9NDMjwsex81Q.jpeg)*Photo by [Denys Nevozhai](https://unsplash.com/@dnevozhai)*

### Positively Identify

Make sure the system is infected with Conficker before treating for it. There is a good chance the system is infected if the following sites cannot be accessed:

* [Symantec.com](http://symantec.com/)

* [Microsoft.com](http://microsoft.com/)

Also, [http://www.confickerworkinggroup.org/infection_test/cfeyechart.html](http://www.confickerworkinggroup.org/infection_test/cfeyechart.html) may be a helpful resource in identifying Conficker.

### Stop Spreading

**Remove Network Access**

Once you’re sure the problem is the Conficker virus, disconnect the system from any networks it may be attached to. If you have a wired connection, unplug the network cable. If you have a wireless connection, turn off or unplug the router.

**Disable Autoplay**

1. Click **Start**

1. Click **Run**

1. Type **gpedit.msc** and press **Enter**

1. In the window that opens, expand **Administrative Templates** under **Computer Configuration**

1. Click **System**

1. In the right pane, double-click **Turn off Autoplay**

1. In the window that opens, select **Enabled** and click **OK**

### Gather Resources

Use a computer that is not infected to download Symantec’s removal tool at: [http://www.symantec.com/security_response/writeup.jsp?docid=2009-011316-0247-99](http://www.symantec.com/security_response/writeup.jsp?docid=2009-011316-0247-99)

Transfer the downloaded file to the infected computer. You will have to use some media such as a burned CD-ROM or a USB flash drive.

Note: If you use a flash drive, the virus will spread itself to the flash drive as soon as you insert it. It is important not to insert the flash drive into any other PC until it is cleaned. See the section below titled **Clean an Infected Flash Drive**.

### Scan and Remove Virus

On the infected system, run the removal tool you downloaded. It will ask you to accept an agreement and then start the scan. Let it run for as long as needed.

When it completes, follow the instructions given by the tool. You may need to restart your machine.

### Protect Yourself

When the removal process is finished, open Internet Explorer and visit [http://update.microsoft.com](http://update.microsoft.com/) and install any priority updates. You may have to restart many times throughout the process. After each restart, visit [Microsoft Update](http://update.microsoft.com/) again until there are no more priority updates to install.

### Clean and Infected Flash Drive

As long as your computer has the latest updates from Microsoft (covered in the **Protect Yourself** section) and autoplay is disabled (covered in the **Stop Spreading** section), you can safely use it to clean an infected flash drive.

**Clean**

1. Insert the drive

1. Transfer any files or folders on the drive that you may want to keep to the computer

1. Open **My Computer**

1. Right-click the drive

1. Click **Format…**

1. In the window that appears, click **Start**

You may want to repeat these steps for any flash drive you own.

**Protect**

Conficker spreads by creating/modifying the **autorun.inf** file on the root of a flash drive. You can protect a drive by preventing the creation of this file. All you have to do is create a folder called **autorun.inf**. With that done, it is impossible to create a file with the name **autorun.inf**. You may want to make the folder read-only:

1. Right-click the folder

1. Click **Properties**

1. Check **Read Only**

1. Click **OK**

### Go a Step Further

You can further scan your system for more infections by using Microsoft’s removal tool: Click **Start**, **Run**, type **mrt** and press **Enter**, follow the steps. You can also scan for spyware using [Spybot](https://www.safer-networking.org/).