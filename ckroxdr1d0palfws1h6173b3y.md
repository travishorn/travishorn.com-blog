---
title: "SQL Query for Counting Records per Day"
datePublished: Sat Aug 06 2016 22:01:02 GMT+0000 (Coordinated Universal Time)
cuid: ckroxdr1d0palfws1h6173b3y
slug: sql-query-for-counting-records-per-day-3936728c8bd
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1627409717233/HYktuKx6S.jpeg
tags: programming, databases, sql

---


This post is part of a project to move my old reference material to my blog. Before 2012, when I accessed the same pieces of code or general information multiple times, I would write a quick HTML page for my own reference and put it on a personal site. Later, I published these pages online. Some of the pages still get used and now I want to make them available on my blog.

![Photo by [Daniel Chen](https://cdn.hashnode.com/res/hashnode/image/upload/v1627409715482/EJHHBt-7O.html)](https://cdn-images-1.medium.com/max/10944/1*HC3yOX5DyVdZkT3y1AuIUw.jpeg)*Photo by [Daniel Chen](https://unsplash.com/@d_che)*

This SQL query will add up the record count per day based on a column called “Timestamp.”

### Transact SQL

```
SELECT    DATEPART(YEAR, Timestamp) AS 'Year',
          DATEPART(MONTH, Timestamp) AS 'Month',
          DATEPART(DAY, Timestamp) AS 'Day',
          COUNT(*) AS 'Visits'
FROM      tblVisits
GROUP BY  DATEPART(DAY, Timestamp),
          DATEPART(MONTH, Timestamp),
          DATEPART(YEAR, Timestamp)
ORDER BY  'Year',
          'Month',
          'Day'
```


### Results

The results of this query will appear as follows:

```
| Year | Month | Day | Visits |
|------|-------|-----|--------|
| 2011 | 2     | 7   | 46     |
| 2011 | 2     | 8   | 40     |
| 2011 | 2     | 9   | 37     |
| 2011 | 2     | 10  | 36     |
| 2011 | 2     | 11  | 41     |
```
