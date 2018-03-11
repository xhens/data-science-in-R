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
# Atomic vectors can contain elements of the same type. The elements of a list 
# can have different types.

# Yes, we can use the `is.vector()` function to understand is the structure is a 
# vector. It will return TRUE or FALSE.

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
# Best response: If possible you should use type specific coercions like 
# `as.numeric()` or `as.character()`. But since lists are heterogenous, this 
# might not work. A more general function is `unlist()`,  which returns the list 
# into a vector of the most general type. Notice this difference:

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
# When we attempt to combine different types they will be coerced to the most 
# flexible type. Types from least to most flexible are: logical, integer, double
#  and character.

str(c("a", 1)) # 1 corced to char
```

```
##  chr [1:2] "a" "1"
```

```r
# As TRUE is encoded as 1 and FALSE as 0, the mean is the number of TRUEs 
# devided by the vector length.
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
# X will combine seveal lists into one. Given a combination of atomic vectors 
# and lists, y will coerce the vectors to lists before combining them.
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
# A. A factor is a vector that can contain only predefined values, and is used 
# to store categorical data. It is stored as an integer with a character string 
# associated with each integer value

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

## More fun with factors


```r
f1 <- factor(letters)
levels(f1) <- rev(levels(f1))
f2 <- rev(factor(letters))
f3 <- factor(letters, levels = rev(letters))
```

The function `rev` reverses the order of an orderable object. What is the difference between f1, f2 and f3? Why?


```r
# Answer
f1 <- factor(letters)
levels(f1) <- rev(levels(f1))

# f1 goes from a to z and when we apply the levels(f1), z will become 1 and a=26

f2 <- rev(factor(letters))

# f2 goes from z to a. but the levels are not changed.

f3 <- factor(letters, levels = rev(letters))

# f3 goes from a - z, but the underlying encoding goes from z = 1 to a = 26.  
# We create the vector with the letters a to z BUT the mapped integer structure 
# 26 to 1. Hence the levels but not the vector are reversed

f3
```

```
##  [1] a b c d e f g h i j k l m n o p q r s t u v w x y z
## Levels: z y x w v u t s r q p o n m l k j i h g f e d c b a
```

```r
# Reversing f3 will give f1

rev(f3)
```

```
##  [1] z y x w v u t s r q p o n m l k j i h g f e d c b a
## Levels: z y x w v u t s r q p o n m l k j i h g f e d c b a
```

## Creating data.frames

Create a data.frame with 26 rows like this: Only the first and the last six rows are shown.
Hint: Instead of the workaround with list you can also use simply `data.frame(column_name
= column_vector, ...)`


```r
aa <- seq(1:26)
bb <- seq(from=4, to=4*26, by=4)
cc <- rep(seq(1, 26, 2), each=2)
df <- data.frame(V1 = aa, V2 = bb, V3 = letters[cc])
head(df)
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":[""],"name":["_rn_"],"type":[""],"align":["left"]},{"label":["V1"],"name":[1],"type":["int"],"align":["right"]},{"label":["V2"],"name":[2],"type":["dbl"],"align":["right"]},{"label":["V3"],"name":[3],"type":["fctr"],"align":["left"]}],"data":[{"1":"1","2":"4","3":"a","_rn_":"1"},{"1":"2","2":"8","3":"a","_rn_":"2"},{"1":"3","2":"12","3":"c","_rn_":"3"},{"1":"4","2":"16","3":"c","_rn_":"4"},{"1":"5","2":"20","3":"e","_rn_":"5"},{"1":"6","2":"24","3":"e","_rn_":"6"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>

```r
tail(df)
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":[""],"name":["_rn_"],"type":[""],"align":["left"]},{"label":["V1"],"name":[1],"type":["int"],"align":["right"]},{"label":["V2"],"name":[2],"type":["dbl"],"align":["right"]},{"label":["V3"],"name":[3],"type":["fctr"],"align":["left"]}],"data":[{"1":"21","2":"84","3":"u","_rn_":"21"},{"1":"22","2":"88","3":"u","_rn_":"22"},{"1":"23","2":"92","3":"w","_rn_":"23"},{"1":"24","2":"96","3":"w","_rn_":"24"},{"1":"25","2":"100","3":"y","_rn_":"25"},{"1":"26","2":"104","3":"y","_rn_":"26"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>

## Combining `data.frames`

