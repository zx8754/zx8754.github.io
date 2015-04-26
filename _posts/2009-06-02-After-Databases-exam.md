---
layout: post
title: After Databases exam
---

It is such a relief... Still cannot put away the SQL queries from my head, need a holiday!

```sql
SELECT vacation, location
FROM world
WHERE sky LIKE '%blue%'
  AND beach NOT IN ('England')
  AND exams='over'
  AND balance NOT NULL
GROUP BY location
HAVING AVG(temperature)>30;
```
