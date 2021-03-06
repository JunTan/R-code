---
title       : 
subtitle    : If-Else Statements and Loops
author      : 
job         : 
framework   : io2012        # {io2012, html5slides, shower, dzslides, ...}
highlighter : highlight.js  # {highlight.js, prettify, highlight}
hitheme     : tomorrow      # 
widgets     : []            # {mathjax, quiz, bootstrap}
mode        : selfcontained # {standalone, draft}
knit        : slidify::knit2slides
---


## If-Else Statements

If we want our script to perform certain operations under specific conditions, we can use if-else statements.  The basic construct looks like:

```r
if (condition1) {
  statement1
} else if (condition2) {
  statement2
} else if (condition3) {
  statement3
} else {
  statement4
}
```

---

## How If-Else Statements work

* `if()` takes a __logical__ condition
* The condition __must be of length one__ 
* It executes the next expression if the condition is true
* If the statement is false, it looks for an `else` clause to perform.  Otherwise it does nothing.

---

## Example

Suppose we write a function called `Check5` that takes in a numeric vector of length 1 (a single number) and returns `TRUE` if the number is less than 5 and `FALSE` otherwise.


```r
Check <- function(x) {
  if (x < 5) {
    return(TRUE)
  } else if (x > 5) {
    return(FALSE)
  }
} 
```

What would Check(5) return?

What would Check(1:5) return?

---

## Returning Values

Functions return values in an explicit return statement

```r
check <- function(x) {
   if (x>0) {
       result <- "Positive"
   }
   else if (x<0) {
       result <- "Negative"
   }
   else {
       result <- "Zero"
   }
   return(result)
}
Check(-2)
```

```
## [1] TRUE
```

---

## Returning Values

What happens if we don't include the return function?


```r
check <- function(x) {
   if (x>0) {
       result <- "Positive"
   }
   else if (x<0) {
       result <- "Negative"
   }
   else {
       result <- "Zero"
   }
}
check(-2)
```

If no explicit return statement is given, the last evaluated expression is returned.

Note: if the last evaluated expressions is saved into an object, the function will not return anything since we didn't call on the object.

---

## Returning Multiple Values

The `return()` function can return only a single object. If we want to return multiple values in R, we can use a list (or other objects) and return it.

```r
AddSubtract <- function(x, y) {
  results <- list(add = x + y, subtract = x - y)
  return(results)
}
AddSubtract(2, 1)
```

```
## $add
## [1] 3
## 
## $subtract
## [1] 1
```

---

## Stop

`stop()` halts function execution and returns an error message.  This is helpful when we want to make sure the user has given meaningful inputs.


```r
CheckEven <- function(x) {
  if (x != as.integer(x)) {
    stop("x is not a whole number")
  } else {
    return(x %% 2 == 0)
  }
}
CheckEven(1)
```

```
## [1] FALSE
```

```r
CheckEven(3.14159)
```

```
## Error in CheckEven(3.14159): x is not a whole number
```

---

## Warning

`warning()` returns a warning message, but does not halt function execution.  This is helpful in letting the user know that something unexpected has happened while still evaluating the function call without returning an error.

```r
Divide = function(x, y) {
  if (y == 0) {warning("Dividing by 0")} 
  return(x/y)
}
```


```r
Divide(1,0)
```

```
## Warning in Divide(1, 0): Dividing by 0
```

```
## [1] Inf
```

---

## For Loops

A for loop executes a set of actions over an index set. The basic construct of a for loop looks like:


```r
for(var in vector) {
  statement
}
```

---

## Example

For loops are typically used by setting the index set to be the indices of an object:


```r
x <- 1:10
for(i in 3:length(x)) {
  x[i] <- x[i-2] + x[i-1]
}
x
```

```
##  [1]  1  2  3  5  8 13 21 34 55 89
```

---

## Example

Though not as common, the for loop can loop over any set, not just indices.


```r
df <- data.frame(first_column = 1:3, second_column = c("a", "b", "c"), third_column = 7:9, stringsAsFactors = FALSE)
names(df)
```

```
## [1] "first_column"  "second_column" "third_column"
```


```r
for (name in names(df)) {
  print(df[[name]])
}
```

```
## [1] 1 2 3
## [1] "a" "b" "c"
## [1] 7 8 9
```

---

## Break

The `break()` function is used inside a loop to stop the iterations and program control resumes at the next statement following the loop.


```r
x <- letters[1:10]
for (i in 1:length(x)) {
  if (x[i] == "f") {break}
  print(x[i])
}
```

```
## [1] "a"
## [1] "b"
## [1] "c"
## [1] "d"
## [1] "e"
```


---

## While Loops

A while loop repeats a statement or block of statements for as many times as a particular condition is `TRUE`.

In the example below, our while loop continually divides some set number n by 2 until an odd number is reached.


```r
n <- 748192
while (n %% 2 == 0) {
  n <- n/2
}
n
```

```
## [1] 23381
```

---

## Initializing Values

Notice that it's often the case that you have some initial value for the loop.  Otherwise the object that you want to change using a loop wouldn't actually exist.  This applies to both for and while loops. 

Here, we initialize a counter variable that tells us how many times a number is divided by 2 before an odd number is reached.

```r
n <- 748192
counter <- 0
while (n %% 2 == 0) {
  n <- n/2
  counter <- counter + 1
}
counter
```

```
## [1] 5
```



