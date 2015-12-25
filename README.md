# An Introduction to R

## Table of Contents
+ [Intalling R](#installing-r) # Instructions on how to install R
+ [Basic Concepts](#basic-concepts) # A review of basic concepts
+ [Why use R?](#why-use-r) # Why you should use R
+ [Mathematical Operations](#mathematical-operations) # Basic arithmetic and operators
+ [Using Functions](#using-functions-to-perform-operations) # Using functions to perform mathematical operations
+ [Data Storage I](#data-storage) # How to store and use stored data
+ [Data Storage II](#data-storage-continued) # Complexities of Data Storage
+ [Data Types](#data-types) # Different types of data that can be stored
+ [Data Storage III](#advanced-data-storage) # Creating complex arrays
+ [Reshaping Data](#reshaping data) # How to reference elements within a data object
+ [Parting Advice](#parting advice) # Words of wisdom from the ancients

## Installing R
The home page for R is [http://www.r-project.org](http://www.r-project.org). You can download the installation file by clicking on the homepage and finding the “download R” link. 

## Basic Concepts
Throughout this tutorial there will be references to **objects** and **functions**. These are the two fundamental units of the R programming language. R stores information as objects and uses functions to interact with the objects. The following quote (not mine) summarizes the relationship quite well.

“Everything that **exists** in R is an object.
Everything that you **do** in R is a function.”

The line between objects and functions can become somewhat blurred because all functions are stored as objects and all objects can only be interacted with via functions. Don't be discouraged by this complexity, just remember the above quote and you will be fine.

## Why use R?
If you are ever feeling overwhelmed by what you are learning or doing in R, never forget that everything you do in R is just a **function** designed for one of the following **four** purposes. Determine which step you are trying to perform, and proceed from there. 

1. Mathematical Operations - Using R as a glorified calculator
2. Storing Data - Storing data in a format that allows you to perform a mathematical operation on it.
3. Reshaping Data - Changing the format of previously stored data so you can perform a different kind of operation on it.
4. Visualize Data - Methods to make graphs or other visual representations of stored data.

Although there are literally hundreds (if not thousands) of ways to do each of these four steps in R, so long as you know a handful of basic methods for each you can accomplish anything.

## The First Rule of R-Club
"Always talk about R!"

Spell things out for future readers of your programs as explicitly as possible – especially for me, because I’m grading you! You want other programmers to easily recognize what you are trying to accomplish with a particular line of code. A quote that I quite like is:

"Always annotate your code, because the **sucker** trying to figure what you did six months from now is going to be **you**."

To do this you should always leave **thorough comments** in your code. Comments in R are always preceded by the # symbol. Anything that follows a # symbol will not be executed by the software.

	> # 2 * 5; everything on this line is a comment none of this will execute if you copy it into R
	
	> 2 * 5 # comments can be placed after a statement. Whatever is in front of the # WILL execute.
	[1] 10

## The Second Rule of R-Club
"The computer is always right."

As beginners, you will often encounter errors. It is easy to become frustrated when the computer does not do what you want. However, remember that the computer "'tis naught but clockwork" and can only do what you tell it to do. If you are getting an error, it is almost certainly **your** fault. Don't despair and don't get mad. Take a deep breath, think about what you are trying to do, and try again.

## Mathematical Operations
If you have ever used a scientific or graphing calculator, then you already intuitively know all the basics of doing arithmetic in R. Yay, you've learned about 20% of R without doing anything! But, let's just start with a quick review anyway.

	# Addition
	> 2+2
	[1] 4

	# Subtraction
	> 5-3
	[1] 2

	# Multiplication
	> 3*4
	[1] 12

	# Division
	> 1/5
	[1] 0.2

	# Make sure you always pay attention to the order of operations (i.e., PEMDAS). Compare the following two 
	# expressions.

	> (1+3)/(5/6)
	[1] 4.8
	> 1+3/5/6
	[1] 1.1

	# Also, be careful about those tricksy negative signs!
	> -5^2
	[1] -25

	> (-5)^2
	[1] 25

You can also perform what are called logical operations. A logical returns a value of either **TRUE** or **FALSE**. Logicals are extraordinarily important in R.

	# Let's ask R if 1 is greater than 0
	> 0 > 1
	[1] FALSE
	
	# What about if 5 is 1/2 of 10?
	> 5 == (1/2 * 10)
	[1] TRUE
	
Notice that we used **==**  to ask if these two quantities are equal. Most **operators** in R are quite straightforward, but some aren't what you'd intuitively expect. Here's a simple list.

+ x **>**  y 	Is x greater than y
+ x **>=** y	Is x greter than or equal to y
+ x **<**  y 	Is x less than y
+ x **<=** y 	Is x less than or equal to y
+ x **==** y 	Is x equal to y
+ x **!=** y 	Is x *not* equal to y

There are a few other special operators in R, but we will worry about them later.

## Using functions to perform operations
Of course, writing out the arithmetic every time wouldn't be any better than just using a calculator. The first benefit that R has over a calculator is it has many **functions** for arithmetic expressions that you can use as shortcuts.

	# For example, if I wanted to take the square root of a number. I could just write out the expression.
	> 4^(1/2)
	[1] 2

	# But, but I can use the sqrt() function to do this as well. Note that all functions have two parts. 
	# The function name - i.e., sqrt - and a set of function arguments – i.e., the object that you want 
	# the function to evaluate, in this case, the number 4. Function arguments are always placed in
	# parentheses.
	> sqrt(4)
	[1] 2

Now you might be thinking, that doesn't really save any time compared to writing out the expression. True, but functions will save you time on more complex expressions.

	# Let's try typing out 10!
	> 10*9*8*7*6*5*4*3*2*1
	[1] 3628800

	# Versus using the factorial function
	> factorial(10) # much clearer!
	[1] 3628800

The second most important benefit of functions is that they explicitly say what your code is doing - remember the first rule of R! Seeing factorial(10) makes it immediately obvious that you are calculating the factorial of ten.

## Data Storage

These simple functions probably still don't seem that impressive if you consider that most calculators already have buttons for square roots, factorials, and the like. However, fucntions in R don't really shine until they are paired with stored data.

Let's consider a quadratic equation, 5x^2+3x+7. Let's say we want to solve this equation for x=4 that's easy to do in the way we've already learned.

	# 5x^2+3x+7, where x=4
	> 5*(4^2)+(3*4)+7
	[1] 99

	# But what if we want to solve this equation for multiple values of x? Let's try solving for x = 1,2,3, and 4.
	# We can do this by telling R to perform the function on all four numbers at once using the c() command. 
	# The c() function tells R to treat the values inside the parentheses, separated by commas, as a single unit.

	> 5*c(1,2,3,4)^2+3*c(1,2,3,4)+7
	[1] 15 33 61 99

	# But what if we had a very long equation and/or a very long list of x values we want solve for? Typing out 
	# all of the x values with c() every time x appears in the equation could get very hard to read very quickly.

	> 9*c(1,2,3,4,5,6,7,8,9,10)^3+6*c(1,2,3,4,5,6,7,8,9,10)^2+8*c(1,2,3,4,5,6,7,8,9,10)+2
	[1]   25  114  323  706 1317 2210 3439 5058 7121 9682

A better alternative involves learning how to store data as an **object**, and then plug the object into equations. The most fundamental type of data object computer science is an **array**. 

An array is essentially a set of values saved to your computer memory that is referenced by a name.

	# Here's an example of how to make an array named "MyArray"
	> MyArray<-array(data=c(1,2,3,4),dim=4)

	# Once we've made the array named MyArray, then typing MyArray will return the values in the array.
	> MyArray
	[1] 1 2 3 4

	# Similarly performing arithmetic on MyArray will apply that expression to all elements in MyArray 
	> MyArray+4
	[1] 5 6 7 9

	# You can input MyArray into a function, and the function will be applied to each element (number) in the array
	> factorial(MyArray)
	[1]  1  2  6 24

	> 5*MyArray^2+3*MyArray+7
	[1] 15 33 61 99

#### ASIDE 1: A brief refresher on the basic concepts
Remember that everything that **exists** in R is an **object** and everything that you **do** is a **function**. In the above example, "MyArray" is an object that you created with the function "Array".

Any time that you want to store the output of a function as an object, you use the "**<-**" operator. In the "MyArray" example, the "**<-**" symbol tells R to store the output of the function on the right, "array( )", under the name on the left - i.e., "MyArray". 
	
	# Here are some other examples
	> BestExample <- sqrt(5)
	> BestExample
	[1] 2.236068
	
	> WinningExample <- factorial(BestExample^2)
	> WinningExample
	[1] 120

## Data Storage Continued
Let's talk some more about the array( ) function, and functions in general. Aside from the **primitive** operations described above (e.g., **+**, **-**, **/**, **>**, **!=**, etc.), functions follow a simple format. They have a name (e.g., sqrt, factorial, array) that is followed by parentheses. Within these parentheses you place one or more **arguments** that tell the function what to do. 

	# Let's review the "MyArray" example again
	> MyArray<-array(data=c(1,2,3,4),dim=4)

The "**data=**" is your way of telling it what data you want stored in the array. In this case, the numbers one, two, three, and four, but you could substitute any list of values or even a single value. 

	# Single value example
	> SingleArray<-array(data=5,dim=1)

In other words data is the **argument** name, and you're telling it what values you want that argument to take. Another way of thinking about this is that you're telling R to temporarily create an object named "data" that the function array( ) should use.  	 
Similarly, "dim=" (short for dimensions) indicates how many values you want in the array. If you indicate more values to the array then you provide, R will simply repeat the values you did give it until the array is full.

	> MyArray<-array(data=c(1,2,3,4,5),dim=10)
	> MyArray
	[1] 1 2 3 4 5 1 2 3 4 5

Beware! This is a really bad behavior that most computer languages would not allow. The potential for introducing error into your calculation in this way isn't trivial, so be precise when defining your array size!

Perhaps the most interesting aspect of "dim=" is that you can **also** give it multiple values with c().

	# Two dimensional array
	> TwoArray<-array(data=c(0,1),dim=c(4,6))
	> TwoArray
	     [,1] [,2] [,3] [,4] [,5] [,6]
	[1,]    0    0    0    0    0    0
	[2,]    1    1    1    1    1    1
	[3,]    0    0    0    0    0    0
	[4,]    1    1    1    1    1    1

Eseentialy, you told R to make 6 arrays with 4 values and store them within a single object named "TwoArray".

	# You can do this as many times as you'd like. For example, 2 sets of 6 sets of arrays with 4 values.
	> ThreeArray<-array(data=c(0,1),dim=c(4,6,2))
	> ThreeArray
	, , 1

	     [,1] [,2] [,3] [,4] [,5] [,6]
	[1,]    0    0    0    0    0    0
	[2,]    1    1    1    1    1    1
	[3,]    0    0    0    0    0    0
	[4,]    1    1    1    1    1    1

	, , 2

	     [,1] [,2] [,3] [,4] [,5] [,6]
	[1,]    0    0    0    0    0    0
	[2,]    1    1    1    1    1    1
	[3,]    0    0    0    0    0    0
	[4,]    1    1    1    1    1    1

We therefore describe arrays based on the number of arrays referenced within them. A single array is a **1-dimensional array**. An array of arrays is a **2-dimensional array**. An array of arrays of arrays is a **3-dimensional array**, and so forth. 

The majority of work done in R is either 1- or 2-dimensional. This has led to the emergence of shorthand terms. A “vector” is a one-dimensional array and a “matrix” is a 2-dimensional array. The overwhelming majority of R users exclusively use vectors or matrices rather than arrays. Importantly, not only is there a difference in terminology, there are actually separate functions, for convenience, that specifically use these terms.

Because the the matrix and vector terminology are so ubiquitous, many R users do not even know about the array() function, and how it relates to vectors and matrices.

	# Compare a 2-dimensional array
	> x<-array(data=c(0,1),dim=c(4,6))
	> x
		[,1] [,2] [,3] [,4] [,5] [,6]
	[1,]    0    0    0    0    0    0
	[2,]    1    1    1    1    1    1
	[3,]    0    0    0    0    0    0
	[4,]    1    1    1    1    1    1

	# With a Matrix
	> y<-matrix(data=c(0,1),nrow=4,ncol=6)
	> y
		[,1] [,2] [,3] [,4] [,5] [,6]
	[1,]    0    0    0    0    0    0
	[2,]    1    1    1    1    1    1
	[3,]    0    0    0    0    0    0
	[4,]    1    1    1    1    1    1

	# We can check if these two objects are identical with the identical() function.
	> identical(x,y)
	[1] TRUE
	
	# Or we can use the "**==**" operator
	> x == y
	[1] TRUE

	# Based on what you've seen above, you may think that you have deduced the appropriate 
	# pattern for writing out a vector.
	> z<-vector(data=c(1,2,3,4),dim=4)
	Error in vector(data = c(1, 2, 3, 4), dim = 4) : 
  	unused arguments (data = c(1, 2, 3, 4), dim = 4)

Nope, doesn't work. Vectors are somewhat more primitive than 1-dimensional arrays, and even though they are **conceptually identical** to a **1-dimensional array**, they are not operationally identical. You can make them just by using the c( ) function we've been using all along.
	
	# Make a vector 
	> MyVector<-c(1,2,3,4)
	> MyVector
	[1] 1 2 3 4

	# Let's make a 1-dimensional array.
	> MyArray<-array(c(1,2,3,4),4)
	> MyArray
	[1] 1 2 3 4

	# You can what type of array something is by using the is()
	> is(MyVector,"vector")
	[1] TRUE

	# Let's test if the 1-D array, MyArray, is identical with the previous vector, MyVector.
	> identical(MyVector,MyArray)
	[1] FALSE

So, fair warning, be careful as to whether you are using vectors, matrices, or arrays.

#### ASIDE 2: To define arguments or not to define arguments
By the way, you may have noticed that I did not specify the arguments "data=" or "dim=" in the above example of the array( ) function. In fact, you may have noticed that for several of the fuctions I have presented thus far that sometimes I defined arguments and sometimes I did not. That is because R is "**smart**" and knows what I meant so long as I put each argument in the **correct order**.

	# This will give me 4-dimensional array, all with the number 4
	> q<-array(4,c(1,2,3,4))

	# This will give me a 1-dimensional array with the numbers 1,2,3, and 4.
	> z<-array(c(1,2,3,4),4)
	
	# Very different! Therefore, you should always define the arguments unless if you KNOW the default order.
	# If you ever want to know the arguments for a function, you can open its help page using "?".
	> ?array
	> ?sqrt

## Data Types
What if we want to store something other than a number? There are a variety of data **types** in R, but there are only a few that you really need to know. Let's begin with the three most basic types.

+ **logical** = **TRUE** or **FALSE**
+ **character** = letters, numbers, and symbols that act like letters
+ **numeric** - numbers that act like numbers

Type **logical** is fairly straightforward it is simply a **TRUE** or a **FALSE** value. Note, however, that if you try to perform basic mathematical operations on a logical vector that R will convert **TRUE** to **1** and **FALSE** to **0**.

	> MyLogical<-c(TRUE,TRUE,FALSE,TRUE,FALSE,TRUE)
	> MyLogical
	[1]  TRUE  TRUE FALSE  TRUE FALSE  TRUE
	
	>MyLogical*3
	[1] 3 3 0 3 0 3
	
	# You can check what type of data you have using the typeof( ) function.
	> typeof(MyLogical)
	[1] "logical"
	
Type **character** is also fairly straightforward. It is basicaly a way of "de-mathing" something. You tell R that something is meant to be a character by using quotation marks "".

	# Create a vector of characters made of letters
	> MyCharacters<-c("Bob","Loves","Lucy","Almost","As","Much","As","He","Loves","R")
	> MyCharacters
	[1] "Bob"    "Loves"  "Lucy"   "Almost" "As"     "Much"   "As"     "He"     "Loves"  "R"
	
	# Create a vector of numbers as characters
	> MyCharacters<-c("1","2","3","4")
	[1] "1" "2" "3" "4"
	
	# Check the type
	> typeof(MyCharacters)
	[1] "character"

By "de-mathing", I mean that R will reject any attempts to do math on your array of characters.

	> sum(MyCharacters)
	Error in sum(MyCharacters) : invalid 'type' (character) of argument
	
**Numeric** is both the simplest and most complicated data type. Basically, numeric data are, you guessed it, numbers that you want to do math on.

	> MyNumeric<-c(1,2,3,4)
	> sum(MyNumeric)
	[1] 10
	
	# Check if MyNumeric is of type numeric
	> is(MyNumeric,"numeric") # Remember the is( ) function from before?
	[1] TRUE
	
	# Check its data type
	> typeof(MyNumeric)
	[1] "double"
	
It gives "double" instead of "numeric"! There are actually several types of numeric data. For our intents and purposes, however, we can consider "double" (the default) to be synonymous with "numeric".
	
## The Third Rule of R-Club
As you progress with R you will learn that keeping track of what **type** of data you are using becomes increasingly important. Most errors that beginners and intermediate useRs encounter are, on some level, a result of using the wrong data type. Therefore, one of the first things you should do when you get an error is double check that you are using the right type of data.

This is worth doing even if you are sure that you entered the data correctly. This is because some R functions, for better or worse, will sometimes **coerce** (convert) your array from one data type into another without your realizing it. In most cases, this is actually convenient, but in other cases it can be the source of much frustration.

## Advanced Data Storage

