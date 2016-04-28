---
layout: post
title: ggplot subset vs xlim
---

Testing if internal subset of ggplot is faster than manual subset.
1. plot with xlim 0.25:0.5 with data range of 0:1
2. subset data within ggplot() for range of 0.25:0.5 then plot
3. subset data for range of 0.25:0.5 then plot

Which one is more efficient? From the test below it seems ggplot's internal subset is just as good as pre-subsetting the data.

```r
library(ggplot2)
library(microbenchmark)

#dummy data 10K points
set.seed(123)
df <- data.frame(x = runif(10000), y = runif(10000))

#manual pre subset
df_sub <- df[ which(df$x >= 0.25 & df$x <= 0.5), ]

#benchmark
microbenchmark(
  ggSubset = {
    ggplot(df,
           aes(x,y)) + geom_point() + xlim(0.25,0.5)},
  manSubset = {
    ggplot(df[ which(df$x >= 0.25 & df$x <= 0.5), ],
           aes(x,y)) + geom_point() + xlim(0.25,0.5)},
  manPreSubset = {
    ggplot(df_sub,
           aes(x,y)) + geom_point() + xlim(0.25,0.5)},
  times = 10000
)

# Unit: milliseconds
#          expr      min       lq     mean   median       uq        max neval cld
#      ggSubset 1.467630 1.566556 1.676105 1.621293 1.678595   8.473086 10000  a 
#     manSubset 1.908660 2.024406 2.184444 2.083419 2.157542 126.873631 10000   b
#  manPreSubset 1.485876 1.566841 1.682353 1.621293 1.680449   9.109401 10000  a 
```
