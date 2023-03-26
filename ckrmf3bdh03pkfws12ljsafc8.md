---
title: "Unix Commands"
datePublished: Thu Jul 28 2016 22:01:01 GMT+0000 (Coordinated Universal Time)
cuid: ckrmf3bdh03pkfws12ljsafc8
slug: unix-commands-a1b34e5e1d60
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1627410420285/q9e8_igox.jpeg
tags: unix, terminal

---


This post is part of a project to move my old reference material to my blog. Before 2012, when I accessed the same pieces of code or general information multiple times, I would write a quick HTML page for my own reference and put it on a personal site. Later, I published these pages online. Some of the pages still get used and now I want to make them available on my blog.

![Photo by [Torkild Retvedt](https://cdn.hashnode.com/res/hashnode/image/upload/v1627410418528/oxrdioGq4.html) under [CC BY-SA 2.0](https://creativecommons.org/licenses/by-sa/2.0/)](https://cdn-images-1.medium.com/max/8288/1*sEoOPC9hLpq-w4H4I7g6gA.jpeg)*Photo by [Torkild Retvedt](https://www.flickr.com/photos/torkildr/) under [CC BY-SA 2.0](https://creativecommons.org/licenses/by-sa/2.0/)*

This is a short list of frequently-used commands for OpenBSD. Many of them are common with most Unix-like platforms. The text is broken into sections designated by the titles in larger print. For each command, the text to type at the prompt will appear first, in monospace font. A short blurb describing the command then follows.

### Logging Out and Shutting Down

```
exit
```


Log current user out. Displays login: prompt for another user

```
halt
```


Shut down the operating system. Same as typing shutdown -h now

```
reboot
```


Reboot the operating system. Same as typing shutdown -r now

### Directory Management

```
pwd
```


Output what directory you’re currently in

```
cd x
```


Change directory to x

```
pushd x
```


Same as cd, but saves the directory you were in when you executed the command. Type popd to return to the saved directory

```
mkdir x
```


Make a new directory called x

```
rmdir x
```


Remove directory x

```
rm -r x
```


Remove directory x and all files and directories within

```
ls
```


List files and directories in current directory. Use -l to get details, -a to get hidden files, -F to id executables, files, and directories

```
find / -name x
```


Search all directories for x

```
mkdir /mnt/sharename
sudo mount -f cifs //servername/sharename -o username=username,password=password /mnt/sharename
```


Mounts a Windows share called sharename on the server called servername. Replace username and password with appropriate credentials.

### File Management

```
rm x
```


Remove file x

```
rm -r x
```


Remove directory x and all files and directories within

```
cp x.txt y.txt
```


Copy x.txt and name the copy y.txt

```
mv x.txt y.txt
```


Move (rename) x.txt to y.txt

```
df -h
```


Display disk usage. The -h switch makes numbers human-friendly

### Disk Management

```
mount /wd0a files
```


Adds the disk wd0 (IDE primary master. wd1 would be IDE primary slave, etc) partition a to directory files. files must exist

```
umount files
```


Unmount the device mounted to the directory files

### Miscellaneous

```
man
```


Unix manual. Type man x for a manual on a specific command

```
x | more
```


Filter that will pause and wait for user input when the command teakes up more than one screen

```
su
```


Elevate access to root privileges

```
ps -aux
```


See resources each process is using. Use | head -5 to just get the top 5 lines of the command

```
mail -s “x” y
```


Send email to y (email address) with subject x. Type your message and press Ctrl+D

```
alias x=’y’
```


Set an alias so when you type x, the command y executes

### Text Viewing and Editing

```
cat x
```


Displays contents of file x on screen

```
vi
```


Opens full-screen and provides file editing utilities. Exit by hitting Esc, type :wq to save and exit or :q! to exit without saving. Learn more about vi at [http://staff.washington.edu/rells/R110/](http://staff.washington.edu/rells/R110/)

### User Management

```
adduser
```


Create new user. Accept defaults for most prompts. Enter username and password when prompted. If you want user to have access to su, type wheel when prompted for what groups to invite user to

```
echo ‘x ALL=(ALL) ALL’ >> /etc/sudoers
```


Add existing user x to the sudoers list, giving them access to sudo command

```
rmuser
```


Remove an existing user

```
cat /etc/passwd | cut -d: -f1 | grep -v \#
```


List existing users

```
id
```


See who you’re logged in as

```
w
```


See what users are currently logged in and what they’re doing

```
chsh -s /bin/ksh x
```


Change the default shell to ksh for user x

### Archiving

```
tar cvzf x.tgz y
```


Combine all files in directory y and write them to a file called x.tgz. Will be written back as a directory when extracted

```
tar tzf x.tgz
```


Display the contents of x.tgz

```
tar xvzf x.tgz
```


Extract contents of x.tgz in current directory

### Apache Control

```
apachectl start
```


Start Apache web server

```
apachectl stop
```


Stop Apache

### Adding Software

```
export PKG_PATH=ftp://ftp.openbsd.org/pub/OpenBSD/4.7/packages/amd64
```


Sets the package path. This is the location new packages will be downloaded from

```
pkg_add -r -v x
```


Install package x

### SSH

```
ssh x@y
```


Connect to an SSH server as x at location y

```
ssh-keygen
```


Generate private and public keys to use with public key authentication. You may leave default filename but enter a long passphrase (16+ characters).

After running, append ~/.ssh/id_rsa.pub to server’s ~/.ssh/authorized_keys file using the command below.

```
cat x >> ~/.ssh/authorized_keys
```


Allow client with public key x to connect using public key authentication. x can be found in client’s ~/.ssh/id_rsa.pub file after running command above.

```
kill -HUP `cat /var/run/sshd.pid`
```


Restart SSH daemon after changing config file

```
kill x
```


Boot a remote user off SSH. x is the PID for their sshd process. The PID can be found by running ps -aux and finding the COMMAND labeled sshd: username

### Helpful Links

[http://www.computerglitch.net/blog/attic/adding-a-new-hard-drive-to-openbsd.html](http://www.computerglitch.net/blog/attic/adding-a-new-hard-drive-to-openbsd.html)
Adding a new hard drive disk

Reboot the operating system. Same as typing shutdown -r now