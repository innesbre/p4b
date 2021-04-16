---
title: "Module 5"
---

The Book of R: chapter 10.2 and 10.3 (loops), 10.2.3 (apply functions)

***

## Module 4 review

* Factors
    * f = factor(x)
    * levels(f)
* Files
    * ASCII
    * write.table
    * read.table
* Conditional statements
    * if, if else if, if else if else
    
***
# Exercise: conditions (10 minutes)

>Write conditional statement code that evaluates an amino acid's physicochemical property and outputs the following, given a single letter amino acid code:

```
"Charged" for R, K, D, E 
"Polar" for Q, N, H, S, T, Y, C
"Amphipathic" for W, Y, M
"Hydrophobic" for A, I, L, M, F, V, P, G
```

### Exercise solution (5 minutes)



# Loops

## Key Programming Concept

# Loops


```r
vectorVariable = c(1,-2,3,-5)

#Let's find all of the positive numbers
if(vectorVariable[1]>0) {
  print("Positive number")
}
```

```
## [1] "Positive number"
```

```r
if(vectorVariable[2]>0) {
  print("Positive number")
}
if(vectorVariable[3]>0) {
  print("Positive number")
}
```

```
## [1] "Positive number"
```

```r
if(vectorVariable[4]>0) {
  print("Positive number")
}

#Problems with this approach:
#1. There is a lot of repeated code
#2. What happens if the vector is really long?
```

* Loops provide a way of efficiently repeating the same code
* Two major kinds of loops: for, while

# For loop e.g. find positive numbers


```r
#Here is the same problem, but solved using a for loop
vectorVariable = c(1,-2,3,-5)
for (value in vectorVariable) {
  if (value > 0) {
    print(value)
  }
}
```

```
## [1] 1
## [1] 3
```

```r
#What is happening here?  Let's "unroll" the loop
vectorVariable = c(1,-2,3,-5)
value = vectorVariable[1]
if (value > 0) {
  print(value)
}
```

```
## [1] 1
```

```r
value = vectorVariable[2]
if (value > 0) {
  print(value)
}
value = vectorVariable[3]
if (value > 0) {
  print(value)
}
```

```
## [1] 3
```

```r
value = vectorVariable[4]
if (value > 0) {
  print(value)
}

#Note: 'value' is just a variable, so can have any name of a variable

#The for loop automatically repeated the same code inside the loop, automatically updating the value variable to the next element of the vector, so it could "loop" through all the vector elements

#here is an idiomatic way to write a for loop that executes a specific number of times:
sum=0
for (i in 1:100) {
  sum = sum + i;   #note: ; indicates the end of a statement, but it is optional in R
}
print (sum)
```

```
## [1] 5050
```

```r
#this is the same as sum(1:100) - in this case, R has a 'high level' command that does the same thing. Internally, sum() uses a loop to do its job.
```

# While loop

* Example: find positive numbers in a vector


```r
vectorVariable = c(1,-2,3,-5)
i=1
while (i <= length(vectorVariable)) {
  if (vectorVariable[i] > 0) {
    print(vectorVariable[i])
  }
  i = i + 1
}
```

```
## [1] 1
## [1] 3
```

```r
#What is happening here?  Let's "unroll" the loop
vectorVariable = c(1,-2,3,-5)
i=1  #i is an index to keep track of our position in the vector. i=1 means to start with the first element of the vector.
if (i <= length(vectorVariable)) { #if i is smaller or equal to the vector length, then we can run our code
  if (vectorVariable[i] > 0) {
    print(vectorVariable[i])
  }
  i = i + 1  #move to the next element - i now equal to 2
}
```

```
## [1] 1
```

```r
if (i <= length(vectorVariable)) { #if i is smaller or equal to the vector length, then we can run our code
  if (vectorVariable[i] > 0) {
    print(vectorVariable[i])
  }
  i = i + 1  #move to the next element - i now equal to 3
}
if (i <= length(vectorVariable)) { #if i is smaller or equal to the vector length, then we can run our code
  if (vectorVariable[i] > 0) {
    print(vectorVariable[i])
  }
  i = i + 1  #move to the next element - i now equal to 4
}
```

```
## [1] 3
```

