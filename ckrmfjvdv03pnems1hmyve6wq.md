---
title: "Day-to-day source control"
datePublished: Thu Oct 25 2018 11:01:01 GMT+0000 (Coordinated Universal Time)
cuid: ckrmfjvdv03pnems1hmyve6wq
slug: day-to-day-source-control-cc7717a32608
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1627410359114/knymS6IkD.png
tags: software-development, web-development, opensource, git

---


Developers need source control. As a project gets bigger, it becomes hard to maintain without a system for managing code changes.

Source control management systems — like [Git](https://git-scm.com/) — are very powerful and include commands that handle so many use cases. It can be hard to wrap your head around how it’s used in day-to-day development. Below, I’ll walk through some of the basic source control features I use on a daily basis.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627410357148/dTUQh1_wC.png)

I use [Git](https://git-scm.com/) for source control. It is far and away the most popular option at the moment; used by hundreds of thousands of open-source projects worldwide.

Unless your operating system comes with Git pre-installed, you will need to [download and install it](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git).

### Starting a new project

If I’m prototyping something small or just testing something out, I might not even decide to use source control. If it’s simple enough, I might even build it on [CodePen](https://codepen.io).

But if I’m going to be doing any sort of development work that I know will grow into something larger, I start out by initializing a Git repository. This isn’t something that I do daily, but it is necessary for the rest of the steps below.

```
> cd C:\users\travis\development
> mkdir my-new-project
> cd my-new-project
> git init
```


Git is now initialized and ready for code commits.

### Working on the master branch

Since I am usually the only developer on any of my given projects, I have some freedom to decide on my own workflow. The simplest and most common method I use is working on `master` and committing code when I get to a good stopping point or finish a feature.

Say I have a single-page website that shows a chart for a given set of data. Here’s the project structure:

```
my-charts/
  data/
    data.tsv
  scripts/
    d3.js
    chart-1.js
  styles/
    main.css
  index.html
```


*Assume that in this example, the directory already has a Git repository initialized on it.*

Now I want to add a new chart to this page.

I modify `index.html` to add a new chart `&lt;div&gt;` and bring in the (not yet created) new script.

```
<html>
  <head>
    <link rel="stylesheet" href="styles/main.css">
    <title>My Charts</title>
  </head>
  <body>
    <div id="chart-1"></div>
    **<div id="chart-2"></div>
    **
    <script src="scripts/d3.js"></script>
    <script src="scripts/chart-1.js"></script>
    **<script src="scripts/chart-2.js"></script>**
  </body>
</html>
```


Then I create `scripts/chart-2.js`:

```
d3.tsv('/data/data.tsv', (err, data) => {
  const chart = d3.select('#chart-2').append('svg');

  /*
   * SNIP!
   */
});
```


When I’m finished and the chart is displaying how I like, I commit this code directly to the master branch.

```
> git add index.html
> git add scripts/chart-2.js
> git commit -m "Added chart 2"
```


And that’s it! Most of my daily work follows this process. To summarize…

1. Make changes

1. Stage with `git add`

1. `commit` with a simple comment explaining changes

Of course, sometimes I start to make changes but decide I want to abandon them and go back to the last commit. That’s as simple as…

```
> git reset --hard
```


Note that any untracked files (one’s that haven’t been `git add`ed) will need to be manually deleted. Also note that a safer, but slightly more advanced method of undoing changes is `[git stash`](https://www.atlassian.com/git/tutorials/saving-changes/git-stash).

### Branching

Somewhat often, I find myself needing to work on a separate branch. I might do this if I will be working on a logically separate feature or if I’m testing a major change that I might want to keep, but might want to abandon. Working on a separate feature branch keeps the master branch clean, which makes your code easier to reason about.

Going back to the chart page example. I would start out by checking out a new branch with a descriptive name

```
> git checkout -b chart_3
```


Then I’ll make the changes and commit them just as before.

```
> git add index.html
> git add scripts/chart-3.js
> git commit -m "Added chart 3"
```


If I’m happy with the changes, I’ll merge them into the master branch.

```
> git checkout master
> git merge chart_3
```


Once merged, the feature branch can be safely deleted.

```
> git branch -d chart_3
```


If I **don’t** like the changes and I want to get rid of the branch **without merging**, I force the delete.

```
> git branch -D chart_2
```


### Working with remotes

The process above includes 99% of the git features I use daily. Most of the time I am not working with remote repositories. However, I do collaborate with others on GitHub at times. It’s a little beyond the scope of this article, but I wrote a separate article for contributing code on GitHub you can find below.
[**Contributing Code on GitHub**
*If you’ve been looking to get into contributing your skills and ideas to the open-source community, this guide should…*travishorn.com](https://travishorn.com/contributing-code-on-github-405a68d8f994)

## When things go wrong ⚠️

Everything I’ve written so far assumes you never make a mistake. Of course this is unreasonable. I make mistakes all the time and have to undo or fix something I accidentally messed up.

### Going back to before everything was broke

The nice thing about source control is that you can use it as a sort-of “time machine.” If I committed changes that broke everything, I can reset back to when things were working. First, I find the last known good commit…

```
> git reflog
```


This gives a big list of all the things I’ve done. Each one has an index that looks like `HEAD@{index}`. I find the one just before I committed bad code and reset to it.

```
> git reset HEAD@{index}
```


### Adding that one small forgotten change

If I’ve already committed but forgot to make one small change, I make the change and then amend the commit.

```
> git add [filename with change]
> git commit --amend
```


Note that this will put you in the commit message editor. You’ll have to know [the basics of vi](http://www.lagmonster.org/docs/vi.html) to get around in this editor.

### Changing the last commit message

Sometimes I commit changes but forget to document it or mis-document it. In those cases I also amend the commit.

```
> git commit --amend
```


### I committed changes to the wrong branch

This happens more often than I’d like to admit. I forget to switch branches and think I’m on one feature branch, but I’m really on master. In those cases, I check out the correct branch, cherry-pick the commit from master, switch back to master and reset the commit.

```
> git checkout correct_branch
> git cherry-pick master
> git checkout master
> git reset HEAD~ --hard
```


### The nuclear option

If I’ve really messed something up, and I know the repository has a clean remote, I just start over! Wipe the files from disk and re-clone…

```
> cd ..
> rmdir /S repo_name
> git clone [https://example.com/repo_name.git](https://example.com/repo_name.git)
> cd repo_name
```


There’s so many more features and workflows using source control, but those are the very basics that I might use on any given day.