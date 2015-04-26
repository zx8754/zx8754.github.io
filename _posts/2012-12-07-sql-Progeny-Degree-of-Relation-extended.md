---
layout: post
title: Progeny - Degree of Relation extended
---

[Progeny](http://www.progenygenetics.com/) is a genetic pedigree and clinical data management software designed to provide user front end and database for any hereditary study. Simply, it is a Sybase back-end database with a shiny point and click front-end and with pedigree drawing functionality.

Users can create all sorts of fields in addition to system fields, like Name, Surname, DOB, etc. Most powerful of all types of fields is the computed fields, which accepts SQL queries.

Degree of Relation and Degree of Relation2 system fields are used to identify kinship, degree of relationship to the [proband](http://en.wikipedia.org/wiki/Proband). Latter is good enough to identify 16 straightforward relatives, e.g.: Father, Mother, Sister, Brother, etc. Anyone else is listed as Unknown. For exampple system fields do not show: cousin, grandchild, and if the aunt/uncle are maternal or paternal. A workaround is the following code for a computed field. This is true for Progeny 8, hopefully this will be [implemented for future versions](http://www.progenygenetics.com/community/topic/716) as a system field.

```sql
CASE
WHEN {Degree of Relation2}=1 OR {Proband status}=1 THEN  'Proband'
WHEN {Degree of Relation2}=2 AND {Gender}=1 AND {Gender Unknown}=0 THEN  'Brother'
WHEN {Degree of Relation2}=2 AND {Gender}=0 AND {Gender Unknown}=0 THEN  'Sister'
WHEN {Degree of Relation2}=3 AND {Gender}=1 AND {Gender Unknown}=0 THEN  'Son'
WHEN {Degree of Relation2}=3 AND {Gender}=0 AND {Gender Unknown}=0 THEN  'Daughter'
WHEN {Degree of Relation2}=4 AND {Gender}=1 AND {Gender Unknown}=0 THEN  'Father'
WHEN {Degree of Relation2}=4 AND {Gender}=0 AND {Gender Unknown}=0 THEN  'Mother'
WHEN {Degree of Relation2}=5 AND {Gender}=1 AND {Gender Unknown}=0 THEN  'Paternal Grandfather'
WHEN {Degree of Relation2}=5 AND {Gender}=0 AND {Gender Unknown}=0 THEN  'Paternal Grandmother'
WHEN {Degree of Relation2}=6 AND {Gender}=1 AND {Gender Unknown}=0 THEN  'Paternal Uncle'
WHEN {Degree of Relation2}=6 AND {Gender}=0 AND {Gender Unknown}=0 THEN  'Paternal Aunt'
WHEN {Degree of Relation2}=7 AND {Gender}=1 AND {Gender Unknown}=0 THEN  'Maternal Grandfather'
WHEN {Degree of Relation2}=7 AND {Gender}=0 AND {Gender Unknown}=0 THEN  'Maternal Grandmother'
WHEN {Degree of Relation2}=8 AND {Gender}=1 AND {Gender Unknown}=0 THEN  'Maternal Uncle'
WHEN {Degree of Relation2}=8 AND {Gender}=0 AND {Gender Unknown}=0 THEN  'Maternal Aunt'
WHEN {Degree of Relation2}=13 AND {Gender}=1 AND {Gender Unknown}=0 AND {Degree of Relation}=0 THEN 'Nephew WifeSide'
WHEN {Degree of Relation2}=13 AND {Gender}=0 AND {Gender Unknown}=0 AND {Degree of Relation}=0 THEN 'Niece WifeSide'
WHEN {Degree of Relation2}=13 AND {Gender}=1 AND {Gender Unknown}=0 AND {Degree of Relation}=2 THEN 'Nephew SiblingSide'
WHEN {Degree of Relation2}=13 AND {Gender}=0 AND {Gender Unknown}=0 AND {Degree of Relation}=2 THEN 'Niece SiblingSide'
WHEN {Degree of Relation2}=14 AND {Gender}=1 AND {Gender Unknown}=0 THEN  'Husband'
WHEN {Degree of Relation2}=14 AND {Gender}=0 AND {Gender Unknown}=0 THEN  'Wife'
WHEN {Degree of Relation2}=15 AND {Gender}=1 AND {Gender Unknown}=0 THEN  'Brother-in-law'
WHEN {Degree of Relation2}=15 AND {Gender}=0 AND {Gender Unknown}=0 THEN  'Sister-in-law'
WHEN {Degree of Relation2}=16 THEN  'Twin of Proband'
WHEN ({Degree of Relation} = 3
     AND (     (SELECT degree2
                 FROM   st_data AS stdatasub
                 WHERE  stdatasub.personnumber = st_data.motherid
                        AND stdatasub.familyid = st_data.familyid) = 6
             OR (SELECT degree2
                 FROM   st_data AS stdatasub
                 WHERE  stdatasub.personnumber = st_data.fatherid
                        AND stdatasub.familyid = st_data.familyid) = 6
         )
) THEN 'Paternal Cousin'
WHEN ({Degree of Relation} = 3
     AND (     (SELECT degree2
                 FROM   st_data AS stdatasub
                 WHERE  stdatasub.personnumber = st_data.motherid
                        AND stdatasub.familyid = st_data.familyid) = 8
             OR (SELECT degree2
                 FROM   st_data AS stdatasub
                 WHERE  stdatasub.personnumber = st_data.fatherid
                        AND stdatasub.familyid = st_data.familyid) = 8
         )
) THEN 'Maternal Cousin'
WHEN ({Degree of Relation} = 2
      AND ((SELECT ISNULL(stdatasub.degree2, 0)
            FROM st_data AS stdatasub
            WHERE stdatasub.personnumber = st_data.motherid
            AND stdatasub.familyid = st_data.familyid) = 3
          OR
            (SELECT ISNULL(stdatasub.degree2, 0)
            FROM st_data AS stdatasub
            WHERE stdatasub.personnumber = st_data.fatherid
            AND stdatasub.familyid = st_data.familyid) = 3
          )
) THEN 'Grandchild'
ELSE 'Unknown'
END
```