```r
if (i <= length(vectorVariable)) { #if i is smaller or equal to the vector length, then we can run our code
  if (vectorVariable[i] > 0) {
    print(vectorVariable[i])
  }
  i = i + 1  #move to the next element - i now equal to 5
}
if (i <= length(vectorVariable)) { #i is no longer smaller than the vector length, so don't run the code
  if (vectorVariable[i] > 0) {
    print(vectorVariable[i])
  }
  i = i + 1
}


#The while and for loops can do the same thing. Sometimes it is more convenient to write a loop one way or another
```

# Reasons to write loops

* You want to loop over all elements of a vector (the most common case in R)


```r
for (i in vectorVariable) {
  if (i > 0) {
    print(i)
  }
}
```

```
## [1] 1
## [1] 3
```

* You want to run a certain number of operations


```r
sum=0
for (i in 1:100) {
  sum = sum + i
}
print (sum)
```

```
## [1] 5050
```

```r
#same as
i=1
sum=0
while (i <= 100) {
  sum = sum + i
  i = i + 1
}
print (sum)
```

```
## [1] 5050
```

* You want to run code until a certain condition occurs e.g. find the first instance of "cancer" in the genes2phenotype file


```r
vectorVariable = c(1,-2,3,-5)
i=1
while (vectorVariable[i] != 3) {
  print(vectorVariable[i])
  i = i + 1
}
```

```
## [1] 1
## [1] -2
```


# Infinite loops

* Be careful that the loops don't go on forever
* If this happens, you need to stop or interrupt the R code. In RStudio, you interrupt the code from the Session menu


```r
vectorVariable = c(1,-2,3,-5)
i=1
while (i <= length(vectorVariable)) {
  if (vectorVariable[i] > 0) {
    print(vectorVariable[i])
  }
  #i = i + 1  #We never update the loop index, so i will never be greater than the vector length, so the loop will never end
}

# Not actually going to run this one, or the code would never knit!
```

# Exercise: Loops 1 (10 minutes)

> Remember to count the amino acids in a protein sequence? Let's do it in a different way here, using loops and named vectors (could also use names in lists)

> Here again the protein sequence:  

```
> sp|P04637|P53_HUMAN Cellular tumor antigen p53 OS=Homo sapiens
MEEPQSDPSVEPPLSQETFSDLWKLLPENNVLSPLPSQAMDDLMLSPDDIEQWFTEDPGPDEAPRMPEAAPPVAPAPAA
PTPAAPAPAPSWPLSSSVPSQKTYQGSYGFRLGFLHSGTAKSVTCTYSPALNKMFCQLAKTCPVQLWVDSTPPPGTRVR
AMAIYKQSQHMTEVVRRCPHHERCSDSDGLAPPQHLIRVEGNLRVEYLDDRNTFRHSVVVPYEPPEVGSDCTTIHYNYM
CNSSCMGGMNRRPILTIITLEDSSGNLLGRNSFEVRVCACPGRDRRTEEENLRKKGEPHHELPPGSTKRALPNNTSSSP
QPKKKPLDGEYFTLQIRGRERFEMFRELNEALELKDAQAGKEPGGSRAHSSHLKSKKGQSTSRHKKLMFKTEGPDSD
```

> Use a for loop and the command strplit to get each amino acid of the sequence to count the occurrences.

> One easy way is to first generate a vector (or list) that has named rows, named for each amino acid, such that, for instance, you keep the number of occurrences of Alanines as occ_aa["A"]. To start, you have to set all the elements of the vector to zero (R has a built in vector called "LETTERS" that contains the whole alphabet)

### Exercise solution (5 minutes)




# Exercise: Loops 2 (10 minutes)

> Read the "gene_condition_source_id.txt" file
dataFrame = read.table("gene_condition_source_id.txt", header=TRUE, sep="\t", fill=TRUE, comment.char = "", quote = "")

> Using a for loop, loop over all the rows of "gene_condition_source_id.txt" and find all diseases linked to TP53

> Do the same thing, but first read in the file again, but set "stringsAsFactors=F" as an argument to read.table()

### Exercise solution (5 minutes)




# Vectorization

## Key Programming Concept in R

### (but not in many other programming languages)

# Vectorization

* Key feature of R that helps do powerful things with a small amount of code (high level)
* Try to to avoid loops and use vectors because loops tend to be slower than vectors in R


```r
vectorVariable = c(1,-2,3,-4)
vectorVariable
```

```
## [1]  1 -2  3 -4
```

```r
vectorVariable * 5
```

```
## [1]   5 -10  15 -20
```

```r
vectorVariable * vectorVariable
```

