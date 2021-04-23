---
title: "Module 3"
---

> Updated 2021/04/23

The Book of R: chapter 3.4 (arrays), 5.1 (lists), 5.2 (data frames)

***

# Module 2 review

* Variable types: vector, matrix
* Vector
    * x = c(1.2,2,54,6)
    * textvec[1:2]
* Matrix
    * firstmat[1,2]
    * firstmat[1:2,2:3]
    
***

## Exercise: String commands (10 minutes)

> We used this code to reverse a string

>`paste(rev(strsplit(DNA, NULL)[[1]]), collapse="")`

>Figure out how this works by trying each of the nested functions, starting from `strsplit(DNA, NULL)`

>Note: `[[1]]` extracts elements within a list (which is a type of vector that can contain other lists)

### Exercise solution (5 minutes)


```r
DNA <- "GATTACA"
str(strsplit(DNA,"A"))
```

```
## List of 1
##  $ : chr [1:3] "G" "TT" "C"
```

```r
str(strsplit(DNA,"A")[1])
```

```
## List of 1
##  $ : chr [1:3] "G" "TT" "C"
```

```r
str(strsplit(DNA,"A")[[1]])
```

```
##  chr [1:3] "G" "TT" "C"
```

```r
str(strsplit(DNA,"A")[[1]][1])
```

```
##  chr "G"
```

```r
strsplit(DNA,"")
```

```
## [[1]]
## [1] "G" "A" "T" "T" "A" "C" "A"
```

```r
identical(strsplit(DNA,NULL),strsplit(DNA,""))
```

```
## [1] TRUE
```

```r
length(NULL)
```

```
## [1] 0
```

```r
rev(strsplit(DNA,"")) #nope!
```

```
## [[1]]
## [1] "G" "A" "T" "T" "A" "C" "A"
```

```r
rev(strsplit(DNA,"")[[1]])
```

```
## [1] "A" "C" "A" "T" "T" "A" "G"
```

```r
paste(rev(strsplit(DNA,"")[[1]]),sep="&")
```

```
## [1] "A" "C" "A" "T" "T" "A" "G"
```

```r
paste(rev(strsplit(DNA,"")[[1]]),collapse="&")
```

```
## [1] "A&C&A&T&T&A&G"
```

```r
paste(rev(strsplit(DNA,"")[[1]]),collapse="")
```

```
## [1] "ACATTAG"
```


# Lists

## Lists: creating

* Lists are vectors that contain a mix of variable types
* Create a list using the list() function


```r
numericVector = c(1, 3, 5)
numericVector
```

```
## [1] 1 3 5
```

```r
#Note: numericVector is just a variable name - we call it numericVector because we define it here to contain numbers, but it could be called any variable name that is allowed.

stringVector = c("abc", "def", "ghi")
stringVector
```

```
## [1] "abc" "def" "ghi"
```

```r
logicVector = c(T, F, T, F)
logicVector
```

```
## [1]  TRUE FALSE  TRUE FALSE
```

```r
listVariable = list(numericVector, stringVector, logicVector, 77)
listVariable
```

```
## [[1]]
## [1] 1 3 5
## 
## [[2]]
## [1] "abc" "def" "ghi"
## 
## [[3]]
## [1]  TRUE FALSE  TRUE FALSE
## 
## [[4]]
## [1] 77
```

## Lists: accessing


```r
listVariable
```

```
## [[1]]
## [1] 1 3 5
## 
## [[2]]
## [1] "abc" "def" "ghi"
## 
## [[3]]
## [1]  TRUE FALSE  TRUE FALSE
## 
## [[4]]
## [1] 77
```

```r
listVariable[1]  #access first list element
```

```
## [[1]]
## [1] 1 3 5
```

```r
listVariable[[1]] #access content of first element
```

```
## [1] 1 3 5
```

```r
listVariable[[1]][2] 
```

```
## [1] 3
```

```r
listVariable[[2]][1] = "ACGT"
listVariable[[2]]
```

```
## [1] "ACGT" "def"  "ghi"
```

```r
listVariable[c(2, 4)]
```

```
## [[1]]
## [1] "ACGT" "def"  "ghi" 
## 
## [[2]]
## [1] 77
```

## Lists: named elements


```r
listVariable = list(numbers=numericVector, strings=stringVector, logic=logicVector, myNum=77)
listVariable
```

```
## $numbers
## [1] 1 3 5
## 
## $strings
## [1] "abc" "def" "ghi"
## 
## $logic
## [1]  TRUE FALSE  TRUE FALSE
## 
## $myNum
## [1] 77
```

```r
listVariable["numbers"]
```

```
## $numbers
## [1] 1 3 5
```

```r
listVariable["myNum"]
```

```
## $myNum
## [1] 77
```

```r
#These are equivalent for named elements
listVariable[[1]]
```

