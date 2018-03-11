---
title: "Data Analytics and Visualizations in R - Exercises"
output:
  html_document:
    df_print: paged
    toc: true
    toc_depth: 4
    number_sections: true
    theme: lumen
    highlight: tango
    code_folding: show
    keep_md: true
  pdf_document:
    toc: true
    toc_depth: 4
    number_sections: true
---

# Basic R Data Structures

***

## Types

What are the scalar types in R?


```r
# Scalars are just vectors of length one
```


## Weirdness of R

What is the major difference between atomic vectors and lists? How can you turn a list into an atomic vector?

In order to check if an object is of a certain type you can use `is.[type](object)` ,e.g. `is.integer(object)`

Can you use the `is.vector()` function to understand whether a data structure is a vector? If
not, what are the functions that you can use for this purpose?


```r
# Atomic vectors can contain elements of the same type. The elements of a list can have different types.

# Yes, we can use the `is.vector()` function to understand is the structure is a vector. 
# It will return TRUE or FALSE.

a <- list(c(1,2,3), "bla", TRUE)
print(a)
```

```
## [[1]]
## [1] 1 2 3
## 
## [[2]]
## [1] "bla"
## 
## [[3]]
## [1] TRUE
```

```r
is.vector(a)
```

```
## [1] TRUE
```

```r
# Best response: If possible you should use type specific coercions like `as.numeric()` or `as.character()`. 
# But since lists are heterogenous, this might not work. A more general function is `unlist()`, 
# which returns the list into a vector of the most general type. Notice this difference:

a <- list(c(1,2,3), "bla", TRUE)
unlist(a)
```

```
## [1] "1"    "2"    "3"    "bla"  "TRUE"
```

```r
as.character(a)
```

```
## [1] "c(1, 2, 3)" "bla"        "TRUE"
```

```r
# Generally you can, but here comes the weird part: `is.vector()` will only return `TRUE` if the vector has 
# no attributes `names`. Therefore more specific functions like `is.atomic()` or `is.list()` functions should 
# be used to test if an object is actually atomic vectoror a list
```


## Atomic Vector Concatenation

What happens when you try to generate an atomic vector with `c()`which is composed of different types of elements? What is the `mean()` 
of a logical vector?


```r
# When we attempt to combine different types they will be coerced to the most flexible type. Types from least
# to most flexible are: logical, integer, double and character.

str(c("a", 1)) # 1 corced to char
```

```
##  chr [1:2] "a" "1"
```

```r
# As TRUE is encoded as 1 and FALSE as 0, the mean is the number of TRUEs devided by the vector length.
mean(c(TRUE, FALSE,FALSE))
```

```
## [1] 0.3333333
```


## Vector Concatenation

Compare X and Y where X and Y are defined as follows. What is the difference?


```r
x <- list(list(1,2), c(3,4))
y <- c(list(1,2), c(3,4))
```


```r
# Answer
# X will combine seveal lists into one. Given a combination of atomic vectors and lists, y will coerce the 
# vectors to lists before combining them.
str(x)
```

```
## List of 2
##  $ :List of 2
##   ..$ : num 1
##   ..$ : num 2
##  $ : num [1:2] 3 4
```

```r
str(y)
```

```
## List of 4
##  $ : num 1
##  $ : num 2
##  $ : num 3
##  $ : num 4
```

## From Vectors to `data.frames`

First, create three named numeric vectors of size 10, 11 and 12 respectively in the following
manner:

* One vector with the “colon” approach: *from:to*
* One vector with the `seq()` function: *seq(from, to)*
* And one vector with the `seq()` function and the by argument: *seq(from, to, by)*

For easier naming you can use the vector `letters` or `LETTERS` which contain the latin alphabet
in small and capital, respectively. In order to select specific letters just use e.g. `letters[1:4]`
to get the first four letters. 
Check their types. What is the outcome? Where do you think does the difference come from?

Then combine all three vectors in a list. Check the attributes of the vectors and the list.
What is the difference and why?

Finally coerce the list to a `data.frame` with `as.data.frame()`. Why does it fail and how can we fix it? 
What happend to the names?

Hint: If list elements have no names, we can access them with the double brackets and an
index, e.g. `my_list[[1]]`


```r
# Answer

# A. create vectors
aa <- 1:10
names(aa) <- letters[aa]
aa
```

