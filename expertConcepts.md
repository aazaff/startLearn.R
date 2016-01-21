# Expert Concepts

Obviously this is a bit of a misnomer. Becoming an expert in R will take a lot more work than a single online tutorial - no matter how awesome it is. Happily, you now probably know more than a lot of people who call themselves experts in R, so there is that.

If you are working through this tutorial as part of the Geoscience 541: Paleobiology course, you *must* do the [expertTest](https://github.com/aazaff/startLearn.R/blob/master/expertTest.md) exercise and hand in your answers at the start of the next lab period.

## Table of Contents

+ [R is for Statistics](#r-is-for-statistics)
+ [Probabilities and Possibilities](#probabilities-and-possibilities)
+ [What is the purpose of statistics](#what-is-the-purpose-of-statistics)
+ [Describing probability distributions](#describing-probability-distributions)
+ [Common probability distributions](#common-probability-distributions)
+ [Describing probability distributions](#describing-probability-distributions)
+ [The Law of Large Numbers](#the-law-of-large-numbers)

## R is for statistics

There are a wide variety of computer programming languages out there. Many are substantially faster, easier, and/or more widely used than R. The reason that we use R is because it is the boss at statistics, which means that to really need to learn a few basic statistical principles to get the most out of your R experience.

This is not meant to be a statistics tutorial, however, so we will keep it light and stick with only key concepts.

Foremost of these is the concept of **probability**. Probability is best understood in terms of **probability distributions** and **possibility distributions**.

## Probabilities and Possibilities

Think of a **distribution** as a wooden barrel. The barrel is full of differently coloured balls. The **possibility distribution** is the *list of colours* in the barrel. The **probability distribution** is the *number of balls of each colour*. 

Let's visualize the barrel as a **vector** in R.

````
# Fill the barrel (a.k.a., our distribution) with colours
> Barrel<-c("Blue","Blue","Blue","Pink","Pink","Viridian","Puce")

# Find how many balls are in the barrel
> length(Barrel)
[1] 7

# Make a vector of the different colours in the barrel using unique( ) - i.e., the possibility distribution
> unique(Barrel)
[1] "Blue"     "Pink"     "Viridian" "Puce" 

# Find the number of unique colours.
> length(unique(Barrel))
[1] 4

# Find how many of each colour are in the barrel using table( ) - i.e., the probability distribution
> table(Barrel)
Barrel
    Blue     Pink     Puce Viridian 
       3        2        1        1 
````

The **probability** of drawing each specific colour is simply the probability distribution divided by its ````sum( )````.

````
# Find the probability of drawing each colour
> table(Barrel) / sum(table(Barrel))
Barrel
     Blue      Pink      Puce  Viridian 
0.4285714 0.2857143 0.1428571 0.1428571 

# Alternatively
> table(Barrel) / length(Barrel)
Barrel
     Blue      Pink      Puce  Viridian 
0.4285714 0.2857143 0.1428571 0.1428571

# Notice that the sum of all probabilities is 1, this will always be the case.
> sum(table(Barrel)/length(Barrel))
[1] 1
````

It's as simple as that, though here is a great example from the Daily Show with Jon Stewart of someone who [does not understand](http://www.cc.com/video-clips/hzqmb9/the-daily-show-with-jon-stewart-large-hadron-collider) the difference between a  **possibility distribution** and a **probability**.

## What is the purpose of statistics

Importantly, statistics is not really concerned about **probabilities**, but rather about **probability distributions**. Statistics can be divided into two endeavours: *describing* probability distributions (**descriptive statistics**) and *comparing* two or more distributions (**inferential statistics**).

A dizzying variety of tests exist for the purpose of comparing distributions, many of which are pre-programed into R or are available for download. Many people confuse knowing how to use some or many of these tests with a mastery of statistics, but this is like confusing skill with a powersaw as understanding carpentry. They are often related, but neither implies the other.

What is really important for a good statistician is that you never lose sight of your true goal, to accurately describe and compare probability distributions. If you lose sight of this, just take a deep breath and try to envision the barrel(s) and its contents, ask yourself what you are trying to learn about the barrel, how would you answer this if the barrel was literally in front of you, and then proceed from there.

## Describing probability distributions

Let's start by making ourselves a basic probability distribution. Because I don't feel like typing out a lengthy distribution, we're going to make use of the ````rep( )```` function, short for *repeat*. Here's a quick rundown of how it works.

````
# Make a new vector that repeats each element of an existing vector twice
> OriginalVector<-c(1,2,3)
> OriginalVector
[1] 1 2 3

> NewVector<-rep(OriginalVector,each=2)
> NewVector
[1] 1 1 2 2 3 3

# Alternatively, make a new vector that repeats each element a unique number of times
# Using the time= argument instead of the each= argument.
> NewVector<-(rep(OriginalVector,times=c(3,4,2)))
> NewVector
[1] 1 1 1 2 2 2 2 3 3
````

So, let's make a basic probability distribution. Let's look at how many hours I've worked each day for the past 31 days.

````
> HoursWorked<-rep(c(6,7,8,9,10,11,12,13),times=c(2,2,11,8,2,2,3,1))
> HoursWorked
[1]  6  6  7  7  8  8  8  8  8  8  8  8  8  8  8  9  9  9  9  9  9  9  9 10 10 11 11 12 12 12 13
````

We call a numerical description of a distribution a **statistic** or a **parameter**. Because the difference between these two won't matter for this class, and because **parameter** is sometimes used interchangeaby with the the R concept of an **argument**, we will only refer to them as statistics.

````
# The arithmetic mean is the most common statistic. It is simply the average of all points.
> mean(HoursWorked)
[1] 8.935484

# Find the median - i.e., is the literal midpoint
> median(HoursWorked)
[1] 9

# The standard deviation is the square root of the average squared distance of all points to the mean
# It is a basic measure of how spread out points are around the mean.
# This is easily calculated using the sd( ) function
> sd(HoursWorked)
[1] 1.730809

# The max( ) and min( ) record the highest and lowest value in a distribution.
> min(HoursWorked)
[1] 6
> max(HoursWorked)
[1] 13

# The mode is the most common value in the distribution. There is no function for calculating the mode in R.
# There is a function called mode( ), but it is unrelated to the statistic.
# We can calculate the mode manually using which( ), table( ), and max( )
> which(table(HoursWorked)==max(table(HoursWorked)))
8 
3 

# Alternatively, you can use the which.max( ) function, which is identical to which(x==max(x)).
> which.max(table(HoursWorked))
8 
3
````

It is often preferable to visualize distributions rather than to summarize them with a statistic. There are a number of ways to do this. Probably the most common of these is a **frequency bar plot**. As you can guess from the name, it plots the relative frequency of values in a distribution as bars - i.e., hist(x) is a visual form of table(x).

You can make a frequency bar plot in R using the ````hist( )```` function, which is short for **histogram**. The two terms can be used interchangeably, but it is better to think of them as **frequency bar plots** because the word **histogram** has a different meaning in some countries. The former is also the more descriptive name - remember the first rule of R-club!

````
> hist(HoursWorked)
````

Although this is a common way of visualizing data, it isn't very good. One thing that you'll notice is how R places the bars *between* the number ranges. This is because ````hist( )```` combines numbers into *bins*. For example, the first bar contains both the 6 and 7 values. The second bar contains just the 8's. That is neither intuitive nor clear nor helpful. You're probably asking yourself why R would do something stupid. It has its reasons, but for now let's try a better way.

What we *really* want is a true **frequency bar plot**, meaning the output of ````table(HoursWorked)```` represented as bars.



