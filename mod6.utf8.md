---
title: "Module 6"
---

> Updated 2021/05/03

The Book of R: chapter 11.1, 11.2 (functions), Glance at ?base::regex

***

# Module 5 review

## Loops

```
for (value in vectorVariable) {
  if (value > 0) {
    print(value)
  }
}
```

## Vectorization

```
vectorVariable[vectorVariable>0]
```   
    
***

## Exercise: Vectorization (10 minutes)

> Create a vector of consecutive numbers from 1 to 100.  
> Note: from:to is the same as seq(from, to)  
> Compute the sum, product and mean  
> Sum just the numbers greater than 50  
> Do this again using `sapply` (which is just fancier lapply).  

### Exercise solution (5 minutes)


```r
myvec <- 1:100

system.time({
  vec_sum <- 0
  vec_prod <- 1
  for (X in myvec) {
    vec_sum <- vec_sum + X
    vec_prod <- vec_prod * X
  }
  vec_mean <- vec_sum / length(myvec)
})
```

```
##    user  system elapsed 
##    0.01    0.00    0.01
```

```r
vec_sum == sum(myvec)
```

```
## [1] TRUE
```

```r
vec_prod == prod(myvec) #damn you, floating point notation
```

```
## [1] FALSE
```

```r
all.equal(vec_prod,prod(myvec))
```

```
## [1] TRUE
```

```r
vec_mean == mean(myvec)
```

```
## [1] TRUE
```

# Functions

## Writing your own functions


```r
#syntax
#Note: myfunction can be any name
myfunction = function(arg1, arg2, ... ) {
  statements
  return(object)
}

#Note: objects inside the function are local to the function i.e. not defined outside. This is called variable scope
#The concept of scope is very important in other languages, but in R, you just need to understand it with functions
```

## Function example


```r
patientRecords = data.frame(
  name=c("patient1", "patient2", "patient3"),
  age=c(46, 49, 50), 
  smoker=c(T, F, T),
  stringsAsFactors=F
)


printRecordRow = function(patientDatabase) {
	#prints a row in a patient database
	v = paste(patientDatabase[1],
	patientDatabase[2], patientDatabase[3])
	return(v)
}

#apply to one row
printRecordRow(patientRecords[1,])
```

```
## [1] "patient1 46 TRUE"
```

```r
#apply the printRecordRow function to all the rows of the patientRecords matrix
apply(patientRecords, 1, printRecordRow)
```

```
## [1] "patient1 46 TRUE"  "patient2 49 FALSE" "patient3 50 TRUE"
```

## Why create functions?

* Organize your code – useful for larger scripts
* Easy to reuse code because it is more modular
    * Collect your own library of useful functions for easy scripting later (can be stored in a separate file and included in all your scripts)
    * More easily share code with others
* Maintain a central place to keep code in case you need to modify/fix something
* Easily repeat code from different places in a single script

## Built in functions

* R has hundreds of built in functions


```r
#e.g. statistical functions
t.test(runif(50), runif(50))
```

```
## 
## 	Welch Two Sample t-test
## 
## data:  runif(50) and runif(50)
## t = 1.1319, df = 97.342, p-value = 0.2604
## alternative hypothesis: true difference in means is not equal to 0
## 95 percent confidence interval:
##  -0.05289424  0.19332191
## sample estimates:
## mean of x mean of y 
## 0.5484590 0.4782452
```

```r
cor.test(runif(50), runif(50),method="pearson")
```

```
## 
## 	Pearson's product-moment correlation
## 
## data:  runif(50) and runif(50)
## t = -0.17853, df = 48, p-value = 0.8591
## alternative hypothesis: true correlation is not equal to 0
## 95 percent confidence interval:
##  -0.3019422  0.2544125
## sample estimates:
##         cor 
## -0.02575941
```

## Exercise: Functions (10 minutes)

> Practice creating a function  
> Create a function called mysum that sums up all the elements of a vector  
> Check that it gives the same results at the sum() function  

