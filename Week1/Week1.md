---
title       : 
subtitle    : Vectors, Data Frames, Subsetting
author      : 
job         : 
framework   : io2012        # {io2012, html5slides, shower, dzslides, ...}
highlighter : highlight.js  # {highlight.js, prettify, highlight}
hitheme     : tomorrow       # 
widgets     : [mathjax]           # {mathjax, quiz, bootstrap}
mode        : selfcontained # {standalone, draft}
knit        : slidify::knit2slides
---

# This Week...

* Data Types
* Vectors
* Data Frames 
* Subsetting

---

# Data Types in R

* Numeric (real numbers) 
* Character (strings of text)
* Logical - (`TRUE` and `FALSE` values)

---

# Vectors

* Vectors are the most basic data type in R
* They can only hold only one type of data

You can create vectors using the command `c`.  

```r
x <- c(3, 4, 8, 2)
```
You can create sequences of numbers using `:`

```r
1:6
```

```
## [1] 1 2 3 4 5 6
```
You can name elements of your vector using the `names()` function

```r
names(x) <- c("a", "b", "c", "d")
```

---

What is the class of the following vectors?

```r
c(1, 2, 5, TRUE, FALSE)
```


```r
c(1, 2, "a", 8, TRUE, "c")
```


---

# Methods of Subsetting

* Logical (logical	vector	where	TRUEs	denote	the	
elements	in	the	subset)
* Position (integer	vector	with	the	positions	of	the	
elements	in	the	subset)
* Exclusion (vector	of	negative	integers	for	
positions	to	exclude	from	subset)
* Inclusion (character	vector	of	names	of	elements	in	the	subset, assuming object has named elements)

---

# Subsetting a Vector
We have the following named vector x

```r
x
```

```
## a b c d 
## 3 4 8 2
```

You can pick out certain elements of the vector using square brackets

How can we subset to get only the first and third elements of our vector `x`?

---


```r
x[c(1, 3)] #Subsetting by Position
```

```
## a c 
## 3 8
```

```r
x[-c(2,4)] #Subsetting by Exclusion
```

```
## a c 
## 3 8
```

```r
x[c("a", "c")] #Subsetting by Names
```

```
## a c 
## 3 8
```

```r
x[c(TRUE, FALSE, TRUE, FALSE)] #Subsetting by Logicals
```

```
## a c 
## 3 8
```

---

# Data Frame
* The most common way of storing data in R is the data frame 
* Consists of a list of named equal-length vectors
* We can make a data frame, using the `data.frame()` command.  

```r
df <- data.frame(x = seq(1, 3), y = c("a", "b", "c"), z = c(T, F, F), stringsAsFactors = F)
df
```

```
##   x y     z
## 1 1 a  TRUE
## 2 2 b FALSE
## 3 3 c FALSE
```

---

# Some commands for data frames and other 2-D Objects

* `nrow` and `ncol` give the number of rows and columns
* `rownames()` and `colnames()` act like `names`, but for the dimensions
* You can combine 2-D objects with `cbind` and `rbind`


```r
x <- data.frame("a" = c(1,2), "b" = c(2,3), "c" = c(3,4))
x
```

```
##   a b c
## 1 1 2 3
## 2 2 3 4
```

```r
y <- data.frame("d" = c(4,5), "e" = c(5,6)) 
y
```

```
##   d e
## 1 4 5
## 2 5 6
```

---


```r
cbind(x,y)
```

```
##   a b c d e
## 1 1 2 3 4 5
## 2 2 3 4 5 6
```


---

# Subsetting an element from a data frame
Suppose we have the following data frame

```r
df
```

```
##   x y     z
## 1 1 a  TRUE
## 2 2 b FALSE
## 3 3 c FALSE
```

How can we access the last element (in the bottom right position)?

---

We can access a specific element in a data frame using `[]`, indexed by row and column

```r
df[3, 3] #Subsetting by Position
```

```
## [1] FALSE
```

```r
df[3, "z"] #Subsetting by Name
```

```
## [1] FALSE
```

```r
df[-c(1,2), -c(1,2)] #Subsetting by Exclusion
```

```
## [1] FALSE
```

```r
df[c(FALSE, FALSE, TRUE), c(FALSE, FALSE, TRUE)] #Subsetting by Logicals
```

```
## [1] FALSE
```

---

We can access a particular column of a data frame using `[]` or `$`
Recall our data frame `df`:

```r
df
```

```
##   x y     z
## 1 1 a  TRUE
## 2 2 b FALSE
## 3 3 c FALSE
```


```r
df$y
```

```
## [1] "a" "b" "c"
```

```r
df[ , 2]
```

```
## [1] "a" "b" "c"
```

```r
df[ , "y"]
```

```
## [1] "a" "b" "c"
```

---

# Sort vs. Order

Suppose we have the following vector `x`:

```r
x <- c(13, 6, 18, 2, 11)
sort(x)
```

```
## [1]  2  6 11 13 18
```

```r
order(x)
```

```
## [1] 4 2 5 1 3
```

`sort` sorts the values of a vector in increasing order

`order` gives the _indices_ (positions) of the values in increasing order

Note: For some vector x, `sort(x)` gives the same result as `x[order(x)]`
