---
title: "xcopy Options for Backup"
datePublished: Sun Jul 31 2016 22:01:02 GMT+0000 (Coordinated Universal Time)
cuid: ckrni2qk40byrems1ects60vq
slug: xcopy-options-for-backup-98ad20f4ec2e
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1627409916586/hXjtJ_7cs.jpeg
tags: command-line, windows, backup

---


This post is part of a project to move my old reference material to my blog. Before 2012, when I accessed the same pieces of code or general information multiple times, I would write a quick HTML page for my own reference and put it on a personal site. Later, I published these pages online. Some of the pages still get used and now I want to make them available on my blog.

![Photo by [Annie Spratt](https://cdn.hashnode.com/res/hashnode/image/upload/v1627409914681/l85aJOyu9.html)](https://cdn-images-1.medium.com/max/8000/1*OCwunCZDifHrMaR5LJP8rg.jpeg)*Photo by [Annie Spratt](https://unsplash.com/@fableandfolk)*

The command-line program **xcopy** can be a useful tool for scheduled backups. It can be used to mass-copy files and folders to another location such as a network attached storage device. Use the options below for best results.

### Command

```
xcopy [source] [destination] /C /E /G /H /I /K /M /R /X /Y
```


### Switch Explanations

* /C — Continues copying even if errors occur. Without this option, a backup could could end unexpectedly because of one corrupt file.

* /E — Copies directories and subdirectories, including empty ones. We want to preserve the exact file and folder structure.

* /G — Allows the copying of encrypted files to destination that does not support encryption. Making sure everything is being copied exactly.

* /H — Copies hidden and system files also. Again, making sure everything is being copied exactly.

* /I — If destination does not exist and copying more than one file, assumes that destination must be a directory.

* /K — Copies attributes. Normal Xcopy will reset read-only attribute. Again, making sure everything is being copied exactly.

* /M — Copies only files with the archive attribute set, turns off the archive attribute. This ensures only files that have changed since last backup are backed up.

* /R — Overwrites read-only files. Since the /K switch is copying all attributes including read-only, use this option to make sure read-only files that were changed are backed up.

* /X — Copies file audit settings (implies /O). Again, making sure everything is being copied exactly.

* /Y — Suppresses prompting to confirm you want to overwrite an existing destination file. Many files are probably going to be overwritten, this will automatically overwrite them without prompting on each one.

### Example

```
xcopy C: \\backup-server\user1 /C /E /G /H /I /K /M /R /X /Y
```