```
## [1] 1 3 5
```

```r
listVariable[["numbers"]]
```

```
## [1] 1 3 5
```

```r
listVariable$numbers  #easiest access method
```

```
## [1] 1 3 5
```

## Exercise: Lists (10 minutes)

* Create a list containing a vector of odd numbers from 1 to 100
    * Hint: you can use the seq() command
* Calculate the sum of this list
    * Hint: use the sum() command
* Repeat this, but name the vector and access its values from the list using its name

### Exercise solution (5 minutes)


```r
mylist <- list(seq(1,10))
myvec <- 1:10
identical(mylist[[1]],myvec)
```

```
## [1] TRUE
```

```r
mylist <- list(odds=seq(1,100,by=2),
               evens=seq(2,100,by=2),
               someothersilliness=c("why","are","we","here","?"))

sum(mylist[[1]])
```

```
## [1] 2500
```

```r
identical(sum(mylist[[1]]),sum(mylist$odds))
```

```
## [1] TRUE
```

```r
sum(mylist[["odds"]])
```

```
## [1] 2500
```

```r
paste(mylist[["someothersilliness"]],collapse=" ")
```

```
## [1] "why are we here ?"
```

# Arrays

## Arrays: creating

* Like matrix, but more than two dimensions
* Only one data type (like vector, matrix)
* Create an array using the array() function


```r
vector1 = c(1,3,5)
vector1
```

```
## [1] 1 3 5
```

```r
vector2 = c(20,21,22,23,24,25)
vector2
```

```
## [1] 20 21 22 23 24 25
```

```r
#Create two 3x3 matrices
arrayVariable = array(c(vector1,vector2),dim = c(3,3,2))
arrayVariable
```

```
## , , 1
## 
##      [,1] [,2] [,3]
## [1,]    1   20   23
## [2,]    3   21   24
## [3,]    5   22   25
## 
## , , 2
## 
##      [,1] [,2] [,3]
## [1,]    1   20   23
## [2,]    3   21   24
## [3,]    5   22   25
```

## Arrays: accessing


```r
arrayVariable
```

```
## , , 1
## 
##      [,1] [,2] [,3]
## [1,]    1   20   23
## [2,]    3   21   24
## [3,]    5   22   25
## 
## , , 2
## 
##      [,1] [,2] [,3]
## [1,]    1   20   23
## [2,]    3   21   24
## [3,]    5   22   25
```

```r
#get matrix 1
arrayVariable[,,1]
```

```
##      [,1] [,2] [,3]
## [1,]    1   20   23
## [2,]    3   21   24
## [3,]    5   22   25
```

```r
#get 3rd row of matrix 2
arrayVariable[3,,2]
```

```
## [1]  5 22 25
```

```r
#get element in row 1, column 3 in matrix 1
arrayVariable[1,3,1]
```

```
## [1] 23
```

```r
#get matrix 2
arrayVariable[,,2]
```

```
##      [,1] [,2] [,3]
## [1,]    1   20   23
## [2,]    3   21   24
## [3,]    5   22   25
```

## Arrays: named elements


```r
#Create the names
rowNames = c("row1","row2","row3")
rowNames
```

```
## [1] "row1" "row2" "row3"
```

```r
columnNames = c("col1","col2","col3")
columnNames
```

```
## [1] "col1" "col2" "col3"
```

```r
matrixNames = c("matrix1","matrix2")
matrixNames
```

```
## [1] "matrix1" "matrix2"
```

```r
arrayVariable = array(c(vector1,vector2), dim = c(3,3,2), dimnames = list(rowNames, columnNames, matrixNames))
arrayVariable
```

```
## , , matrix1
## 
##      col1 col2 col3
## row1    1   20   23
## row2    3   21   24
## row3    5   22   25
## 
## , , matrix2
## 
##      col1 col2 col3
## row1    1   20   23
## row2    3   21   24
## row3    5   22   25
```

```r
#Access by names:
arrayVariable[,,"matrix1"]
```

```
##      col1 col2 col3
## row1    1   20   23
## row2    3   21   24
## row3    5   22   25
```

```r
arrayVariable["row2","col1","matrix1"]
```

```
## [1] 3
```

Does each matrix in an array need to be the same shape?


```r
array()
```

```
## [1] NA
```


# Data frames

## Data frames: creating

* Used to store data tables
* List of equal sized vectors
* Create a data.frame using the data.frame() function


```r
name = c("patient1", "patient2", "patient3")
name
```

```
## [1] "patient1" "patient2" "patient3"
```

```r
age = c(46, 49, 50)
age
```

```
## [1] 46 49 50
```

```r
smoker = c(T, F, T)
smoker
```

```
## [1]  TRUE FALSE  TRUE
```

