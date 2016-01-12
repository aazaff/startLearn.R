# Intermediate Concepts

This section covers intermediate R concepts, and is meant to be read through after completeing the [beginnerConcepts](https://github.com/aazaff/startLearn.R/blob/master/beginnerConcepts.md) tutorial and the [beginnerTest](https://github.com/aazaff/startLearn.R/blob/master/beginnerTest.md) exercise.

## Table of Contents

+ [Subscripting and subsetting with logicals](#subscripting-and-subsetting-with-logicals)
+ [Rewriting elements using logical subscripts](#rewriting-elements-using-logical-subscripts)
+ [Writing your own functions in R](#writing-your-own-functions-in-r)
+ [A mystical Right of Passage](#a-mystical-rite-of-passage)
+ [Subsetting and iterating with functionals](#subsetting-and-iterating-in-a-single-step)
+ [Functions for summarizing data](#functions-for-summarizing-data)

## Subscripting and subsetting with logicals

Perhaps the biggest benefit of **[ ]** notation is that we can perform complex subscripting operations within them. The most powerful of these is the **which( )** function, which finds the **index** (a.k.a., the position) of **TRUE** values in a logical array. In other words **which( )** is short for the phrase: *which of the elements in this array are TRUE values*.

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

This isn't very impressive since it is circular. We asked which elements had a value of **TRUE**, so of course the values of those elements is **TRUE**. But, what if we don't start out with logical data?

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

You can combine logical statements using the **&** (and) and **|** (or) operators.

````
# Find numbers that are greater than 5 AND less than 9
> MyVector[which(MyVector > 5 & MyVector < 9)]
[1] 6 6 7

# Find numbers that are greater than 5 or equal 3
> MyVector[which(MyVector > 5 | MyVector == 3)]
[1] 6 6 3 7 9 3
````

## Rewriting elements using logical subscripts

The true power of **which( )** doesn't become apparent until you want to start **rewriting** elements of a data object. 

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
[1] "5"     "6"     "seven" "5"     "5"     "6"  

# R automatically coerced the numbers to characters.
````

That's fairly straightforward, but what if we want to overwrite multiple elements in an array?

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

We can also perform logicals on two and three-dimensional arrays, but it can be somewhat more complicated.

````
# Let's attempt to do some logicals on a matrix of numerical data
> MyMatrix<-matrix(data=c(1,2,3,4,5),nrow=5,ncol=5)
> MyMatrix
     [,1] [,2] [,3] [,4] [,5]
[1,]    1    1    1    1    1
[2,]    2    2    2    2    2
[3,]    3    3    3    3    3
[4,]    4    4    4    4    4
[5,]    5    5    5    5    5

# Let's try and find which elements of the matrix are equal to 2
> which(MyMatrix==2)
[1]  2  7 12 17 22
````

It worked in the sense that we didn't get an error, but you might think that the output is a little strange. 

We got a one-dimensional response even though the matrix is two-dimensional. The output of **which( )** is set to be one-dimensional (i.e, a vector), so it cannot give us a two-dimensional response of both row and column indexes. 

Rather than give you an error, however, R performs a bit of a hack on your behalf. R converts your matrix from **two-dimensional** data into **one-dimensional** data and then performs its logical subscripting on the new **vector**.

````
# In other words, it does something analagous to this.

# Convert your matrix to a vector
> NewVector<-as(MyMatrix,"vector")
> NewVector
[1] 1 2 3 4 5 1 2 3 4 5 1 2 3 4 5 1 2 3 4 5 1 2 3 4 5

# Find the positions of the new vector equal to two.
> which(NewVector==2)
[1]  2  7 12 17 22
````

This is a nice convenience, but what if we really wanted to know the two-dimensional (row *and* column coordinates), rather than the one-dimensional vector coordinates?

## Writing your own functions in R

There are a variety of ways to handle this problem, but let's consider writing a custom function instead. Before we begin writing new functions, let's review the basic components of a function that we covered in the basicConcepts(https://github.com/aazaff/startLearn.R/blob/master/beginnerConcepts.md#differences-between-array-the-object-and-array--the-function) tutorial.

Function Component | Description
-------- | --------
Name | All functions must have a name (except in the unique case of **functionals**, which we will discuss later).
Argument(s) | The objects that you want the function to affect. Use commas to separate multiple arguments if necessary.
Body | The body of the function is what you want the function to *do* to the argument(s) you gave it. Each individual expression should be written on its own unique line. The body is always contained in squiggly brackets **{ }**.

You create a new function using the **function( )** function. Say that five times fast! The basic outline of a **function** is as follows.

     Name <- function (Arguments) {Body}

I think that the name and arguments part is relatively straightforward. It is the body part that is new. Let's consider a simple example function that multiplies its **argument** by the number three.

     > MultiplyThree <- function ( Argument ) { Argument * 3 }
     > MultiplyThree(4)
     [1] 12
     
Such a simple function probably doesn't seem that impressive considering that we could already multiply numbers without writing a new function. However, custom functions are much more impressive when you realize that you can evaluate many expressions at once within them.

Let's look at an example of a more complex function I wrote called **calcHaversine( )**. The Haversine function calculates the shortest arc distance between two points on the Earth. Imagine if I wanted to calculate the distances between hundreds, thousands, or hundreds of thouands of points! It is certainly much easier to have a function than it is to write out the calculation every single time.

````     
# Calculates the geodesic distance between two points on the Earth specified by 
# Latitude and Longitude (in radians) using the Haversine formula. 
# The Haversine formula is a special case of the spherical law
# of cosines, which is a special case of the law of cosines.
calcHaversine<-function(Long1,Lat1,Long2,Lat2) {
     Radius<-6371 # radius of the Earth (km)
     DeltaLong<-(Long2-Long1) # Difference between longitudes
     DeltaLat<-(Lat2-Lat1) # Difference between latitudes
     A<-sin(DeltaLat/2)^2+cos(Lat1)*cos(Lat2)*sin(DeltaLong/2)^2
     C<-2*asin(min(1,sqrt(A)))
     Distance<-Radius*C
     return(Distance) # The distance in km
     }
````

Even this example isn't quit impressive enough - it's still just basic arithmetic - because it doesn't utilize the two most powerful features in computer science: **conditionals** and **loops**. Custom functions, and computer programming in general, doesn't really start to shine until you understand how to use **conditionals** and **loops**.

The **if( )** function asks if a logical expression evaluates to TRUE or FALSE. If it does evaluate to TRUE then it performs an action. If it evaluates to FALSE, then nothing happens. 

````
# The basic form of an if statement
if (condition) {do this}

# A simple example
> x <- 4
> if (x < 5) {
     x<-5
     }
````

## Automating repetitive tasks

There are a number of functions built into R that will allow us to repeat an operation over and over again. 

There are three fundamental types of repetition that can be found in most computer science lanugages: **repeat( )**, **while( )**, and **for( )**. Luckily, anything that can be achieved with **repeat( )** can also be achieved with **while( )** or **for( )**, so you only need to learn two of three. Congrats!

The **while( )** function tells R to repeat an expression (or multiple expressions) while a certain **logical** expression evaluates to **TRUE** and stop when that condition becomes **FALSE**.

````
# while (Condition) {Expressions}, is the basic format of while( )

# Let's create an object named start with a numeric value of 1
> Start<-1

# Let's ask R to keep adding 1 to the value of Start until start equals 20
> while(Start != 20) {
    Start<-Start+1
    }

# Check if R did what we asked
> Start
[1] 20
````

There are a few things you should notice about the above example. First, notice that we used curly brackets **{ }** to tell R what expressions it should peform - i.e., Start + 1 - while the condition - i.e., Start != 20 - is **FALSE**. These curly brackets are very important and if you are writing a long function with many steps, you might forget to put the brackets in at the right places. 

For this reason, whenever we use an opening curly bracket **{** we tab the start of each successive line until we reach the final closing **}** bracket. This makes it easier to remember and check if we put the curly bracket in the correct place.

Second, notice that we are generous about starting a new line for each expression. The final curly bracket got a whole line to itself! Remember that R (usually) does not evaluate white space (spaces, tabs, and returns), so use these freely to structure your data in a way that is easy to visualize (for your benefit and mine!).

````
# For example, a more complicated use of while( ) with multiple statements.

> Start<-1

# We set the expression to repeat while Start is less than or equal to 20
# Note that we put each expression on a different line. This is good coding practice.
> while(Start <= 20) {
    Start<-Start+1
    Start<-Start-3
    Start<-Start+2
    }
````

You may have noticed that the above example never stops running! If you look closely, you'll see that it is mathematically impossible for Start to ever become higher than 20! This means that while( ) will ***never stop***. You can hit the **ESC** button on your keyboard to stop the expression from executing.

Always make sure that your **while( )** condition will actually stop at some point.

The **for( )** function, also called **for( ) loops**, are a safer alternative to **while( )**. For loops follow a special format that is very similar to a **while( )** loop.

     while (Condition) {Expressions}
     for (Counter in Vector) {Expressions}

The **for( ) loop** creates a temporary object known as the **counter**. It will then evaluate the given **expressions** enclosed in the **{ }** where the counter is set to equal each element of the given vector.

     # For example
     > MyVector<-1:15
     > MyVector
     [1]  1  2  3  4  5  6  7  8  9 10 11 12 13 14 15
     
     > for (Counter in MyVector) {
          MyVector[Counter]<-Counter+1
          }
     > MyVector
     [1]  2  3  4  5  6  7  8  9 10 11 12 13 14 15 16
     
You do not need to use the name **counter** for the **counter**. It is customary in most programming languages to name your counter using a singe lower case letter, usually "i", "j", "q", or "t". 

     # A counter named i
     for (i in c("a","b","c","d") {
          i
          }
     [1] "a"
     [1] "b"
     [1] "c"
     [1] "d"



## A mystical rite of passage
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

First, let's consider making the function more versatile, so that it can return phrases other than "hello, world.". We'll do this by adding an **argument** to the function. We'll call this argument **Phrase**. Furthermore, we should also give the function a more versatile name, like **Print**, to reflect the more generic nature of the function.

As a general rule all **objects** that you create in R, whether **functions**, **arguments**, or **data arrays**, should always have a name that is descriptive of what it does. Vague names like x or y should never be used (except in cases of famous math equations, like the slope of a line or a quadratic formula).

````
# Write a more general function function as outlined
> Print<-function(Phrase) {
    return(Phrase)
    }
> Print("hello, world.")
[1] "hello, world."
````
  
Also notice that the function began with a single line that (1) created a function object named print and (2) defined the functon arguments. We then ***created a new line*** for the body of the function, i.e., the command to **return( )** the argument **Phrase**. It is always good practice to put each individual command of a function on a new line. This makes it easier to read.

## Subsetting and iterating in a single step
