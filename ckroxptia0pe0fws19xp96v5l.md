---
title: "Rapid prototyping with CSS keyword colors"
datePublished: Thu Jan 17 2019 11:01:02 GMT+0000 (Coordinated Universal Time)
cuid: ckroxptia0pe0fws19xp96v5l
slug: rapid-prototyping-with-css-keyword-colors-faf2eb89369e
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1627409589404/ZbCapefLh.png
tags: css, web-design, color, data-visualisation-1

---


When quickly creating prototypes, color may play an important role. Especially for things like data visualization. However, choosing colors can be a time consuming processes. In the case of prototyping, one may pick colors quickly and tweak them later. One method of choosing colors quickly is to use CSS keyword colors.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627409576279/_aGTlpBl_.png)

Colors in CSS can be described a few different ways:

* Hexadecimal string notation: `#FF6347`

* RGB functional notation: `rgb(255, 99, 71)`

* HSL functional notation: `hsl(9, 72, 64)`

* Keyword: `tomato`

![All of the methods above describe the same color](https://cdn.hashnode.com/res/hashnode/image/upload/v1627409578825/q3f9MYKpB.png)*All of the methods above describe the same color*

Let’s focus on the last method: keyword.

MDN web docs [define CSS keyword colors](https://developer.mozilla.org/en-US/docs/Web/HTML/Applying_color#Keywords) as…
> A set of standard color names have been defined, letting you use these keywords instead of numeric representations of colors if you choose to do so and there’s a keyword representing the exact color you want to use.

David Bau hosts [an awesome resource for finding these keyword colors](http://davidbau.com/colors/).

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627409580379/19ZqK8fGv.png)

I love how his list is sorted by lightness and hue so you can find like colors quickly.

If I’m designing a simple bar chart…

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627409583193/RajGJnJ3w.png)

…and I want to give it some color right away, I can look over the keyword list and find a couple I think might work.

```
.bar-cats { fill: tomato; }
.bar-dogs { fill: blue; }
```


![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627409584528/KFtGrM5Wi.png)

Just taking a quick glance at this, I think the blue is too harsh and I’d like to soften it. So I can look at the list and see a nearby blue I think might work.

```
.bar-cats { fill: tomato; }
.bar-dogs { fill: dodgerblue; }
```


![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627409586097/c2jN5_LP0.png)

Later, I may tweak these even further using more specific hex codes but for now the prototype looks good.

%[https://codepen.io/travishorn/pen/dqxaXW]

Besides some of the basic colors like black, white, red, etc. I’ve memorized a few keyword colors in different hues that I like to reuse when prototyping a new design. Some of these include:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627409587536/1gsDU9R9D.png)

These allow me to get up and running quickly with a colorful design. Which keyword colors do you think look good when quickly prototyping a design? Or do you use another system like memorizing hex codes or using a color picker in your IDE?