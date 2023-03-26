---
title: "HTML5: New Features"
datePublished: Mon Aug 04 2014 19:29:56 GMT+0000 (Coordinated Universal Time)
cuid: ckrmetho903nmfws1as8ra3i5
slug: html5-new-features-7cb89f0d09ea
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1627410515500/V4ocgEco8.png
tags: web-development, html, html5

---


The HTML5 specification is still in working draft, but as web development is currently shifting towards an “agile” process, browsers already implement many of these features, with more support coming everyday. Please note that this document does not cover 100% of the new elements and attributes in the new specification, but I do want to highlight some interesting parts.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627410514183/AVpz_Srk8.png)

### Structure Elements

These elements are meant to give more semantic meaning to the structure of a web page. Rather than using &lt;div&gt;s with classes, the most commonly used components of a page’s structure have been given their own tag.

section

Represents a generic document or application section. Use this versus the div element when your document has clear “sections” within it.

```
<section>High School</section>
<section>Middle School</section>
<section>Elementary School</section>
```


article

Represents an independent piece of content of a document. This tag could be used to define a full blog post or other written piece.

```
<article>
  High school is a term used in parts of the English-speaking world to describe institutions which provide all or part of secondary education, but not always the highest years of basic education, which can be called a "secondary school" or "secondary college" or other terms, depending on the nation or region. The phrase "high school" is often incorporated into the name of the related institutions.
</article>
```


Represents a piece of content that is only slightly related to the rest of the page. Asides have been a part of web design for a long time. Normally, you will see them as side-bars that link or have information to other content on the site.

```
<aside>For more information about this site, please visit the "About" section.</aside>
```


hgroup

A lot of times, the top of a page will contain consecutive headings to describe where the user is on the site. &lt;hgroup&gt; defines this collection of headings.

```
<hgroup>
  <h3>Schools</h3>
  <h4>Elementary Schools</h4>
</hgroup>
```


This one should be pretty obvious. The header of a page usually contains information about the site, including the site name, page title, and sometimes navigation.

```
<header>My Site</header>
```


Another obvious one. Many sites have footer information about the site or page near the bottom of the page. This information doesn’t necessarily relate to the content of the page, so it should be semantically sectioned off with &lt;footer&gt;.

```
<footer>Copyright 2012</footer>
```


In addition to headers, footer, articles, and asides, I can think of one other page element that consistently pops up on websites, the navigation. You can now define your navigation with the &lt;nav&gt; tag.

```
<nav>
  <ul>
    <li><a href="#">Home</a></li>
    <li><a href="#">About</a></li>
    <li><a href="#">Links</a></li>
  </ul>
</nav>
```


figure & figcaption

A figure in a written book would be something like a photo or a chart. These elements help to implement those ideas on the web. &lt;figure&gt; can be used to define a single unit of an article that, while possibly being referenced in the body text, could be taken away without damaging the text.

```
<figure>
  <img src="https://www.google.com/images/srpr/logo3w.png" alt="Google">
  <figcaption>Google's logo.</figcaption>
</figure>
```


There are many new form elements and input types in this latest version of HTML5. All of them aim to make more specific elements you would commonly find on web forms.

datalist

This element enables you to create a list that can then be used with an &lt;input&gt; to provide “autocomplete” functionality. Write your datalist and link it with &lt;input list=””&gt;. The browser will allow the user to write in his own answer and/or select from the datalist.

```
<datalist id="levels">
  <option value="High">
  <option value="Middle">
  <option value="Elementary">
</datalist>

<input list="levels">
```


keygen

An interesting element that’s been around since Netscape, but it now supported under the HTML5 specification. It will basically request the browser to generate a public key, which your server can use to create and sign a certificate to give back to the browser. With the certificate signed by your server, the user can access secure parts of your site where you’re requiring a valid certificate.

```
<keygen name="key">
```


Input Types

All the types below are new valid values for the &lt;input type=””&gt; attribute.

* tel: A telephone number

* search: Search terms

* url: A URL

* email: An email address

* datetime: A date and/or time

* number: A number

* range: A number within a specific range

* color: A hex value of a color

Code:

