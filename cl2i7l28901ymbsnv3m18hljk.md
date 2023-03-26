---
title: "SQL Group with Most Recent Record Each"
datePublished: Wed Apr 27 2022 23:29:01 GMT+0000 (Coordinated Universal Time)
cuid: cl2i7l28901ymbsnv3m18hljk
slug: sql-group-with-most-recent-record-each
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1651084908979/riMHTrx5w.png
tags: databases, sql

---

Say you have a table with student test data.

```sql
SELECT      t.*
FROM        tests t
ORDER BY    t.student_id,
            t.taken_on
```

| student_id | score | taken_on   |
|:-----------|------:|:-----------|
| 1          |  1750 | 2021-08-30 |
| 1          |  1717 | 2021-11-16 |
| 1          |  1786 | 2022-02-08 |
| 2          |  1550 | 2021-08-30 |
| 2          |  1493 | 2021-11-16 |
| 2          |  1627 | 2022-02-08 |
| 3          |  1441 | 2021-08-30 |
| 3          |  1528 | 2022-02-07 |

Imagine that you want to see the most recent score each student received.

To put it another way, you want to query this table and see only 1 row per `student_id`. The row should be the one with the most recent `taken_on`.

The desired result looks like this:

| student_id | score | taken_on   |
|:-----------|------:|:-----------|
| 1          |  1786 | 2022-02-08 |
| 2          |  1627 | 2022-02-08 |
| 3          |  1528 | 2022-02-07 |

Let's start simple. You need to get a query that shows the most recent `taken_on` grouped by `student_id` first.

```sql
SELECT      student_id,
            MAX(taken_on) latest_taken_on
FROM        tests
GROUP BY    student_id
```

Result:

| student_id | latest_taken_on  |
|:-----------|:-----------------|
| 1          | 2022-02-08       |
| 2          | 2022-02-08       |
| 3          | 2022-02-07       |

You could also add things like a `WHERE` clause to that query if you need only the most recent test where a certain condition was met.

Next, you should revisit the original query.

```sql
SELECT      t.*
FROM        tests t
ORDER BY    t.student_id,
            t.taken_on
```

But this time, `JOIN` the "latest date" query you just wrote. You'll want to join on the `student_id` and `taken_on` date columns.

```sql
SELECT      t.*
FROM        tests t
JOIN        (
  SELECT      student_id,
              MAX(taken_on) latest_taken_on
  FROM        tests
  GROUP BY    student_id
            ) l
ON          t.student_id = l.student_id
AND         t.taken_on = l.latest_taken_on
ORDER BY    t.student_id
```

Those `JOIN` parameters ensure the query only returns rows where the `student_id` and `taken_on` columns match the latest date for each student.

Note that `t.taken_on` in the `ORDER BY` clause is no longer necessary because you're already ordering on `t.student_id` and there will only be one each.

| student_id | score | taken_on   |
|:-----------|------:|:-----------|
| 1          |  1786 | 2022-02-08 |
| 2          |  1627 | 2022-02-08 |
| 3          |  1528 | 2022-02-07 |

That's it. The results are exactly what we were aiming for; the row with the latest `taken_on` for each `student_id` is returned.