```
##  a  b  c  d  e  f  g  h  i  j 
##  1  2  3  4  5  6  7  8  9 10
```

```r
bb <- seq(1, 11)
names(bb) <- letters[bb]
bb
```

```
##  a  b  c  d  e  f  g  h  i  j  k 
##  1  2  3  4  5  6  7  8  9 10 11
```

```r
cc <- seq(1, 12, by=1)
names(cc) <- letters[cc]

typeof(aa)
```

```
## [1] "integer"
```

```r
typeof(bb)
```

```
## [1] "integer"
```

```r
typeof(cc)
```

```
## [1] "double"
```

```r
# B. Combine all three vectors in a list
 
my_list <- list(aa, bb, cc)
my_list
```

```
## [[1]]
##  a  b  c  d  e  f  g  h  i  j 
##  1  2  3  4  5  6  7  8  9 10 
## 
## [[2]]
##  a  b  c  d  e  f  g  h  i  j  k 
##  1  2  3  4  5  6  7  8  9 10 11 
## 
## [[3]]
##  a  b  c  d  e  f  g  h  i  j  k  l 
##  1  2  3  4  5  6  7  8  9 10 11 12
```

```r
attributes(aa)
```

```
## $names
##  [1] "a" "b" "c" "d" "e" "f" "g" "h" "i" "j"
```

```r
attributes(bb)
```

```
## $names
##  [1] "a" "b" "c" "d" "e" "f" "g" "h" "i" "j" "k"
```

```r
attributes(cc)
```

```
## $names
##  [1] "a" "b" "c" "d" "e" "f" "g" "h" "i" "j" "k" "l"
```

```r
attributes(my_list)
```

```
## NULL
```

```r
# C. Coerce to data.frames

# my_df <- as.data.frame(my_list)# fails

# Fixing the length
my_list[[1]] <- c(my_list[[1]], NA, NA)
my_list[[2]] <- c(my_list[[2]], NA)

my_df <- as.data.frame(my_list)
names(my_df) <- LETTERS[1:3]
my_df
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":[""],"name":["_rn_"],"type":[""],"align":["left"]},{"label":["A"],"name":[1],"type":["int"],"align":["right"]},{"label":["B"],"name":[2],"type":["int"],"align":["right"]},{"label":["C"],"name":[3],"type":["dbl"],"align":["right"]}],"data":[{"1":"1","2":"1","3":"1","_rn_":"a"},{"1":"2","2":"2","3":"2","_rn_":"b"},{"1":"3","2":"3","3":"3","_rn_":"c"},{"1":"4","2":"4","3":"4","_rn_":"d"},{"1":"5","2":"5","3":"5","_rn_":"e"},{"1":"6","2":"6","3":"6","_rn_":"f"},{"1":"7","2":"7","3":"7","_rn_":"g"},{"1":"8","2":"8","3":"8","_rn_":"h"},{"1":"9","2":"9","3":"9","_rn_":"i"},{"1":"10","2":"10","3":"10","_rn_":"j"},{"1":"NA","2":"11","3":"11","_rn_":"k"},{"1":"NA","2":"NA","3":"12","_rn_":""}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>

## Attributes
Take again our `data.frame` from Question 5.

* Change the row names and the column names of the `data.frame` to capital letters (or small letters, if they 
are already capital.
* Change the `class` attribute to *list*. What happens?
* Change it now to any name you like. What happens now? What happens if you remove the class attribute


```r
# Answer
# A. One possible way through attributes

attributes(my_df)
```

```
## $names
## [1] "A" "B" "C"
## 
## $row.names
##  [1] "a" "b" "c" "d" "e" "f" "g" "h" "i" "j" "k" "" 
## 
## $class
## [1] "data.frame"
```

```r
attr(my_df, "names") <- letters[1:3]
attr(my_df, "row.names") <- LETTERS[1:12]
my_df
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":[""],"name":["_rn_"],"type":[""],"align":["left"]},{"label":["a"],"name":[1],"type":["int"],"align":["right"]},{"label":["b"],"name":[2],"type":["int"],"align":["right"]},{"label":["c"],"name":[3],"type":["dbl"],"align":["right"]}],"data":[{"1":"1","2":"1","3":"1","_rn_":"A"},{"1":"2","2":"2","3":"2","_rn_":"B"},{"1":"3","2":"3","3":"3","_rn_":"C"},{"1":"4","2":"4","3":"4","_rn_":"D"},{"1":"5","2":"5","3":"5","_rn_":"E"},{"1":"6","2":"6","3":"6","_rn_":"F"},{"1":"7","2":"7","3":"7","_rn_":"G"},{"1":"8","2":"8","3":"8","_rn_":"H"},{"1":"9","2":"9","3":"9","_rn_":"I"},{"1":"10","2":"10","3":"10","_rn_":"J"},{"1":"NA","2":"11","3":"11","_rn_":"K"},{"1":"NA","2":"NA","3":"12","_rn_":"L"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>

