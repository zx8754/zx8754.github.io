---
layout: post
title: Postcode validator for Oracle
---

Here is another function for Oracle that validates the UK postcodes. The function will return 1 if the postcode is valid. Although valid doesn’t mean it exists, it is only valid in accordance with [CabinetOffice](http://www.cabinetoffice.gov.uk/govtalk/schemasstandards/e-gif/datastandards/address/postcode.aspx) validation rules.

```sql
CREATE OR REPLACE FUNCTION Is_postcode
(postcode VARCHAR2)
RETURN NUMBER
IS
BEGIN
IF (postcode IS NOT NULL
AND (Regexp_like(postcode,'[ABCDEFGHIJKLMNOPRSTUWYZ]{1}[0-9]{1} [0-9]{1}[ABDEFGHJLNPQRSTUWXYZ]{1}[ABDEFGHJLNPQRSTUWXYZ]{1}','i')
OR Regexp_like(postcode,'[ABCDEFGHIJKLMNOPRSTUWYZ]{1}[0-9]{1}[0-9]{1} [0-9]{1}[ABDEFGHJLNPQRSTUWXYZ]{1}[ABDEFGHJLNPQRSTUWXYZ]{1}','i')
OR Regexp_like(postcode,'[ABCDEFGHIJKLMNOPRSTUWYZ]{1}[ABCDEFGHKLMNOPQRSTUVWXY]{1}[0-9]{1} [0-9]{1}[ABDEFGHJLNPQRSTUWXYZ]{1}[ABDEFGHJLNPQRSTUWXYZ]{1}','i')
OR Regexp_like(postcode,'[ABCDEFGHIJKLMNOPRSTUWYZ]{1}[ABCDEFGHKLMNOPQRSTUVWXY]{1}[0-9]{1}[0-9]{1} [0-9]{1}[ABDEFGHJLNPQRSTUWXYZ]{1}[ABDEFGHJLNPQRSTUWXYZ]{1}','i')
OR Regexp_like(postcode,'[ABCDEFGHIJKLMNOPRSTUWYZ]{1}[0-9]{1}[ABCDEFGHJKSTUW]{1} [0-9]{1}[ABDEFGHJLNPQRSTUWXYZ]{1}[ABDEFGHJLNPQRSTUWXYZ]{1}','i')
OR Regexp_like(postcode,'[ABCDEFGHIJKLMNOPRSTUWYZ]{1}[ABCDEFGHKLMNOPQRSTUVWXY]{1}[0-9]{1}[ABEHMNPRVWXY]{1} [0-9]{1}[ABDEFGHJLNPQRSTUWXYZ]{1}[ABDEFGHJLNPQRSTUWXYZ]{1}','i'))) THEN
RETURN 1;
ELSE
RETURN 0;
END IF;
END is_postcode;
```
