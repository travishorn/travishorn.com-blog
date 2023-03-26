---
title: "Version Control"
datePublished: Mon Apr 02 2012 05:00:00 GMT+0000 (Coordinated Universal Time)
cuid: ckrnhpi4y0bv1ems1gw2cetdz
slug: version-control-9505fd1d6b4
tags: github, version-control, git

---


The advent of electronic data brought new challenges to the way we keep revisions of information. Keeping track of what happens to a piece of data becomes very important in a digital setting where many additions, deletions, and changes can occur quickly. In print, revisions of books are often kept track of with edition numbers such as “1st edition” and “2nd edition.” In the digital world, a file can be changed hundreds of times in a day, which requires the use of software to aid in versioning these files.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627409963898/CSOGbZ0Lt.png)

When developing software, often times **everything** is put into [version control](http://en.wikipedia.org/wiki/Revision_control); from the data to the logic. Sometimes multiple people are working on the same feature, or a person could be working on multiple features on different versions of the software. Version control keeps this process sane.

While a file is versioned, changes are tracked in increments. If, for some reason, you need to revert back to a previous version (say something horrible happened while editing some files), you can easily do this. Another great feature is being able to create a “branch” of the software. You basically work on a copy of the code. If you like the changes when you’re done, you can attempt to merge the branch back in with the “trunk,” which may have changes from other developers since you branched your copy. If there are conflicts, you’ll have to resolve those, but with a proper development strategy, you can decrease conflicts and increase the ease of resolving them. Features such as “file locking” can help with this.

A good place to start looking into version control is to take a look at some version control systems (VCS). A couple popular options are [Subversion](http://subversion.apache.org/) and [Git](http://git-scm.com/). While these systems work in different ways, they attempt to serve the same purpose: change management. I’ve found my way to Git with my latest projects. Code that is stored in Git repositories can easily be shared with the world online with the increasingly popular site, [GitHub](https://github.com/).

If you do not have your code in version control, you should really consider the benefits and ask yourself “why not?”