```r
# Or through accessor functions

names(my_df) <- LETTERS[1:3]
row.names(my_df) <- letters[1:12]
my_df
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":[""],"name":["_rn_"],"type":[""],"align":["left"]},{"label":["A"],"name":[1],"type":["int"],"align":["right"]},{"label":["B"],"name":[2],"type":["int"],"align":["right"]},{"label":["C"],"name":[3],"type":["dbl"],"align":["right"]}],"data":[{"1":"1","2":"1","3":"1","_rn_":"a"},{"1":"2","2":"2","3":"2","_rn_":"b"},{"1":"3","2":"3","3":"3","_rn_":"c"},{"1":"4","2":"4","3":"4","_rn_":"d"},{"1":"5","2":"5","3":"5","_rn_":"e"},{"1":"6","2":"6","3":"6","_rn_":"f"},{"1":"7","2":"7","3":"7","_rn_":"g"},{"1":"8","2":"8","3":"8","_rn_":"h"},{"1":"9","2":"9","3":"9","_rn_":"i"},{"1":"10","2":"10","3":"10","_rn_":"j"},{"1":"NA","2":"11","3":"11","_rn_":"k"},{"1":"NA","2":"NA","3":"12","_rn_":"l"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>

```r
# B. 

attr(my_df, "class") <- "list"
my_df
```

```
## $A
##  [1]  1  2  3  4  5  6  7  8  9 10 NA NA
## 
## $B
##  [1]  1  2  3  4  5  6  7  8  9 10 11 NA
## 
## $C
##  [1]  1  2  3  4  5  6  7  8  9 10 11 12
## 
## attr(,"row.names")
##  [1] "a" "b" "c" "d" "e" "f" "g" "h" "i" "j" "k" "l"
## attr(,"class")
## [1] "list"
```

```r
# Answer - the data.frame coerced to a list

# C
attr(my_df, "class") <- "Batman"
my_df
```

```
## $A
##  [1]  1  2  3  4  5  6  7  8  9 10 NA NA
## 
## $B
##  [1]  1  2  3  4  5  6  7  8  9 10 11 NA
## 
## $C
##  [1]  1  2  3  4  5  6  7  8  9 10 11 12
## 
## attr(,"row.names")
##  [1] "a" "b" "c" "d" "e" "f" "g" "h" "i" "j" "k" "l"
## attr(,"class")
## [1] "Batman"
```

```r
# Answer - Nothing changes
```

## Factors

* What is the difference between a Factor and a Vector?
* Create a vector of length 30 with three levels *Rita Repulsa, Lord Zedd* and *Rito Revolto* and equal length for each level
* What happens if you replace the second element of the vector with *Shredder*


```r
# Answer
# A. A factor is a vector that can contain only predefined values, and is used to store categorical data.
# It is stored as an integer with a character string associated with each integer value

# B.

x <- gl(n=3, k=10, length=30, labels=c("Rita Repulsa", "Lord Zedd", "Rito Revolto"))
str(x)
```

```
##  Factor w/ 3 levels "Rita Repulsa",..: 1 1 1 1 1 1 1 1 1 1 ...
```

```r
levels(x)
```

```
## [1] "Rita Repulsa" "Lord Zedd"    "Rito Revolto"
```

```r
attributes(x)
```

```
## $levels
## [1] "Rita Repulsa" "Lord Zedd"    "Rito Revolto"
## 
## $class
## [1] "factor"
```

```r
# C
x[2] <- "Shredder"
```

```
## Warning in `[<-.factor`(`*tmp*`, 2, value = "Shredder"): invalid factor
## level, NA generated
```

```r
# It doesn't work. We get the error 'NA generated'
```

