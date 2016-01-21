# Expert Concepts

Obviously this is a bit of a misnomer. Becoming an expert in R will take a lot more work than a single online tutorial - no matter how awesome it is. Happily, you now probably know more than a lot of people who call themselves experts in R, so there is that.

If you are working through this tutorial as part of the Geoscience 541: Paleobiology course, you *must* do the [expertTest](https://github.com/aazaff/startLearn.R/blob/master/expertTest.md) exercise and hand in your answers at the start of the next lab period.

## Table of Contents

+ [R is for Statistics](#r-is-for-statistics)
+ [Probabilities and Possibilities](#probabilities-and-possibilities)
+ [What is the purpose of statistics](#what-is-the-purpose-of-statistics)
+ [Common probability distributions](#common-probability-distributions)
+ [Describing probability distributions](#describing-probability-distributions)
+ [The Law of Large Numbers](#the-law-of-large-numbers)

## R is for statistics

There are a wide variety of computer programming languages out there. Many are substantially faster, easier, and/or more widely used than R. The reason that we use R is because it is the boss at statistics, which means that to really need to learn a few basic statistical principles to get the most out of your R experience.

Foremost of these is to understand the concept of **probability**. The best way to do that is by mastering the concept of **probability distributions** and **possibility distributions**.

## Probabilities and Possibilities

Think of a **distribution** as a barrel. The barrel is full of differently colored balls. The **possibility distribution** is the *colors* in the barrel. The **probability distribution** is the *number of balls of each color*. 

Let's visualize the barrel as a **vector** in R.

````
# Fill the barrel (a.k.a., our distribution) with colors
> Barrel<-c("Blue","Blue","Blue","Pink","Pink","Viridian","Puce")

# Find how many balls are in the barrel
> length(Barrel)
[1] 7

# Make a vector of the different colors in the barrel using unique( ) - i.e., the possibility distribution
> unique(Barrel)
[1] "Blue"     "Pink"     "Viridian" "Puce" 

# Find the number of unique colors.
> length(unique(Barrel))
[1] 4

# Find how many of each color are in the barrel using table( ) - i.e., the probability distribution
> table(Barrel)
Barrel
    Blue     Pink     Puce Viridian 
       3        2        1        1 
````

The **probability** of drawing each specific color is simply the probability distribution divided by the ````sum( )```` of the probability distribution.

````
# Find the probability of drawing each color
> table(Barrel) / sum(table(Barrel))
Barrel
     Blue      Pink      Puce  Viridian 
0.4285714 0.2857143 0.1428571 0.1428571 

# Alternatively
> table(Barrel) / length(Barrel)
Barrel
     Blue      Pink      Puce  Viridian 
0.4285714 0.2857143 0.1428571 0.1428571 
````

It's as simple as that, though here is a great example from the Daily Show with Jon Stewart of someone who [does not understand](http://www.cc.com/video-clips/hzqmb9/the-daily-show-with-jon-stewart-large-hadron-collider) the difference between a  **possibility distribution** and a **probability**.

## What is the purpose of statistics

Importantly, statistics is not really about probabilities, but rather about probability distributions. We want to be able to describe probability distributions (**descriptive statistics**) and compare distributions (**inferential statistics**). 

## Common probability distributions

What is not as simple, however, is that we often do not know the underlying probability distribution - i.e., we don't literally have a barrel of balls, deck of cards, set of dice, fliping coin, or whatever in front of us.
