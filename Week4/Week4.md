---
title       : 
subtitle    : Writing Functions
author      : 
job         : 
framework   : io2012        # {io2012, html5slides, shower, dzslides, ...}
highlighter : highlight.js  # {highlight.js, prettify, highlight}
hitheme     : tomorrow      # 
widgets     : [mathjax]            # {mathjax, quiz, bootstrap}
mode        : selfcontained # {standalone, draft}
knit        : slidify::knit2slides
---


## Functions

R has many built-in functions like `mean()`, `class()`, and `quantile()`, each with its own specified set of arguments.

`function()` allows you to define your own function in R with specified arguments. The structure of a basic function will look something like this:

```
function_name <- function(arg1, arg2) {
  expression1
  expression2
  expression3
  return_statement
}
```

---

## Example
Here is a simple squaring function:

```r
SqPlusOne <- function(x) {
  y <- x^2
  y + 1
}

SqPlusOne(1:5)
```

```
## [1]  2  5 10 17 26
```
* `Square` is the __name__ of the function
* `x` is an __argument__ of the function
* `y <- x^2` is an expression
* The two lines between the curly braces form the __body__ of the function.

---

## Steps to Writing a Function

There are five steps to writing a function:

1. Explain: Describe the task in words
2. Concrete: Write code for a specific example
3. Abstract: Identify the variables and decide if they are required or have defaults
4. Encapsulate: Wrap the code into a function where the parameters are the general variables
5. Test: Check the function works as expected with your original data AND try the function on test cases with other data 

---

## Step 1: Explain

We want to write a function called `CylinderVol` that takes in two arguments:

* r: radius of the base circle
* h: height of the cylinder

The function should return the volume of a cylinder with the given radius and height. 

---

## Step 2: Concrete 

Suppose we have a cylinder with radius 2 and height 1. 

We know that the volume of a cylinder is given by $Volume = \pi r^2 h$ 

Plugging in values to our formula, we get that the volume of the cylinder is equal to:

```r
pi * 2^2 * 1
```

```
## [1] 12.56637
```

---

## Step 3: Abstract

We need two variables in order to calculate the volume of a cylinder: 

1. radius - which we will denote by r
2. height - which we will denote by h

We can make these required variables OR we can specify default values for these. For purposes of this example, we will specify default values for them, namely 1 and 1. 

---

## Step 4: Encapsulate

We now write a generalized function that takes in two variables, r and h, with default values 1 and 1, that will return the volume of a cylinder with specified radius and height.


```r
CylinderVol <- function(r = 1, h = 1) {
  pi * r^2 * h
}
```

---

## Step 5: Test

Recall: From our concrete example, we found that the volume of a cylinder with radius 2 and height 1 is equal to 12.5663706

Let's test our function:

```r
CylinderVol(r = 2, h = 1)
```

```
## [1] 12.56637
```

```r
CylinderVol(r = 1, h = 1)
```

```
## [1] 3.141593
```

---

## Specifying Arguments

You can call on a function using the argument names

```r
CylinderVol(r = 1, h = 2)
```

```
## [1] 6.283185
```

If you don't specify argument names, `R` will then assume that the arguments are ordered.

```r
CylinderVol(1, 2)
```

```
## [1] 6.283185
```

---

## Specifying Arguments

Using argument names lets you declare them out of order:

```r
CylinderVol(h = 2, r = 1)
```

```
## [1] 6.283185
```

Be careful not to list arguments out of order without specifying the names.

```r
CylinderVol(2, 1)
```

```
## [1] 12.56637
```

---

## Default v. Required Arguments

We set `1` and `1` as the __default values__ of `r` and `h`, respectively.  If not explicitly stated in a function call, `R` will assume those values.


```r
CylinderVol()
```

```
## [1] 3.141593
```

---

## Default v. Required Arguments

Suppose we had written the function `CylinderVol` making `r` and `h` required values. Then, not specifying the arguments in the function call will return an error. 


```r
CylinderVol <- function(r, h) {
  pi * r^2 * h
}

CylinderVol()
```

```
## Error in CylinderVol(): argument "r" is missing, with no default
```

---

## Your Turn

We want to write a function called SubsetMean that takes in:

* x: a numeric vector
* upper.bound: a value by which you want to subset out all elements in x greater than or equal to this value (give this a default value of 10)

The function should return:

* the mean value of all elements in the vector less than upper.bound

---


```r
SubsetMean <- function(x, upper.bound = 10) {
  subset <- x[x < upper.bound]
  subset.mean <- mean(subset)
  return(subset.mean)
}

SubsetMean(1:15)
```

```
## [1] 5
```

```r
SubsetMean(c(1,3,5,7,9,NA,11,13,15))
```

```
## [1] NA
```

Note: if our specified vector x has any NA elements, then SubsetMean will return NA. This is a default setting of the `mean()` function that can be overwritten with the argument `na.rm = TRUE`. 

---

## The ... argument

The `...` argument in a function allows the function to accept additional arguments when it is called and these arguments can be passed along in a function call inside our function.

We can specify this additional argument of the `mean()` function in our main function call `SubsetMean()` if we specify the `...` argument when writing the function.


```r
SubsetMean <- function(x, upper.bound = 10, ...) {
  subset <- x[x < upper.bound]
  subset.mean <- mean(subset, ...)
  return(subset.mean)
}

SubsetMean(c(1,3,5,7,9,NA,11,13,15), na.rm = TRUE)
```

```
## [1] 5
```



