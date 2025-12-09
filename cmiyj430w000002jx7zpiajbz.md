---
title: "A SQL Window Function Case Study"
datePublished: Tue Dec 09 2025 12:00:45 GMT+0000 (Coordinated Universal Time)
cuid: cmiyj430w000002jx7zpiajbz
slug: a-sql-window-function-case-study
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1764964243592/1d7888cd-0877-4431-9bc8-4a6c8788d61f.png
tags: sql, aggregation, window-function, rownumber

---

Imagine you need to run a report showing every student enrolled during the academic year. The catch? The report specification requires one row per student.

If a student transferred schools mid-year, the database holds records for both their old school and their new one. The goal is to filter through the history and grab only the most recent enrollment.

Here is a look at the data structure. We have an enrollment table that tracks when a student starts and ends at a specific school:

```sql
CREATE TABLE enrollment (
    id INT AUTO_INCREMENT PRIMARY KEY,
    school VARCHAR(50) NOT NULL,
    student_id INT NOT NULL,
    started_at DATE NOT NULL,
    ended_at DATE
);

INSERT INTO enrollment (school, student_id, started_at, ended_at) VALUES
('Washington Elementary', 1, '2025-08-15', '2025-09-10'),
('Washington Elementary', 2, '2025-08-15', NULL),
('Washington Elementary', 3, '2025-08-15', '2025-10-20'),
('Central Elementary', 1, '2025-08-15', NULL);
```

Your first instinct may be to run a standard `SELECT` query to see what you are working with:

```sql
SELECT
  e.school,
  e.student_id,
  e.started_at,
  e.ended_at
FROM enrollment e
ORDER BY
  e.student_id;
```

This is where the problem becomes obvious. Student #1 appears twice. They started at Washington Elementary, left in September, and moved to Central Elementary.

If you send the report in this state, the student count would be inflated. You need a way to tell SQL: *"Look at this student's history, sort it by date, and pick the top one.”*

## The Window Function

The `ROW_NUMBER()` function is perfect for this. It allows us to assign a ranking to each row, resetting the count whenever we hit a new student.

Here is the strategy:

1. Partition the data by `student_id`.
    
2. Order the data so the most recent date is at the top.
    
3. Assign a **1** to that top row.
    

There is one small wrinkle: **Active students**.

If a student is currently enrolled, their `ended_at` column is `NULL`. In standard SQL sorting, `NULL` values can behave unpredictably depending on your database settings. To ensure active students always appear as the "most recent," use `COALESCE` to treat `NULL` as a date far in the future (9999-12-31).

Here is the query with the ranking logic applied:

```sql
SELECT
  e.school,
  e.student_id,
  e.started_at,
  e.ended_at,
  ROW_NUMBER() OVER (
    PARTITION BY e.student_id
    ORDER BY
      COALESCE(e.ended_at, '9999-12-31') DESC,
      e.started_at DESC
  ) AS rn
FROM enrollment e;
```

If you run this, you'll see a new column called `rn`. The student's most recent enrollment is marked with a **1**, and their previous enrollments are marked **2**, **3**, etc.

## The Final Cleanup

Now that the rows are ranked, you just need to filter out everything that isn't a **1**.

Because you can't use a window function directly in a `WHERE` clause, wrap the logic in a CTE (common table expression). This creates a temporary "ranked" table that you can query against:

```sql
WITH ranked_enrollment AS (
  SELECT
    e.school,
    e.student_id,
    e.started_at,
    e.ended_at,
    ROW_NUMBER() OVER (
      PARTITION BY e.student_id
      ORDER BY
        COALESCE(e.ended_at, '9999-12-31') DESC,
        e.started_at DESC
    ) AS rn
  FROM enrollment e
)
SELECT
  re.*
FROM ranked_enrollment re
WHERE
  re.rn = 1;
```

That’s it. By filtering `WHERE rn = 1`, you strip out the historical duplicates and deliver exactly what was asked for: a clean list containing only the most recent status for every student.