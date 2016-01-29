# Introduction

The following are a set of self-test questions designed to complement the [intermediate Concepts](https://github.com/aazaff/startLearn.R/blob/master/intermediateConcepts.md) tutorial. Note that there is also some carry over of concepts from the [basicConcepts](https://github.com/aazaff/startLearn.R/blob/master/beginnerConcepts.md) tutorial.

## A logical matrix

We are going to make our own dataset for this exercise using random data. Normally, because the data is random, we would not be able to see the same results. However, if we use the ````set.seed( )```` function, we will all generate the same "random" results.

````
# First we need to set the seed
> set.seed(541)

# We are ultimately going to make an 8 by 12 matrix of random logical data. 
# This means that it will need to have 8 * 12 = 96 elements.

# We will use the sample( ) function to randomly sample a vector of TRUE and FALSE values to populate our matrix.
> MatrixElements<-sample(c(TRUE,FALSE), size = 96, replace=TRUE)

# Make a 2-dimensonal array populated with the MatrixElements vector.
> MyMatrix<-array(data=MatrixElements,dim=c(8,12))
> MyMatrix
      [,1]  [,2]  [,3]  [,4]  [,5]  [,6]  [,7]  [,8]  [,9] [,10] [,11] [,12]
[1,] FALSE  TRUE  TRUE  TRUE FALSE FALSE FALSE  TRUE  TRUE  TRUE  TRUE FALSE
[2,] FALSE FALSE FALSE  TRUE FALSE  TRUE  TRUE  TRUE  TRUE FALSE FALSE  TRUE
[3,] FALSE FALSE  TRUE  TRUE  TRUE  TRUE FALSE FALSE  TRUE  TRUE  TRUE  TRUE
[4,]  TRUE FALSE FALSE  TRUE  TRUE FALSE  TRUE  TRUE  TRUE  TRUE FALSE  TRUE
[5,]  TRUE  TRUE FALSE  TRUE  TRUE  TRUE FALSE  TRUE FALSE  TRUE FALSE FALSE
[6,]  TRUE  TRUE FALSE  TRUE FALSE FALSE  TRUE  TRUE  TRUE  TRUE FALSE FALSE
[7,]  TRUE FALSE  TRUE FALSE  TRUE FALSE FALSE  TRUE  TRUE  TRUE  TRUE FALSE
[8,] FALSE FALSE  TRUE  TRUE FALSE  TRUE  TRUE  TRUE FALSE  TRUE  TRUE FALSE
````

#### Section 1 Questions
1. What does the ````REPLACE=```` argument of the ````sample( )```` function do?

2. Using````as(MyMatrix,"numeric")```` will not convert ````MyMatrix```` to numeric data! Can you think of a property of logicals that you can use to convert the logicals to 0's and 1's other than the ````as( )```` function?

3. If you wanted to check if **all** of the elements in each row are true, how would you do this?

## A numeric matrix

Let's create a numeric matrix next.

````
# First we need to set the seed
> set.seed(154)

# We will randomly sample a vector of numeric data
> MatrixElements<-sample(c(1,2,3,4,5,6,7,8,9,10), size = 96, replace=TRUE)
> MyMatrix<-matrix(MatrixElements,8,12)

> MyMatrix
     [,1] [,2] [,3] [,4] [,5] [,6] [,7] [,8] [,9] [,10] [,11] [,12]
[1,]    8    4    7    9    3    2    8    2    9     2     3     5
[2,]    8    6    8   10    5   10    7    9    1    10     8    10
[3,]    1    6    1    6    9    5    5    1    8     4    10     1
[4,]    7    1    5   10    2    2    3    7    7     2     9    10
[5,]    9    6    7    5    9    9    7    1    4     3    10     2
[6,]    1    2    9    9    2   10    8   10   10     4     7     3
[7,]    9    9    1    2   10    6    6    9    7    10     8     8
[8,]    8    3    7    7    7   10    7    7    7     7     4     3
````

#### Section 2 Questions
1. How many times does the number 7 occur in ````MyMatrix````?

2. How do you find the sum of each column?

3. How do you find the product of each column?

4. How would you change every instance of the number 10 to 12?

5. How many values in ````MyMatrix```` are greater than 3 and less than 8?

6. How do you change the elements of column 12 into **character data**, while keeping columns 1- 11 as numeric data??

7. Find which rows of MyMatrix have a sum >70. Make a *new* version of MyMatrix where the 13th column is a set of ````TRUE```` and ````FALSE```` values denoting which rows have a sum >70. (Hint: What type of object allows you to store both logical and numeric data at once?)
