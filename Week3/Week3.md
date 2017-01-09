---
title       : 
subtitle    : Lists, matrices, arrays, and the apply functions
author      : 
job         : 
framework   : io2012        # {io2012, html5slides, shower, dzslides, ...}
highlighter : highlight.js  # {highlight.js, prettify, highlight}
hitheme     : tomorrow      # 
widgets     : [mathjax]            # {mathjax, quiz, bootstrap}
mode        : selfcontained # {standalone, draft}
knit        : slidify::knit2slides
---

## This week...

* Lists
* Matrices
* Arrays
* `lapply()`, `sapply()`, and `apply()`

---

## Lists
Lists, unlike vectors, can contain more than one data type. We create lists using the `list()` command.


```r
list("a", 1, FALSE, 5 + 3i)
```

```
## [[1]]
## [1] "a"
## 
## [[2]]
## [1] 1
## 
## [[3]]
## [1] FALSE
## 
## [[4]]
## [1] 5+3i
```

---

# More list examples


```r
# List with lists
list("fruits", c("apples", "bananas", "cranberries"), list("other things"))
```

```
## [[1]]
## [1] "fruits"
## 
## [[2]]
## [1] "apples"      "bananas"     "cranberries"
## 
## [[3]]
## [[3]][[1]]
## [1] "other things"
```

```r
# Named list
list(fruits = c("apples", "bananas", "cranberries"))
```

```
## $fruits
## [1] "apples"      "bananas"     "cranberries"
```

---

# Consider the following list:


```r
mylist <- list('first' = data.frame(col1 = letters[1:2], col2 = letters[3:4]), 
          'second' = c("e", "f", "g"), 
          'third' = list(object1 = "h", object2 = "i"))
mylist
```

```
## $first
##   col1 col2
## 1    a    c
## 2    b    d
## 
## $second
## [1] "e" "f" "g"
## 
## $third
## $third$object1
## [1] "h"
## 
## $third$object2
## [1] "i"
```

---

## Single v. Double Brackets


```r
mylist[1]
```

```
## $first
##   col1 col2
## 1    a    c
## 2    b    d
```

```r
class(mylist[1])
```

```
## [1] "list"
```

Subsetting a list using single brackets will always return another list. We can consider this a sub-list of our original list containing only the elements we specified.

In this case, our sub-list contains only one element: a data frame.

---

## Single v. Double Brackets


```r
mylist[[1]]
```

```
##   col1 col2
## 1    a    c
## 2    b    d
```

```r
class(mylist[[1]])
```

```
## [1] "data.frame"
```

Whereas subsetting with single brackets returns a list of specified elements, subsetting with double brackets will return any specified element.

In this case, subsetting with double brackets returns the data frame, rather than a list containing the data frame.

Note: Double brackets allow only a single element to be selected by position or name, whereas single brackets allow indexing by vectors.

---

## Accessing items in a list
There are two main methods of extracting elements from a list:

