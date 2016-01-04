# Intermediate Concepts

This section covers intermediate R concepts, and is meant to be read through after completeing the [beginnerConcepts](https://github.com/aazaff/startLearn.R/blob/master/beginnerConcepts.md) tutorial and the [beginnerTest](https://github.com/aazaff/startLearn.R/blob/master/beginnerTest.md) exercise.

+ [How to name elements in different data objects](#how-to-name-elements-in-different-data-objects)
+ [Subscripting and subsetting with logicals](#subscripting-and-subsetting-with-logicals)
+ [Overwriting elements using logical subscripts](#overwriting-elements-using-logical-subscripts)
+ [Automating repetitive tasks with loops](#automating-repetitive-tasks-with-loops)
+ [Writing your own functions in R](#writing-your-own-functions-in-r)
+ [Subsetting and iterating in a single step](#subsetting-and-iterating-in-a-single-step)

## How to name elements in different data objects

In the Beginner Concepts tutorial, we used the **names( )** function to add names to a one-dimensional array (vector). Unfortunately, **names( )** is not a flexible function because it does not work with all data objects. As a general rule, use **names( )** if you are naming a **vector** or a **list**. Use **dimnames( )** if you are naming an **array** or **data.frame**.

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

Perhaps the biggest benefit of **[ ]** notation is that we can perform complex subscripting operations within them. The most powerful of these is the **which( )** function, which finds the **index** (a.k.a., the position) of **TRUE** values in a logical array.

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

You can write out very complex logical statements using the **&** (and) and **|** (or) operators. So long as your expression resolves to a vector of logical values.

````
# Find numbers that are greater than 5 AND less than 9
> MyVector[which(MyVector > 5 & MyVector < 9)]
[1] 6 6 7

# Find numbers that are greater than 5 or equal 3
> MyVector[which(MyVector > 5 | MyVector == 3)]
[1] 6 6 3 7 9 3
````

## Overwriting elements using logical subscripts

The true power of **which( )** doesn't become apparent until you want to start **overwriting** elements of a data object. 

Consider when we used the **dimnames( )** function, wre you must specify the dimension you want to name using the format **dimnames(object)[[n]]**. Although we pretended that **n** in this case stood for the dimension you are referencing, that is not exactly true.

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

Even though the matrix is two-dimensional we are getting a one-dimensional response. The output of **which( )** is set to be one-dimensional, so it cannot give us a two-dimensional response of both row and column indexes. 

Rather than give you an error, however, R performs a bit of a hack on your behalf. R converts your matrix from **two-dimensional** data into **one-dimensional** data and then performs its logical subscripting on the new **vector**.

````
# In other words, it does something like this.
> NewVector<-as(MyMatrix,"vector")
> NewVector
[1] 1 2 3 4 5 1 2 3 4 5 1 2 3 4 5 1 2 3 4 5 1 2 3 4 5
> which(NewVector==2)
[1]  2  7 12 17 22
````

This is a nice convenience, but what if we really want to know the two-dimensional, rather than one-dimensional, coordinates? One thing we might attempt is to is to look at *each individual row* of the array, and find which columns *in that row* have the value we are looking for.

````
# Create a 2-dimensional matrix
> MyMatrix<-matrix(data=c(2,7,5,9,6,8,2,8,1,10,8,10,3,2,5,2,8,2,8,3,9,2,7,2,2),nrow=5,ncol=5)
> MyMatrix
     [,1] [,2] [,3] [,4] [,5]
[1,]    2    8    8    2    9
[2,]    7    2   10    8    2
[3,]    5    8    3    2    7
[4,]    9    1    2    8    2
[5,]    6   10    5    3    2

# Let's also name the rows and columsn for clarity
> dimnames(MyMatrix)[[1]]<-c("row1","row2","row3","row4","row5")
> dimnames(MyMatrix)[[2]]<-c("col1","col2","col3","col4","col5")
> MyMatrix
     col1 col2 col3 col4 col5
row1    2    8    8    2    9
row2    7    2   10    8    2
row3    5    8    3    2    7
row4    9    1    2    8    2
row5    6   10    5    3    2

# Now, let's ask for each row, which columns have the number two
> which(MyMatrix["row1",]==2)
col1 col4 
   1    4 
> which(MyMatrix["row2",]==2)
col2 col5 
   2    5 
> which(MyMatrix["row3",]==2)
col4 
   4 
> which(MyMatrix["row4",]==2)
col3 col5 
   3    5 
which(MyMatrix["row5",]==2)
> col5 
   5
````

Now we know the appropriate **[row,column]** coordinates of all 2's in the matrix if we pair the input and the output of each **which( )** statement. 

However, this approach is still unsatisfactory for two reasons. First, what if we had a large number of rows (hundreds or thousands) that we wanted to analyze? Typing out a **which( )** statement for each row would be exhausting and a waste of our time. Second, the way this output is organized isn't very clear, which is a violation of the first rule of R! Not to mention the fact that it also requires us to manually pair the rows with the columns, rather than giving us a single array of coordinates.

## Automating repetitive tasks

Resolving the first of these problems is fairly easy as there are a number of functions built into R that will allow us to repeat an operation over and over again. The ability to automate repetitive tasks is the whole reason we use computers in the first place!

In all of computer science, there are three fundamental units of repetition: **repeat( )**, **while( )**, and **for( )**. Luckily, anything that can be achieved with **repeat( )** can also be achieved with **while( )** and **for( )**, so you only need to learn two of three. Congrats!

The **while( )** function tells R to repeat an expression (or multiple expressions) while a certain **logical** expression evaluates to **TRUE** and stop when that condition becomes **FALSE**.

````
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

Second, notice that we are generous about starting a new line for each expression - the final curly bracket got a whole line to itself! Remember that R does not evaluate white space (spaces, tabs, and returns), so use these freely to structure your data in a way that is easy to visualize (for your benefit and mine!).

````
# For example, a more complicated use of while( ) with multiple statements

> Start<-1
# We set the expression to repeat while Start is less than or equal to 20
# Note that we put each expression on a different line. This is good coding practice.
> while(Start <= 20) {
    Start<-Start+1
    Start<-Start-2
    Start<-Start*2
    Start<-Start/3
    }
````

You may have noticed that the above example of a **while( )** function never stops running. If you look closely, you'll see that it is mathematically impossible for Start to ever become higher than 20! This means that while( ) will ***never stop***. You can hit the **ESC** button on your keyboard to stop the expression from executing.

In the old days, hackers would attack computer by making them run infinite while( ) loops until they crashed from the strain. It is more of an annoyance nowadays than a danger, but always make sure that your **while( )** condition will actually stop at some point.

The **for( )** function, also called **for( ) loops**, are a safer alternative to **while( )**.

````

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