```r
patientRecords = data.frame(name, age, smoker)
patientRecords
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["name"],"name":[1],"type":["chr"],"align":["left"]},{"label":["age"],"name":[2],"type":["dbl"],"align":["right"]},{"label":["smoker"],"name":[3],"type":["lgl"],"align":["right"]}],"data":[{"1":"patient1","2":"46","3":"TRUE"},{"1":"patient2","2":"49","3":"FALSE"},{"1":"patient3","2":"50","3":"TRUE"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>

## Data frames: accessing


```r
patientRecords
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["name"],"name":[1],"type":["chr"],"align":["left"]},{"label":["age"],"name":[2],"type":["dbl"],"align":["right"]},{"label":["smoker"],"name":[3],"type":["lgl"],"align":["right"]}],"data":[{"1":"patient1","2":"46","3":"TRUE"},{"1":"patient2","2":"49","3":"FALSE"},{"1":"patient3","2":"50","3":"TRUE"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>

```r
#Just like matrices
patientRecords[1,] #row 1
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":[""],"name":["_rn_"],"type":[""],"align":["left"]},{"label":["name"],"name":[1],"type":["chr"],"align":["left"]},{"label":["age"],"name":[2],"type":["dbl"],"align":["right"]},{"label":["smoker"],"name":[3],"type":["lgl"],"align":["right"]}],"data":[{"1":"patient1","2":"46","3":"TRUE","_rn_":"1"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>

```r
patientRecords[,1] #column 1
```

```
## [1] "patient1" "patient2" "patient3"
```

```r
patientRecords[1,2] #element at row 1, column 2
```

```
## [1] 46
```

```r
patientRecords$age
```

```
## [1] 46 49 50
```

## Exercise: Data frames (10 minutes)

>Read the "gene_condition_source_id.txt" file used in Assignment 1 into a data frame variable (see assignment 1 for the command)

>Look at the top few rows using the head() command

>How many rows and columns does the file have? Try the str() command. Also try nrow() and ncol()

>Use names() to find the column names 

### Exercise solution (5 minutes)


```bash
wc -l gene_condition_source_id.txt
```

```
## 11062 gene_condition_source_id.txt
```



```r
list.files(path=".") # "." == the current working directory
```

```
##  [1] "_site.yml"                    "ASCII-Table-wide.svg"        
##  [3] "docs"                         "fileOutput.png"              
##  [5] "gene_condition_source_id.txt" "index.Rmd"                   
##  [7] "mod2.Rmd"                     "mod3.Rmd"                    
##  [9] "mod4.Rmd"                     "mod5.Rmd"                    
## [11] "mod6.Rmd"                     "mod7.Rmd"                    
## [13] "mod8.Rmd"                     "mod9.Rmd"                    
## [15] "mydata.txt"                   "p4b.Rproj"                   
## [17] "README.md"                    "recipebookTOC.png"           
## [19] "site_libs"                    "sum.R"                       
## [21] "test.txt"                     "yeast_properties.txt"
```

```r
db <- read.table("gene_condition_source_id.txt",
                 header=T,sep="\t",quote="",comment.char="",
                 colClasses="character")
nrow(db)
```

```
## [1] 11061
```

```r
identical(colnames(db),names(db))
```

```
## [1] TRUE
```

```r
str(db)
```

```
## 'data.frame':	11061 obs. of  9 variables:
##  $ X.GeneID       : chr  "2" "2" "144568" "53947" ...
##  $ AssociatedGenes: chr  "A2M" "A2M" "A2ML1" "A4GALT" ...
##  $ RelatedGenes   : chr  "" "" "" "" ...
##  $ ConceptID      : chr  "C0002395" "C0002395" "C1833692" "C0599990" ...
##  $ DiseaseName    : chr  "Alzheimer disease" "Alzheimer disease" "Otitis media, susceptibility to" "p phenotype" ...
##  $ SourceName     : chr  "MONDO" "Human Phenotype Ontology" "MONDO" "NCBI curation" ...
##  $ SourceID       : chr  "MONDO:0004975" "HP:0002511" "MONDO:0008162" "" ...
##  $ DiseaseMIM     : chr  "104300" "104300" "166760" "111400" ...
##  $ LastUpdated    : chr  "Feb 19 2020" "Feb 19 2020" "Feb 16 2016" "Feb 16 2016" ...
```

```r
rownames(db)[1:10]
```

```
##  [1] "1"  "2"  "3"  "4"  "5"  "6"  "7"  "8"  "9"  "10"
```


****

# Assignment 2 hints:
I expect a tarball containing a single folder with any input data required to run your scripts (ie. the FASTA file), any output files generated by your scripts, **and your scripts themselves!**  For this week, there is no report needed.  Your scripts should answer any questions themselves (ie. 'stringnumber_practice.R' should be filled in with all the code needed to generate solutions for each question).



****
