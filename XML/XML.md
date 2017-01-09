---
title       : XML
subtitle    : 
author      : 
job         : 
framework   : io2012        # {io2012, html5slides, shower, dzslides, ...}
highlighter : highlight.js  # {highlight.js, prettify, highlight}
hitheme     : tomorrow      # 
widgets     : []            # {mathjax, quiz, bootstrap}
mode        : selfcontained # {standalone, draft}
knit        : slidify::knit2slides
---




## XML Example


```r
<movie mins="126" lang="en">
  Good Will Hunting
</movie>
```

* one single element `movie`
* start-tag: `<movie>`
* end-tag: `</movie>`
* content: `Good Will Hunting`
* xml elements can have **attributes**
  + attributes: `mins` (minutes) and `lang` (language)
  + attributes are attached to the element's start tag
  + attribute values must be quoted

---

## XML Hierarchy Structure


```r
<Root>
  <child_1>...</child_1>
  <child_2>...</child_2>
    <subchild>...</subchild>
  <child_3>...</child_3>
</Root>
```

* An XML document can be represented with a __tree structure__
* An XML document must have one single `Root` element
* The `Root` may contain child elements
* A child element may contain subchild elements

---

## Well-Formedness

We say that an XML document is well-formed when it obeys the basic syntax rules of XML. Some of those rules are:
* one root element containing the rest of elements
* properly nested elements
* self-closing tags
* attributes appear in start-tags of elements
* attribute values must be quoted
* element names and attribute names are case sensitive

---

## XML Tree Structure

Each element can have:
* a name
* any number of attributes
* optional content
* other nested elements

Traversing the tree:

There is always a unique path from the root node to any given node

---

## XPath

XPath

* is a language for finding information in an XML document
* uses path expressions to select nodes or node-sets in an XML document
* works by identifying patterns to match data or content


We can specify paths through the tree structure:
* based on node names
* based on node content
* based on a node's relationship to other nodes

---

## Selecting Nodes

The main path expressions (i.e. symbols) are:

Symbol         | Description
-------------- | ---------------------------------------------
/              | selects from the root node
//             | selects from nodes anywhere
.              | selects from the current node
..             | selects from the parent of the current node
@              | selects attributes
[]             | square brackets indicate attributes

---

## XPath Predicates

* XPath Predicates (in square brackets []) allow you to find a specific node or nodes that contains a specific value
* You can use the usual comparison operators <, <=, etc
* A major difference is that equality is = and not ==
* In addition, the word 'or' is used in place of | 

---

## Selecting Unknown Nodes

XPath wildcards can be used to select unknown XML elements

Symbol         | Description
-------------- | ---------------------------------------------
*              | matches any element node
@*             | matches any attribute node
node()         | matches any node of any kind

---

## XML package

Handy functions for parsing XML:

Function       | Description
------------   | ---------------------------------------------
xmlParse       | read an XML file into R
xmlValue       | retrieve text content of a node (including content of all child nodes)
xmlSize        | return the number of child nodes
xmlName        | return the tag name of a node
xmlGetAttr     | return the attribute value of the specified attribute

---

## xmlApply, xmlSApply

* `xmlApply`: XML version of lapply; returns a list
* `xmlSApply`: XML version of sapply; returns a simpler data structure if possible
* Each takes an XMLNode object as its primary argument. They iterate over the node's children nodes, invoking the given function.

---

## Functions that take XPath expressions

* `getNodeSet(xmlTree, xpath expression)`
returns a list of XML nodes from `xmlTree` that satisfy the XPath expression

* `xpathSApply(xmlTree, xpath, function)`
the function (e.g. `xmlValue`) is applied to those nodes in the XML tree that satisfy the XPath expression. The return value is a vector when possible. 
  + `xpathApply` always returns a list.

---

## Example 1

Write the xpath that will select all jobs with `priority` corresponding to "critical" or "high".


```r
x <- xmlParse('<jobs>
  <job priority="critical" name="finish project" />
  <job priority="low" name="clear rubbish" />
  <job priority="low" name="pet cat" />
  <job priority="medium" name="eat" />
  <job priority="high" name="make phone call" />
</jobs>')
```

---

## Example 1


```r
getNodeSet(x, "//job[@priority='critical' or @priority='high']")
```

```
## [[1]]
## <job priority="critical" name="finish project"/> 
## 
## [[2]]
## <job priority="high" name="make phone call"/> 
## 
## attr(,"class")
## [1] "XMLNodeSet"
```

---

## Example 2

Write the xpath to select all persons older than 25.


```r
x <- xmlParse('<persons>
  <person firstName="Renan" lastName="Pazinni" age="28" />
  <person firstName="Richard" lastName="Wang" age="20" />
  <person firstName="Catherine" lastName="Ladha" age="24" />
  <person firstName="Trammy" lastName="Burgess" age="27" />
</persons>')
```

---

## Example 2


```r
getNodeSet(x, '//person[@age > 25]')
```

```
## [[1]]
## <person firstName="Renan" lastName="Pazinni" age="28"/> 
## 
## [[2]]
## <person firstName="Trammy" lastName="Burgess" age="27"/> 
## 
## attr(,"class")
## [1] "XMLNodeSet"
```

---

## Example 3

Write the xpath that will select all document elements beneath the node `linkList` with the name `A`.


```r
x <- xmlParse('<document xmlns:xlink="http://www.w3.org/1999/xlink">
  <linkList name="A">
    <document xlink:href="15024" />
    <document xlink:href="15028" />
  </linkList>
  <linkList name="B">
    <document xlink:href="15030" />
    <document xlink:href="15032" />
  </linkList>
</document>')
```

---

## Example 3


```r
getNodeSet(x, "//linkList[@name = 'A']/*")
```

```
## [[1]]
## <document xlink:href="15024"/> 
## 
## [[2]]
## <document xlink:href="15028"/> 
## 
## attr(,"class")
## [1] "XMLNodeSet"
```



