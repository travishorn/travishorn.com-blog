---
title: "Windows XP Malware Cleanup"
datePublished: Sat Jul 30 2016 22:01:02 GMT+0000 (Coordinated Universal Time)
cuid: ckrmeqbm903mhfws15cujcb7t
slug: windows-xp-malware-cleanup-8d6d858c1e40
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1627410605476/MWy1UxS26.jpeg
tags: sysadmin, security, windows

---


This post is part of a project to move my old reference material to my blog. Before 2012, when I accessed the same pieces of code or general information multiple times, I would write a quick HTML page for my own reference and put it on a personal site. Later, I published these pages online. Some of the pages still get used and now I want to make them available on my blog.

![Photo by [Carl Nenzen Loven](https://cdn.hashnode.com/res/hashnode/image/upload/v1627410603205/Z0qLLcuhx.html)](https://cdn-images-1.medium.com/max/11092/1*NlKxEMZ9S-ExqN_cGn7rjQ.jpeg)*Photo by [Carl Nenzen Loven](https://unsplash.com/@archduk3)*

This guide describes a method for cleaning up a Windows computer infected with malicious software, also known as malware. It is designed for Windows XP but other versions of Windows may be similar. Follow the steps below, in order, to rid a system of malware.

### A Note on Downloading

Many steps in this guide require you to download and install and/or run application from the Internet. However, if the computer is infected with malware, it may have limited or no network connectivity. Keep in mind that you always have the option of downloading on a separate, clean computer and transferring it to the infected one with a flash drive or CD.

It is important to note that many malware applications will attempt to spread themselves by copying themselves to any removable media that is inserted into the machine hosting it. If you insert a flash drive into an infected machine, take care to scan and remove any infections on it before inserting it back into a clean computer. This can be done by scanning the flash drive at the same time as scanning the hard disk(s) as described in the section Installing and Using Anti-Malware below.

### Booting Up

1. Boot the PC into **Safe Mode with Networking**

1. Press the power button

1. Tap **F8** until the **Windows Advanced Options Menu** appears

1. Use the arrow keys to select **Safe Mode with Networking**

1. Press **Enter**

1. You may be prompted to select the operating system. If there is more than one option, select the one that is most likely the main operating system

1. You may be prompted to select a user to log in with. If it is an option, attempt to log in as **Administrator**. Otherwise, choose the one that is most likely the main user

1. Let the operating system load completely before continuing

### Checking and Fixing Network Connectivity

Many malware applications will attempt to disable network connectivity to prevent themselves from being removed. This includes setting a malicious proxy and/or replacing your HOSTS file with a malicious version. Follow the steps below to resolve the problem.

**Removing a Malicious Proxy**

Many malware applications will attempt to set a proxy to prevent Internet access. Windows will use this proxy to attempt to reach the Internet. With a malicious proxy in place, this attempt to access the Internet will fail. Follow the steps below to remove a malicious proxy.

1. Click **Start**, **Control Panel**

1. Double-Click **Internet Options**

1. Click the **Connections** tab

1. Under **Local Area Network (LAN)** settings, click the **LAN settings** button

1. If checked, uncheck the box labeled **Use a proxy server for your LAN**

1. Click **OK** on the **Local Area Network (LAN) Settings** dialog box

1. Click **OK** on the **Internet Properties** dialog box

1. Close the **Control Panel** window

**Reverting the HOSTS File**

Many malware applications will replace your **HOSTS** file with a malicious version. Windows will check the **HOSTS** file first before accessing any network locations. If a malicious version is in place, this will prevent proper Internet access. Follow the steps below to revert the **HOSTS** file to its default state.

In this section, you will need to use a downloadable application called **hosts-perm.bat**. Please refer to the section above labeled **A Note on Downloading** for more information regarding this topic.

1. Open a web browser such as **Internet Explorer** or **Firefox**

1. Visit [http://download.bleepingcomputer.com/bats/hosts-perm.bat](http://download.bleepingcomputer.com/bats/hosts-perm.bat)

1. Save the file to your computer

1. When the download completes, you can close the web browser

1. Run downloaded file, **hosts-perm.bat**

1. Delete the file **C:\Windows\System32\Drivers\etc\HOSTS**

1. Replace HOSTS with one of the following, depending on the operating system:

* [Windows XP](http://download.bleepingcomputer.com/misc/host-files/windows-xp/hosts)

* [Windows Vista](http://download.bleepingcomputer.com/misc/host-files/windows-vista/hosts)

* [Windows 2003 Server](http://download.bleepingcomputer.com/misc/host-files/windows-2003-server/hosts)

* [Windows 2008 Server](http://download.bleepingcomputer.com/misc/host-files/windows-2008-server/hosts)

* [Windows 7](http://download.bleepingcomputer.com/misc/host-files/windows-7/hosts)

### Kill Running Malware Processes

Some malware processes may already be running. You can attempt to kill them by using a downloadable application called **rkill.com**. Please refer to the section above labeled **A Note on Downloading** for more information regarding this topic.

1. Open a web browser such as **Internet Explorer** or **Firefox**

1. Visit [http://www.bleepingcomputer.com/download/rkill/](http://www.bleepingcomputer.com/download/rkill/)

1. Click the first **Download Now** button and save the file

1. When the download is complete, you can close the web browser

1. Run **rkill.com** (Note: Many malware applications will prevent executables from running. Therefore, it is strongly recommended to rename **rkill.com** to **iexplore.exe** or **winlogon.exe** as these are the same name as system files that malware will usually allow to run)

### Installing and Using Anti-Malware

There are many applications that will attempt to find and remove malware. The steps below will describe how to use the free version of **Malwarebytes’ Anti-Malware**. However, other free options include:

* [McAfee Stinger](http://www.mcafee.com/us/downloads/free-tools/stinger.aspx)

* [Microsoft Malicious Software Removal Tool](http://www.microsoft.com/security/pc-security/malware-removal.aspx)

* [Microsoft Security Essentials](http://www.microsoft.com/en-us/security_essentials/default.aspx)

* [Spybot — Search & Destroy](http://www.safer-networking.org/en/home/index.html)

* [SUPERAntiSpyware](http://www.superantispyware.com/)

In this section, you will need to use a downloadable application called **Malwarebytes’ Anti-Malware**. Please refer to the section above labeled **A Note on Downloading** for more information regarding this topic.

1. Open a web browser such as **Internet Explorer** or **Firefox**

1. Visit [https://www.malwarebytes.com/antimalware/](https://www.malwarebytes.com/antimalware/)

1. Click **FREE DOWNLOAD**

1. Click **FREE DOWNLOAD** once again on the next page

1. When the download is complete, you can close the web browser

1. Using the downloaded file, install **Malwarebytes’ Anti-Malware**, following the prompts and accepting all the default options

1. You may be prompted to update definitions, follow any prompts to do so

1. Select the option labeled **Perform full scan**

1. Click the **Scan** button

1. You may be prompted to select which drives you want to scan. Select any attached hard drives

1. When the scan completes, a message will appear alerting you that the scan completed successfully. Click **OK**

1. Click the **Show Results** button

1. Click the **Remove Selected** button

1. If prompted to restart, accept and restart the computer

**What if Malwarebytes’ Anti-Malware was Unsuccessful?**

You may come across a piece of malware that **Malwarebytes’** was unable to remove. If this is the case, please make sure **Malwarebytes’** is completely up-to-date and run the scan again.

1. Open **Malwarebytes’ Anti-Malware**

1. Click the **Update** tab

1. Click the **Check for Updates** button

If **Malwarebytes’** is up-to-date and is still unsuccessful at removing the malware, try one of the other applications listed above such as **Spybot — Search & Destroy** or **SUPERAntiSpyware**.

If those applications are unsuccessful, you may have to remove the malware manually. Unfortunately, the process is different for each situation. However, if you can identify what piece of malware is infecting the computer, you may be able to research removal instructions online using a search engine. To identify what piece of malware is infecting the computer, find a specific window title or message text generated by the malware and search for that phrase online. Once you know the name of the malware, you can attempt to find removal instructions by searching for the name of the malware followed by “removal instructions.”

### Installing Updates

Once the computer has been cleared of any infections, you will want to update Windows and other installed software with the latest updates. This will prevent many infections in the future by closing known security holes.

**Updating Windows**

1. Open **Internet Explorer**

1. Visit [http://windowsupdate.microsoft.com/](http://windowsupdate.microsoft.com/)

1. Click the **Express** button

1. Follow any prompts and restart as necessary

1. Repeat steps **1–4** until no further updates are needed

Optional: Set **Automatic Updates** to run

1. Click **Start**, **Control Panel**

1. Double-click **Automatic Updates**

1. Select the option labeled **Automatic (Recommended)**

1. Choose a day and time to install updates

1. Click **OK** on the **Automatic Updates** dialog box

1. Close the **Control Panel** window

**Updating Other Applications**

You will also want to update other installed applications as necessary. This guide will not cover these topics because each application will have its own update procedure. If you need help, research the topic using a search engine such as [Google](http://google.com/) or [Bing](http://bing.com/). A search query like “update Firefox” will usually get good results. Some important applications that you may want to update include:

* [Adobe Flash](http://www.adobe.com/downloads/updates/)

* [Adobe Reader](http://www.adobe.com/downloads/updates/)

* [Mozilla Firefox](http://www.mozilla.com/en-US/firefox/update/)

* [Oracle Java](http://www.java.com/en/download/help/java_update.xml)

### System Cleanup

There may also be problems with temporary files, registry, and/or startup items. Follow the steps below to use [**CCleaner](http://www.piriform.com/ccleaner)** to clean these files up. This section also includes defragmenting the hard disk(s).

**Installing and Running CCleaner**

1. Open a web browser such as **Internet Explorer** or **Firefox**

1. Visit [https://www.piriform.com/ccleaner/download](https://www.piriform.com/ccleaner/download)

1. Click **Download** and save the file to your computer

1. When the download completes, you can close the web browser

1. Using the downloaded file, install **CCleaner**, following the prompts and accepting all the default options

1. Click the **Run Cleaner** button

1. Click **OK** on the message that appears

1. When the process completes, click on **Registry**

1. Click the **Scan for Issues** button

1. When the scan completes, click the **Fix selected issues…** button

1. Click **Yes** on the message that appears

1. Save the registry backup to a location on the hard drive (in case you need to revert back to it later)

1. Click the **Fix All Selected Issues** button

1. When the process completes, click the **Close** button

1. Click **Tools**

1. Click the **Startup** button

1. Select any unnecessary and/or unsafe processes and click the **Disable button** (Research processes online to determine whether they are unnecessary and/or unsafe)

1. Close **CCleaner**

**Defragmenting the Hard Disk(s)**

1. Click **Start**, **All Programs**, **Accessories**, **System Tools**, **Disk Defragmenter**

1. Select the first hard disk in the list that hasn’t been defragmented recently

1. Click the **Defragment** button

1. Repeat steps **2–3** until all disks have been defragmented

1. Close **Disk Defragmenter**

### Preventative Measures

There are some measures you can take to prevent future infections. These include supplying the user with information about safe browsing habits and installing Firefox with an ad blocker.

**Information**

You can educate the user on safe browsing habits. Some resources include:

* [Oregon State University — Safe Browsing Habits](http://oregonstate.edu/helpdocs/security/safe-browsing)

* [PC Pitstop — Safe Surfing](http://www.pcpitstop.com/spycheck/safesurfing.asp)

* [Tucows — How to Prevent Viruses, Beyond the Obvious](http://www.tucows.com/article/554)

**Firefox with uBlock Origin**

Another method is to install a secure browser with ad-blocking features to prevent the user from even seeing unsafe download links. Firefox with [uBlock Origin](https://addons.mozilla.org/en-US/firefox/addon/ublock-origin/) is a good solution. This solution requires that you educate the user that they should be using **Firefox** instead of another browser such as **Internet Explorer** when they want to access the Internet.

Installing

1. Open a web browser such as **Internet Explorer**

1. Visit [https://www.mozilla.org/en-US/firefox/new/](https://www.mozilla.org/en-US/firefox/new/)

1. Click **Free Download** and save the file to the computer

1. When the download completes, you can close the browser

1. Using the downloaded file, install **Firefox** (following any prompts along the way)

1. Once **Firefox** is installed, click the menu button, then **Add-Ons**

1. In the search bar, type “uBlock Orgin” and press **Enter**

1. Click the **Install** button next to **uBlock Origin**

1. If you are prompted to restart **Firefox**, accept and restart

1. You can close **Firefox** when these steps are complete

Making **Firefox** the **Default Browser**

1. In **Firefox**, click the menu button, then **Options**

1. Click **Advanced**

1. Check the box labeled **Always check to see if Firefox is the default browser on startup**

1. Click the **Check Now** button

1. If a message appears that alerts you that Firefox is not currently set as your default browser, click **Yes**

1. Click **OK** on the Options dialog box

1. You can close **Firefox** when these steps are complete

Once Firefox is set as the default browser, you may want to turn off **Internet Explorer**’s feature that will check to make sure it’s the default browser on each startup:

1. Open **Internet Explorer**

1. When the message appears informing you that **Internet Explorer** is not currently the default browser, uncheck the box labeled **Always perform this check when starting Internet Explorer**. If this message does not appear, skip **step 3**.

1. Click **No**

1. Close **Internet Explorer**

## Final Steps

1. Close any open windows

1. Open and close any newly installed/updated applications and go through any first-run settings wizards in them

1. Restart the PC to make sure everything is working properly

1. Shut Down

## Related Resources

* [Bing](http://www.bing.com/) — Search engine by Microsoft

* [Bleeping Computer](http://www.bleepingcomputer.com/) — Computer help and malware removal guides

* [Google](http://www.google.com/) — Search engine

* [Malwarebytes](http://www.malwarebytes.org/) — Developer of Anti-Malware application

* [McAfee](http://www.mcafee.com/us/) — Developer of Stinger application

* [Microsoft Support](http://support.microsoft.com/) — Windows help and support

* [Microsoft TechNet](http://technet.microsoft.com/en-us/) — IT-related support for Microsoft products

* [Mozilla](http://www.mozilla.org/) — Developer of Firefox web browser

* [Newegg](http://www.newegg.com/) — Online store for computer-related products

* [Oregon State University](http://oregonstate.edu/) — Publisher of Safe Browsing Habits document

* [PC Pitstop](http://www.pcpitstop.com/) — Publisher of Safe Surfing document

* [Piriform](http://www.piriform.com/) — Developer of CCleaner application

* [Safer Networking](http://www.safer-networking.org/en/home/index.html) — Developer of Spybot — Search & Destroy application

* [SUPERAntiSpyware](http://www.superantispyware.com/) — Developer of SUPERAntiSpyware application

* [Tucows](http://www.tucows.com/) — Publisher of How to Prevent Viruses, Beyond the Obvious

* [Wikipedia](http://www.wikipedia.org/) — Online, community-edited encyclopedia reference