```
## [1]  1  4  9 16
```

```r
sum(vectorVariable)
```

```
## [1] -2
```

```r
#similarly, you can use: prod(), mean(), sd(), min(), max()

stringVariable = c("first", "last")
stringVariable
```

```
## [1] "first" "last"
```

```r
paste(stringVariable, "name")
```

```
## [1] "first name" "last name"
```

```r
paste(stringVariable, c("name1", "name2"))  #note how vectorization applies the transformation to all strings in the string vector
```

```
## [1] "first name1" "last name2"
```

# Logical selection from a vector


```r
vectorVariable>0
```

```
## [1]  TRUE FALSE  TRUE FALSE
```

```r
vectorVariable==0
```

```
## [1] FALSE FALSE FALSE FALSE
```

```r
vectorVariable<0
```

```
## [1] FALSE  TRUE FALSE  TRUE
```

```r
vectorVariable[c(T,F,T,F)]
```

```
## [1] 1 3
```

```r
vectorVariable[vectorVariable>0]
```

```
## [1] 1 3
```

```r
ifelse(vectorVariable>0,"positive number", "negative number")
```

```
## [1] "positive number" "negative number" "positive number" "negative number"
```



```r
#remember: for loop to find positive numbers in a vector
vectorVariable = c(1,-2,3,-5)
for (value in vectorVariable) {
  if (value > 0) {
    print(value)
  }
}
```

```
## [1] 1
## [1] 3
```

```r
#the vectorization way of doing this is:
vectorVariable[vectorVariable>0]
```

```
## [1] 1 3
```

```r
#much easier!
```

# Vectorization with data frames


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
patientRecords = data.frame(name, age, smoker, stringsAsFactors=F)

#What is going on here?
patientRecords$age>48
```

```
## [1] FALSE  TRUE  TRUE
```

```r
patientRecords[patientRecords$age>48]  #oops
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["age"],"name":[1],"type":["dbl"],"align":["right"]},{"label":["smoker"],"name":[2],"type":["lgl"],"align":["right"]}],"data":[{"1":"46","2":"TRUE"},{"1":"49","2":"FALSE"},{"1":"50","2":"TRUE"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>

```r
patientRecords[ , patientRecords$age>48]  #oops
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["age"],"name":[1],"type":["dbl"],"align":["right"]},{"label":["smoker"],"name":[2],"type":["lgl"],"align":["right"]}],"data":[{"1":"46","2":"TRUE"},{"1":"49","2":"FALSE"},{"1":"50","2":"TRUE"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>

```r
patientRecords[patientRecords$age>48 , ]
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":[""],"name":["_rn_"],"type":[""],"align":["left"]},{"label":["name"],"name":[1],"type":["chr"],"align":["left"]},{"label":["age"],"name":[2],"type":["dbl"],"align":["right"]},{"label":["smoker"],"name":[3],"type":["lgl"],"align":["right"]}],"data":[{"1":"patient2","2":"49","3":"FALSE","_rn_":"2"},{"1":"patient3","2":"50","3":"TRUE","_rn_":"3"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>

# "apply" function

* applies a function to a matrix


```r
#Create a new matrix with number 1 to 30
x = matrix(1:30, nrow=5, ncol=6)
x
```

```
##      [,1] [,2] [,3] [,4] [,5] [,6]
## [1,]    1    6   11   16   21   26
## [2,]    2    7   12   17   22   27
## [3,]    3    8   13   18   23   28
## [4,]    4    9   14   19   24   29
## [5,]    5   10   15   20   25   30
```

```r
#Sum the rows
apply(x, MARGIN=1, FUN=sum)
```

```
## [1]  81  87  93  99 105
```

```r
#or equivalently
apply(x, 1, sum)
```

```
## [1]  81  87  93  99 105
```

```r
#Sum the columns
apply(x, 2, sum)
```

```
## [1]  15  40  65  90 115 140
```

# lapply()

* Similar to apply, but for lists


```r
#Compute the mean for all the columns in a data.frame (which is also a list)
lapply(patientRecords, mean)
```

```
## Warning in mean.default(X[[i]], ...): argument is not numeric or logical:
## returning NA
```

```
## $name
## [1] NA
## 
## $age
## [1] 48.33333
## 
## $smoker
## [1] 0.6666667
```

```r
#You may see lots of other apply functions in R code e.g. mapply, sapply, tapply, vapply
```


***
