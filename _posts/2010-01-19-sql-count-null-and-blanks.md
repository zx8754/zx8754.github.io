---
layout: post
title: MS SQL count NULLs and blanks.
---

Some of the records have either `NULL` or blank strings `''`. Need to count the ones which are not NULL and not blanks string. Combined use of `COUNT` and `NULLIF`.

Solution:

```sql
DECLARE @table TABLE(X NVARCHAR)

INSERT INTO @table VALUES ('')
INSERT INTO @table VALUES (NULL)
INSERT INTO @table VALUES ('')
INSERT INTO @table VALUES (NULL)
INSERT INTO @table VALUES ('y')
INSERT INTO @table VALUES (NULL)
INSERT INTO @table VALUES ('y')
INSERT INTO @table VALUES ('y')

SELECT COUNT(NULLIF(X,''))
FROM @table
```
