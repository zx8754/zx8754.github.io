---
layout: post
title: R print for loop progress
---

Sometimes we may want to monitor progress of our code within the *for loop* in *R*. `print()` wouldn't work within *for loop*, unless we flush it out explicitly.

```r
for(i in 1:100){
  
  #progress - output Start time
  startTime <- Sys.time()
  print(paste(i,"Start",startTime))
  flush.console()
  
#some extensive cool code that takes a long time...

  #progress - output End time
  endTime <- Sys.time()
  print(paste(i,"End", endTime,
              " | Duration:", 
              #duration in minutes
              round((as.numeric(endTime)-as.numeric(startTime))/60,2)
  ))
  flush.console()
  
  } #END forloop
```
