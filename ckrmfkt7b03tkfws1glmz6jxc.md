---
title: "My first competition as a hackathon noob"
datePublished: Sun Oct 28 2018 23:48:34 GMT+0000 (Coordinated Universal Time)
cuid: ckrmfkt7b03tkfws1glmz6jxc
slug: my-first-competition-as-a-hackathon-noob-a9f443a65819
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1627410353941/6aLDH-s_0.png
tags: hackathon, web-development, nodejs

---


I recently competed in Node Knockout.
> A 48-hour hackathon featuring [Node.js](http://nodejs.org/). It’s an online, virtual competition with contestants worldwide.

I can’t quite remember where I heard about it. I’m sure it was from a weekly newsletter or from someone on Twitter. This post documents how the whole process went for me. It may be interesting or helpful to others interested in an event like this.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627410341968/j05NEhZOf.png)

### Reading up

Before I decided to join, I visited the website and read up on the rules. I found these to be the most important:

1. Node.js: All applications must be built using Node.js

1. Timeline: You have 48 hours to develop the web app

1. Teams: Each competing team has between 1 and 4 members.

1. Source Control: Node Knockout provides a private Git repository for each team. You should push often to show progress.

1. Deployment: Teams are given 500 free dyno hours on [Heroku](https://heroku.com) to deploy their application

1. Libraries: Public and freely available libraries are allowed and encouraged. Paying for something not available to everyone else is not allowed.

1. Scoring: Winners are picked by votes from judges, contestants, and the public.

There are five awards. Each for a different category (Judges favorite, design, etc). Each award includes prizes from sponsors such as gift cards and free products and services.

### Research and preparation

When I decided to join, the competition was 20 days away. The rules state that teams should feel free to work on the concept of the application before the competition starts. I got to work right away researching what more experienced hackathoners had to say about competing.

I did a bunch of searching on how to prepare and compete. I kept notes throughout my research.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627410344517/xGu_WYuply.png)

Some of the things I wrote down included technical details such as important dates & times for the competition. I also listed obligations I already had on my calendar around the time of the competition. I wrote a few lines about…

* My goals

* Choosing the project

* Preparing

* Timelines

My main goal was just to participate and create *something*. I wasn’t going for any awards. I knew my chances of that were very slim, being that I’m a team of one and it’s my first competition.

I decided to make a mind map to help decide what kind of project to build. Some of the things I included on the map were…

* Technologies

* Frameworks/libraries

* Concepts I was interested in

I kept writing related concepts to each of the above until I found something I thought would be fun to build: a tool to create visualizations from raw data.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627410347504/Anr0tt2Oc.png)

I wrote out a wire-frame model of how the finished product might look. I also wrote some of the things I wanted to make sure the app included and how it might work.

Once that work was done, all I had to do was dream as the competition got closer.

### The hackathon starts

The start time was at 7PM in my timezone.

