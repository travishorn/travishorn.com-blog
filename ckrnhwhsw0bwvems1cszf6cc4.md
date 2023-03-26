---
title: "Import Contacts into Outlook from an Email"
datePublished: Sat Jul 23 2016 22:01:01 GMT+0000 (Coordinated Universal Time)
cuid: ckrnhwhsw0bwvems1cszf6cc4
slug: import-contacts-into-outlook-from-an-email-5ceede70875
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1627409945227/HjLUNvvtvl.jpeg
tags: email

---


This post is part of a project to move my old reference material to my blog. Before 2012, when I accessed the same pieces of code or general information multiple times, I would write a quick HTML page for my own reference and put it on a personal site. Later, I published these pages online. Some of the pages still get used and now I want to make them available on my blog.

![Photo by [Mathyas Kurmann](https://cdn.hashnode.com/res/hashnode/image/upload/v1627409943281/he4O39bQB.html)](https://cdn-images-1.medium.com/max/11694/1*f4EPoVCvhmPsBSprivGe-Q.jpeg)*Photo by [Mathyas Kurmann](https://unsplash.com/@mathyaskurmann)*

This guide may be useful if you have an email that was sent to a group of people and you want to add all recipients to your contacts. This is most efficient for emails that were sent to a large group (like your entire company).

1. Open **Outlook** and locate an email that was sent out to the whole district

1. Right-click on the email in the list

1. Click **Message Options…**

1. In the **Internet headers** box, highlight everything in the **To** field and copy it to the clipboard

1. Open up a text editor such as **Notepad**

1. Paste the contents of the clipboard

1. Use your text editor’s **find & replace** feature to find
 “ (That’s space quote)
and replace it with
 (Nothing. Leave blank)

1. Use your text editor’s **find & replace** feature to find
“ < (That’s quote space less-than)
and replace it with
, (That’s comma space)

1. Use your text editor’s **find & replace** feature to find
>, (That’s greater-than comma)
and replace it with
 (Nothing. Leave blank.)

1. Save the file somewhere as **contacts.csv**

1. In **Outlook**, click **File**, **Import and Export…**

1. Select **Import from another program or file** and click **Next**

1. Select **Comma Separated Values (Windows)** and click **Next**

1. Click **Browse** and select the **contacts.csv** file

1. Under **Options**, select **Replace duplicates with items imported** (this will overwrite any old information and prevent duplicate entries) and click **Next**

1. Select **Contacts** and click **Next**

1. Click the box to the left of **Import “contacts.csv” into folder: Contacts**

1. Drag the **first value** to the **Name field**

1. Drag the **second value** to the **Email field**

1. Click **OK**

1. Click **Finish**