* By position, with `[[]]`
* By name (if it's named) with `$` or `[[]]`

---

# How can we extract the vector from the second element (named 'second') of `mylist`?


```r
mylist[[2]] #by position
```

```
## [1] "e" "f" "g"
```

```r
mylist$second #by name (using $)
```

```
## [1] "e" "f" "g"
```

```r
mylist[["second"]] #by name (using [[ ]])
```

```
## [1] "e" "f" "g"
```

---

## Accessing Nested Items

A unique feature of lists is that lists are recursive objects. That is, you can have a list stored as an element inside of another list. 

How can we access 'object2' from the third element of `mylist`?


```r
mylist$third[[2]] #by position
```

```
## [1] "i"
```

```r
mylist$third$object2 #by name (using $)
```

```
## [1] "i"
```

```r
mylist$third[["object2"]] #by name (using [[ ]])
```

```
## [1] "i"
```

---

## Accessing and Assigning Names in a list
Similar to vectors, the `names` function can be used to get and change the names in a list


```r
x <- list('first' = "a", 'second' = "b", 'third' = "c")
names(x)
```

```
## [1] "first"  "second" "third"
```

```r
names(x) <- c("apples", "bananas", "cherries")
x
```

```
## $apples
## [1] "a"
## 
## $bananas
## [1] "b"
## 
## $cherries
## [1] "c"
```

---

## Matrices 
Matrices and arrays are n-dimensional generalizations of vectors. Like vectors, matrices can only hold one data type.

We can create a matrix by using the command `matrix()` and specifying a vector of numbers (or characters), the number of rows (and/or columns), and whether to fill the matrix by row or column. 


```r
m <- matrix(1:6, nrow = 2, byrow = FALSE)
m
```

```
##      [,1] [,2] [,3]
## [1,]    1    3    5
## [2,]    2    4    6
```

Matrices behave a lot like data frames in terms of naming and subsetting.

---

## Accessing and assigning names of a matrix

Similar to data frames, we can use the `rownames()` and `colnames()` function to access and assign names to the rows and columns of our matrix.

```r
m
```

```
##      [,1] [,2] [,3]
## [1,]    1    3    5
## [2,]    2    4    6
```

```r
rownames(m) <- c("r1", "r2")
colnames(m) <- c("c2", "c2", "c3")
m
```

```
##    c2 c2 c3
## r1  1  3  5
## r2  2  4  6
```

---

## Subsetting a Matrix

Again, similar to data frames, we can subset a matrix using the four methods of subsetting we learned for data frames

* Logical 
* Position
* Exclusion 
* Inclusion 

Suppose we wanted to extract the element $5$ out of our matrix `m`


```r
m
```

```
##    c2 c2 c3
## r1  1  3  5
## r2  2  4  6
```

---

## Subsetting a Matrix


```r
m[1, 3] #Subsetting by Position
```

```
## [1] 5
```

```r
m["r1", "c3"] #Subsetting by Name
```

```
## [1] 5
```

```r
m[-2, -c(1,2)] #Subsetting by Exclusion
```

```
## [1] 5
```

```r
m[c(TRUE, FALSE), c(FALSE, FALSE, TRUE)] #Subsetting by Logicals
```

```
## [1] 5
```

---

## Arrays
We can create an array by using the `array()` function and specifying a vector of numbers (or characters), the dimensions of the array (rows, numbers, and pages), and (optional) the names for the dimensions.

The third dimension of an array, typically called a page, acts like a matrix and can be treated as such.

---


```r
a <- array(1:24, 
           dim = c(4, 3, 2),
           dimnames = list(c("one", "two", "three", "four"), 
                           c("ein", "zwei", "drei"),
                           c("un", "deux")))
a
```

```
## , , un
## 
##       ein zwei drei
## one     1    5    9
## two     2    6   10
## three   3    7   11
## four    4    8   12
## 
## , , deux
## 
##       ein zwei drei
## one    13   17   21
## two    14   18   22
## three  15   19   23
## four   16   20   24
```

---

## Subsetting an Array

Suppose we wanted to extract the element $21$ out of our array `a`

```r
a[1, 3, 2] #Subsetting by Position
```

```
## [1] 21
```

```r
a["one", "drei", "deux"] #Subsetting by Name
```

```
## [1] 21
```

```r
a[-c(2, 3, 4), -c(1,2), -1] #Subsetting by Exclusion
```

```
## [1] 21
```

```r
a[c(T, F, F, F), c(F, F, T), c(F, T)] #Subsetting by Logicals
```

```
## [1] 21
```


---

## Apply functions and when to use them

When do you use the `apply()` family?  Almost anytime you would want to loop in R!  If your work involves iteratively creating a list, vector, dataframe, etc, then you should be using a `lapply` type of a function.  Some EXCEPTIONS:

1. Don't use `lapply` when the loop is recursive.  i.e. New elements depend on previous ones
2. You don't actually want output (e.g. you only want print statements)

---

## Example

Imagine you've loaded a data file, like the one below, that uses $-99$ to represent missing values. You want to replace all the $-99$s with `NA`s.


```
##    a  b c   d   e f
## 1  1  6 1   5 -99 1
## 2 10  4 4 -99   9 3
## 3  7  9 5   4   1 4
## 4  2  9 3   8   6 8
## 5  1 10 5   9   8 6
## 6  6  2 1   3   8 5
```

---

## First Response

We can solve this problem using repeated subsetting and assignment


```r
df$a[df$a == -99] <- NA
df$b[df$b == -99] <- NA
df$c[df$c == -99] <- NA
df$d[df$d == -99] <- NA
df$e[df$e == -99] <- NA
df$f[df$f == -99] <- NA
```

---

## Functions

We could write a function, removing the chance of messing up the -99:


```r
fix_missing <- function(x) {
  x[x == -99] <- NA
  x
}

fix_missing(df$a)
```

```
## [1]  1 10  7  2  1  6
```

---

## Repeated Function Calls

This doesn't solve having to call the function on every column though:


```r
df$a <- fix_missing(df$a)
df$b <- fix_missing(df$b)
df$c <- fix_missing(df$c)
df$d <- fix_missing(df$d)
df$e <- fix_missing(df$e)
df$f <- fix_missing(df$f)
```

---

## `lapply()` solution


```r
df <- lapply(df, fix_missing)
df
```

```
## $a
## [1]  1 10  7  2  1  6
## 
## $b
## [1]  6  4  9  9 10  2
## 
## $c
## [1] 1 4 5 3 5 1
## 
## $d
## [1]  5 NA  4  8  9  3
## 
## $e
## [1] NA  9  1  6  8  8
## 
## $f
## [1] 1 3 4 8 6 5
```

---

## Lapply

The most basic form of the apply family of functions is `lapply()`.  It takes a list and returns a list.  If you give it any other object, it will run `as.list` on it first.

```
lapply(X = list_like_object, FUN, ...)
```

The `...` allows you to pass arguments to the function `FUN`.

---

## Example

Suppose we want to know how many players are in each team in this list

```r
players <- list(
warriors = c('curry', 'iguodala', 'thompson', 'green'),
cavaliers = c('james', 'shumpert', 'thompson'),
rockets = c('harden', 'howard')
)

lapply(players, length)
```

```
## $warriors
## [1] 4
## 
## $cavaliers
## [1] 3
## 
## $rockets
## [1] 2
```

---

## Sapply

`sapply` is very similar to `lapply`.  The only difference is that instead of returning a list, `sapply` attempts to simplify the output down to a vector, matrix, or array if possible.


```r
df <- data.frame(a = 1:3, b = 4:6)
lapply(df, median)
```

```
## $a
## [1] 2
## 
## $b
## [1] 5
```

```r
sapply(df, median)
```

```
## a b 
## 2 5
```


---

## Apply

The `apply` function can be used with matrices, arrays, and data frames and works in almost the same way as the `lapply` function, except it allows us to specify the dimension over which we want to apply our function.

```
apply(X = n_dim_object, MARGIN, FUN, ...)
```

Based on the standard ordering of dimensions, use:
* 1: row
* 2: column
* 3: pages

---

## Example 

Suppose we have the following data frame, and we want to find the mean value of each row, ignoring the NA values. 


```r
df <- data.frame(a = 1:3, b = 4:6, c = c(7, NA, 9))
df
```

```
##   a b  c
## 1 1 4  7
## 2 2 5 NA
## 3 3 6  9
```

Recall: we can pass arguments into the function using the `...`.  

---

## Example



```r
apply(df, 1, mean, na.rm = TRUE)
```

```
## [1] 4.0 3.5 6.0
```

---

## Anonymous functions

The `FUN` in the `lapply` function call doesn't have to be a base `R` function.  It can be one that we've written up ourselves or even a nameless (anonymous) one that you write inside the `lapply` call itself. Reserve anonymous functions for short expressions.  If it takes over a line of code, define the function outside `lapply` for readability.


```r
# Note that the function doesn't take on a name
lapply(1:3, function(x) x^2)
```

```
## [[1]]
## [1] 1
## 
## [[2]]
## [1] 4
## 
## [[3]]
## [1] 9
```

---

## Subsetting with lapply

Recall our variable `mylist`. Suppose we only wanted to subset out only the second element of each element inside of `mylist`


```r
mylist
```

```
## $first
##   col1 col2
## 1    a    c
## 2    b    d
## 
## $second
## [1] "e" "f" "g"
## 
## $third
## $third$object1
## [1] "h"
## 
## $third$object2
## [1] "i"
```

---

We can write an anonymous function inside of out `lapply` call that will subset out only the second elements


```r
lapply(mylist, function(x) x[2])
```

```
## $first
##   col2
## 1    c
## 2    d
## 
## $second
## [1] "f"
## 
## $third
## $third$object2
## [1] "i"
```


