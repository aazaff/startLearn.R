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

#### A mystical rite of passage
***"The only way to learn a new programming language is by writing programs in it. The first program to write is the same for all languages: print the words 'hello, world.'"*** - Kernighan and Ritchie

Using what we learned about writing functions in R, let's make one that that prints hello world.

````
  # First, we create a function object named HelloWorld using function( ).
  # Then we define what the function does using { }.
  # In this case we tell it to return the character data, "hello, world."
  > HelloWorld <- function( ) { return("hello, world.") }
  > HelloWorld()
  [1] "hello, world."
````

Congratulations, you've written your first working function! You are now a true initiate into the wonderful world of computer science and R. Of course, there are a few improvements that we can make to this function.

First, lets consider making the function more versatile, so that it can return phrases other than "hello, world.". We'll do this by adding an **argument** to the function. We'll call this argument **Phrase**. Furthermore, we should also give the function a more versatile name, like **Print**, to reflect the more generic nature of the function.

As a general rule all **objects** that you create in R, whether **functions**, **arguments**, or **data arrays**, should always have a name that is descriptive of what it does. Vague names like x or y should never be used (except in cases of famous math equations, like the slope of a line or a quadratic formula).

````
  # Write our function as outlined
  > Print<-function(Phrase) {
      return(Phrase)
      }
  > Print("hello, world.")
  [1] "hello, world."
````
  
Also notice that the function began with a single line that (1) created a function object named print and (2) defined the functon arguments. We then ***created a new line*** for the body of the function, i.e., the command to **return( )** the argument **Phrase**. It is always good practice to put each individual command of a function on a new line. This makes it easier to read.

## Subsetting and iterating in a single step
