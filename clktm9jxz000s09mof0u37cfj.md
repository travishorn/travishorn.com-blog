---
title: "Unlocking the Power of Public Key Authentication in OpenSSH: A Comprehensive Guide"
datePublished: Wed Aug 02 2023 11:00:09 GMT+0000 (Coordinated Universal Time)
cuid: clktm9jxz000s09mof0u37cfj
slug: unlocking-the-power-of-public-key-authentication-in-openssh-a-comprehensive-guide
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1689273702720/a142acd7-dce6-4bc2-a258-c89285b45184.png
tags: security, ssh, openssh, public-key-cryptgraphy, server-administration

---

Public key authentication is a fundamental aspect of secure and efficient communication in the realm of server administration. In today's interconnected world, where safeguarding sensitive data is paramount, OpenSSH stands out as a reliable and widely-used tool for secure remote access. In this blog article, we will delve into the intricacies of working with public key authentication in OpenSSH. We will explore the step-by-step process of preparing a server, generating an SSH key pair, adding a public key to a user account, establishing a secure connection using SSH keys, configuring hosts for seamless access, disabling password authentication for enhanced security, and even revoking access when necessary. Whether you're a seasoned system administrator or just starting your journey in server management, this guide will equip you with the knowledge and practical skills to leverage the power of public key authentication with confidence. Let's dive in and unlock the secrets of OpenSSH's public key authentication!

## **Prerequisites**

* OpenSSH server is running on your server
    
* OpenSSH client is installed on your workstation
    

## **Prepare the Server**

The steps in this section need to be done once per local user account on the server.

Log in to the server user account you want to use.

Create a directory called `.ssh` in the home directory.

```bash
mkdir ~/.ssh
```

Set the directory's permissions so only you, the owner can read, write, and execute it.

```bash
chmod 700 ~/.ssh
```

Create an empty `authorized_keys` file.

```bash
touch ~/.ssh/authorized_keys
```

Set the file's permissions so only you, the owner can read and write to it.

```bash
chmod 600 ~/.ssh/authorized_keys
```

Anyone who can provide a private key that matches the public keys stored in this file will have access to the account.

## **Generate an SSH Key Pair**

Next, create a key pair. A key pair is a public key and a matching private key. The public key will be added to the previously created `authorized_keys` file.

On your workstation (not the server), open up a **Terminal**.

Create a directory called `.ssh` in your home directory.

```bash
mkdir ~/.ssh
```

Change into the directory.

```bash
cd `/.ssh
```

Generate a key pair.

```bash
ssh-keygen -f [your name]@[location of workstation]-[timestamp] -C [your name]@[location of workstation]-[timestamp]
```

For example:

```bash
ssh-keygen -f myname@work-2023-05-01 -C myname@work-2023-05-01
```

> You can replace `[your name]@[location of workstation]-[timestamp]` with anything you want. But I recommend the above because it will help identify a key by the name of the person it belongs to, the location it will be used from, and the date it was created.

Upon running the command, you will be asked for a passphrase. You can choose anything you want here, or you can leave it blank.

> Make sure you understand [the security implications of leaving the passphrase blank](https://superuser.com/questions/261361/do-i-need-to-have-a-passphrase-for-my-ssh-rsa-key) before you do it.

Two files are created. One without a file extension (the private key) and one with the file extension `.pub` (the public key).

Example:

* `myname@work-2023-05-01`
    
* [`myname@work-2023-05-01.pub`](mailto:myname@work-2023-05-01.pub)
    

`myname@work-2023-05-01` contains the private key that will need be provided any time you try to connect to the server. It is not recommended to copy this file to another machine. If you need to connect from multiple machines, create multiple key pairs. Protect this file like a password.

[`myname@work-2023-05-01.pub`](mailto:myname@work-2023-05-01.pub) contains the public key that can be shared. Any user on any device which has this key entered into its `authorized_keys` file grants you access to the user account (as long as you can provide the private key).

## **Add a Public Key to a User**

Once you have a key pair, you'll want to add the public key to a local user account on the server.

From your workstation, copy the public key into the server user's `authorized_keys` file.

```bash
cat ~/.ssh/[public key filename].pub | ssh [your username]@[your server's IP address or hostname] "cat >> ~/.ssh/authorized_keys"
```

Be sure to replace `[public key filename]` with the actual public key filename, `[your username]` with your actual username on the server, and `[your server's IP address/hostname]` with your server's actual IP address or hostname.

> If you're using a virtual machine, you're connecting from the host machine, and you have port 22 forwarded, you can use [`localhost`](http://localhost) as your server's hostname.

The command will ask for the user's current password so it can log in and append the public key to the `authorized_keys` file.

## **Connecting to the Server using an SSH Key**

From your workstation, connect to the server, providing the SSH private key for authentication.

```bash
ssh [your username]@[your server's IP address/hostname] -i ~/.ssh/[private key filename]
```

You will be asked for the key pair's passphrase (if any). Once authenticated, you will be connected to your server as before.

### **Using a Configured Host**

The command you must execute to connect to your server via SSH is tedious to type out. You can alleviate this with a configured host.

On your workstation, create or edit `~/.ssh/config`.

```bash
Host myserver
  HostName myserver
  User myusername
  Port 22
  IdentityFile ~/.ssh/[private key filename]
```

Make sure to replace `myserver` and `myusername` with the appropriate values.

With that file saved, you can execute the much shorter `ssh` command.

```bash
ssh myserver
```

> While the `HostName` must be set to your server's actual IP address or hostname, the `Host` can be set to anything you like. It is like an alias that identifies the configured host.

## **Disabling Password Authentication**

Once SSH key authentication is set up and working correctly, I suggest disabling the less-secure password authentication method.

Edit `/etc/ssh/sshd_config`. Somewhere in the file, add the following line.

```bash
PasswordAuthentication no
```

Restart the SSH server.

```bash
sudo systemctl restart sshd
```

From now on, users cannot use a password to authenticate their SSH sessions with the server; they must provide a valid private key.

## **Removing Access**

If you need to remove access from a particular key pair, edit the server user's `~/.ssh/authorized_keys` file. and delete the line which has the public key you want to remove.

Your server is now ready to handle remote connections with public key authentication. Mastering the art of working with public key authentication in OpenSSH opens up a world of secure and efficient remote access possibilities. By following the step-by-step guide outlined in this article, you have gained valuable insights into preparing servers, generating key pairs, configuring user accounts, establishing secure connections, and enhancing overall security by disabling password authentication. Armed with this knowledge, you can confidently navigate the intricate landscape of server administration, ensuring the confidentiality and integrity of your data. Embracing public key authentication in OpenSSH empowers you to take full control of your remote access workflows while upholding the highest standards of security. So, go forth, explore, and harness the power of public key authentication to revolutionize your server management practices.

Cover photo by [Tony Litvyak](https://unsplash.com/@justatony?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/photos/a-close-up-of-a-pine-tree-branch-0ftahD2om9I?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText).