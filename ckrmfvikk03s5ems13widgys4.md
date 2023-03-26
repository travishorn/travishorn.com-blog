---
title: "Semantic versioning with Git tags"
datePublished: Thu May 09 2019 11:01:01 GMT+0000 (Coordinated Universal Time)
cuid: ckrmfvikk03s5ems13widgys4
slug: semantic-versioning-with-git-tags-1ef2d4aeede6
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1627410228890/gMcMTWnK2.png
tags: github, web-development, version-control

---


Why should we put thought into version numbering? What’s the best system for versioning? How can we implement it? I recently starting thinking about these questions. After doing research, I came to a decision and created a process for myself. You may find it helpful, too.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627410224931/Ee4NctYB-.png)

### Why do we need versions and version numbers?

Say you’ve created an awesome library for categorizing cats. Developers can add a new cat to the system by calling it like this:

```
awesomeLibrary.addCat({
  name: 'Fluffy',
  color: 'white',
});
```


But later you decide your library shouldn’t just be limited to cats, but should support all kinds of pets. You change the interface so adding a pet is done like this:

```
awesomeLibrary.addPet({
  type: 'cat',
  name: 'Fluffy',
  color: 'white',
});
```


You might then call the old code version 1 and the new code version 2. This keeps things clear when referring to documentation about your library. It also helps other developers understand which version is newest so they know they’re using the version that has the most recent features and bug fixes.

### What is semantic versioning?

Calling the first version v1 and incrementing the number each time there is a code change works okay (v2, v3, v4, etc.). But there’s a better way that gives users and other developers more information about what to expect from the version.

To simplify, a semantic version contains three parts. For example.

```
v1.2.3
```


The first number is the **major** version, which only gets incremented when you make incompatible API changes. For example, when you rename `addCat()` to `addPet()`. Anyone trying to use `addCat()` in the new version will encounter an error.

The second number is the **minor** version, which gets incremented when you add functionality in a backwards-compatible manner. For example, say you want to add a new **optional** property to the `addPet()` function to keep track of the pet’s size.

```
awesomeLibrary.addPet({
  type: 'cat',
  name: 'Fluffy',
  color: 'white',
  size: 'small',
});
```


Developers could upgrade your library to the latest version and it wouldn’t break anything they’ve already written. But it **would** enable the new functionality to track pet size if they decided to build that in to their app.

The third number is the **patch** version, which gets incremented when you make backwards-compatible bug fixes. For example, if you had a bug that caused an error any time the user tried to add a `'big'` pet, you could fix it and increment the third (patch) number. The code change to fix the bug didn’t change how your library is used and didn’t add any new features, it just fixed a problem.

### Initial development phase

There is one big exception: In the initial development phase — before 1.0.0 — your API usually changes rapidly. The simplest thing to do during this phase is to increment the minor version with each release. It’s common for projects to rapidly go through 0.1.0 to 0.2.0 to 0.3.0 and so on before the project is ready to be used in production — when it becomes version 1.0.0 and beyond.

The only other thing I’ll say about it is that you must reset **patch** to 0 when you increment **minor**. Similarly, you must reset both **minor** and **patch** to 0 when you increment **major**. For example, if your library is at version 1.2.3 and you make a breaking change, the new version would be 2.0.0.

More information is available at the [official Semantic Versioning website](https://semver.org/). I highly recommend reading through this short page. There’s even a helpful FAQ.

### Where to record version numbers

A common place to record version numbers is in [Git tags](https://git-scm.com/book/en/v2/Git-Basics-Tagging). You tag a version **after** committing the changes.

Here are the basic steps. They assume you’ve already [staged the changes](https://githowto.com/staging_changes).

1. `git commit -m "Short description of changes"`

1. `git tag -a v0.2.0 -m "version 0.2.0"`

### package.json

If you are using npm (or `package.json` in general), there is a [place to store the version already in `package.json`](https://docs.npmjs.com/files/package.json#version). You could manually update the version number in that file. But there’s an even easier way.

```
npm version minor
```


Replace `minor` with whichever level change you’re making (major, minor, or patch).

This will update the version in `package.json`. If there’s a Git repository in the current directory, it will also automatically add a tag in Git!

Again, commit **then** bump version.

1. `git commit -m "Short description of changes"`

1. `npm version minor`

### Working with Git remotes

You may be using [Git remotes](https://git-scm.com/book/en/v2/Git-Basics-Working-with-Remotes). Some common uses of Git remotes are a private repository in your company or a public repository on [GitHub](https://github.com/).

If you’re using Git remotes, you have to explicitly push tags to them.

This can be done after pushing code changes.

```
git push origin master
git push origin master --tags
```


Or you can do it all in one go.

```
git push origin master --follow-tags
```


To make git follow tags on every push by default, you can enable this setting:

```
git config push.followTags true
```


### Putting it all together

Let’s see an example of the whole process.

Create a new directory and move into it.

```
mkdir awesome-library
cd awesome-library
```


Initialize a new Git repository.

```
git init
```


Add the remote repository. I created one on GitHub using their web interface.

```
git remote add origin https://github.com/travishorn/awesome-libarary.git
```


Create `package.json`.

```
{
  "name": "awesome-library",
  "version": "0.0.1"
}
```


Create `index.js`.

```
module.exports = {
  addCat(cat) {
    /* snip */
    return cat;
  },
};
```


Stage the changes.

```
git add package.json
git add index.js
```


Commit the changes.

```
git commit -m "Added addCat function"
```


Bump the version. For the sake of example, I’ll say my library is ready to be used in production. That means I’ll bump the **major** version so it’s v1.0.0.

```
npm version major
```


Push the changes and the tag.

```
git push origin master --follow-tags
```


The tagged version now shows up on the GitHub repository.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627410226920/8UTw46oFRe.png)

### Breaking change

Now say I make a breaking change. I’m removing `addCat()` and replacing it with `addPet()`.

```
module.exports = {
  addPet(pet) {
    /* snip */
    return pet;
  },
};
```


Stage and commit.

```
git add index.js
git commit -m "Support all pets"
```


Bump version. The breaking change means we bump **major** again.

```
npm version major
```


Push.

```
git push origin master --follow-tags
```


### Feature addition and patches/bug fixes

The process for these types of changes is exactly the same as above, except you should replace `major` with…

* `minor` for feature additions

* `patch` for patches and bug fixes

These are the basics I use when tracking version on my projects. It’s a very popular way and I’d recommend it to anyone.