---
title: "npm Package Store"
datePublished: Mon Sep 29 2014 23:30:00 GMT+0000 (Coordinated Universal Time)
cuid: ckroxt1qd0ovzems12o1fborl
slug: npm-package-store-58a2040b6efa
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1627409532341/CXuLD0-W6.png
tags: javascript, nodejs, npm

---


I recently read on The Changelog a short post about a tool called Go Package Store.
> *An app that displays updates for the Go packages in your GOPATH.*

It has a simple interface that lets you see and update Go packages. According to the Changelog, “Every package ecosystem needs this.” I loved the idea and decided to replicate the project for npm.

## Presenting [npm Package Store](https://github.com/travishorn/npm-package-store)

A web app that displays updates for your globally installed npm modules. Inspired by Go Package Store.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627409530948/-TS3k37fB.png)

## Installation

```
> git clone [https://github.com/travishorn/npm-package-store.git](https://github.com/travishorn/npm-package-store.git)
> cd npm-package-store
> npm start
```


Then just go to [http://127.0.0.1:3000](http://127.0.0.1:3000/)

You can modify the port npm Package Store runs on with the PORT environment variable.

## To do

There’s plenty of room for improvement on this project.

* I’ve already [published it as an npm module](https://www.npmjs.org/package/npm-package-store), but similar to Go Package Manager, I’d like to make this into a binary that can be installed and executed globally, instead of a cloned repository.

* The web app should show the changelog between the currently installed version and the latest version. npm tracks certain changes in the package metadata, but this doesn’t include commit messages. npm Package Store will have to follow the repository property in package.json and query GitHub’s API to see changes.

* It should be possible to search through installed modules.

* It should also be possible to search and install modules in the npm registry.

* I’d also like to offer more options than just “update.” These might include “uninstall”, “downgrade”, etc.

See the code and contribute on GitHub: [https://github.com/travishorn/npm-package-store](https://github.com/travishorn/npm-package-store)

*Originally published on my old blog on September 29, 2014.*