---
layout: post
title: IS_NUMERIC and IS_DATE for Oracle
---

Switchover from MS to Oracle SQL syntax takes some time. Suddenly all simple queries start to punish you with syntax error messages. Suddenly familiar very useful functions seize to exist. Some Googling will show that it is better to create your own functions. That is why I created/replicated some of MS functions for Oracle.

This function will return 1 if the string is numeric (0-9).

```sql
CREATE OR REPLACE FUNCTION IS_NUMERIC
(param IN VARCHAR2)
RETURN NUMBER
IS
dummy NUMBER;
BEGIN
dummy := To_number(param);
RETURN 1; --success
EXCEPTION
WHEN OTHERS THEN
RETURN 0; --fail
END is_numeric;
```

Following one is to check if the string is a valid date. In this version acceptable date format is `DDMMYYYY`, which can be modified.

```sql
CREATE OR REPLACE FUNCTION IS_DATE
(str LONG)
RETURN NUMBER
IS
the_date DATE;
BEGIN
the_date := To_date(str,'DDMMYYYY'); -- Modify date format here
RETURN 1; --success
EXCEPTION
WHEN OTHERS THEN
RETURN 0; --fail
END;
```
