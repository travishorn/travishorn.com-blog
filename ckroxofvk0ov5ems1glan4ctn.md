---
title: "What if a project uses tabs and you use spaces?"
datePublished: Thu Dec 06 2018 11:01:00 GMT+0000 (Coordinated Universal Time)
cuid: ckroxofvk0ov5ems1glan4ctn
slug: what-if-a-project-uses-tabs-and-you-use-spaces-fdb236afdf82
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1627409601213/9OZYd2lTLy.png
tags: code, programming, web-development, vscode

---


There are two types of developers. Those who use tabs to indent code and those who use spaces. It seems like a minor difference that shouldn’t matter at all. But start this conversation with any group of developers and you will quickly see how intense the argument can get. We fight as if this is our chosen ideology at the heart of our being.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627409598211/XBzLdN_N9.png)

And so, a perfectly reasonable interview question might ask “What if a project uses tabs and you use spaces?” If you use tabs, go ahead and switch the question around.

To the uninitiated, the answer is simple: “I would just use tabs.”

But seasoned developers know this isn’t a viable solution. Once you are stuck in your ways, and you have convinced yourself your way is the best, it’s not so easy to switch. Consider also, that you may be working on multiple projects where some of them use tabs and some use spaces.

The solution — and a great answer to this interview question — is to configure your editor to insert tabs (or whichever method has been agreed-upon in the project).

### Aside: The “space bar” spaces developer

Within the “spaces are better” camp, there are another two sub-types of developers. Those who use the Tab key to insert their chosen number of spaces, and those who actually tap the space bar until they get the indentation they like.

Unfortunately, for this second sub-type of developer there isn’t a perfect solution out there. It’s hard for code editors to determine when you are indenting, and automatically switch your spaces to tabs. You may need to train yourself to hit the tab key once, instead of the space bar two or four times.

### Configuring your editor

The choice of code editor or IDE is another personal choice developers argue over. However, most of them should allow you to configure your indentation type.

I use Visual Studio Code. In this editor, you can simply…

1. Open the project folder with **Ctrl+K**, **Ctrl+O**

1. Bring up the command pallet with **Ctrl+Shift+P**

1. Type “workspace settings” and press **Enter**

1. Under **Editor: Insert Spaces**, uncheck the checkbox (or check, if using spaces)

You can also set the tab site under **Editor: Tab Size**. Most projects will use 2 or 4.

With these settings in place, any file you open in this folder will automatically use tabs, even if you’ve previously configured your editor to insert spaces when pressing Tab.

Other files in other folders will continue to use spaces just as you like.

### Consider using Prettier

[Prettier](https://prettier.io/) is an opinionated code formatter. Prettier doesn’t care how you write your JavaScript. It simply comes in after you’re done and formats all your code in a standard way. Nobody likes what Prettier does to their own code, but everyone likes what it does to everyone else’s code.

If your team decides to use Prettier, you can be sure that each source code file will use the same formatting standards.

You can [install it as a CLI](https://prettier.io/docs/en/cli.html), [use the API](https://prettier.io/docs/en/api.html) to format code during builds, or [integrate it into your editor](https://prettier.io/docs/en/editors.html).