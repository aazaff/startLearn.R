# Expert Concepts

Expert is an exaggeration. Becoming an expert in R will take a lot more work than a single online tutorial - no matter how awesome it is. Happily, you now probably know more than a lot of people who call themselves experts in R, so there is that.

If you are working through this tutorial as part of the Geoscience 541: Paleobiology course, you *must* do the [expertTest](https://github.com/aazaff/startLearn.R/blob/master/expertTest.md) exercise and hand in your answers at the start of the next lab period.

## Table of Contents

+ [R is for Statistics](#r-is-for-statistics)
+ [Probabilities and Possibilities](#probabilities-and-possibilities)
+ [What is the purpose of statistics](#what-is-the-purpose-of-statistics)
+ [Describing distributions with statistics](#describing-distributions)
+ [Common probability distributions](#common-probability-distributions)
+ [The Law of Large Numbers](#the-law-of-large-numbers)
+ [Testing if two distributions are different](#testing-if-two-distributions-are-different)

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

Now you know more than [this guy on the Daily Show with Jon Stewart](http://www.cc.com/video-clips/hzqmb9/the-daily-show-with-jon-stewart-large-hadron-collider). You're winning.

## What is the purpose of statistics

Importantly, statistics is not really concerned about **probabilities**, but rather about **probability distributions**. Statistics can be divided into two endeavours: *describing* probability distributions (**descriptive statistics**) and *comparing* two or more distributions (**inferential statistics**).

A dizzying variety of tests exist for the purpose of comparing distributions, many of which are pre-programed into R or are available for download. Many people confuse knowing how to use some or many of these tests with a mastery of statistics, but this is like confusing skill with a powersaw as understanding carpentry. They are often related, but neither implies the other.

What is really important for a good statistician is that you never lose sight of your true goal, to accurately describe and compare probability distributions. If you lose sight of this, just take a deep breath and try to envision the barrel(s) and its contents, ask yourself what you are trying to learn about the barrel, how would you answer the question if the "barrel" was *literally in front of you*, and then proceed from there.

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

We call a numerical description of a distribution a **statistic** or a **parameter**. Because the difference between these two won't matter for this class, and because **parameter** is sometimes used interchangeaby with the R concept of an **argument**, we will only refer to them as statistics.

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

It is often preferable to visualize distributions rather than to summarize them with a statistic. There are a number of ways to do this. Probably the most common of these is a **frequency bar plot**. As you can guess from the name, it plots the relative frequency of values in a distribution, i.e., ````table(x)````, as bars.

You can make a kind of frequency bar plot in R using the ````hist( )```` function, which is short for **histogram**. The two terms can be used interchangeably, though as you will see below that is not entirely accurate. Regardless, it is better to think of them as **frequency bar plots** because the word **histogram** has a different meaning in some countries. The former is also the more descriptive name - remember the first rule of R-club!

````
> hist(HoursWorked)
````

Although this is a common way of visualizing data, it isn't very good. One thing that you'll notice is how R places the bars *between* the number ranges. This is because ````hist( )```` groups values together and plots the groups rather than each unique value. For example, the first bar contains both the 6 and 7 values. The second bar contains just the 8's. That is neither intuitive nor clear nor helpful. You're probably asking yourself why R would do something so stupid. It has its reasons, but for now let's try a better way.

What we *really* want is a genuine **frequency bar plot**, meaning the output of ````table(HoursWorked)```` represented as bars. Luckily, this is easily achieved.

````
# The function is literally called barplot( )
> barplot(table(HoursWorked))
````

So much clearer! Generally, it is often more helpful to do a ````barplot( )```` of ````table( )```` than ````hist( )````. Of course, this is not to say that ````barplot( ) ```` works in all situations.

 ````
 > MyVector<-c(1,1,2,2.1,2.2,2.3,2.4,2.5,2.6,2.7,2.8,2.9,3,4,5)
 > mean(MyVector)
[1] 2.566667
 > median(MyVector)
 [1] 2.5
 
 > barplot(table(MyVector)
 ````
 
The output is accurate, but it doesn't adequately give us a sense for the distribution in terms of its spread (standard deviation) or central tendency (mean, median, and mode) of the data. Yes, *techinically* the mode is 1, but the overwhelming majority of data points fall between 2 and 3 - hence the mean and median of ~2.5. Ideally, you want that to be illustrated in your visualization, just like its reflected in the numerical statistics.

````
# This works much better than barplot( )!
hist(MyVector)
````

The difficulty here is that there are, broady speaking, two opposing types of data. **Discrete** data is data that can only take on a specific set of values within a range - e.g., all the integers between 1 and 10. **Continuous** data is data that can take on *any* value within a range. A true bar plot doesn't describe **continuous** data well, but a histogram does - and vice-versa. This is why  ````hist( )```` groups values together, it is a crude attempt to make continuous data visually discrete. 

Just because ````hist( )```` is better than ````barplot( )```` for **continuous** data is no excuse for using it. There are better ways yet to visualize your data. Let's try the ````density( )```` function.

````
> plot(density(MyVector))
````

The data is now represented as a curve rather than as bars, leaving you free from making any assumptions about the appropriate width of bars while conveying all of the same information. People generally don't like to use kernal density plots because their underlying [calculation](http://www.mvstat.net/tduong/research/seminars/seminar-2001-05/) is much more complex than a histogram. However, nobody is asking you to do it by hand in this class, R will do it all for you.

## Common probability distributions

**Continuous** data presents more problems then just how to visualize it. How can we randomly sample a continuous distribution? Up until now we've been making a **vector** and sampling from that vector. 

````
# Set the seed so we all get the same "random" answer
> set.seed(121)
> SimpleVector<-c(1,2,3,4,5)
> sample(SimpleVector,1)
[1] 2
````

That certainly won't work for continuous data, since it is literally impossible to type out an infinite number of points. Luckily, R will do it for us. But first, let's learn about three of the most common distributions.

Distribution | Description | Example
------ | ----- | ----- 
Uniform | All numbers are equally common | Uniform<-c(1,2,3,4,5)
Gaussian | Numbers become steadily less common away from the mean | Gaussian<-c(1,2,2,3,3,3,4,4,5)
Galton | An exponentiated Gaussian distribution | Galton<-exp(Gaussian)

There are several other common distributions - e.g., degenerate, binomial, multinomial, and poisson - that you might encounter in a statistics class, but the most important for this class are the three above. The Gaussian distribution is also known as the **normal** distribution and Galtons distribution is also known as the **log-normal** distribution. I mention this because R uses the normal and log-normal terminology for its functions, so you should be aware of it. 

````
# You can draw a random sample from a uniform distribution using runif( ) - i.e., random uniform
# Draw 10 random samples from a uniform distribution ranging from zero to 1
> runif(10,min=0,max=1)
[1] 0.9515923 0.5431508 0.7627956 0.5508387 0.4210237 0.4676918 0.6132936 0.2377114 0.2546702 0.9468806

# You can draw a random sample from a Gaussian distribution using rnorm( ) - i.e., random gaussian
# Draw 10 samples from a gaussian distribution with a mean of zero and a standard deviation of 1
> rnorm(10,mean=0,sd=1)
[1]  0.7816194 -1.1589419 -0.5563308  0.2983903  0.6919530 -2.0429660  0.1896299 -0.5431431 -1.1201011 -1.5369761

# You can draw a random sample from a Galton distribution using rlnorm( ) - i.e., random log-normal
# Draw 10 samples from a galton distribution with a mean of zero and a standard deviation of 1
> rlnorm(10,meanlog=0,sdlog=1)
[1] 0.1896190 1.4985969 0.2277766 4.4691594 0.8538720 1.1006628 0.1688041 0.2154464 3.7198323 2.0975491
````

## The Law of Large Numbers

You may have noticed for our draw from the random uniform distribution, that it does not seem particularly uniform. Let's take a look at it in a density plot.

````
# Set the seed so we all get the same "random" answer
> set.seed(121)
> plot(density(runif(10,min=0,max=1)))
````

It doesn't look uniform at all! The density of a uniform function should be flatish across the range from its minimum to maximum. It should look something akin to a plateau or mesa. 

