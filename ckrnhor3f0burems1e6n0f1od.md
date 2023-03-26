---
title: "jQuery.expire"
datePublished: Fri Apr 27 2012 05:00:00 GMT+0000 (Coordinated Universal Time)
cuid: ckrnhor3f0burems1e6n0f1od
slug: jquery-expire-4ab0450a5553
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1627409967578/FEAM4zmyr.png
tags: jquery, javascript

---


The latest jQuery plugin I’ve developed is called expire. I intend it to be used as a simple way to set start and end times to pages. All that’s required is a start time, and end time, and an action to perform if the current time is outside that range. Obviously, the script depends on the client having JavaScript enabled, so I wouldn’t use it for any applications where it’s critical the user doesn’t access the content outside the time range. For that, I would rely on server-side scripting.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627409966383/z4J_-ovut.png)

One thing this plugin can do that server-side code can’t is being able to expire a page while the user is on it. For example, say you have a form that you want users to be able to complete, but you want to stop anyone from submitting after a certain deadline. With a server-side language, you can prevent the user from accessing the page and submitting once the deadline is passed, but you can’t prevent him from accessing the page before the deadline, and then leaving it open even after the deadline passes. You could stop him when he tries to submit, but not while he’s filling out the form.

One of the drawbacks I came across while developing this plugin was the current datetime being pulled from the client. This presents two problems:

1. All the user has to do is set their local machine’s time to be within your time range, and access will be granted again.

1. Time ranges are relevant to the user’s timezone, meaning the page will be expired for different users at different times globally.

To get around this, I provide an option for pulling the atomic time from [a time service](http://json-time.appspot.com/), and also providing an option to specify a global timezone. You can also just use the user’s local timezone, but time from the time service. The timezone will be pulled from [another service](http://ipinfodb.com/) based on the user’s IP address.

As always, this plugin is open-source. Feel free to [fork and modify](https://github.com/travishorn/jquery-expire). I’d also appreciate any help fixing the known bugs already in the code.

On a side-note, I’m excited for jQuery’s [new plugins site](http://blog.jquery.com/2011/12/08/what-is-happening-to-the-jquery-plugins-site/) that will be held together by git repositories and [CommonJS packages](http://wiki.commonjs.org/wiki/Packages/1.0). I’m including a package.json file in all my plugin repositories in anticipation for the site launch. However, the current package spec is living, so changes may come to those as the spec changes.