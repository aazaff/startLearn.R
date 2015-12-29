## Introduction

The following are a set of self-test questions designed to complement the [startLearn.R](https://github.com/aazaff/startLearn.R/blob/master/README.md) tutorial. These exercises are only meant to be attempted **after** you have finished reading the startLearn.R tutorial.

## Loading in Data

We are going to make our own dataset for this exercise using random data. Normally, because the data is random, we would not be able to see the same results. However, if we use the **set.seed( )** function, we will all generate the same "random" results.

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

#### Problem Set 1
1. If you wanted to convert our **matrix** of **logicals** to **numerics** (i.e., 0 and 1) using the as( ) function, how would you do this?

2. The as( ) function does not convert **MyMatrix** to numeric data. Can you think of a property of logicals that you can use to convert the logicals to 0's and 1's other than the as( ) function?

3. If you wanted to check if **all** of the elements in each row and each column is true, how would you do this? 


