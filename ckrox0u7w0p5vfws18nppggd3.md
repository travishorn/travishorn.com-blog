---
title: "Link Directly to Google’s “I’m Feeling Lucky” Feature"
datePublished: Mon Aug 01 2016 22:01:02 GMT+0000 (Coordinated Universal Time)
cuid: ckrox0u7w0p5vfws18nppggd3
slug: link-directly-to-googles-i-m-feeling-lucky-feature-65ebe7b552bd
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1627409799037/cnftiP-F9.jpeg
tags: web-development, google

---


This post is part of a project to move my old reference material to my blog. Before 2012, when I accessed the same pieces of code or general information multiple times, I would write a quick HTML page for my own reference and put it on a personal site. Later, I published these pages online. Some of the pages still get used and now I want to make them available on my blog.

![Photo by [João Silas](https://cdn.hashnode.com/res/hashnode/image/upload/v1627409797260/Cxs5TAWw6.html)](https://cdn-images-1.medium.com/max/10368/1*wryHsxwdnC2_-0eC0zkHzA.jpeg)*Photo by [João Silas](https://unsplash.com/@joaosilas)*

Creating a link that uses the **I’m Feeling Lucky** feature enables you to link people directly to a page that can help them. In other words, you can simulate doing a Google search for the person and returning the top result.

### An Example

Say I want to show someone how to enable JavaScript in their browser but I don’t know which browser they are using. The long way would be to list out instructions for the most popular browsers. A quicker way would be to ask the person which browser they are using (through a text input), do a Google search and take them to the site listed as the top result.

### URL

The URL structure is:

```
http://www.google.com/search?q=term&btnI
```


The most important part of the URL is **&btnI**. If you omit it, Google just returns a regular results page. With **&btnI** appended to the end, Google takes you directly to the top result for the search term (which is, in this case, “term”).

Please note that the last letter is a capital “i”.

### Using the Example Above

You can ask users which browser they are using in a text input. If the user inputs “firefox” and submits the form, you can have your app/page redirect to:

```
http://www.google.com/search?q=enable+javascript+firefox&btnI
```


Similarly, if the user entered “Internet Explorer”, your form can redirect them to:

```
http://www.google.com/search?q=enable+javascript+internet+explorer&btnI
```


Demo: [Enable JavaScript in Internet Explorer](http://www.google.com/search?q=enable+javascript+internet+explorer&btnI)

### Other Advantages

Using this method will always harness the power of using Google to decide the most relevant page. If Microsoft changes the way you enable JavaScript, you don’t have to keep your users updated or change the web form you created; Google will still return the top result as if the user searched for “enable JavaScript Internet Explorer.”

### Caveat

This seems to work for some queries, but not all. While, all of the examples in this post have worked so far, here’s an example of one that **doesn’t** work:

```
http://www.google.com/search?q=github+foo+bar+foobar&btnI
```


Instead of redirecting you to the first search result, the regular Google results are shown. I’m not sure why this is. Google doesn’t seem to have any documentation to support this API in the first place.

If you are running into this issue, [Lorenzo Alali](https://medium.com/@lorenzoalali) pointed me to [this topic on the Alfred Forums](https://www.alfredforum.com/topic/5927-force-im-feeling-lucky/?do=findComment&comment=36422) where a workaround is discussed. [DuckDuckGo](https://duckduckgo.com) (a competing search engine) provides the same functionality as Google’s “I’m Feeling Lucky” button. They call it “I’m Feeling Ducky”. Here’s how you would use it:

```
https://duckduckgo.com/?q=!ducky+github+foo+bar+foobar
```


The difference between this approach and the Google approach is that:

1. You are harnessing the power of DuckDuckGo’s search instead of Google

1. It seems to work in cases where Google fails