---
title: "Ordered Lists in HTML"
datePublished: Tue Jun 20 2017 22:31:01 GMT+0000 (Coordinated Universal Time)
cuid: ckrmevrlo03kaems10a6mdejr
slug: ordered-lists-in-html-a4621e17532b
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1627410497004/izSCb3s4a_.jpeg
tags: css, web-development, html

---


One of my highest rated answers on Stack Overflow is in response to a very simple question.
> Is it possible to specify a starting number for an ordered list?

In this post, I’ll do a deep dive into the standard way to represent ordered lists in HTML: the `&lt;ol&gt;` element. Along the way, I’ll answer the question above.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627410494910/-1focvMr-W.jpeg)

First of all, if you’re only reading this to find out how to specify a starting number for an ordered list, and you don’t want to wade through the rest of the post, the simplest method is to use the `start` attribute. Example: `&lt;ol start="5"&gt;...&lt;/ol&gt;`

With that out of the way…

### What is the &lt;ol&gt; element?

According to the [Mozilla Developer Network](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/ol)…
> The **HTML `&lt;ol&gt;` element **represents an ordered list of items, typically rendered as a numbered list.

In your web browser, it’s typically rendered to look like this:

1. First item

1. Second item

1. Third item

1. and so on…

Of course — just as with almost all HTML elements — you can style it with CSS.


### When to use &lt;ol&gt;

To represent a list of items in writing, you have a few options:

* In a sentence, separated by commas

* In a vertical list with bullet points (like the one you’re reading now)

* In a numbered (or otherwise ordered) list

If there are only a few items, you might just write them out directly in a sentence.
> My cat’s names are Fluffy, Smokey, and Tiger.

If there are many items, but the order does not matter, you could use a bulleted list.
> **Native Trees to Missouri**
- Dogwood
- Pea/Bean
- Willow
- Elm
- Olive
- Birch
- Rose
- Beech
- Pine
- Maple
- Walnut

However, if the items in your list follow a specific order, you’d can best represent this to the reader as an ordered list. This is where the `&lt;ol&gt;` element comes in. Ordered lists are typically numbered, but can also be organized with Roman numerals or alphabetically ordered letters. Ordered lists are good for things like a series of steps to guide the reader or a listing of competitors where you want to display how they placed in a competition.

A good test to determine whether your list should be ordered or not is to take a random item in the list and place it somewhere else in the list. Does the list still make sense? You should use an unordered list `&lt;ul&gt;`. Does the list no longer make sense? You should use an ordered list `&lt;ol&gt;`.

**Ordered list example 1: How to do the Hokey Pokey**

1. You put your right foot in

1. You take your right foot out

1. You put your right foot in

1. Shake it all about

**Ordered list example 2: Top Countries by Population**

1. China

1. India

1. United States

1. Indonesia

### How to use &lt;ol&gt;

As far as HTML elements go, `&lt;ol&gt;` is pretty simple. The only element allowed inside of it is `&lt;li&gt;` (list item). And the only elements allowed inside of `&lt;li&gt;` are nested `&lt;ol&gt;` and `&lt;ul&gt;` (unordered list).

```
<ol>
  <li>First item</li>
  <li>Second item</li>
  <li>Third item</li>
</ol>
```


There is no need to type out the number next to each list item. The browser will automatically add numbers in ascending order.

For nested lists, follow this format:

```
<ol>
  <li> First item
    <ol>
      <li>First sub-item</li>
      <li>Second sub-item</li>
      <li>Third sub-item</li>
    </ol>
  </li>
  <li>Second item</li>
  <li>Third item</li>
</ol>
```


### ARIA

