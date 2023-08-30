---
title: "Step-by-Step Guide: Setting Up a Git Repository Hosting Server"
datePublished: Wed Aug 30 2023 11:00:09 GMT+0000 (Coordinated Universal Time)
cuid: cllxmler7000009kw1qks58ho
slug: step-by-step-guide-setting-up-a-git-repository-hosting-server
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1689274982587/02f53192-0535-4fd2-b8b9-274c8d60f71a.png
tags: hosting, github, server, version-control, git

---

If you're looking for a reliable and secure way to host your Git repositories, setting up your own server can be a great solution. In this step-by-step guide, we will walk you through the process of creating your own Git repository hosting server, from prerequisites to configuration, storage management, and even implementing public key authentication. By following these straightforward instructions, you'll gain complete control over your codebase and enjoy seamless collaboration with your team members.

## The Why

While hosting providers like GitHub and GitLab offer convenient and feature-rich solutions for hosting Git repositories, there are scenarios where hosting your own repository becomes a preferred option. Firstly, hosting your own Git repository provides you with complete control over your code and infrastructure. This level of control ensures that you can customize your server environment, implement specific security measures, and tailor the setup to meet your organization's unique requirements. Additionally, self-hosting allows for enhanced privacy and data sovereignty, as your code remains within your own network. This can be crucial for organizations dealing with sensitive or proprietary code. By hosting your own Git repository, you can have a higher degree of autonomy, customization, and security, making it an appealing choice for those seeking full ownership and control over their codebase.

## **Prerequisites**

* A working Linux server machine
    
* You are logged in as a non-root user with sudo privileges
    

## **Install git**

If it isn't already installed, install [git](https://git-scm.com/).

```bash
sudo apt update
sudo apt install -y git
```

## **Global Configuration**

Set a default initial branch name in git

```bash
git config --global init.defaultBranch master
```

Feel free to change the name `master` above to anything you like. While `master` is the traditional default, `main` is also common.

Configure git to reconcile divergent branches by merging changes, rather than rebasing.

```bash
git config --global pull.rebase false
```

> `pull.rebase false` is a preference. But either `true` or `false`, the configuration option must be set.

## **Create Storage Directory**

You'll need a place to store any git repositories. To keep things organized, create a separate directory for them.

```bash
mkdir ~/repos
```

Your server is now ready to host git repositories. Next, you should start creating new repositories on the server that developers can pull from and push to.

## **Create and set permissions**

Initialize a bare repository in the home directory.

```bash
git init --bare ~/repos/[repo name]
```

Change `[repo name]` to a name of your choosing. Example: `my-app`

Write a descriptive name or short description of the repository.

```bash
echo "Some short description" > ~/repos/[repo name]/description
```

Now developers who can authenticate as this local user can clone, pull from, and push to this repository.

## **Working with the Remote Repository**

First, clone the repository from the server to your development machine.

```bash
git clone ssh://[username]@[your server's IP address/hostname][path to repository]
```

Example:

```bash
git clone ssh://myuser@myserver/home/myuser/repos/my-app
```

> You can use [`localhost`](http://localhost) as the hostname as long as...
> 
> 1. you're using a virtual machine,
>     
> 2. you're running `git clone` from the host machine, and
>     
> 3. you have port 22 forwarded
>     

You will be prompted to enter password for `myuser`.

A new directory named `my-app` (or whatever your repository is named) is created on your development machine. Change into it.

```bash
cd myrepo
```

You can pull any new changes from the remote repository at any time.

```bash
git pull
```

And you can push your changes to it.

```bash
git push
```

## **Public Key Authentication**

If you haven't set up public key authentication...

1. you will need to share the password for `myuser` with each developer who needs pull/push access.
    
2. Developers will also need to enter the password each time they pull/push.
    

Neither of the above conditions is ideal. You can solve both of these issues by setting up public key authentication.

Make sure you have...

* set up public key authentication on your server
    
* added your public key to the server user's `authorized_keys` file
    
* added your server as a configured host on your workstation
    

[I wrote a separate blog article with instructions for all of these things.](https://travishorn.com/unlocking-the-power-of-public-key-authentication-in-openssh-a-comprehensive-guide)

With a configured host, users can interact with a server using the alias in the configuration file.

```bash
git clone ssh://myuser@myserver/home/myuser/repos/myrepo
```

> The name `myserver` here is the name of the configured host set up in `~/.ssh/config`, not necessarily the server's hostname (although they can be the same).

You will be asked for your SSH key passphrase rather than the server user's password. This eliminates the need to share that password.

If you don't want to enter the SSH key passphrase each time, you can use an SSH agent (recommended - find more information online) or you can use a key pair with an empty passphrase (not recommended).

Setting up your own Git repository hosting server is an efficient and secure way to manage your code. With the clear steps we've outlined in this guide, you can easily create a server tailored to your needs, ensuring the confidentiality and accessibility of your codebase. By taking ownership of your Git hosting, you can streamline your development workflow, enhance collaboration, and maintain complete control over your repositories. Start hosting your Git repositories on your own server today and experience the benefits firsthand.

Cover photo by [Tony Litvyak](https://unsplash.com/@justatony?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/photos/a-close-up-of-a-bunch-of-green-leaves-U26V8mqFUBY?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText).