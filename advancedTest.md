# Introduction

The following are a set of self-test questions designed to complement the [advancedConcepts](/blob/master/advancedConcepts.md) tutorial. These exercises are only meant to be attempted **after** you have finished the advancedConcepts tutorial.

## A Mystical Rite of Passage

The only way to learn a new programming language is by writing programs in it. The first program (function) to write is the same for all languages. You must print the words, "Hello, world." *This is a sacred rite of passage, cherish it!*

1. Write a function that returns the phrase "Hello, World."


## Problem Set

1. Load the ````iris```` dataset we used in the earlier tests. Write a function that takes ````iris```` as its argument, and returns three subsets of the data.frame split by the three different types of species (saved as a single object).

2. Write a function that takes ````iris```` as its argument. The function should, for each row, *add* **Sepal.Length** and **Petal.Length** *if* **Sepal.Width** is > 3.1. It should *substract* **Petal.Length** from **Sepal.Length** *if* **Sepal.Width** is <3.1. The answer should be returned as a vector.

3. Load the ````mtcars```` dataset we used in the earlier tests. Write a function that takes a number of cylinders as its argument. Have the function return the average miles per gallon (column **mpg**) for *all* cars with that many cylinder (column **cyl**).

4. Write a function that simulates 1,000,000 powerball drawings. A powerball drawing takes a random **sample** of 5 numbers (without replacement) from 1 through 69, plus one powerball number ranging from 1 through 26. The function should return a single object recording all of your draws.

5. Write a function that take a single set of lottery numbers (as a vector) as its **argument**. As before, write a function that simulates 1,000,000 powerball drawings. Have the function return a ````TRUE```` or ````FALSE````  value if you won any of the drawings.
