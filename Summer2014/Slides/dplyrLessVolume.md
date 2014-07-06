Less Volume, More Creativity with dplyr
=================================================
author: R Pruim
date: 2014 CVC Workshop

---
title: "Less Volume, More Creativity with dplyr"
author: "R Pruim"
date: "May 29, 2014"
output: ioslides_presentation
---

Less Volume, More Creativity with dplyr
=======================================




## 5 commands

```r
 mutate()     # add columns to data
 summarise()  # reduce data to 1 summary row
 select()     # subset of columns
 filter()     # subset of rows
 arrange()    # put rows in desired order
```

## Plus 1

```r
  group_by()  # split/apply/combine
```

mutate()
==================

```r
Births <- mutate( Births78, 
  ldate=mdy(date) )
head(Births, 2)
```

```
    date births dayofyear      ldate
1 1/1/78   7701         1 2078-01-01
2 1/2/78   7527         2 2078-01-02
```


```r
Births <- mutate( Births, 
  ldate=mdy(date) - years(100),
  wday=wday(ldate, label=TRUE, abbr=TRUE))
head(Births, 2)
```

```
    date births dayofyear      ldate wday
1 1/1/78   7701         1 1978-01-01  Sun
2 1/2/78   7527         2 1978-01-02  Mon
```

mutate()
==================


```r
xyplot( births ~ ldate, data=Births,  
  groups=wday, type='l',
  auto.key=list(columns=4, 
      lines=T, points=F),
  par.settings=list(
      superpose.line=list( lty=1 ) ))
```

<img src="dplyrLessVolume-figure/unnamed-chunk-6.png" title="plot of chunk unnamed-chunk-6" alt="plot of chunk unnamed-chunk-6" height="400" style="display: block; margin: auto;" />

summarise()
===========
`summarise()` creates a 1-row summary of the data

```r
summarise(Births, 
  mean=mean(births), 
  sum=sum(births), 
  sd=sd(births)
)
```

```
  mean     sum    sd
1 9132 3333239 817.9
```
select()
========
We can use `select()` to reduce to desired columns:

```r
Births <- select(Births, 
  ldate, births, wday)        # keep only these
head(Births, 2)
```

Or we can think about what gets dropped:

```r
Births <- select(Births, 
  -date, -dayofyear)         # drop these
head(Births, 2)
```

```
  births      ldate wday
1   7701 1978-01-01  Sun
2   7527 1978-01-02  Mon
```

filter()
========
`filter()` selects a subset of rows:

```r
Monday <- filter(Births, wday=="Mon")
summarise( Monday, mean=mean(births) )
```

```
  mean
1 9371
```

```r
summarise( Births, mean=mean(births) )
```

```
  mean
1 9132
```

arrange()
=========
`arrange()` arranges the rows in a desired order:


```r
head( arrange(Births, births) )   # sort by number of births
```

```
  births      ldate wday
1   7135 1978-04-30  Sun
2   7193 1978-04-16  Sun
3   7304 1978-04-23  Sun
4   7382 1978-05-14  Sun
5   7388 1978-05-07  Sun
6   7399 1978-06-04  Sun
```

But wait, there's more: group_by()
==========

```r
summarise( group_by(Births, wday),  
    mean = mean(births), 
    min=min(births), 
    max=max(births) )
```

```
Source: local data frame [7 x 4]

   wday mean  min   max
1   Sun 7951 7135  8711
2   Mon 9371 7527 10414
3  Tues 9709 8433 10711
4   Wed 9498 8606 10703
5 Thurs 9484 7915 10499
6   Fri 9626 8892 10438
7   Sat 8309 7527  9170
```

group_by() -- chaining syntax
==========

```r
Births %>%
  group_by(wday) %>%
  summarise(mean = mean(births), 
    min=min(births), max=max(births) ) %>%
  mutate(range = max - min) %>%
  arrange(mean)
```

```
Source: local data frame [7 x 5]

   wday mean  min   max range
1   Sun 7951 7135  8711  1576
2   Sat 8309 7527  9170  1643
3   Mon 9371 7527 10414  2887
4 Thurs 9484 7915 10499  2584
5   Wed 9498 8606 10703  2097
6   Fri 9626 8892 10438  1546
7  Tues 9709 8433 10711  2278
```

Working with bigger data
=====================
`dplyr` is designed to work with data of different formats and sizes.
 * separates syntax from storage 
   * modular design will allow for additional back ends over time
   * already supports `MySQL`, `SQLite`, `PostgreSQL`
 * provides cleverer data-frame-like objects for local (`tbl_`)
 * designed for speed
 
*Skills learned on small data immediately transfer to large data settings*
 * Another example of less volume, more creativity

Big Data Example -- Airlines
============================















```
Error in mysqlNewConnection(drv, ...) : 
  RS-DBI driver: (Failed to connect to database: Error: Unknown MySQL server host 'rucker.smith.edu' (2)
)
```
