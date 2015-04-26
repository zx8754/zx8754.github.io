---
layout: post
title: NHS Number validator for Oracle
---

Everyone registered with the NHS in England and Wales has their own NHS Number. Following function validates the NHS Number. It will return 1 if it is valid NHS number. The rules for validation is based on NHS DATA MODEL AND DICTIONARY.

```sql
CREATE OR replace FUNCTION Is_nhsnumber(newnhsno VARCHAR2)
RETURN NUMBER
IS
  check_digit  NUMBER;
  my_nhsnumber VARCHAR2(20) := Trim(newnhsno);
BEGIN
  IF newnhsno IS NULL --Return 9 if it is NULL
  THEN
    RETURN 9;
  ELSIF --All Digits/exactly 10 digits
  Translate(my_nhsnumber, '0123456789', '9999999999') = '9999999999' THEN
    check_digit := 11 - Mod(To_number(Substr(my_nhsnumber, 1, 1)) * 10 +
                            To_number(Substr(my_nhsnumber
    ,
                   2
                   , 1
                   )
                   ) * 9 + To_number(Substr(my_nhsnumber, 3, 1)) * 8 +
                   To_number(Substr(my_nhsnumber, 4, 1)) * 7 +
                   To_number(Substr(my_nhsnumber, 5, 1)) * 6 +
                   To_number(Substr(my_nhsnumber, 6, 1)) * 5 +
                   To_number(Substr(my_nhsnumber, 7, 1)) * 4 +
                   To_number(Substr(my_nhsnumber, 8, 1)) * 3 +
                   To_number(Substr(my_nhsnumber, 9, 1)) * 2, 11);
  ELSE
    RETURN 0;
  END IF;
  IF 1 = --check digit
     CASE
       WHEN check_digit = 11 THEN CASE
                                    WHEN 0 = To_number(Substr(my_nhsnumber, 10,
                                                       1)
                                             )
                                       THEN 1
                                    ELSE 0
                                  END
       ELSE CASE
              WHEN check_digit = To_number(Substr(my_nhsnumber, 10, 1)) THEN 1
              ELSE 0
            END
     END THEN
    RETURN 1;
  ELSE
    RETURN 0;
  END IF;
END is_nhsnumber;
```
=====
Comments:

*Anonymous* 23 March 2010 at 09:41  
please do this same thing for php

*Tokhir Dadaev* 23 March 2010 at 20:49  
Unfortunately, I’m not familiar with PHP.

*Anonymous* 20 November 2014 at 10:34  
Brilliant! Just what I was looking for. Thanks for your help. :o)
