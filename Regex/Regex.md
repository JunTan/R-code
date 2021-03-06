---
title       : 
subtitle    : Regular Expressions 
author      : 
job         : 
framework   : io2012        # {io2012, html5slides, shower, dzslides, ...}
highlighter : highlight.js  # {highlight.js, prettify, highlight}
hitheme     : tomorrow      # 
widgets     : []            # {mathjax, quiz, bootstrap}
mode        : selfcontained # {standalone, draft}
knit        : slidify::knit2slides
---


## Syntax

* __Literal__ characters are matched only by the character itself
* A __character class__ is matched by any single member of the specified class
* __Modifiers__ operate on literal characters, character classes, or combinations of the two. 

---

## Named Character Classes

Name      | Description
--------- | --------------------------------
[:alpha:] | all alphabetic
[:digit:] | digits 0123456789
[:alnum:] | all alphabetic and numeric
[:lower:] | lower case alphabetic
[:upper:] | upper case alphabetic
[:punct:] | punctuation characters
[:blank:] | blank characters (space or tab)

---

# Character Classes

* Square brackets denote character classes

* `[a-z]` matches any one lowercase letter
* `[a-zA-Z]` matches any one letter
* `[LATE]` matches one of the four letters `L`, `A`, `T`, `E`.
* `[LATE]{4}` matches `"LATE"`, but it also matches `"TEAL"`


```r
char_class <- "[LATE]{4}"
grepl(char_class, "TEALING")
```

```
## [1] TRUE
```

---

## Character classes

* All metacharacters mentioned lose their special meanings inside of character classes
* The only exception is `^`, which when placed at the beginning means "everything but the characters in this class"


```r
# Everything but the letters E, A, and T
neg_class <- "[^EAT]{3}"

# BAT contains A and T
grepl(neg_class, "BAT")
```

```
## [1] FALSE
```

```r
# Bat does not contain any of the specified characters 
grepl(neg_class, "Bat")
```

```
## [1] TRUE
```

---

## Qualifiers
The following special symbols, called __metacharacters__ have special meanings in regular expressions.

Qualifier | >=   | <=   | Description
--------- | ---- | ---- | ----------------------
?         | 0    | 1    | at most 1
+         | 1    | Inf  | at least 1
*         | 0    | Inf  | any number of
{m,n}     | m    | n    | between m and n
{,n}      | 0    | n    | at most n
{m,}      | m    | Inf  | at least m
{n}       | n    | n    | exactly n

---

## Other metacharacters
These symbols also represent something other than their _literal_ meaning:

Metacharacter | Meaning   | Example Usage  | Description
------------- | --------- | -------------- | -------------------
()            | group     | "(ham){3}"     | matches "hamhamham"
.             | anything  | ".{3}"         | matches any 3 characters (e.g "ham", "h@m", "h49")
^             | beginning | "\^ham"        | only matches "ham" if it appears at the beginning of the string
^ (inside []) | not       | "[\^[0-9]]{3}" | matches any 3 characters that are not numbers (i.e. "ham" but not "h4m")    
$             | end       | "ham$"         | only matches "ham" if it appears at the end of the string


* Note: a \^ as the first character in a pattern signifies "the beginning of the string", but a \^ as the first character in a character class [] signifies "not"

---

## Escaping Special Meanings

* R uses the double-slash `\\` to __escape__ special meanings
* Examples
  
  * "a.b" matches the letter a, then any character, then the letter b
  * "a\\\\.b" matches the literal string "a.b"
  * "a[.]b" also works since all metacharacters, with the except of of \^, lose their special meanings inside of []

---

# Functions
Function       | Description
-------------- | ---------------------------------------------
sub            | replaces first matched pattern
gsub           | replaces all matched patterns
grep           | returns indices of elements for which there was a match
grepl          | returns logical vector with `TRUE` denoting that the element contained a match
regexpr        | returns index of first match
gregexpr       | returns indices of all matches
substr         | splits string at matched pattern

