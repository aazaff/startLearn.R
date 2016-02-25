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

Functions have a **name**, an **argument**, and one or more actions that it performs. All of a function's actions are known as the **body**. You need to define each of these three things using the following format to create a new function.

````R
# Importantly, notice the new { } syntax.
FunctionName <- function ( FunctionArguments ) { Body }
````

Let's say that we wanted to make a function that adds **s** to whatever singular word we give it.

````R
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

Our ````Pluralise( )```` function didn't work because objects created inside of the function body are not accesible outside of the function. You need to explicitly tell R that you want to see what is inside of the function. There are several ways to do this, but there are only two that are important for you know right now.

The first of these is the ````print( )```` function. This function will *print* whatever arguments you give it to your R terminal. You can use this inside or outside of function bodies.

````R
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

````R
# Create a function that double the argument
> DoubleIt<-function(Argument) {
    Answer<-Argument*2
    return(Answer)
    }
> DoubleIt(2)
[1] 4

# Importantly, elements of the function after return( ) will not execute.
> DoubleIt<-function(Argument) {
    Answer<-Argument*2
    return(Answer)
    Answer<-Answer*2
    }
> DoubleIt(2)
[1] 4
````

## Controlling if code executes

Betweeen ````return( )```` and ````#```` you now know two ways to ensure that R doesn't execute code. However, these are fairly crude mechanisms. Much more powerful and explicit is the ````if( )```` statement, which tells R to only execute a command *if its argument is TRUE.*

````R
# The basic if( ) statement format
> if ( Condition ) {
    Body
    }
````

A function that tells us if a number is even. We're going to use the %% operator from the very beginning of the beginnerConcepts tutorial. Remember that %% finds the remainder of a x divided by y.

````R
# Our Evenness function
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

````R
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

There are a number of functions built into R that will allow us to repeat an operation over and over again. The three fundamental units of repetition in R are ````repeat( )````, ````while( )````, and ````for( )````. Luckily, anything that can be achieved with ````repeat( )```` can also be achieved with````for( )```` and you will rarely need to use ````while( )````, which means you only need to learn one of three. Congrats!

The ````for( )```` function follows a similar format to ````function( )```` and ````if( )````, in that it is followed by ````{ }```` which enclose the **body** of tasks that you wanted automated.

````R
# The basic format of a for( ) loop
> for (Counter in Vector) { Commands }

# Or better yet, following good coding practice
> for (Counter in Vector) {
    Commands
    }
````

You supply every ````for( )```` loop with with a **counter** and a **vector**. The loop will execute the commands of the loop one time for each element in the vector. For example, the loop will repeat six times if you supply it an initial vector with six elements. The **counter** will sucessively take on the value of all the elements in the vector.

````R
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

However, as we discussed earlier, it is a little inconvenient to just have a ton of text returned by ````print( )````. It would be nice if we could store the output of our ````for( )```` loop, say as a vector.

````R
# Try the function as before using return( ) instead of print( )
> for (Day in Week) {
    return(Day)
    }
Error: no function to return from, jumping to top level
````

Nope! ````return( )```` only works with functions. How can we get the output of our loop saved as a vector?

````R
# We could try saving it as a vector?
> for (Day in Week) {
    NewVector<-c(Day)
    }
> NewVector
[1] "Friday"
````

Not quite! The loop creates a new version of NewVector every time. Only the last element of the loop, "Friday" is saved. What we really want is for NewVector to exist outside of the loop, so it isn't recreated each time.

````R
# Create an array of blank values, and the appropriate length
> FinalArray<-array(data=NA,dim=5)
> for (Day in Week) {
    FinalArray<-Day
    }
> FinalArray
[1] "Friday"
````

Nope! Still doesn't work! Because we still have ````FinalArray<-Day```` within the loop, which just rewrites our original version of ````FinalArray````. What we *really* want is to rewrite individual **elements** of the ````FinalArray```` array, not the entire array. Luckily, we already know how to rewrite specific elements of an array using **subscripts**.

But how do we specify the correct **subscript** for each loop? The answer goes back to how we construct the loop in the first place. Instead of having our **counter** be a character specifying the **value** in each element of the vector ````Week````, we should have **counter** reference the **index** of elements in the vector.

````R
# Take a look
> FinalArray<-array(data=NA,dim=5)
> WeekIndex<-1:length(Week)

> for (Index in WeekIndex) {
    FinalArray[Index]<-Week[Index]
    }
    
> FinalArray
[1] "Monday"    "Tuesday"   "Wednesday" "Thursday"  "Friday" 
````

Nice! It totally worked. Granted, all we ended up doing is recreating the original ````Week```` vector. However, you could easily imagine adding in additional expressions into the body. The bottom line is that it is often easier to make your **counter** well... *count*. If the counter records which iteration of the loop you are in, you can use this to **index** other arrays (or lists) accordingly.

## The power to accomplish anything.

Although ````function( )````, ````if( )````, and ````for( )```` were presented last in the tutorial, it is not an exaggeration to say that these functions are the backbone of computer programming. If you truly master these three operations and **subscripting**, then there is literally nothing that you cannot accomplish in R or life.

Of course, there are great many functions still out there for you to learn. As you progress you will doubtlessly find more convenient and efficient ways of doing things than constant ````if/else( )```` and  ````for( )```` statements. Indeed, the mark of a truly high-level R progammer is often thought (controversially) to be measurable by how readily he or she can circumvent cumbersome loops.

Nevertheless, if you do not know a more efficient way to accomplish something, you can *always* fall back on the these basics.
