---
layout: post
title: MS Excel useful functions
---

##### Names of the days of the week from date  
`=CHOOSE(WEEKDAY(A1),"Sun","Mon","Tue","Wed","Thu","Fri","Sat")`

##### Age grouping   
`=LOOKUP(A1,{0,30,40,50,60,70,80,90,100},{"0-29","30-39","40-49","50-59","60-69","70-79","80-89","90-99","100-109"})`

##### Get Field from Delimited Text String   
This crazy looking solution was originally posted at [EXCELFOX](http://www.excelfox.com/forum/f22/get-field-delimited-text-string-333/).  
`=TRIM(MID(SUBSTITUTE(A1,delimiter,REPT(" ",999)),fieldnumber*999-998,999))`

##### Calculate Odds Ratio from Standard Error and Beta  
`Odds Ratio =EXP(Beta)`  
`P value =CHIDIST((Beta/Standard Error)^2,2)`

##### Date difference - DATEDIF - hidden function  

> For some reason, Microsoft has decided not to document this function in any other versions. DATEDIF is treated as the drunk cousin of the Formula family. Excel knows it lives a happy and useful life, but will not speak of it in polite conversation. - [Chip Pearson](http://www.cpearson.com/excel/datedif.aspx)

Date difference betweend two dates, e.g.: DOB and DOD.

`=DATEDIF(A1,B1,"y")`

Following also gives difference in years but with precision. Get difference in days - `d` and multiply with year fraction.

`=DATEDIF(A1,B1,"d")*0.00273790700698851`
