---
title: "Quick Git Fixes"
datePublished: Wed Sep 10 2025 11:00:27 GMT+0000 (Coordinated Universal Time)
cuid: cmfdvbvc4001f02l68qzqhuvq
slug: quick-git-fixes
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1757428128423/dba04adf-55cd-47d8-be0f-1c6041c280cc.png
tags: workflow, git, versioning, commands, troubleshooting

---

Git is a powerful tool, but it can also be a source of frustration when things go wrong. I’ve documented the process I use to quickly fix some common issues. Keep reading to explore some common Git problems and their solutions, helping you navigate your way back to a smooth workflow.

## Go Back in Time

Sometimes you do something in Git that messes everything up. You can use `reset` to “go back in time” to a point where things *were* working.

Git records reference logs when the tips of branches and other references are updated in the local repository. First, review the reference log:

```bash
git reflog
```

You will see a list of everything you’ve done in the repository. Each reference has an index `HEAD@{index}`. Find the one before you broke everything. Then, reset to that point:

```bash
git reset HEAD@{index}
```

## Add One Small Change After a Commit

Sometimes you commit changes and then realize you forgot one small change. You can amend the commit.

First, make your small change. Then, stage it:

```bash
git add <your_changed_file>
```

Amend the commit:

```bash
git commit --amend --no-edit
```

## Change the Last Commit Message

Sometimes you make a commit, but then realize your commit message was missing information or had a typo. You can change it:

```bash
git commit --amend
```

## Move a Commit to the Correct Branch

Sometimes you accidentally commit to the wrong branch. First, check out the correct branch (the one you meant to commit to):

```bash
git checkout <name_of_the_correct_branch>
```

Cherry pick the last commit from the incorrect branch (the one you actually committed to on accident):

```bash
git cherry-pick <name_of_the_incorrect_branch>
```

The commit is now on the correct branch. Still, you must delete it commit from the incorrect branch.

First, check out the incorrect branch:

```bash
git checkout <name_of_the_incorrect_branch>
```

Now, hard reset back one commit:

```bash
git reset HEAD~ --hard
```

## Start Over from a Remote

Sometimes things are so messed up on your local machine, but you know the remote repository is in a good state, and you want to start over from that point.

First, go up one directory:

```bash
cd ..
```

Remove the repository directory:

```bash
rm -rf <repository_directory>
```

Clone the repository from the remote:

```bash
git clone <remote_url>
```

I hope these quick fixes help keep your workflow smooth and efficient. Keep experimenting, learning, and don't hesitate to refer back to this guide whenever you need a quick solution.