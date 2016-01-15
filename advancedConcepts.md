# Advanced Concepts

This section covers advanced R concepts, and is meant to be read through after completeing the previous tutorials and exercises in this [repository](https://github.com/aazaff/startLearn.R/blob/master/README.md). Attempt the [advancedTest](https://github.com/aazaff/startLearn.R/blob/master/advancedTest.md) after you are finished reading through this tutorial.

## Table of Contents

+ [Writing your own functions](#writing-your-own-functions)
+ [Controlling if code executes](#controlling-if-code-executes)
+ [Automating repetitive tasks](#automating-repetitive-tasks)
+ [The power to accomplish anything](#the-power-to-accomplish-anything)

## Writing your own functions

There are lots of great and user-friendly statistics programs out there. The reason why **R** is superior to all others is that you aren't limited to the functions that come installed with the program. You can write your own, or import functions written by other people.

Writing a new function requires the ````function( )```` function. Say that five times fast! 

Functions have a **name**, an **argument**, and one or more actions that it performs. All of a functions actions are known as the **body**. In order to create a function, you need to define these three things using the following format.

````
# Importantly, notice the new { } syntax.
FunctionName <- function ( FunctionArguments ) { Body }
````

Let's say that we wanted to make a function that adds **s** to whatever singular word we give it.

````
# Write out a function that takes a "SingularWord" as an argument and adds s
> Pluralise<-function(SingularWord) {
    PluralWord<-c(SingularWord,"s")
    }

# Let's test it out!
> Pluralise("Donut")
````

Wait a minute nothing happened!? 

#### The sixth rule of R-club.
***"What happens in the function body, stays in the function body."***

Our ````Pluralise```` function didn't work because objects created inside of the function body are not accesible outside of the function. You need to explicitly tell R that you want to see what is inside of the function. There are several ways to do this, but there are only two that are important for you know right now.

The first of these is the ````print( )```` function. This function will *print* whatever arguments you give it to your R terminal. You can use this inside or outside of function bodies.

````
> print("It works, I swear.")
[1] "It works, I swear."

> MyFunction<-function(Argument) {
    print(Argument)
    }
    
> MyFunction("Totally works.")
[1] "Totally works." 
````

It worked! Unfortunately ````print( )```` isn't very flexible, as it will only print your output as a **character** string. Since we mostly want to use R for math purposes, we'll want something more flexible. Also, if the function output is really large, we probably don't want to flood our screens with unnecessary text. One option is to use the ````return( )```` function.

Using ````return( )````  automatically stops the function and returns whatever **object** is its **argument**.

````
# Create a function that double the argument
> MyFunction<-function(Argument) {
    Answer<-Argument*2
    return(Answer)
    }
> MyFunction(2)
[1] 4

# Importantly, elements of the function after return( ) will not execute.
> MyFunction<-function(Argument) {
    Answer<-Argument*2
    return(Answer)
    Answer<-Answer*2
    }
> MyFunction(2)
[1] 4
````

## Controlling if code executes

Betweeen ````return( )```` and ````#```` you now know two ways to ensure that R doesn't execute code. However, these are fairly crude mechanisms. Much more powerful and explicit is the ````if( )```` statement, which tells R to only execute a command *if its argument is TRUE.*

````
# The basic if( ) statement format
> if ( Condition ) {
    Body
    }

# A function that tells us if a number is even. 
# We're going to use the %% operator from the very beginning of the beginnerConcepts tutorial.
# Remember that %% finds the remainder of a x divided by y.
> Evenness<-function(Number) {
    if (Number %% 2 == 0) {
        print("Even!")
        }
    }

# Since 
> 4 %% 2 == 0
[1] TRUE

# Therefore...
> Evenness(4)
[1] "Even!"

# What if we want the function to also tell us if the number is odd?
# We can use the else { } statement. 
> Evenness<-function(Number) {
    if (Number %% 2 == 0) {
        print("Even!")
        }
    else {
        print("Odd :(")
        }
    }

> Evenness(5)
[1] "Odd :("
````

Notice that ````else{ }```` is a unique function that does not use any parenthesis! This is because its **argument** - i.e., the logical condition - was *already defined* in the original  ````if( )```` statement. Importantly, this means that you would never use ````else{ }```` without a preceeding ````if( )```` statement, though you can use ````if( )```` without defining an ````else{ }````. 

The if/else paradigm is intimately linked to a binary TRUE/FALSE paradigm, but what if we want to control an outcome where there are more than two cases?

For example, a function that convert from pounds to kilometers if we give it a weight measurement, from inches to centimeters if we give it a length, and from farenheit to centigrade for a temperature.

````
# We can use the else if ( ) { } format!
> Conversion<-function(Unit, Value) {
    if (Unit=="Pounds") {
        return(Value/2.2)
        }
    else if (Unit=="Inches") {
        return(Value*2.54)
        }
    else if (Unit=="Farenheit") {
        return((Value-32)*(5/9))
        }
    }

# Give it a weight
> Conversion("Pounds",5)
[1] 2.272727

# Give it a length
> Conversion("Inches",5)
[1] 12.7

# Give it a temperature
> Conversion("Farenheit",5)
[1] -15
````        

You may have noticed that we used three ````return( )```` functions in our ````Conversion```` function. You might ask, but didn't we learn that ````return( )```` stops a function from executing? Remember, whatever comes after an if, else if, or else statement only executes if the logical condition evaluates to ````TRUE````.

## Automating repetitive tasks
 
While if/else is pretty powerful, especially when combined with your own functions, the most powerful tool at your disposal in R is the ability to automate repetitive tasks. That's actually the whole reason we even use computers. 

There are a number of functions built into R that will allow us to repeat an operation over and over again. The three fundamental units of repetition in R are ````repeat( )````, ````while( )````, and ````for( )````. Luckily, anything that can be achieved with ````repeat( )```` or ````while( )```` can also be achieved with````for( )````, so you only need to learn one of three. Congrats!

The ````for( )```` function follows a similar format to ````function( )```` and ````if( )````, in that it is followed by ````{ }```` which enclose the **body** of tasks that you wanted automated.

````
# The basic format of a for( ) loop
> for (Counter in Vector) { Commands }

# Or better yet, following good coding practice
> for (Counter in Vector) {
    Commands
    }
````

You supply every ````for( )```` loop with with a **counter** and a **vector**. The loop will execute the commands of the loop one time for each element in the vector. For example, the loop will repeat six times if you supply it an initial vector with six elements. The **counter** will sucessively take on the value of all the elements in the vector.

````
# Here's another way to think of it. Start with a vector.
> Week<-c("Monday","Tuesday","Wednesday","Thursday","Friday")

# Make a for loop that prints each day of the week
> for (Day in Week) {
    print(Day)
    }
[1] "Monday"
[1] "Tuesday"
[1] "Wednesday"
[1] "Thursday"
[1] "Friday"

# This is conceptually identical to
> Day<-Week[1]
> print(Day)
[1] "Monday"
> Day<-Week[2]
> print(Day)
[1] "Tuesday"
> Day<-Week[3]
> print(Day)
[1] "Wednesday"

# etc..
````

## The power to accomplish anything.

Although ````function( )````, ````if( )````, and ````for( )```` were presented last in the tutorial, it is not an exaggeration to say that these functions are the backbone of computer programming. If you truly master these three concepts and subscripting, then there is literally nothing that you cannot accomplish in R or life.
