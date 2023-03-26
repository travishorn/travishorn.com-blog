---
title: "Contributing Code on GitHub"
datePublished: Mon Apr 09 2012 05:00:00 GMT+0000 (Coordinated Universal Time)
cuid: ckroxlym20pccfws12a7oa2ip
slug: contributing-code-on-github-405a68d8f994
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1627409618791/DuI1ZZhwu.png
tags: github, git

---


If you’ve been looking to get into contributing your skills and ideas to the open-source community, this guide should be able to help you out. The first step is finding a project you want to contribute to on GitHub. You can be a part of any project you see on the site. Take a look at projects’ Issues page to see what bugs you might be able to fix or what features you may be able to add.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627409617514/7Kgr3-g14.png)

These instructions are for Linux. They assume you already have Git installed. If you don’t, try `sudo apt-get install git`. More instructions can be found in the [Git documentation on Installing Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git).

### Forking and Cloning the Project

You will only have to complete this section once for every project you want to contribute to.

1. In your web browser, visit the project’s repository on [http://github.com](http://github.com/)

1. Click the **Fork** button. This creates a copy of the repository on *your* account.

1. At a terminal, run `git clone https://github.com/username/reponame.git` where `username` is your GitHub username and `reponame` is the name of the repository you forked.

1. Change directory into the project directory with `cd reponame`

1. Run `git remote add upstream https://github.com/username/reponame.git `Where `username` is the **original** project owner’s username and `reponame` is once again the name of the repository.

Note: We’re using `upstream` as the remote’s name here, but you can use anything you want.

### Working on the Code Locally

Follow these steps any time you want to make changes to the code, such as fixing a bug or adding a feature.

1. Pull the latest commits before working: `git pull upstream master`. This step is important to make sure you’re working with the latest code from the original project.

1. Run tests if they exist. Follow project instructions for this; They can usually be found in the project’s **README** file

1. Checkout the latest commits from your forked project: `git pull origin master`. This step is important to make sure you’re working with the latest code from *your* forked project.

1. Create a new branch (or switch to branch if it already exists):
`git checkout -b branchname` Where `branchname` is a name to indicate what you’ll be working on. For example: `bug_1234`

1. Make your changes. Open/create source files; Save changes

1. If the project uses them, add unit tests and make sure all tests pass. Again, follow project instructions.

1. Stage files for tracking by running `git add filename `where `filename` is the name of a file you’ve changed.

1. Repeat step 7 for all changed files. Use `git status` to see all changed files.

1. Commit changes to local repository by running `git commit -m “Brief description of changes”`

### Pushing Changes to GitHub and Requesting a Pull

When you’re done working locally and ready to push online for others to use, follow the steps below. It’s worth mentioning one more time to make sure all tests pass. I strongly discourage pushing code where any tests fail.

1. Push branch to your repository on GitHub by running `git push origin -u branchname` Where `branchname` is the name you chose earlier

1. Visit your forked project on [http://github.com](http://github.com/)

1. Select the branch you’ve been working on from the **Branch** dropdown menu

1. Click the **New pull request** button

1. Write a **Title** and **Comment** describing the changes in detail.

1. Click the **Create pull request** button

*This post first appeared on my old blog on April 9, 2012. It was last updated October 26, 2018.*