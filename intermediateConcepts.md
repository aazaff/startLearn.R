# Intermediate Concepts

This section covers intermediate R concepts, and is meant to be read through after completeing the [beginnerConcepts](https://github.com/aazaff/startLearn.R/blob/master/beginnerConcepts.md) tutorial and the [beginnerTest](https://github.com/aazaff/startLearn.R/blob/master/beginnerTest.md) exercise.

+ [How to name elements in different data objects](#how-to-name-elements-in-different-data-objects)
+ [Subscripting and subsetting with logicals](#subscripting-and-subsetting-with-logicals)
+ [Automatically repeating a function multiple times](#automatically-repeating-a-function-multiple-times)
+ [Writing your own functions in R](#writing-your-own-functions-in-r)
+ [Subsetting and iterating in a single step](#subsetting-and-iterating-in-a-single-step)

## How to name elements in different data objects

In the Beginner Concepts tutorial, we used the **names( )** function to add names to a one-dimensional array (vector). Unfortunately, **names( )** is not a flexible function because it does not work with all data objects. In fact, there is ***no single function*** for naming that works in all cases. This means that you are going to have to remember a few different commands, and ***know when to use each***.

Naming Function | When to use it
--------------- | -------------------
**names( )** | Will work with any object, but only allows you to name the **first dimension** of the object.
**rownames( )** |	Names the second dimension (rows) of an **array**. It requires an array with 2 or more dimensions.
**colnames( )** |	Names the first dimesion (columns) of an **array**. It requires an array with 2 or more dimensions.
**dimnames( )** |	Renames the **nth** dimension of an **array**, **dimnames(Array)[[n]]**. Can also name all dimensions of an array at once using a list. Will not work for dimensionless data objects - i.e., vectors and lists.

This may seem a bit overwhelming at first, but ***it's not as bad as it first seems***. As a general rule, use **names( )** if you are naming a **vector** or a **list**. Use **dimnames( )** if you are naming an **array** or *data.frame**.

````
# Create a vector and name it using names( )
> MyVector<-c(1:3)
> names(MyVector)<-c("Joe","Frank","Bob")
> MyVector
Joe Frank   Bob 
  1     2     3 

# Create a 1 dimensional array and name it using dimnames( )
> MyArray<-array(data=c(1,2,3),dim=3)
> dimnames(MyArray)[[1]]<-c("Joe","Frank","Bob")
> MyArray
Joe Frank   Bob 
  1     2     3 

# Create a 2-dimensional array and name each dimension using dimnames( )
> TwoArray<-array(data=c(1,2,3,4,5,6),dim=c(2,3))

# Name the rows of TwoArray (i.e., the first dimension)
> dimnames(TwoArray)[[1]]<-c("Monday","Tuesday")
> TwoArray
        [,1] [,2] [,3]
Monday     1    3    5
Tuesday    2    4    6

# Name the columns of TwoArray (i.e., the second dimension)
> dimnames(TwoArray)[[2]]<-c("Joe","Frank","Bob")
> TwoArray
        Joe Frank Bob
Monday    1     3   5
Tuesday   2     4   6


# Create a list of all the previous examples and name each object in the list.
> MyList<-list(MyVector,MyArray,TwoArray)
[[1]]
  Joe Frank   Bob 
    1     2     3 

[[2]]
  Joe Frank   Bob 
    1     2     3 

[[3]]
        Joe Frank Bob
Monday    1     3   5
Tuesday   2     4   6

# Name each object in the list by example
> names(MyList)<-c("Vector Example","1D Array Example","2D Array Example")
> MyList
$`Vector Example`
  Joe Frank   Bob 
    1     2     3 

$`1D Array Example`
  Joe Frank   Bob 
    1     2     3 

$`2D Array Example`
        Joe Frank Bob
Monday    1     3   5
Tuesday   2     4   6
````

## Subscripting and subsetting with logicals

## Iterating over an array

## Writing your own functions

#### A mystical rite of passage
"The only way to learn a new programming language is by writing programs in it. The first program to write is the same for all languages: print the words ***hello, world.***" - Kernighan and Ritchie

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
