---
title: "Greater Than & Less Than Formatting in Excel"
datePublished: Fri Jul 22 2016 22:01:01 GMT+0000 (Coordinated Universal Time)
cuid: ckrnhx2ev0cb6fws17o2l2n7q
slug: greater-than-less-than-formatting-in-excel-9163c0ddab81
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1627409939930/OgAaoxvbL.jpeg
tags: excel

---


This post is part of a project to move my old reference material to my blog. Before 2012, when I accessed the same pieces of code or general information multiple times, I would write a quick HTML page for my own reference and put it on a personal site. Later, I published these pages online. Some of the pages still get used and now I want to make them available on my blog.

![Photo by [William Iven](https://cdn.hashnode.com/res/hashnode/image/upload/v1627409938278/ed7QNSY1I.html)](https://cdn-images-1.medium.com/max/8576/1*5ICdZXW8jn5DTyGyXPJIxQ.jpeg)*Photo by [William Iven](https://unsplash.com/@firmbee)*

If you have a column that lists positive and negative values, you can format it to be easier to read by making negative values appear red while positive values appear green (or vice-versa depending on the situation).

1. Select all the cells to which you want to apply the formatting

1. On the **Home** tab, click **Conditional Formatting** > **Highlight Cells Rules** > **Greater Than…**

1. In the box that appears, type **0** in the first box and choose **Custom Format…** in the second

1. A new box will appear, allowing you to customize the formatting. In the **color** box, select **Green**

1. Click **OK**, and then click **OK** once more to close both boxes. Now, any number in your selected area that is greater than 0 will be green

1. We’re halfway there. With the same cells still selected, on the **Home** tab, click **Conditional Formatting** > **Highlight Cells Rules** > **Less Than…**

1. In the box that appears, type **0** in the first box and select **Custom Format…** in the second

1. A new box will appear, allowing you to customize the format. In the **color** box, select** Red**

1. Click **OK**, and then click **OK** once more to close both boxes