### Exercise solution (5 minutes)


```r
mysum <- function(X) {
  OUTPUT <- 0
  for (Y in X) {
    OUTPUT <- OUTPUT + Y
  }
  return(OUTPUT)
}

mysum(X=1:100)
```

```
## [1] 5050
```

```r
abunchofrandomnumbers <- sapply(LETTERS,function(X) runif(50))

apply(abunchofrandomnumbers,1,mean) == rowMeans(abunchofrandomnumbers)
```

```
##  [1] TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE
## [16] TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE
## [31] TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE
## [46] TRUE TRUE TRUE TRUE TRUE
```

```r
apply(abunchofrandomnumbers,2,mean) == colMeans(abunchofrandomnumbers)
```

```
##    A    B    C    D    E    F    G    H    I    J    K    L    M    N    O    P 
## TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE 
##    Q    R    S    T    U    V    W    X    Y    Z 
## TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE
```

```r
str(apply(abunchofrandomnumbers,c(1,2),mean))
```

```
##  num [1:50, 1:26] 0.899 0.88 0.547 0.119 0.468 ...
##  - attr(*, "dimnames")=List of 2
##   ..$ : NULL
##   ..$ : chr [1:26] "A" "B" "C" "D" ...
```

```r
abunchofrandomnumbers <- sapply(LETTERS,function(X) runif(50),simplify=F)

sapply(abunchofrandomnumbers,mean)
```

```
##         A         B         C         D         E         F         G         H 
## 0.4993679 0.4460410 0.5310570 0.5152304 0.5326633 0.5672856 0.5038940 0.5305225 
##         I         J         K         L         M         N         O         P 
## 0.5400734 0.5443648 0.5345693 0.5134127 0.4618431 0.4065028 0.4866052 0.5905945 
##         Q         R         S         T         U         V         W         X 
## 0.4697848 0.4637667 0.4913529 0.5231346 0.4791998 0.4070239 0.4504411 0.5439804 
##         Y         Z 
## 0.5163089 0.5833473
```


# Regular expressions

* Useful tool to find patterns in strings

* Regular expressions (regex) = very powerful pattern matching in strings


```r
string = "aaabbbccc"
#"aaa" matches
#"abb" matches
#"abbbb" does it match?

#You’ve already been using regular expressions e.g.
DNA = "ACGTGA"
sub("A", "T", DNA)
```

```
## [1] "TCGTGA"
```

```r
sub("GA", "CT", DNA)
```

```
## [1] "ACGTCT"
```

```r
gsub("A", "T", DNA)
```

```
## [1] "TCGTGT"
```

* There are lots of ways to specify a pattern – it’s a mini language within a language

string = "aaabbbccc"
"a.a" matches "aaa"
. means any one character

"a*b" matches
* means zero or more of the previous character

Example: [+-]?[0-9]+\.[0-9]+
matches a number like -43.221


## Regular expression quick reference

| aaa             | exact pattern                                                                  |
|-----------------|--------------------------------------------------------------------------------|
| .               | any character                                                                  |
| aa\|bb           | aa or bb                                                                       |
| ()              | group a pattern e.g. (y\|n)o matches yo and no                                  |
| []              | character set e.g. [abc] matches a single character, either a, b or c; a-z     |
| [^]             | not character set e.g. [^abc] matches any single character except a, b or c    |
| [:digit:] or \\d | a range of standard character classes are available                            |
| [:alpha:]       |                                                                                |
| *               | 0 or more of the previous character or group                                   |
| +               | 1 or more                                                                      |
| ?               | 0 or 1                                                                         |
| {n,m}           | a specific number of repeats (e.g. 2 to 5).                                    |
|                 | Also {n,} = n to infinity and {n} = exactly n                                  |
| ^               | start of string                                                                |
| $               | end of string or before a \\n                                                   |
| \\\\b             | word boundary e.g. \\bword\\b (\\B matches non word boundaries)                   |
|                 | Also \\< and \\> match start and end of a word                                   |
| \\\\N             | After the regular expression is run:                                           |
|                 | backreference the Nth pattern matched in a ()  (up to 9 patterns)              |
| ignore.case=T   | case insensitive match                                                         |
| \\\\+ \\\\^ \\\\      | ‘escape’ a metacharacter – use a special character like + or ^ in your pattern |

