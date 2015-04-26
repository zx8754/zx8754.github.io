---
layout: post
title: Valid postcode generator for Oracle
---

Lately I have been looking at validation rules of the UK postcodes and came up with a function for Oracle that validates any UK postcode. While at it I decided to create another function that would return valid postcode. The only use for this function could be to generate dummy data.

There are six types of valid postcodes:

Type | Format | Example
---|---|---
1 | AN NAA | M1 1AA  
2 | ANN NAA | M60 1NW  
3 | AAN NAA | CR2 6XH  
4 | AANN NAA | DN55 1PT  
5 | ANA NAA | W1A 1HQ  
6 | AANA NAA | EC1A 1BB  

Following function will return in random order all types of postcodes. It doesn't mean they exist, but they pass the validation rules.

```sql
CREATE OR REPLACE FUNCTION Rand_postcode
RETURN NVARCHAR2
AS
postcode NVARCHAR2(8);
x NUMBER;
i1 NVARCHAR2(23) := 'ABCDEFGHIJKLMNOPRSTUWYZ';
i2 NVARCHAR2(23) := 'ABCDEFGHKLMNOPQRSTUVWXY';
i3 NVARCHAR2(14) := 'ABCDEFGHJKSTUW';
i4 NVARCHAR2(12) := 'ABEHMNPRVWXY';
n1 NVARCHAR2(10) := '0123456789';
o1 NVARCHAR2(20) := 'ABDEFGHJLNPQRSTUWXYZ';
BEGIN
x := Round(dbms_random.Value(1,6),0);
postcode := Substr(i1,Round(dbms_random.Value(1,23)),1);
CASE x
WHEN 1
THEN postcode := postcode
||Substr(n1,Round(dbms_random.Value(1,10)),1);
WHEN 2
THEN postcode := postcode
||Substr(n1,Round(dbms_random.Value(1,10)),1)
||Substr(n1,Round(dbms_random.Value(1,10)),1);
WHEN 3
THEN postcode := postcode
||Substr(i2,Round(dbms_random.Value(1,23)),1)
||Substr(n1,Round(dbms_random.Value(1,10)),1);
WHEN 4
THEN postcode := postcode
||Substr(i2,Round(dbms_random.Value(1,23)),1)
||Substr(n1,Round(dbms_random.Value(1,10)),1)
||Substr(n1,Round(dbms_random.Value(1,10)),1);
WHEN 5
THEN postcode := postcode
||Substr(n1,Round(dbms_random.Value(1,10)),1)
||Substr(i3,Round(dbms_random.Value(1,14)),1);
ELSE postcode := postcode
||Substr(i2,Round(dbms_random.Value(1,23)),1)
||Substr(n1,Round(dbms_random.Value(1,10)),1)
||Substr(i4,Round(dbms_random.Value(1,12)),1);
END CASE;
postcode := postcode
||' '
||Substr(n1,Round(dbms_random.Value(1,10)),1)
||Substr(o1,Round(dbms_random.Value(1,20)),1)
||Substr(o1,Round(dbms_random.Value(1,20)),1);
RETURN postcode;
END rand_postcode;
```

=====

**Comments:**

*M Umar* 22 July 2010 at 18:57
Hi, Is there any oracle function which can validate user given post code? For example if i give any postcode and it will validate it and return validate or unvalidate?

*Tokhir Dadaev* 22 July 2010 at 22:45
I have my own version, see [Postcode validator for Oracle](http://zx8754.github.io/sql-Postcode-validator-for-Oracle/)
This function returns 1 if the postcode is valid, if null or invalid it returns 0.

