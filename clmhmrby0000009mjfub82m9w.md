---
title: "The Simple Guide to Installing Node.js using APT and NodeSource"
datePublished: Wed Sep 13 2023 11:00:09 GMT+0000 (Coordinated Universal Time)
cuid: clmhmrby0000009mjfub82m9w
slug: the-simple-guide-to-installing-nodejs-using-apt-and-nodesource
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1689275725963/f6fd7ed9-c028-4426-b343-ca5a4ec6c246.png
tags: nodejs, installation, lts, apt

---

Installing Node.js is a crucial step for developers looking to leverage its powerful features and vast ecosystem. In this comprehensive guide, we will walk you through the process of installing Node.js using APT (Advanced Package Tool) and NodeSource. By following these straightforward instructions, you'll be equipped with the latest (LTS or current) version of Node.js, supporting a great development experience and access to a wide range of Node.js libraries and frameworks.

## **Installation**

Binary distributions of [Node.js](https://nodejs.org/en) are provided by [NodeSource](https://nodesource.com/). Installing Node.js on Debian Linux via the NodeSource distributions is an officially supported method.

First, install curl.

```bash
sudo apt update
sudo apt install -y curl
```

Assume the `root` user.

```bash
sudo -s
```

Install Node.js via NodeSource.

```bash
curl -fsSL https://deb.nodesource.com/setup_18.x | bash - && apt-get install -y nodejs
```

The above command will install Node.js version 18.x, which was the latest LTS release as of this writing. If a new release is available, or you want to use another version, simply change `18` to the major version number you wish to install.

Exit the `root` user shell.

```bash
exit
```

Make sure Node.js is installed correctly by getting its version number.

```bash
node -v
```

It should display something like:

```bash
v18.16.1
```

> The exact version number might vary depending on which version you installed and/or when you are reading this guide.

## Upgrading

After following the steps above, minor and patch upgrades to the version of Node.js you installed can be managed via APT. You can update sources like so:

```bash
sudo apt update
```

With that done, your operating system is now aware of any updates to Node.js. You can upgrade your system like so:

```bash
sudo apt upgrade
```

This will upgrade all packages on your system as usual, which now *includes Node.js*.

### Upgrading Major Versions

Since the source list we told APT about only includes version Node.js 18 (or whichever version you chose), it will only upgrade Node.js within that major release. This is a safe practice to adhere to. However, if you are ready to upgrade to a newer major release, follow the steps below.

First, remove the current version of Node.js.

```bash
sudo apt remove nodejs
```

Remove the NodeSource APT list for the current version.

```bash
sudo rm /etc/apt/sources.list.d/nodesource.list
```

Set up the new source and install Node.js.

```bash
curl -fsSL https://deb.nodesource.com/setup_20.x | bash - && apt-get install -y nodejs
```

Notice the `20` in the command above. This means APT will now manage Node.js versions 20.x from now on. Change `20` to whichever version you want to install.

## Conclusion

You are now ready to run and serve Node.js apps. You may choose to write and/or run completely custom Node.js apps, or you may choose to go with a framework like Next.js.

Installing Node.js and managing it using APT and NodeSource is a straightforward process that allows developers to quickly set up a reliable and up-to-date Node.js environment. By following the steps outlined in this guide, you can easily install Node.js on your system, ensuring you have access to the latest stable features and improvements. With Node.js in place, you can unleash the full potential of JavaScript, build scalable applications, and tap into the vast ecosystem of Node.js packages.

Cover photo by [simmel](https://unsplash.com/@simmelx?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/photos/a-lone-tree-in-the-middle-of-a-desert-1vBXUCb-bXQ?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText).