---
layout: post
title: Subset rows and columns
---

We want to subset a text file on rows and columns, where rows and columns numbers are read from a file. Excluding header (row 1) and rownmaes (col 1).

**inputFile.txt**   

```
header	62	9	3	54	6	1
25	2	3	4	5	6	7
96	1	1	1	1	0	1
72	3	3	3	3	3	3
18	0	1	0	1	1	0
82	1	0	0	0	0	1
77	1	0	1	0	1	1
15	7	7	7	7	7	7
82	0	0	1	1	1	0
37	0	1	0	0	1	0
18	0	1	0	0	1	0
53	0	0	1	0	0	0
57	1	1	1	1	1	1
```

**subsetCols.txt**   

```
2,3,5
```

**subsetRows.txt**   

```
1,3,7
```

Solution using cut and awk loop   

```
# define vars
fileInput=inputFile.txt
fileRows=subsetRows.txt
fileCols=subsetCols.txt
fileOutput=result.txt

# cut columns and awk rows
cut -f`cat $fileCols` $fileInput | sed '1d' | awk -v s=`cat $fileRows` 'BEGIN{split(s, a, ","); for (i in a) b[a[i]]} NR in b' > $fileOutput
```

**Output file: result.txt**   

```
2       3       5
3       3       3
7       7       7
```
