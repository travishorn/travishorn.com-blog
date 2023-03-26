---
title: "Web accessibility"
datePublished: Thu Aug 14 2014 22:44:54 GMT+0000 (Coordinated Universal Time)
cuid: ckrni5gna0cedfws1gasrdqqa
slug: web-accessibility-9ce6af95fbe4
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1627409902804/BaiGzqW3y.png
tags: web-design, accessibility, web-accessibility

---


Consider users accessing your site from platforms other than the major browsers: mobile devices, screen-readers, and crawlers, for example.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627409901462/sjxA_IeAw.png)

When you write with accessibility in mind, your code is written more [semantically](http://boagworld.com/dev/semantic-code-what-why-how/) and, therefor, more logical and clear. The code will be easier to maintain this way, as well.
> *HTML was originally intended as a means of describing the content of a document, not as a means to make it appear visually pleasing. Semantic code returns to this original concept and encourages web designers to write code that describes the content rather than how that content should look.*

Some accessibility information:

* [Section508](http://www.section508.gov/)

* [WAI](http://www.w3.org/WAI/)

* [WAI-ARIA](http://www.w3.org/WAI/intro/aria)

* [WCAG 2](http://www.w3.org/TR/WCAG20/)

* [MobiForge](http://mobiforge.com/) (for mobile development)

Federal agencies are legally required to make their sites accessible to people with disabilities according to [Section 508](http://www.section508.gov/) of the Rehabilitation Act of 1973.

For example, consider the following element.

```
<img src="img/graph.png">
```


The linked image may appear as a beautiful graph as long as it’s being viewed by a human without vision issues, using a standard browser. However, to a crawler or a blind person, it means nothing. Instead, the above element can be re-written like so:

```
<img src="img/graph.png" alt="Graph showing an average increase of $1K in sales every year for the past decade.">
```


The alt text describes the image for a user who otherwise cannot view it.

At the very least, you should be using [ARIA attributes](http://www.w3.org/WAI/PF/aria/states_and_properties#state_prop_def) such as aria-labelledby to show relation between a label and the object it labels, and aria-hidden to indicate that an element is not visible to any user.

## Crawlers & robots.txt

Your site needs to be accessible to crawlers so that it can be effeciently indexed by search engines and available to users. It’s also important to understand how [robots.txt](http://en.wikipedia.org/wiki/Robots_exclusion_standard) and search engine crawlers work.
> *The Robot Exclusion Standard, also known as the Robots Exclusion Protocol or robots.txt protocol, is a convention to prevent cooperating [web crawlers](http://en.wikipedia.org/wiki/Web_crawler) and other [web robots](http://en.wikipedia.org/wiki/Web_robot) from accessing all or part of a website which is otherwise publicly viewable. Robots are often used by search engines to categorize and archive web sites, or by webmasters to proofread source code. The standard is different from, but can be used in conjunction with, [Sitemaps](http://en.wikipedia.org/wiki/Sitemaps), a robot inclusion standard for websites.*

Know that not all crawlers and robots follow robots.txt. Some are programmed to misbehave and will ignore or actively disobey robots.txt.

*Accessbility icon created by [Visual Pharm](http://icons8.com/).*

*Originally published on my old blog on August 14, 2014.*