Now take the previous data.frame from Question 10 and reproduce the following `data.frame`.
Only the first and the last six rows are shown
**Hint:** In order to combine to data.frames by column you can use `cbind(df1, df2, ...)`

help(cbind)

```r
df[,1] <- NULL
dd <- rev(rep(seq(1, 26, 2), each = 2))
ee <- seq(0, 1.6, length.out = 26)
df2 <- data.frame(V4 = dd, V5 = ee)
binded_df <- cbind(df2, df)
head(binded_df)
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":[""],"name":["_rn_"],"type":[""],"align":["left"]},{"label":["V4"],"name":[1],"type":["dbl"],"align":["right"]},{"label":["V5"],"name":[2],"type":["dbl"],"align":["right"]},{"label":["V2"],"name":[3],"type":["dbl"],"align":["right"]},{"label":["V3"],"name":[4],"type":["fctr"],"align":["left"]}],"data":[{"1":"25","2":"0.000","3":"4","4":"a","_rn_":"1"},{"1":"25","2":"0.064","3":"8","4":"a","_rn_":"2"},{"1":"23","2":"0.128","3":"12","4":"c","_rn_":"3"},{"1":"23","2":"0.192","3":"16","4":"c","_rn_":"4"},{"1":"21","2":"0.256","3":"20","4":"e","_rn_":"5"},{"1":"21","2":"0.320","3":"24","4":"e","_rn_":"6"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>

```r
tail(binded_df)
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":[""],"name":["_rn_"],"type":[""],"align":["left"]},{"label":["V4"],"name":[1],"type":["dbl"],"align":["right"]},{"label":["V5"],"name":[2],"type":["dbl"],"align":["right"]},{"label":["V2"],"name":[3],"type":["dbl"],"align":["right"]},{"label":["V3"],"name":[4],"type":["fctr"],"align":["left"]}],"data":[{"1":"5","2":"1.280","3":"84","4":"u","_rn_":"21"},{"1":"5","2":"1.344","3":"88","4":"u","_rn_":"22"},{"1":"3","2":"1.408","3":"92","4":"w","_rn_":"23"},{"1":"3","2":"1.472","3":"96","4":"w","_rn_":"24"},{"1":"1","2":"1.536","3":"100","4":"y","_rn_":"25"},{"1":"1","2":"1.600","3":"104","4":"y","_rn_":"26"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>

## Computation on data.frames

Create the data.frame *df* with `df <- as.data.frame(matrix(runif(9e6), 3e3, 3e3))`
This will create a data.frame with 3000 columns and rows and a total of 9mil values.

Now compute the sum of any row, then compute the sum of any column. Measure the time
for both operations. Why are the times different, although the size is the same?

* **Hint1:** The time is measured with the function `system.time(my_function_call())`, e.g: `system.time(mean(my_vector))`
* **Hint2:** The sum can be computed with the sum function `sum(my_vector)`
* **Hint2:** Columns and rows are selected by single brackets. Rows: `df[row_number,]`, Columns: `df[,col_number]`


```r
# Answer

df <- as.data.frame(matrix(runif(9e6), 3e3, 3e3))

# rows
system.time(res <- sum(df[1,]))
```

```
##    user  system elapsed 
##   0.023   0.000   0.023
```

```r
res
```

```
## [1] 1490.538
```

```r
# columngs
system.time(res2 <- sum(df[,1]))
```

```
##    user  system elapsed 
##       0       0       0
```

```r
res2
```

```
## [1] 1518.813
```

```r
# Look at the structure of the objects over which we are computing the sum
# Column
str(df[1,])
```