According to the ARIA spec, `&lt;ol&gt;` may have [descriptive roles](https://www.w3.org/TR/wai-aria/roles). Here are the roles and when you would use them.

* directory —references to members of a group, such as a table of contents

* group —contains interface objects with are not intended to be included in a page summary or table of contents

* listbox — allows the user to select one or more items from list of choices

* menu — offers choices to the user. Often a list of actions or functions the user can invoke

* menubar — is part of a menu bar similar to those found in Windows, Mac, and Gnome desktop applications

* radiogroup —references a group of radio buttons (where only a single entry can be checked at any one time)

* tablist — references a series [tabpanel](https://www.w3.org/TR/wai-aria/roles#tabpanel) elements

* toolbar — a collections of commonly used function buttons or controls represented in compact visual form

* tree — may contain sub-level nested groups that can be collapsed and expanded

* presentation — used to change the look of the page but does not have all the functional, interactive, or structural relevance implied by the element type

### &lt;ol&gt;’s attributes

In addition to the [global HTML attributes](https://developer.mozilla.org/en-US/docs/Web/HTML/Global_attributes), `&lt;ol&gt;` also supports the following attributes.

**Compact**

I would stay away from the `compact` attribute. It is deprecated, its behavior isn’t standard, depends on the user agent, and doesn’t work in all browsers. In the past, it was used to render the list in a compact style. Nowadays, all styling can be done using CSS. To give an effect similar to the `compact`attribute, try setting `line-height: 80%` in CSS.

**Reversed**

A new attribute introduced in HTML5 is the `reversed` attribute. You can use it to indicate that the items are listed in reverse order. In the browser, the list will be displayed with numbers in descending order. If your list, for example, contains ten items, the first item will be labeled 10, the next 9, the next 8, and so on. To use this attribute, use `&lt;ol reversed&gt;` or `&lt;ol reversed="reversed"&gt;`.

Warning: This attribute works in Chrome, Firefox, and Safari, but **does not work** in Edge, Internet Explorer, or Opera.

**Start**

This attribute is the catalyst for this post. It was deprecated in HTML4 and then reintroduced in HTML5. It specifies the start value for numbering the individual list items. To use the `start` attribute, set the value to the number you want to start with.

```
<ol start="5">
  <li>Will be labeled 5</li>
  <li>Will be labeled 6</li>
  <li>And so on...</li>
</ol>
```


Note that even if you are representing your ordered list using letters (a, b, c) or Roman numerals (i, ii, iii), you will still specify the `start` attribute with a number. For example, the following list will start with the letter “C”.

```
<style>
  ol { list-style-type: upper-alpha; }
</style>

<ol start="3">
  <li>Will be labeled as C</li>
  <li>Will be labeled as D</li>
  <li>And so on...</li>
</ol>
```


**Type**

Another attribute you typically want to stay away from is `type`. However, it does have its purpose. You would only use this attribute if the value of the list number matters, such as in legal or technical documents where items are to be referenced by their number/letter. The `type` attribute indicates the numbering type.

* `a` indicates lowercase letters

* `A` indicated uppercase letters

* `i` indicates lowercase Roman numerals

* `I` indicates uppercase Roman numerals

* `1` indicates numbers (default)

If you want this functionality, but items aren’t referenced by their number/letter elsewhere in your document, opt **not** to use the `type` attribute, but instead use the `list-style-type` CSS property, which I’ll cover below.

### Styling &lt;ol&gt;

You can style an ordered list using CSS just as you would with any other element. For example, you can set `color` or `font-family`, etc.

I want to bring special attention to the `list-style-type` property. It gives you the ability to change the item’s marker (number, letter, etc). Here are some hand-picked values that you may want to use:

* decimal (default) — Decimal numbers (1, 2, 3…)

* decimal-leading-zero — Decimal numbers padded by initial zeros (01, 02, 03…)

* lower-roman and upper-roman — Lowercase and uppercase Roman numerals (i, ii, iii… and I, II, III…)

* lower-greek — Classical greek alpha, beta, gamma… (α, β, γ…)

* lower-alpha and upper-alpha — Lowercase and uppercase ASCII letters (a, b, c… and A, B, C…)

* none — No item marker is shown. The list can still semantically have an order to it without numbered markers being displayed visually to the user.

### Using counter-increment instead of start

One final thing I want to point out comes from an [answer by Adam Grant on Stack Overflow](https://stackoverflow.com/a/30109692/307338).

`&lt;ol&gt;`'s `start` attribute has a shortcoming. If you start a list, then stop to interject some other content, then begin the list again, you **could** use `&lt;ol start="..."&gt;` to continue numbering where you left off. But what happens when you add a new item to the first part of the list? You **must** update the `start` attribute of the second `&lt;ol&gt;` to keep it consistent. This situation is not ideal. Instead, you can use the CSS properties `counter-increment` and `counter-reset`, which will keep the number consistent automatically.

First, remove the numbered markers from both lists:

```
ol { list-style-type: none; }
```


Then, show a counter before **each `&lt;li&gt;` element**:

```
ol li:before {
  counter-increment: someCounterName;
  content: counter(someCounterName) ". ";
}
```


Finally, make the counter reset on the first `&lt;ol&gt;` only:

```
ol:first-of-type { counter-reset: someCounterName; }
```


With this solution, the numbers stay consistent in both lists automatically, no matter how many items you add to either list.


I may continue writing posts that go further in depth regarding my highest voted or viewed Stack Overflow answers. If people get some benefit from seeing those short answers, making posts like these may help even further.