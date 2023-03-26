---
title: "Excel Formula to Scale Data from 0 to 1"
datePublished: Thu Mar 11 2021 12:02:30 GMT+0000 (Coordinated Universal Time)
cuid: ckroy2kp90pigfws11v784xxj
slug: excel-formula-to-scale-data-from-0-to-1-64008ffc2013
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1627409425079/_Jyru69VY.png
tags: excel, data-visualisation-1

---


Say we have a set of data in column A.

```
469
396
600
177
240
155
204
454
278
233
```


And we want to scale the data so that the lowest value equates to 0 and the highest value equates to 1.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627409416446/8sv3sZJND.png)

If you just want a quick formula to do this, you can copy the line below. However, you may consider reading further to really understand how it works.

```
=(A1 - MIN(A:A)) / (MAX(A:A) - MIN(A:A))
```


First, figure out the minimum value in the set.

```
=MIN(A:A)
```


In this case, it’s `155`.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627409418401/wkfzbgxOw.png)

Now for each value, figure out the difference from the minimum.

```
=A1 - MIN(A:A)
```


Looking at each value…

```
=A1  - MIN(A:A)          = 469 - 155          = 314 
=A2  - MIN(A:A)          = 396 - 155          = 241
=A3  - MIN(A:A)          = 600 - 155          = 445
=A4  - MIN(A:A)          = 177 - 155          =  22
=A5  - MIN(A:A)          = 240 - 155          =  85
=A6  - MIN(A:A)          = 155 - 155          =   0
=A7  - MIN(A:A)          = 204 - 155          =  49
=A8  - MIN(A:A)          = 454 - 155          = 299
=A9  - MIN(A:A)          = 278 - 155          = 123
=A10 - MIN(A:A)          = 233 - 155          =  78
```


![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627409420060/Gu4gDxfZ_.png)

Next, figure out the largest difference. This is simply the different between `MAX(A:A)` and `MIN(A:A)`.

```
=MAX(A:A) - MIN(A:A)          = 600 - 155          = 445
```


![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627409421477/5S77x_gi4c.png)

For each value, calculate what percentage it’s difference is when compared to this maximum difference.

```
=(A1  - MIN(A:A)) / (MAX(A:A) - MIN(A:A))        = (469 - 155) / 445
=(A2  - MIN(A:A)) / (MAX(A:A) - MIN(A:A))        = (396 - 155) / 445
=(A3  - MIN(A:A)) / (MAX(A:A) - MIN(A:A))        = (600 - 155) / 445
=(A4  - MIN(A:A)) / (MAX(A:A) - MIN(A:A))        = (177 - 155) / 445
=(A5  - MIN(A:A)) / (MAX(A:A) - MIN(A:A))        = (240 - 155) / 445
=(A6  - MIN(A:A)) / (MAX(A:A) - MIN(A:A))        = (155 - 155) / 445
=(A7  - MIN(A:A)) / (MAX(A:A) - MIN(A:A))        = (204 - 155) / 445
=(A8  - MIN(A:A)) / (MAX(A:A) - MIN(A:A))        = (454 - 155) / 445
=(A9  - MIN(A:A)) / (MAX(A:A) - MIN(A:A))        = (278 - 155) / 445
=(A10 - MIN(A:A)) / (MAX(A:A) - MIN(A:A))        = (233 - 155) / 445
```


The result is what we’re looking for.

```
=(A1  - MIN(A:A)) / (MAX(A:A) - MIN(A:A))          = 0.71
=(A2  - MIN(A:A)) / (MAX(A:A) - MIN(A:A))          = 0.54
=(A3  - MIN(A:A)) / (MAX(A:A) - MIN(A:A))          = 1.00
=(A4  - MIN(A:A)) / (MAX(A:A) - MIN(A:A))          = 0.05
=(A5  - MIN(A:A)) / (MAX(A:A) - MIN(A:A))          = 0.19
=(A6  - MIN(A:A)) / (MAX(A:A) - MIN(A:A))          = 0.00
=(A7  - MIN(A:A)) / (MAX(A:A) - MIN(A:A))          = 0.11
=(A8  - MIN(A:A)) / (MAX(A:A) - MIN(A:A))          = 0.67
=(A9  - MIN(A:A)) / (MAX(A:A) - MIN(A:A))          = 0.28
=(A10 - MIN(A:A)) / (MAX(A:A) - MIN(A:A))          = 0.18
```


![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627409422879/gKXJ7QJ0k.png)

As you can see, the lowest value `155` is scaled to `0.00` while the highest value `600` is scaled to `1.00`. All the other values are proportionately somewhere in between.

This can be useful when used on multiple columns to normalize the data and make scores or comparisons.

Once again, the final formula is:

```
=(A1 - MIN(A:A)) / (MAX(A:A) - MIN(A:A))
```
