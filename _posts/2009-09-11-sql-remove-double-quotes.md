---
layout: post
title: MS SQL remove double quotes
---

We have a table with field values in double quotes, like: `“blabla”`.
Those quotes need to be removed for every field value, desired result: `blabla`

Solution:

```sql
DECLARE @I INT
DECLARE @MAX INT
DECLARE @STMT NVARCHAR(MAX)
DECLARE @TABLE NVARCHAR(MAX)

SET @TABLE = 'table_name'
SET @MAX = NUMBER_OF_COLUMNS
SET @I = 0

WHILE @I <= @MAX
BEGIN
SET @STMT = ' UPDATE ' + @TABLE + ' SET [COLUMN ' + CAST(@I AS NVARCHAR) + '] = REPLACE([COLUMN ' + CAST(@I AS NVARCHAR) + '],''"'','''') WHERE 1=1'
EXEC( @STMT)
SET @I = @I + 1
END
```
