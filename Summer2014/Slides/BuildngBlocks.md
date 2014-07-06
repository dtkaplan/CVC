R Building Blocks
========
author: R Pruim
date: 2014 CVC Workshop

Everything is an object
=============


In R, 

* everything is an object
* every object has a  type/class 
    
    ```r
    class("hello")
    ```
    
    ```
    [1] "character"
    ```
    
    ```r
    class(HELPrct)
    ```
    
    ```
    [1] "data.frame"
    ```
    
    ```r
    class(class)
    ```
    
    ```
    [1] "function"
    ```
  
Some types of objects you should know about
===============

Character: text

```r
name <- "John Doe"
```
Numeric: numbers

```r
x <- 27.5
```

