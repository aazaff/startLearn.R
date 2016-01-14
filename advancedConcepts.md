# Advanced Concepts

This section covers advanced R concepts, and is meant to be read through after completeing the previous tutorials and exercises in this [repository](https://github.com/aazaff/startLearn.R/blob/master/README.md).

## Table of Contents

+ [Writing your own functions](#writing-your-own-functions)
+ [Automating repetitive tasks](#automating-repetitive-tasks)
+ [Controlling if code executes](#controlling-if-code-executes)
+ [The power to accomplish anything](#the-power-to-accomplish-anything)

## Writing your own functions

There are lots of great and user-friendly statistics programs out there. The reason why **R** is superior to all others is that you aren't limited to the functions that come installed with the program. You can write your own, or import functions written by other people.

Writing a new function requires the ````function( )```` function. Say that five times fast! 

Functions have a **name**, an **argument**, and one or more **actions** that it performs. All of a functions **actions** are known as the **body**. In order to create a function, you need to define these three things.

````
FunctionName <- function ( FunctionArguments ) { Body }
````

Let's say that we wanted to make a function that adds **s** to whatever word we give it.

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
***"What happens in the body stays in the body."***

Our ````Pluralise```` function didn't work because objects created inside of the function body are not accesible outside of the function. You need to explicitly tell R that you want to see what is inside of the function. There are several ways to do this, but there are only two that are important for you know right now.

The first of these is the ````print( )```` function. This function will *print* whatever object you put insdie of the argument to your R terminal. You can use this inside or outside of function bodies.

````
> print("It works, I swear.")
[1] "It works, I swear."

> MyFunction<-function(Argument) {
    print(Argument)
    }
    
> MyFunction("Totally Works")
[1] "Totally Works" 
````

It worked! Okay well, 

The only way to learn a new programming language is by writing programs in it. The first program (function) to write is the same for all languages. You must print the words, "Hello, world." *This is a sacred rite of passage, cherish it!*




## Automating repetitive tasks
  
The ability to automate repetitive tasks is the whole reason we use computers. There are a number of functions built into R that will allow us to repeat an operation over and over again. 

The three fundamental units of repetition in R are ````repeat( )````, ````while( )````, and ````for( )````. Luckily, anything that can be achieved with ````repeat( )```` or ````while( )```` can also be achieved with````for( )````, so you only need to learn one of three. Congrats!

The ````for( )```` function follows a similar format to the ````function( )```` function, in that it is followed by ````{ }```` which enclose the tasks that you wanted automated.

````
# The basic format of a for( ) loop
for (Counter in Vector) { Commands }

# Or better yet, following good coding practice
for (Counter in Vector) {
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

