# Intermediate Concepts

This section covers intermediate R concepts, and is meant to be read through after completeing the [beginnerConcepts]() tutorial and exercises.

+ [Using lists and subscripts to name arrays](#using-lists-and-subscripts-to-name-arrays)
+ [Subscripting and subsetting with logicals](#subscripting-and-subsetting-with-logicals)
+ [Automatically repeating a function multiple times](#automatically-repeating-a-function-multiple-times)
+ [Writing your own functions in R](#writing-your-own-functions-in-r)
+ [Subsetting and iterating in a single step](#subsetting-and-iterating-in-a-single-step)

## Using lists and subscripting to name arrays

In the Beginner Concepts tutorial, we used the **names( )** function to add names to a one-dimensional array (vector). Unfortunately, **names( )** is not a flexible function because it does not work with all data objects. In fact, there is ***no single function*** for naming that works in all cases. This means that you are going to have to remember a few different commands, and ***know when to use each***.

Naming Function | When to use it
--------------- | -------------------
names() | Names will work with any data object, but it will only ever allow you to name the **first dimension** of the object. For example, you can name the columns in matrix using names( ), but you cannot rename the rows.
rownames( ) |	rownames( ) automatically names the second dimension (the rows) of an **array**. It requires an array with more than two dimensions.
colnames( ) |	colnames( ) automatically names the first dimesion (the columns) of an **array**. It requires an array with more than two dimensions.
dimnames( ) |	dimnames( ) can be used to rename the **nth** dimension of any **array** if you specify the desired dimension with a doublesubscript **dimnames[[n]]**. You can also name all dimensions at once using a list.  Dimnames will not work for dimensionless data objects - i.e., vectors and lists.

## Subscripting and subsetting with logicals

## Iterating over an array

## Writing your own functions

## Subsetting and iterating simultaneously
