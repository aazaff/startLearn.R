# Intermediate Concepts

This section covers intermediate R concepts, and is meant to be read through after completeing the [beginnerConcepts](https://github.com/aazaff/startLearn.R/blob/master/beginnerConcepts.md) tutorial and the [beginnerTest](https://github.com/aazaff/startLearn.R/blob/master/beginnerTest.md) exercise.

+ [How to name elements in different data objects](#how-to-name-elements-in-different-data-objects)
+ [Subscripting and subsetting with logicals](#subscripting-and-subsetting-with-logicals)
+ [Automation and overwriting elements](#automation-and-overwriting-elements)
+ [Writing your own functions in R](#writing-your-own-functions-in-r)
+ [Subsetting and iterating in a single step](#subsetting-and-iterating-in-a-single-step)

## How to name elements in different data objects

In the Beginner Concepts tutorial, we used the **names( )** function to add names to a one-dimensional array (vector). Unfortunately, **names( )** is not a flexible function because it does not work with all data objects. In fact, there is ***no single function*** for naming that works in all cases. This means that you are going to have to remember a few different commands, and ***know when to use each***.

Naming Function | When to use it
--------------- | -------------------
**names( )** | Will work with any object, but only allows you to name the **first dimension** of the object.
**rownames( )** |	Names the second dimension (rows) of an **array**. It requires an array with 2 or more dimensions.
**colnames( )** |	Names the first dimesion (columns) of an **array**. It requires an array with 2 or more dimensions.
**dimnames( )** |	**dimnames(Array)[[n]]** renames the **nth** dimension of an **array**. Will not work for dimensionless data objects - i.e., vectors and lists.

This may seem a bit overwhelming at first, but ***it's not as bad as it first seems***. As a general rule, use **names( )** if you are naming a **vector** or a **list**. Use **dimnames( )** if you are naming an **array** or **data.frame**.

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
> names(MyList)<-c("VectorExample","1D ArrayExample","2D ArrayExample")
> MyList
$`VectorExample`
  Joe Frank   Bob 
    1     2     3 

$`1D ArrayExample`
  Joe Frank   Bob 
    1     2     3 

$`2D ArrayExample`
        Joe Frank Bob
Monday    1     3   5
Tuesday   2     4   6
````

You may have noticed that in the MyList example, the name for each **element** of the list was prefaced with a **$**. The dollar sign is a special operator that works with lists and is equivalent to the **[[ ]]** notation we've been using thus far.

````
# Calling by element index using double brackets
> MyList[[1]]
  Joe Frank   Bob 
    1     2     3

# Calling by element name using double brackets
> MyList[["Vector Example"]]
  Joe Frank   Bob 
    1     2     3
    
# Call by element name using double brackets. Note that there are no "" used when using the $ sign.
> MyList$VectorExample
  Joe Frank   Bob 
    1     2     3
````

**$** is widely used in the R community, so you should be aware of it. However, it is fairly inflexible and will not work in many situations where **[ ]** or **[[ ]]** works.

````
# $ won't work if the element name has spaces
> MyList$1D ArrayExample
Error: unexpected numeric constant in "MyList$1"

# $ won't work for anything other than lists and data frames.
> MyVector$Joe
Error in MyVector$Joe : $ operator is invalid for atomic vectors

# $ will only work for columns of data frames, not rows.
> MyFrame<-data.frame(TwoArray)
> MyFrame
        Joe Frank Bob
Monday    1     3   5
Tuesday   2     4   6

# It works for columns
> MyFrame$Joe
[1] 1 2

# But not for rows
> DataFrame$Tuesday
NULL
````

Since **[[ ]]** and **[ ]** do the same things, but better... there is really no good reason to use **$**. 

## Subscripting and subsetting with logicals

Perhaps the biggest benefit of **[ ]** notation is that we can perform complex subscripting operations within them. The most powerful of these is the **which( )** function, which finds the **index** (a.k.a., the position) of TRUE values in a logical array.

````
# Create a vector of logical values, where the first element and fifth element are TRUE
> MyLogical<-c(TRUE,FALSE,FALSE,FALSE,TRUE)
> MyLogical
[1]  TRUE FALSE FALSE FALSE  TRUE

# Use which to find which elements of MyLogical are TRUE
> which(MyLogical)
[1] 1 5
````

Now, you might ask, how does this help? Well now that you you have the index positions, you can reference those elements of the array directly.

````
# Display the elements of MyLogical that are TRUE
> MyLogical[which(MyLogical)]
[1] TRUE TRUE
````

This isn't very impressive since it is circular. We asked which elements had a value of TRUE, so of course the values of those elements is TRUE. But, what if we don't start out with logical data?

````
# What if we want to see all the elements in array that are greater than 5 and what those elements are?
> MyVector<-c(2,6,4,5,6,1,3,4,7,9,3)
> MyVector
[1] 2 3 4 5 6 1 3 4 7 9 3

# We merely need to convert our numeric data into logical using the appropriate logical operator.
> MyLogical<-MyVector > 5
> MyLogical
[1] FALSE FALSE FALSE FALSE  TRUE FALSE FALSE FALSE  TRUE  TRUE FALSE

# Find which elements of MyLogical are TRUE and display them.
> MyVector[which(MyLogical)]
[1] 6 6 7 9

# We can do all of this in a single step.
> MyVector[which(MyVector > 5)]
[1] 6 6 7 9
````

You can write out very complex logical statements using the **&** (and) and **|** (or) operators. So long as your expression resolves to a single TRUE or FALSE value.

````
# Find numbers that are greater than 5 AND less than 9
> MyVector[which(MyVector > 5 & MyVector < 9)]
[1] 6 6 7

# Find numbers that are greater than 5 or equal 3
> MyVector[which(MyVector > 5 | MyVector == 3)]
[1] 6 6 3 7 9 3
````

## Automation and overwriting elements

The true power of **which( )** doesn't become apparent until you want to start **overwriting** elements of a data object. 

Consider when we used the **dimnames( )** function. You'll remember that you had to specify the dimension you wanted to give names each time by using the format **dimnames(object)[[n]]**. Although we pretended that **n** in this case stood for the dimension you are referencing that is exactly true. 

In literal terms, **dimnames( )** is creating a blank list of objects - hence why you need to use the **[[ ]]** notation - where each element of the list is meant to be a vector of names. When you write **dimnames(object)[[n]]<-c("name1","name2",...)** you are telling it to **overwrite** the blank element of the dimnames list with the vector **c("name1","name2",...)**. It's just for convenience that these element positions correspond to the dimensions of the array.

Let's try an example

````
# Let's make a practice data frame
> MyArray<-array(data=c(5,6,4,5,5,6),dim=6)
> MyArray
[1] 5 6 4 5 5 6

# Let's imagine that we know the third element of the array, the number 4, should actually be a 7.
> MyArray[3]<-7
> MyArray
[1] 5 6 7 5 5 6

# Remember that you cannot mix types!
> MyArray[3]<-"seven"
> MyArray
[1] "5"     "6"     "seven" "5"     "5"     "6"  # R automatically coerces the numbers to characters.
````

That's fairly straightforward, but what if we want to overwrite multiple elements?

````
# Create an array
> MyArray<-array(data=c(5,6,4,5,5,6),dim=6)
> MyArray
[1] 5 6 4 5 5 6

# Change all values of 5 to 9
> MyArray[which(MyArray==5)]<-9
> MyArray
[1] 9 6 4 9 9 6
````

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
