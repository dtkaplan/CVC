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
 summarise()  # 1-row summary
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
require(mosaic); require(lubridate)
Births2 <- mutate(Births78, ldate=mdy(date))
head(Births2, 2)
```

```
    date births dayofyear      ldate
1 1/1/78   7701         1 2078-01-01
2 1/2/78   7527         2 2078-01-02
```


```r
Births2 <- mutate(Births2, 
  ldate = mdy(date) - years(100),
  wday = wday(ldate, label = TRUE, abbr = TRUE))
head(Births2, 2)
```

```
    date births dayofyear      ldate wday
1 1/1/78   7701         1 1978-01-01  Sun
2 1/2/78   7527         2 1978-01-02  Mon
```

mutate()
==================


```r
xyplot( births ~ ldate, data = Births2,  
  groups = wday, type = 'l',
  auto.key = list(
    columns = 4, lines = TRUE, points = FALSE),
  par.settings = list(superpose.line=list(lty=1)))
```

<img src="dplyrLessVolume-figure/unnamed-chunk-6.png" title="plot of chunk unnamed-chunk-6" alt="plot of chunk unnamed-chunk-6" height="400" style="display: block; margin: auto;" />

summarise()
===========
`summarise()` creates a 1-row summary of the data

```r
summarise(Births2, 
  total = sum(births), 
  mean = mean(births), 
  sd = sd(births)
)
```

```
    total mean    sd
1 3333239 9132 817.9
```
select()
========
We can use `select()` to reduce to desired columns:

```r
Births2 <- select(Births2, 
                  ldate, births, wday)
head(Births2, 2)
```

Or we can think about what gets dropped:

```r
Births2 <- select(Births2,  
                  -date, -dayofyear)
head(Births2, 2)
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
Sunday <- filter(Births2, wday == "Sun")
summarise(Sunday, 
          mean = mean(births), n = n(), 
          total = sum(births))
```

```
  mean  n  total
1 7951 53 421400
```

arrange()
=========
`arrange()` arranges the rows in a desired order:


```r
head( arrange(Births2, births) )   # fewest births
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

arrange()
=========

```r
tail( arrange(Births2, births) )   # most births
```

```
    births      ldate  wday
360  10498 1978-09-26  Tues
361  10499 1978-09-21 Thurs
362  10502 1978-08-15  Tues
363  10605 1978-12-19  Tues
364  10703 1978-09-06   Wed
365  10711 1978-09-19  Tues
```

But wait, there's more: group_by()
==========

```r
summarise( 
  group_by(Births2, wday),  
    mean = mean(births), 
    min = min(births), 
    max = max(births),
    total = sum(births))
```

```
Source: local data frame [7 x 5]

   wday mean  min   max  total
1   Sun 7951 7135  8711 421400
2   Mon 9371 7527 10414 487309
3  Tues 9709 8433 10711 504858
4   Wed 9498 8606 10703 493897
5 Thurs 9484 7915 10499 493149
6   Fri 9626 8892 10438 500541
7   Sat 8309 7527  9170 432085
```

group_by() -- chaining syntax
==========

```r
Births2 %>%
  group_by(wday) %>%
  summarise(mean = mean(births), 
    min=min(births), max = max(births)) %>%
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

Data from UCSC Genome Bioinformatics
======================================

The example below queries a UCSC Genome Bionformatics to find information
about genes.

* [main web site](https://genome.ucsc.edu/)
    * [table browser](https://genome.ucsc.edu/cgi-bin/hgTables?org=human)
    * [UCSC SQL info](https://genome.ucsc.edu/goldenPath/help/mysql.html)

* `RMySQL` package  -- allows access to data in `MySQL` databases
* `dplyr` package -- process SQL just like data frame

Connecting to the database
=================

```r
# connect to a UCSC database
UCSCdata <- src_mysql(
  host="genome-mysql.cse.ucsc.edu",
  user="genome",
  dbname="mm9")
# grab one of the many tables in the database
KnownGene <- tbl(UCSCdata, "knownGene")
```

Grabbing Chromosome 1 data
===========================

```r
# Get the gene name, chromosome, start and end sites for genes on Chromosome 1
Chrom1 <-
  KnownGene %>% 
  select( name, chrom, txStart, txEnd ) %>%
  filter( chrom == "chr1" )
```


```r
Chrom1 %>% mutate(length=(txEnd - txStart)/1000) -> Chrom1l
Chrom1l
```

```
Source: mysql 5.6.10-log [genome@genome-mysql.cse.ucsc.edu:/mm9]
From: knownGene [3,056 x 5]
Filter: chrom == "chr1" 

         name chrom txStart   txEnd  length
1  uc007aet.1  chr1 3195984 3205713   9.729
2  uc007aeu.1  chr1 3204562 3661579 457.017
3  uc007aev.1  chr1 3638391 3648985  10.594
4  uc007aew.1  chr1 4280926 4399322 118.396
5  uc007aex.2  chr1 4333587 4350395  16.808
6  uc007aey.1  chr1 4481008 4483816   2.808
7  uc007aez.1  chr1 4481008 4486494   5.486
8  uc007afa.1  chr1 4481008 4486494   5.486
9  uc007afb.1  chr1 4481008 4486494   5.486
10 uc007afc.1  chr1 4481008 4486494   5.486
..        ...   ...     ...     ...     ...
```

Caution: Chrom1 is not a data frame
=========

```r
class(Chrom1)
```

```
[1] "tbl_mysql" "tbl_sql"   "tbl"      
```
* For efficiency, the full data are not pulled from the database until needed 

* the `mutate()` command can only use a limited set of operations
(those available in SQL) -- can't do `log()` inside `mutate()`, for example

Collecting the data
===================
* We can `collect()` the data into data frame.  

* Necessary for plotting


```r
Chrom1df <- collect(Chrom1l)       # collect into a data frame
histogram( ~length, data=Chrom1df, xlab="gene length (kb)" )
```

![plot of chunk unnamed-chunk-19](dplyrLessVolume-figure/unnamed-chunk-19.png) 


Baby Names
===========


```r
# fetch these from http://www.calvin.edu/~rpruim/data/BabyNames.rda
load("../../Data/BabyNames.rda")  # your path will be different
head(BabyNames, 2)
```

```
  name sex count year
1 Mary   F  7065 1880
2 Anna   F  2604 1880
```

Adding in the ranks
===================
Let's add the rank of each name (within sex) for each year.

Give it a try.

Adding in the ranks
===================


```r
BabyNamesWithRank <- 
  BabyNames %>%
    group_by(year, sex) %>%
    mutate(rank = rank(-count))
head(BabyNamesWithRank)
```

```
Source: local data frame [6 x 5]
Groups: year, sex

       name sex count year rank
1      Mary   F  7065 1880    1
2      Anna   F  2604 1880    2
3      Emma   F  2003 1880    3
4 Elizabeth   F  1939 1880    4
5    Minnie   F  1746 1880    5
6  Margaret   F  1578 1880    6
```

Explore Your Name
=================

How has the rank of your name changed over time?  Create a plot.

Explore Your Name
=================


```r
BabyNamesWithRank %>% 
  filter( name=="Randy" & sex=="M" ) -> Randy
xyplot( rank ~ year, data = Randy)
```

![plot of chunk unnamed-chunk-22](dplyrLessVolume-figure/unnamed-chunk-22.png) 