## Regular expression examples


```r
a = "sdfsabcdefghba0dsgggfe0haaabbbccc"
#grep returns the vector index of the match
#grepl returns TRUE if there is a match
#regexpr returns a list, including match index and length; gregexpr returns all matches
grep("b.d", a)
```

```
## [1] 1
```

```r
grepl("b.d", a)
```

```
## [1] TRUE
```

```r
regexpr("b.d", a)
```

```
## [1] 6
## attr(,"match.length")
## [1] 3
## attr(,"index.type")
## [1] "chars"
## attr(,"useBytes")
## [1] TRUE
```

```r
regexpr("b.d", a)[1]
```

```
## [1] 6
```

```r
gregexpr("b.d", a)
```

```
## [[1]]
## [1] 6
## attr(,"match.length")
## [1] 3
## attr(,"index.type")
## [1] "chars"
## attr(,"useBytes")
## [1] TRUE
```

```r
# b.d			  #The dot will match any character
# b[ce]d		#the [ce] matches a single 'c' or a single 'e'.
# b[ce]+d	  #the [ce]+ matches any number of c's & e's (at least one)
# b[ce]*d		#the [ce]* also can match the empty string.
# b\\sd		  #the \s matches blank or tab (so-called	'whitespace')
# b\\s+d		#the \s+ matches any number of whitespaces (at least one) 
# a|b+		  #what will this match?
# (a|b)+		#and this?

#Note: to run these yourself, add them to e.g. gregexpr("b.d", a), where "b.d" is the regular expression above and “a” is the string variable to search.

gregexpr("b.d", a)
```

```
## [[1]]
## [1] 6
## attr(,"match.length")
## [1] 3
## attr(,"index.type")
## [1] "chars"
## attr(,"useBytes")
## [1] TRUE
```

* string = "Kevin Bacon"
* (K|k)evin (B|b)acon
* Start (^) and end (\$) of a string
    * ^(Kevin|Bacon|Portman|Roberts)
    * (Kevin|Bacon|Portman|Roberts)$
    * ^(Bacon|Portman|Roberts)
* Find all vowels
    * (A|E|I|O|U|Y|a|e|i|o|u|y)
    * [AEIOUYaeiouy]
* How many matches?


```r
length(gregexpr("g", a)[[1]])
```

```
## [1] 4
```

## Exercise: regular expressions (10 minutes)

> DNA = "ACGATGATACTGATGACGGGCGCCA"  
> Match the regular expression "G..A" and use the results to identify the actual match in the original string  
> Use gregexpr  
> attr(x, "match.length") can be used to find the match length in the vector returned from gregexpr  
> ?substr  

### Exercise solution (5 minutes)


```r
DNA = "ACGATGATACTGATGACGGGCGCCA"  
matches <- gregexpr("G..A",DNA)[[1]]

matches
```

```
## [1]  6 22
## attr(,"match.length")
## [1] 4 4
## attr(,"index.type")
## [1] "chars"
## attr(,"useBytes")
## [1] TRUE
```

```r
substr(
  DNA,
  start=matches[1],
  stop=matches[1] + attr(matches,"match.length")[1] - 1
)
```

```
## [1] "GATA"
```

```r
sapply(X=seq_along(matches),
       FUN=function(X) {
         return(
           substr(
             DNA,
             start=matches[X],
             stop=matches[X] + attr(matches,"match.length")[X] - 1
           )
         )
       })
```

```
## [1] "GATA" "GCCA"
```


***