```
## 'data.frame':	1 obs. of  3000 variables:
##  $ V1   : num 0.0845
##  $ V2   : num 0.938
##  $ V3   : num 0.437
##  $ V4   : num 0.685
##  $ V5   : num 0.329
##  $ V6   : num 0.088
##  $ V7   : num 0.367
##  $ V8   : num 0.913
##  $ V9   : num 0.618
##  $ V10  : num 0.35
##  $ V11  : num 0.42
##  $ V12  : num 0.2
##  $ V13  : num 0.841
##  $ V14  : num 0.883
##  $ V15  : num 0.51
##  $ V16  : num 0.232
##  $ V17  : num 0.71
##  $ V18  : num 0.0496
##  $ V19  : num 0.799
##  $ V20  : num 0.998
##  $ V21  : num 0.23
##  $ V22  : num 0.428
##  $ V23  : num 0.469
##  $ V24  : num 0.607
##  $ V25  : num 0.809
##  $ V26  : num 0.0907
##  $ V27  : num 0.147
##  $ V28  : num 0.393
##  $ V29  : num 0.254
##  $ V30  : num 0.944
##  $ V31  : num 0.672
##  $ V32  : num 0.113
##  $ V33  : num 0.841
##  $ V34  : num 0.48
##  $ V35  : num 0.862
##  $ V36  : num 0.441
##  $ V37  : num 0.499
##  $ V38  : num 0.0138
##  $ V39  : num 0.316
##  $ V40  : num 0.346
##  $ V41  : num 0.336
##  $ V42  : num 0.253
##  $ V43  : num 0.486
##  $ V44  : num 0.0899
##  $ V45  : num 0.0642
##  $ V46  : num 0.736
##  $ V47  : num 0.541
##  $ V48  : num 0.115
##  $ V49  : num 0.243
##  $ V50  : num 0.117
##  $ V51  : num 0.96
##  $ V52  : num 0.883
##  $ V53  : num 0.0313
##  $ V54  : num 0.0677
##  $ V55  : num 0.326
##  $ V56  : num 0.447
##  $ V57  : num 0.421
##  $ V58  : num 0.0913
##  $ V59  : num 0.627
##  $ V60  : num 0.423
##  $ V61  : num 0.765
##  $ V62  : num 0.37
##  $ V63  : num 0.997
##  $ V64  : num 0.776
##  $ V65  : num 0.219
##  $ V66  : num 0.608
##  $ V67  : num 0.41
##  $ V68  : num 0.272
##  $ V69  : num 0.648
##  $ V70  : num 0.172
##  $ V71  : num 0.989
##  $ V72  : num 0.199
##  $ V73  : num 0.0972
##  $ V74  : num 0.213
##  $ V75  : num 0.535
##  $ V76  : num 0.171
##  $ V77  : num 0.754
##  $ V78  : num 0.776
##  $ V79  : num 0.3
##  $ V80  : num 0.208
##  $ V81  : num 0.775
##  $ V82  : num 0.332
##  $ V83  : num 0.88
##  $ V84  : num 0.795
##  $ V85  : num 0.466
##  $ V86  : num 0.808
##  $ V87  : num 0.171
##  $ V88  : num 0.866
##  $ V89  : num 0.626
##  $ V90  : num 0.484
##  $ V91  : num 0.554
##  $ V92  : num 0.37
##  $ V93  : num 0.31
##  $ V94  : num 0.222
##  $ V95  : num 0.217
##  $ V96  : num 0.236
##  $ V97  : num 0.685
##  $ V98  : num 0.986
##  $ V99  : num 0.111
##   [list output truncated]
```

```r
# Row
str(df[,1])
```

```
##  num [1:3000] 8.45e-02 3.19e-05 9.78e-02 6.85e-01 3.63e-01 ...
```

```r
# As we can see the extracted column is a numeric vector. But the extracted
# row is a list. Under the hood the sum function is iterating in C/Fortran
# over the specific structure. Iterating over a native array of doubles is
# faster, than iterating over a structure, where at each position, the value
# has to be retrieved from an object possibly strored somewhere further away
# in memory.
```

## Missing Values

* If NA is just a placeholder for a missing value of the same type and Infinity is of type
double, why is Infinity plus NA not Infinity?

**Hint:**

```r
paste(paste(rep((Inf - Inf), 20), collapse = ""), "Batman!")
```

```
## [1] "NaNNaNNaNNaNNaNNaNNaNNaNNaNNaNNaNNaNNaNNaNNaNNaNNaNNaNNaNNaN Batman!"
```


```r
# Answer:
# Because infinity is not a well defined number, but a concept with
# the type 'double' in R. Adding or subtracting any number from infinity
# will give infinity. However NA is placeholder for any value of the same
# type and therefore also for infinity. As infinity plus/minus infinity
# is not defined, adding NA to infinity can theoretically lead to
# Nan (not a number). Therefore Inf + NA will produce NA.

Inf + NA
```

```
## [1] NA
```

```r
Inf + 1
```

```
## [1] Inf
```

```r
Inf - Inf
```

```
## [1] NaN
```

