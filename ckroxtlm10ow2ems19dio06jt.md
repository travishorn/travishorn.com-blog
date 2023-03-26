---
title: "Need part of a string? Just use slice()"
datePublished: Thu May 23 2019 12:01:02 GMT+0000 (Coordinated Universal Time)
cuid: ckroxtlm10ow2ems19dio06jt
slug: need-part-of-a-string-just-use-slice-3636f2325791
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1627409527600/Fy65Rxqsc.png
tags: programming, javascript, web-development

---


In JavaScript, there are three prototype methods for getting part of a string. Which one should you use? The answer may depend on the context of your problem, but in the interest of keeping things simple, I say just use .slice().

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627409524058/I7uFRwo3Z.png)

### How does slice() work?

`[String.prototype.slice()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/slice) takes two parameters.

1. `beginIndex` — The index of the first character to return

1. `endIndex` — The index of the last character to return

```
'The quick brown fox'.slice(4, 9);
// Returns 'quick'.
```


It may be helpful to think of an “index” as the spaces between characters, starting at zero.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627409526013/xaPcwfpog.png)

`endIndex` is optional. If you leave it out, `.slice()` will simply return characters all the way to the end of the string.

```
'The quick brown fox'.slice(4);
// Return 'quick brown fox'.
```


### I like using LEFT() and RIGHT() in other languages

Instead of writing `LEFT('The quick brown fox', 3)`, you can write…

```
'The quick brown fox'.slice(0, 3);
```


Instead of writing `RIGHT('The quick brown fox', 3);`, you can write…

```
'The quick brown fox'.slice(-3);
```


This works because when `startIndex` is negative, `.slice()` starts from the end of the string and works backwards.

### Why not substring()?

`[String.prototype.substring()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/substring) is very similar to `.slice()`. There are two main differences.

If either parameter in `.substring()` is negative, it’s treated as zero.

```
'The quick brown fox'.substring(-3);
// Returns 'The quick brown fox'.
```


If `startIndex` is greater than `endIndex`, `.substring()` will swap the two parameters. This is different than `.slice()` which will simply return an empty string.

```
'The quick brown fox'.substring(9, 4);
// Returns 'quick'.

'The quick brown fox'.slice(9, 4);
// Returns '';
```


So why not use `.substring()` instead? Well, simply because you have to choose one. And `String.prototype.slice()` has a parallel method for arrays called `[Array.prototype.slice()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/slice). Learning and using `.slice()` means you can use it for two different datatypes.

### I thought you said there were three methods?

There are. The last one is `[String.prototype.substr()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/substr). It also takes two parameters. The first parameter is the usual start character index. But the second parameter is the length to extract.

```
'The quick brown fox'.substr(4, 5);
// Returns 'quick'.
```


I suggest staying away from this method. Not only does it not have a parallel array method, it is not part of the core JavaScript language and may be removed in the future. It is considered a legacy function.

### My suggestion

Keep it simple. Learn and use `.slice()` for all your string and array slicing needs.