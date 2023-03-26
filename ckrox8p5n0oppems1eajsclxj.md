---
title: "My preferred development environment"
datePublished: Fri Mar 18 2016 22:31:00 GMT+0000 (Coordinated Universal Time)
cuid: ckrox8p5n0oppems1eajsclxj
slug: my-preferred-development-environment-d7d55d8af8f6
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1627409753666/UYxk4dEkt.jpeg
tags: programming, javascript, web-development

---


Every developer has their own preferred environment; from the hardware, to the operating system, IDE, and other tools. I’ll go into detail about what I like to use.

![Photo by [Farzad Nazifi](https://cdn.hashnode.com/res/hashnode/image/upload/v1627409745826/mMiHpkssq.html)](https://cdn-images-1.medium.com/max/8576/1*D3N9V5tcxLtdG-8TAz7UJg.jpeg)*Photo by [Farzad Nazifi](https://unsplash.com/euwars)*

I can’t live without at least two monitors or at least one very wide-screen display. It’s important to me to be able to view resource material, code, and what I’m coding all at the same time. This cuts back on the time it takes to switch windows and re-contextualize my brain to what I’m looking at.

The keyboard doesn’t matter too much as long as it is full-sized with the Insert/Delete group, arrow keys, and full number pad. The mouse matters even less but I do prefer to have dedicated buttons to go back and forward through folders and web pages.

![[Logitech MK710](https://cdn.hashnode.com/res/hashnode/image/upload/v1627409747174/OPwGAQ5sB.html)](https://cdn-images-1.medium.com/max/2000/1*gFm8aiAS3JyEx4CEdnXnbw.jpeg)*[Logitech MK710](https://secure.logitech.com/en-us)*

I can and have worked in [Mac](https://www.apple.com/osx/) and Linux (like [Ubuntu](http://www.ubuntu.com/desktop)) environments, but I am very familiar with [Windows](https://www.microsoft.com/en-us/windows/features), so I prefer it. However, most of the tools I use are available on all platforms. Having other systems around to test projects as I go along is a nice-to-have option, but not necessary as, with web development, the browser’s rendering engine matters more than the operating system.

I have used so many different editors and IDEs over the years. I started on the Windows built-in Notepad way back in the day. Since then I’ve had experience with [Visual Studio](https://www.visualstudio.com/en-us/visual-studio-homepage-vs.aspx), [Eclipse](https://eclipse.org/), [Notepad++](https://notepad-plus-plus.org/), [Vim](http://www.vim.org/), [VS Code](https://code.visualstudio.com/), [Atom](https://atom.io/), and [Sublime Text](https://www.sublimetext.com/). Once I started using Sublime Text, I fell in love. It is simple and extensible. Something about it just flows with how I work.

![Sublime Text](https://cdn.hashnode.com/res/hashnode/image/upload/v1627409748642/vNI3sR28u.png)*Sublime Text*

I couldn’t talk about Sublime Text without sharing the packages I use with it. The most important is [Package Control](https://packagecontrol.io/), which provides a system to easily install and remove other packages.

Next is [Emmet](http://emmet.io/), which allows you to type shortcuts in HTML and then “expand” them into full code. For example, you can type…

```
table>(thead>tr>th*3)+(tbody>tr>td*3)
```


and press Tab. Emmet will automatically “expand” the code to…

```
<table>
  <thead>
    <tr>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td></td>
      <td></td>
      <td></td>
    </tr>
  </tbody>
</table>
```


Next I have [SublimeLinter](http://www.sublimelinter.com/en/latest/) with [SublimeLinter-jscs](https://packagecontrol.io/packages/SublimeLinter-jscs) and [SublimeLinter-jshint](https://packagecontrol.io/packages/SublimeLinter-jshint). These are indispensable tools for constantly checking your code for style consistency and run-time errors. I have set up a list of style and linting rules to follow to make sure my code stay consistent and syntax error-free.

I also have language bundles for various languages that I use. These equip Sublime Text with the understanding of different languages. Such as how they should be highlighted and how indentations should be made.

Also in my environment, I like to include various browsers. I prefer to work and test in [Chrome](https://www.google.com/chrome/browser/desktop/), but I also keep [Firefox](https://www.mozilla.org/en-US/firefox/new/), [Internet Explorer](http://windows.microsoft.com/en-us/internet-explorer/download-ie), and [Edge](https://www.microsoft.com/en-us/windows/microsoft-edge) close by. I tend to stick to writing well-supported and simple code so that I don’t have to worry about client inconsistencies. However, I still like to do sanity checks every once in a while since there still can be significant difference across browsers.

![Icons for Chrome, FIrefox, Internet Explorer, Opera, and Safari](https://cdn.hashnode.com/res/hashnode/image/upload/v1627409750383/_dqDvB_BP.jpeg)*Icons for Chrome, FIrefox, Internet Explorer, Opera, and Safari*

I like to use [npm](https://www.npmjs.com/) to module-ize and describe distinct development projects. I use npm to integrate testing frameworks like [Mocha](https://mochajs.org/) and [Chai](http://chaijs.com/). If a project gets large, I like to use [Gulp](http://gulpjs.com/) (or, to a lesser extent [Grunt](http://gruntjs.com/)) to run tasks such as concatenating & minifying CSS & JS, running tests, checking code coverage and deploying to continuous integration systems.

Finally, I like to use a source management system such as [git](https://cdn.hashnode.com/res/hashnode/image/upload/v1627409751740/OQz9VlWhb.html). I’ve used [Subversion](https://subversion.apache.org/) in the past but git just works so well for managing source code. It makes tracking changes and collaborating with others quick and efficient. Simply put, you check out code, make modifications, and check it back in. This lets you manage different versions of the same project and lets you branch out or roll back specific pieces of code.

![Branching illustration from [git-scm.com](https://git-scm.com/)](https://cdn-images-1.medium.com/max/2000/1*_ZRn2tF03XoxEaVIdfgjVw@2x.png)*Branching illustration from [git-scm.com](https://git-scm.com/)*

While all of the above is my preferred development environment, the most important thing for me is to be able to adapt and use the best tools for the job or the tools that are already being used in the project.