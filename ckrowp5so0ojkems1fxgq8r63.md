---
title: "What considerations do I make while building a web application or site?"
datePublished: Thu Mar 17 2016 22:31:01 GMT+0000 (Coordinated Universal Time)
cuid: ckrowp5so0ojkems1fxgq8r63
slug: what-considerations-do-i-make-while-building-a-web-application-or-site-76f374e9e608
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1627409853514/rKT7X8JF7.jpeg
tags: javascript, web-development

---


I will break this problem down into six categories:

* User interface

* Security

* Performance

* Search engine optimization (SEO)

* Maintainability

* Technology

![Photo by [Jay Wennington](https://cdn.hashnode.com/res/hashnode/image/upload/v1627409851548/bP405iitH.html)](https://cdn-images-1.medium.com/max/9792/1*JN5atqW0tbV9y0JYFtb32Q.jpeg)*Photo by [Jay Wennington](https://unsplash.com/jaywennington)*

### User interface

I like to rely on third-party frameworks for user interface to keep things simple and easy-to-use for the end user. Sometimes you can take risks with user interface but for the most part, users will have a better time navigating your site if you stick to the interface elements they are used to.

I think clean layouts help people get things done faster. I believe in stripping away all the junk on a page until the only the useful parts remain. Once this is done, you can add a bit of style that can give your site some character.

Keep in mind that many of today’s users will be using a smaller screen on a mobile device. Considerations should be made that allow your site to look good on all screens.

### Security

Security is hard. It is a constant race to stay ahead of malicious users. The biggest mistake you can make is to try to come up with your own scheme for security. Experts work day in and day out to develop secure systems and even those fail eventually.

I’ve always relied on test and proven systems and frameworks like [Passport](http://passportjs.org/) to handle the difficult task of security. There are many contributors to Passport and projects like it that work very hard to stay up-to-date with the latest security considerations.

Another major piece of securing a site is to use SSL/TLS. It is the best system the web has come up with for providing safe two-way communication with users. It is even easier to implement now with certificate providers like [Let’s Encrypt](https://letsencrypt.org/).

### Performance

It’s interesting that most desktops and laptops are now able to run light applications without issue, but the fastest growing market is mobile devices where those same applications will probably run much slower.

It starts with coding in an efficient manner and not including large frameworks you may not need. As your site grows, always keep in mind how additional code and features will affect performance.

Not everyone will have a blazing fast system to run your JavaScript-heavy web application. I like to use [PageSpeed Tools](https://developers.google.com/speed/pagespeed/) to analyze and optimize my sites following best practices for the web.

### Search engine optimization

There is a whole industry dedicated to this subject. For the developer, the easiest way to stay compliant is to code things semantically. Write code that describes what your content means, not how it is supposed to look or how it is supposed to behave.

If the goal is to lay out a page with a main content area and a sidebar, you may be tempted to use a table like so:

```
<table>
  <tbody>
    <tr>
      <td style="width: 75%;">Content</td>
      <td style="width: 25%;">Sidebar</td>
    </tr>
  </tbody>
</table>
```


While this *will* work, it is incorrect on a semantic level. Tables should only be used for tabular data. Think of the real purpose of a table: to show data in columns and rows with labels. Our use-case does not fit.

Instead, let’s use a semantic approach:

```
<style>
  content, aside { float: left; }
  content        { width: 75%; }
  aside          { width: 25%; }
</style>

...

<article>Content</article>
<aside>Sidebar</aside>
```


This is much better. Search engines will have a much easier time understanding your content when it is laid out in elements that describe it. You’ll also notice that the styles are separated, creating an even clearer concept of what is content, and what isn’t.

I’ve found that most SEO issues disappear when you code in this style because your code is optimized for search engines by default.

### Maintainability
> Always code as if the guy who ends up maintaining your code will be a violent psychopath who knows where you live.

— John F. Woods

I read this quote a long time ago and I think about it from time to time as I try to write code as maintainable as possible. In reality, you probably won’t need to worry about a violent psychopath trying to maintain your code, but it doesn’t hurt to code as if that’s the case. It just helps the project and everyone involved.

Some of the techniques I’ve used keep a project maintainable include…

* Writing small, clearly defined modules

* Succinctly commenting code where necessary

* Re-writing hard-to-understand code

* Using a versioning system like git

* Writing unit tests

* Writing documentation (even automatically using tools like [JSDoc](https://github.com/jsdoc3/jsdoc))

### Technology

Technology considerations for the developer usually have to do with what features are made available to the back-end application by the server and what features are made available to the front-end by the user’s browser.

Depending on what server technology and applications you have running on the back-end, your applications design might drastically change. An familiarity with the technology available to you is important.

It gets even tougher on the front-end. A user may be using any browser or device they choose. It is best to stick with widely supported features, which can be checked on the excellent site, [Can I use…](http://caniuse.com/). In some cases, polyfills may be available that will “fill in” the missing functionality in browsers that don’t support a certain feature. These can be reliable once thoroughly tested.

While there are many other areas for consideration when building an application or site, these few should get any developer up and running to make a user friendly, secure, performant, optimized, maintainable, and accessible site.