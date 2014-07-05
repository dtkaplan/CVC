----
title: "RStudio Introduction"
author: "Randy Pruim"
date: "May 14, 2013"
output: html_document
---


RStudio Orientation
========================================================

* Panes (4 with varying layout)
* Tabs (within a frame)

It All looks the Same
========================================================

Web version and desktop version look and act very similarly.

In the web version, students see the same interface regardless of what computer they are working on.


Console
========================================================

Randy moved to the top because, on a projector in a classroom, it's easier for students to see.

How to make the font bigger: 
* in a browser
* in preferences

Starting Out
===============================

Tell students just to pretend that it's their TI calculator.

"If you can use a TI calculator, you can learn to use R"

It takes a bit of time to get started.  When you start with a TI calculator, you can't do anything for the first week until you learn.  R will take time to learn as well, but not long and it wil be worth it.

Assignment
===============================


```r
product <- 3*8
```

Other form of assignment, `=`

Nick adds: Important to figure out what the minimal set of operations is.

Pedagogical point: Best not to mix and match.


Arrow Keys
================================

Use the arrow keys to recall previous commands

There's also a history tab in RStudio.

Pedagogical tip: Put the history tab at the top and leave it visible, so students can see the command that was just typed.

Pedagogical tip: All the commands from the class are distributed in a history file that the students can read.

Can use search in history command.  [Need an exercise for these things]

Arithmetic and basic functions
===============================


```r
log(10)
```

```
[1] 2.303
```

```r
log10(10)
```

```
[1] 1
```

```r
log(10,base=10)
```

```
[1] 1
```


Functions names and a bit of syntax: parentheses, commas between arguments, named arguments.  You can't get around using functions and assignment.
* Always the same: name, open paren, comma separated list of inputs (which may or may not have names), close paren.
* Typically a slot or two at the beginning without names, then, depending on the function, a set of names specific to that function.
* Programmers have tried to make sensible choices for related functions so that you can guess
* Hit tab after the open paren and a list of the arguments will show up in a tool tip

More information about a function
================================

Type ? followed by the name of the function.  A help page will appear.
* Argument explanations
* See also sections
* Examples:
    * Run examples with `example(log)`
    
Documentation is written by people.  Some write lovely documentation, some is "almost damaging".

Packages
===============================

Much of the functionality is provided by "packages".  
* On our server, several of the most widely used packages are pre-loaded.  
* Many (thousands) of packages available via CRAN, bioconductor, and other sources.

Pedagogical point: Pick a handful of packages that you use all the time.

In RStudio, use Packages pane.  Checkboxes show which ones are already loaded.

MOSAIC Package
================================

You'll want to use this.

A collection of ideas that we've tried to make systematic to make beginning statistics with R more systematic.
* Also calculus!
* mosaic also loads the `lattice` package for graphics.

Make your first plot
=================================


```r
histogram( ~ eruptions, data=faithful )
```

```
Error: could not find function "histogram"
```


* Function name `histogram`
* 

Graphics systems
==================================

We'll recommend that you use `lattice`.

* Base plots are good for other purposes, 
* ggplot2 is good for other purposes (like writing)

Files
==================================

There's a file system on the cloud, on the RStudio server.

File browser that's something like what's on your own computer.

"RStudio is Facebook for statistics."  If you want to use Facebook, you can leave and come back to it on another computer; it's in the cloud.

Getting files into your RStudio account is a two stage process:

1. Upload your file from the source to the cloud/server under your file system.
2. Import the file on your filesystem

QUESTION Server versus Desktop
==============================

Should you use RStudio server for students.

* Yes, if you have an IT person who can read basic documentation.

* For scores of students, contact Danny (Kaplan@macalester.edu) to use our server for your first experience.

Files in your directory
=======================

`USCOTS13materials` is a shared directory

Workspace
============================

Type the name of an object to see what it is.

DTK adds: Instructors should know about `class()` to help them see what's going on.

Opening a File
============================

R Markdown, R Sweave (Latex and R), R scripts

FILE/New File/[pick the file type]

R Script
=============================

Open a file where you can type R code.  
   3+8
   histogram( ~ eruptios, data=faithful )

* Hit "Run" to run the current line
* "Source" will run the whole file.

Three workflows for commands
============================

1. Type live in the console
2. store commands in R Script file
3. RMarkdown

============================


Things Randy didn't yet do
=========================

Incomplete expressions
=========================

What's a complete expression?

The continuation prompt

Escape to get out of the incomplete expression.

Apropos
=================================
Type ?? followed by some word and a list of relevant functions will appear

Other features
=================================

* How to control the number of digits printed.
* Built-in data frames and `head()`, `favstats()`

QUESTIONS
==================================

Why does the quantity you want the mean of go after the tilde?