```
<label for="phoneNumber">Phone number:</label>
<input type="tel" id="phoneNumber"><br>

<label for="search">Search:</label>
<input type="search" id="search"><br>

<label for="webSite">Web site:</label>
<input type="url" id="webSite"><br>

<label for="emailAddress">Email address:</label>
<input type="email" id="emailAddress"><br>

<label for="dateOfBirth">Date of birth:</label>
<input type="datetime" id="dateOfBirth"><br>

<label for="age">Age:</label>
<input type="number" min="0" max="100" id="age"><br>

<label for="volume">Volume:</label>
<input type="range" min="0" max="100" step="10" id="volume"><br>

<label for="favoriteColor">Favorite color:</label>
<input type="color" id="favoriteColor">
```


### Other Elements

These other elements are not necessarily related to the structure of a document or a form.

video

This is a popular new element. Right now, Flash is the most common technology used to show audio and video on the web. The &lt;video&gt; tag attempts to change that, offering native support in the browser rather than a plugin like Flash.

```
<video src="video.ogv" controls></video>
```


audio

Similar to &lt;video&gt;, but with audio data only.

```
<audio src="audio.ogg" controls></audio>
```


track

Specifies external timed track information for the &lt;video&gt; element. This can be used for subtitles and captions.

```
<video src="video.ogv" controls>
  <track kind="subtitles" src="video.en.vtt" srclang="en" label="English">
</video>
```


progress

Used to indicate the status of a task. This will normally create a progress bar that can be manipulated through JavaScript.

```
<progress max="100" value="25">25%</progress>
```


meter

Similar to &lt;progress&gt;, but used to show measurement of something rather than progress.

```
<meter max="100" value="50">50 out of 100</meter>
```


canvas

Another popular new element. Renders dynamic bitmap images on the fly. This is useful for graphs and games.

```
<canvas id="rectangle"></canvas>

<script>
  var canvas = document.getElementById('rectangle');
  var context = canvas.getContext('2d');

  context.fillRect(0, 0, 50, 50);
</script>
```


The &lt;details&gt; element is an interesting feature that will show whatever is in the &lt;summary&gt; content, and then show everything the rest of the content when clicked. This could be useful for a number of purposes, including a FAQ-style page.

```
<details>
  <summary>Levels</summary>

  <ul>
    <li>High School</li>
    <li>Middle School</li>
    <li>Elementary School</li>
  </ul>
</details>
```


### Attributes

Input Attributes

* autofocus: Places the cursor in this input on page load

* placeholder: Text to be displayed in input is empty

* form: Defines what form the element belongs to

* required: Will prompt user to complete the input if blank

Code:

```
<label for="autoFocus">Will be autofocused:</label>
<input type="text" autofocus id="autoFocus"><br>

<label for="placeHolder">Has a placeholder:</label>
<input type="text" placeholder="See!" id="placeHolder"><br>

<label for="tiedToForm">Tied to a separate form:</label>
<input type="text" form="otherForm" id="tiedToForm">

<form id="otherForm" action="#"></form>

<label for="isRequired">Cannot be blank when submitted:</label>
<input type="text" required>
```


contenteditable

Add this attribute to any element to make it editable. I can see a lot of use coming out of this tag for web applications.

```
<div contenteditable>
  You can edit this div.
</div>
```


data-*

A long-overdue feature of HTML. You can assign custom attributes to any element, just by prefixing it with data-.

```
<ul>
  <li data-level="high">Hypertext High School</li>
  <li data-level="middle">Markup Middle</li>
  <li data-level="elementary">Element Elementary</li>
</ul>
```


draggable & dropzone

dropzone can be used to specify an area of a page the user can drop things into, either from their desktop or from any draggable element on the page. Use other attributes to tell dropzone what to do with dropped content.

```
<div draggable="true">I can be dragged</div>

<div dropzone="copy file:image/png file:image/gif file:image/jpeg" ondrop="receive(event, this)">
  <p>Drop an image here!</p>
</div>

<script>
  function receive(event, element) {
    alert('Dropped!');
  }
</script>
```


I wouldn’t use any of these features without having relevent fallbacks in place due to how new they are and how sporadic the support may be. However, many of these have workarounds and shivs to simulate support even if the browser doesn’t natively support them yet.

*This post first appeared on my old blog on May 4, 2012.*