On the day-of, a few hours before start time, I received an email from Node Knockout notifying me of the private repository they set up on [GitHub](https://github.com/) for me. Shortly after, I authenticated with my GitHub account and cloned the empty repo to my local computer.

When the competition started, I opened up a terminal and set up a new project with [Vue CLI](https://cli.vuejs.org/). The CLI allowed me to merge the incoming boilerplate with the existing blank repository.

For the next hour, I removed a lot of Vue’s starter template files and components and replaced them with a blank slate for my project. I called my project **CSV Insight**. I also added a “vote for me” link to the bottom of the web app.

### Getting to deployment

One of the tips I learned was to push early and often. I spent another hour getting set up on Heroku and writing code for building and deploying. When I was finished, I called it version 0.2.0 and pushed to GitHub. I enabled automatic deploys on Heroku so it would pick up pushes and deploy new versions automatically.

The first version of my app was live at [https://csvinsight.herokuapp.com/](https://csvinsight.herokuapp.com/). I went back to my team page on Node Knockout and entered the URL so judges and others would know where to find it.

I still had some time before I needed sleep so started building the framework for the multiple components of my app for the next hour and a half. It was getting late so I committed the code, tagged it as version 0.3.0, pushed to GitHub, checked the Heroku deployment and went to bed.

### Day two

I woke up the next morning and got back to it. I coded a way to select a CSV file and have it loaded into a [Vuex](https://vuex.vuejs.org/) store. I also figured out a way to parse the headers and use the parsed list to set axes on a chart. That took about a half hour and was a big enough change that I tagged it 0.4.0 and pushed/deployed again.

Around this time, I was ready to start building column chart. I added [D3js](https://d3js.org/) and got started. It quickly became clear that there are so many different things to worry about when parsing CSV data. The [Papa Parse](https://www.papaparse.com/) library makes it so much easier so I added it, too.

I got the axis-plotting working and it had been about an hour since I pushed a new version. The timing seemed right so I tagged the code 0.5.0 and pushed.

### Minimum viable product

I kept working. A half hour later I had bars on the chart which changed dynamically when new axes were chosen.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627410350044/0hAL5NS4p.gif)

I called this version 0.6.0 and pushed to deploy. At this point, there were 31 hours left in the competition. Even though it was only around noon, I decided to stop for the day because I was happy with where the project was and I had other obligations. I quickly wrote up a `TODO.md` file with a list of things I knew I wanted to accomplish.

Towards the end of the day, I had a little bit more time so I reopened the project and looked at the “to do” list. Without a list like that, it takes much more time to get “back into it.” I really recommend maintaining a TODO file between coding sessions.

I fixed a few minor syntax errors, adding some more styling, added a license (I went with [the MIT license](https://opensource.org/licenses/MIT)), and fleshed out the README. I used PurpleBooth’s README-Template.md as the basic outline for my README. It makes it easy to remember what things are useful in a file like this. Things such as:

* A description

* Installation guide

* Deployment instructions

* Licensing

Once that was done I headed to bed for the night.

### Releasing version 1.0.0

The next morning, with about 10 hours to go in the competition, I got coding again. I spent an hour adjusting the wording and design of the app. I’m not a talented graphic designer, so I just stuck with most of [Bootstrap](https://getbootstrap.com/)’s default styles, but changed the colors a little bit.

I then did some playing around with different data sets. With no huge issues, I was happy with the app. I deemed the codebase release material and tagged it version 1.0.0. I pushed the repository, watch Heroku build it, and visited the URL. All good!

I figured it might be nice to show what the web app looks like to anyone reading about it, so I took a screenshot and added it to the top of the README.

### Getting help with an HTTPS issue

Around this time, I noticed a problem with my application URL on Node Knockout. The Heroku URL worked, the Node Knockout URL displayed under **Submit Entry** worked, but the big application URL at the top of the **Team Dashboard** did not work.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627410352038/On78g_DBo.png)

When visiting the Node Knockout application URL, I was getting HTTPS errors. Visiting the URL with regular HTTP was fine, though.

I sought help and found that the Node Knockout website had a link to their [Slack](https://slack.com/) channel in the footer. I joined the channel and described the issue. Even including the screenshot above.

Within minutes, I had a few people offering possible solutions and saying they’ll look into it. Including people from Heroku and a Node Knockout organizer. It turns out it wasn’t anything wrong with *my* configuration, but rather Node Knockout’s. The proposed solution was just to use HTTP.

Just before the end of the coding session, I submitted my entry, which consisted of answering a bunch of questions about my team and my app, including instructions to judges on how to use it.

### Judging

By the time the coding portion had ended, I had made 50 commits in 48 hours. My app was at version 1.0.2 and ready to be voted on and judged.

Voting took place over the next 7 days. Votes came in from participants, expert judges, and the general public. I took it upon myself as a participant to look at some of the other projects and give votes and ratings where I thought they were due. I was happy to see that others did the same for me.

### Winners announcement

After 7 days were up, scores were calculated and winners were announced. I wasn’t one of the winners, but as I said earlier, I wasn’t expecting to win anything as I was a single-person team and it was my first hackathon. My goal was just to participate and make something I liked. I did succeed in that aspect.

I really enjoyed the experience and I think I’ll try to get involved with more hackathons and competitions in the future.

Below you’ll find a link to the entry page for my team/app.
[**CSV Insight - Node Knockout**
*Node Knockout is a 48-hour hackathon featuring Node.js. It's an online, virtual competition with contestants worldwide.*www.nodeknockout.com](https://www.nodeknockout.com/entries/79-fixed-measure)