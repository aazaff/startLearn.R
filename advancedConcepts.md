# Advanced Concepts

This section covers advanced R concepts, and is meant to be read through after completeing the previous tutorials and exercises in this [repository](https://github.com/aazaff/startLearn.R/blob/master/README.md).

## Table of Contents

+ [Writing your own functions](#writing-your-own-functions)
+ [Automating repetitive tasks](#automating-repetitive-tasks)
+ [Controlling if code executes](#controlling-if-code-executes)
+ [The power to accomplish anything](#the-power-to-accomplish-anything)

 ## Automating repetitive tasks
  
The ability to automate repetitive tasks is the whole reason we use computers in the first place! There are a number of functions built into R that will allow us to repeat an operation over and over again. There are three fundamental units of repetition in R and computer science: ````repeat( )````, ````while( )````, and ````for( )````. Luckily, anything that can be achieved with ````repeat( )```` or ````while( )```` can also be achieved with````for( )````, so you only need to learn one of three. Congrats!

The ````for( )```` function follows a similar format to the ````function( )```` function, in that it is followed by ````{ }```` which enclose the tasks that you wanted automated.

````
# The basic format of a for( ) loop
for (counter in vector) { Commands }

# Or better yet, following good coding practice
for (counter in vector) {
    Commands
    }
````
