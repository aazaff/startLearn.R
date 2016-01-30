# Expert Concepts

Expert is an exaggeration. Becoming an expert in R will take a lot more work than a single online tutorial - no matter how awesome it is. Happily, you now probably know more than a lot of people who call themselves experts in R, so there is that.

If you are working through this tutorial as part of the Geoscience 541: Paleobiology course, you *must* do the [expertTest](https://github.com/aazaff/startLearn.R/blob/master/expertTest.md) exercise and hand in your answers at the start of the next lab period.

## Table of Contents

+ [R is for Statistics](#r-is-for-statistics)
+ [Probabilities and Possibilities](#probabilities-and-possibilities)
+ [What is the purpose of statistics?](#what-is-the-purpose-of-statistics)
+ [Describing distributions with statistics](#describing-distributions-with-statistics)
+ [Common sampling distributions](#common-sampling-distributions)
+ [The Law of Large Numbers](#the-law-of-large-numbers)
+ [Testing if two distributions are different](#testing-if-two-distributions-are-different)

## R is for statistics

There are a wide variety of computer programming languages out there. Many are substantially faster, easier, and/or more widely used than R. The reason that we use R is because it is the boss at statistics. You really need to learn a few basic statistical principles to get the most out of your R experience.

This is not meant to be a statistics tutorial, however, so we will keep it light and stick with only key concepts.

Foremost of these is the concept of **probability**. Probability is best understood in terms of a **sampling distribution** and a **possibility distribution**.

## Probabilities and Possibilities

Think of a **distribution** as a wooden barrel. The barrel is full of differently coloured balls. The **possibility distribution** is the *list of colours* in the barrel. The **sampling distribution** is the *number of balls of each colour*. 

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

# Find how many of each colour are in the barrel using table( ) - i.e., the sampling distribution
> table(Barrel)
Barrel
    Blue     Pink     Puce Viridian 
       3        2        1        1 
````

The **probability** of drawing each specific colour is simply the sampling distribution divided by its ````sum( )````.

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

Importantly, statistics is not really concerned about **probabilities**, but rather about **sampling distributions**. Statistics can be divided into two endeavours: *describing* sampling distributions (**descriptive statistics**) and *comparing* two or more sampling distributions (**inferential statistics**).

A dizzying variety of tests exist for the purpose of comparing distributions, many of which are pre-programed into R or are available for download. Many people confuse knowing how to use some or many of these tests with a mastery of statistics, but this is like confusing skill with a powersaw as understanding carpentry. They are often related, but neither implies the other.

What is really important for a good statistician is that you never lose sight of your true goal, to accurately describe and compare sampling distributions. If you lose sight of this, just take a deep breath and try to envision the barrel(s) and its contents, ask yourself what you are trying to learn about the barrel, how would you answer the question if the "barrel" was *literally in front of you*, and then proceed from there.

## Describing distributions with statistics

Let's start by making ourselves a basic sampling distribution. Because I don't feel like typing out a lengthy distribution, we're going to make use of the ````rep( )```` function, short for *repeat*. Here's a quick rundown of how it works.

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

So, let's make a basic sampling distribution using ````rep( )````. Let's look at how many hours I've worked each day for the past 31 days.

````
> HoursWorked<-rep(c(6,7,8,9,10,11,12,13),times=c(2,2,8,10,2,2,3,2))
> HoursWorked
[1]  6  6  7  7  8  8  8  8  8  8  8  8  9  9  9  9  9  9  9  9  9  9 10 10 11 11 12 12 12 13 13
````

We call a numerical description of a distribution a **statistic** or a **parameter**. Because the difference between these two won't matter for this class, and because **parameter** is sometimes used interchangeaby with the R concept of an **argument**, we will only refer to them as statistics.

````
# The arithmetic mean is the most common statistic. It is simply the average of all points.
> mean(HoursWorked)
[1] 9.16129

# Find the median - i.e., is the literal midpoint
> median(HoursWorked)
[1] 9

# The standard deviation is the square root of the average squared distance of all points to the mean
# It is a basic measure of how spread out points are around the mean.
# This is easily calculated using the sd( ) function
> sd(HoursWorked)
[1] 1.845658

# The max( ) and min( ) record the highest and lowest value in a distribution.
> min(HoursWorked)
[1] 6
> max(HoursWorked)
[1] 13

# The mode is the most common value in the distribution. There is no function for calculating the mode in R.
# There is a function called mode( ), but it is unrelated to the statistic.
# We can calculate the mode manually using which( ), table( ), and max( )
> which(table(HoursWorked)==max(table(HoursWorked)))
9 
4 

# Alternatively, you can use the which.max( ) function, which is identical to which(x==max(x)).
> which.max(table(HoursWorked))
9 
4
````

It is often preferable to visualize distributions rather than to summarize them with a numerical statistic. There are a number of ways to do this. Probably the most common of these is a **frequency bar plot**. As you can guess from the name, it plots the relative frequency of values in a distribution, i.e., ````table(x)````, as bars.

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
 
The output is accurate, but it doesn't adequately give us a sense for the distribution in terms of its spread (standard deviation) or central tendency (mean, median, and mode) of the data. Yes, *techinically* the mode is 1, but the overwhelming majority of data points fall between 2 and 3 - hence the mean and median of ~2.5. Ideally, you want that to be illustrated in your visualization, just like it is reflected in the numerical statistics.

````
# This works much better than barplot( )!
> hist(MyVector)
````

The problem here is that there are, broady speaking, two opposing types of data. **Discrete** data is data that can only take on a specific set of values within a range - e.g., all the integers between 1 and 10. **Continuous** data is data that can take on *any* value within a range. A true bar plot doesn't describe **continuous** data well, but a histogram does - and vice-versa. This is why  ````hist( )```` groups values together, it is a crude attempt to make continuous data visually discrete. 

Just because ````hist( )```` is better than ````barplot( )```` for **continuous** data is no excuse for using it. There are better ways yet to visualize your data. Let's try the ````density( )```` function.

````
# We need to use two functions. 
# The density( ) function calculates the kernal density
# Kernal density( ) is basically a histogram represented as a line
# rather than as bars. The plot( ) function is for plotting it.
> plot(density(MyVector))
````

The data is now represented as a curve rather than as bars, leaving you free from making any assumptions about the appropriate width of bars while conveying all of the same information. People generally don't like to use kernal density plots because their underlying [calculation](http://www.mvstat.net/tduong/research/seminars/seminar-2001-05/) is much more complex than a histogram. However, nobody is asking you to do it by hand in this class, R will do it all for you.

## Common sampling distributions

**Continuous** data presents more problems then just how to visualize it. How can we randomly sample a continuous distribution? Up until now we've been making a **vector** and sampling from that vector. 

````
# Set the seed so we all get the same "random" answer
> set.seed(121)
> SimpleVector<-c(1,2,3,4,5)
> sample(SimpleVector,1)
[1] 2
````

That certainly won't work for continuous data, since it is literally impossible to type out an infinite number of points. Luckily, R will do it for us. Here are three of the most important sampling distributions that you can randomly sample form in R.

Distribution | Description | Example
------ | ----- | ----- 
Uniform | All numbers are equally common | Uniform<-c(1,2,3,4,5)
Gaussian | Numbers become steadily less common away from the mean | Gaussian<-c(1,2,2,3,3,3,4,4,5)
Galton | An exponentiated Gaussian distribution | Galton<-exp(Gaussian)

There are several other common distributions - e.g., degenerate, binomial, multinomial, and poisson - that you might encounter in a statistics class, but the most important for this class are the three above. The Gaussian distribution is also known as the **normal** distribution and Galton is also known as the **log-normal** distribution - because it will be Gaussian if you log it. I mention this because R uses the normal and log-normal terminology for its functions. 

````
# Set the seed
> set.seed(121)

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

It doesn't look uniform at all! The density of a uniform function should be flatish across the range from its minimum to maximum. It should look something akin to a plateau or mesa. Let's try again, but this time using a large number of draws.

````
# Try it again with 100,000 draws
> plot(density(runif(100000,min=0,max=1)))
````

Much better. The larger the sample size, the closer you get to the underlying sampling distribution. 

How large is large enough? The honest answer is that the appropriate sample size is context dependent. Some of the most famous statistical tests (Fischers exact test, Student's t-test) were designed for sample sizes as small 4, but I've seen papers that insist on sampling intensities as high as 10,000,000 random draws. Most statisticians seem to consider a random sample size of about 30 as adequate, but this is only a rule of thumb. 

In general, you should view any scientific study with <30 samples with *extreme skepticism*. Unfortunatley, an enormous amount of science, especially medical research, is conducted at such sample sizes. Never forget, in the words of Sherlock Holmes, "Data! Data! Data! I cannot make bricks without clay." Despite all of our efforts to the contrary, no amount of statistical manipulation can truly make up for a data shortage.

## Testing if two distributions are different

A large part of statistics is comparing two or more distributions in some way. Probably the most common question is to ask whether two distribution are different. What do I mean by different? Great question! There are lots of ways that distributions can be different, and you want to make sure that the difference you are testing for matches the conceptual question you are asking. 

Let's take a look at the ````PlantGrowth```` dataset. This dataset records the outcomes of an experiments treatment to increase plant growth. There is a control ( ````ctrl ````) group and two experimental groups ( ````tr1 ```` and  ````trt2 ````) that recieved different experimental fertilizer treatments to increase  ````weight```` gain.

````
# Load in the dataset
> data(PlantGrowth)

# Preview the data
> PlantGrowth
   weight group
1    4.17  ctrl
2    5.58  ctrl
3    5.18  ctrl
4    6.11  ctrl
5    4.50  ctrl
6    4.61  ctrl
7    5.17  ctrl
8    4.53  ctrl
9    5.33  ctrl
10   5.14  ctrl
11   4.81  trt1
12   4.17  trt1
13   4.41  trt1
14   3.59  trt1
15   5.87  trt1
16   3.83  trt1
17   6.03  trt1
18   4.89  trt1
19   4.32  trt1
20   4.69  trt1
21   6.31  trt2
22   5.12  trt2
23   5.54  trt2
24   5.50  trt2
25   5.37  trt2
26   5.29  trt2
27   4.92  trt2
28   6.15  trt2
29   5.80  trt2
30   5.26  trt2
````

Our question for the day is whether or not plants in treatment group 1 grew more or less, on average, than plants in the control group.

````
# Make a Control subset
> Control<-PlantGrowth[which(PlantGrowth[,"group"]=="ctrl"),]

# Make a Virginica subset
> Treatment<-PlantGrowth[which(PlantGrowth[,"group"]=="trt1"),]

# Ask whether the average weight of the control group is equal to the average weight of the treated group.
> mean(Control[,"weight"]) == mean(Treatment[,"weight"])
[1] FALSE

# See the difference of the means more precisely
> mean(Control[,"weight"])
[1] 5.032

> mean(Treatment[,"weight"])
[1] 4.661

# Find the difference of the means
> mean(Control[,"weight"])-mean(Treatment[,"weight"])
[1] 0.371
````

Wow! The treated group actually grew *less* than the control group. Case solved, right? Not so fast. It's possible that we got different means for the two groups just by chance. What we really want is some measure of *probability* that tells us how likely it is the two distributions are genuinely different. The best way to do this is by repeating (replicating) the experiment over and over again, ideally enough times for the law of large numbers to apply - i.e., >30 replications. Unfortunately, this would take a long time, but we can mimic replication using basic statistical testing in R.

Let's rephrase the question in more statistical terms. We start with an initial hypothesis: there is no difference in weight between the control group and the plants treated witht he first fertilizer. Or phrased differently, we hypothesize that our control group samples and treated samples are just random samples *drawn from the same sampling distribution* - i.e., out of the same barrel.

The best way to test this is to create a simulations where there is indeed no difference between the two groups, and see how many times we see a difference *at least as strong* as the one we originally observed. Here it is probably easier to just show you.

````
# Reconduct the original experiment by sampling a new set of "hypothetical"
# Control and Treatment group length by joining their distribution.
# Find the difference between the means of each. Repeat this process 100 times, 
# and see in how many instances the difference between the means is greater than
# 0.371.

> compareWeights<-function(Control,Treatment,Iterations=100) {
    
    # Create a hypothetical sampling distribution named barrel 
    # using BOTH the observed Control and Treatment
    Barrel<-c(Control[,"weight"],Treatment[,"weight"])
    
    # Create a 1-dimensional array to store the output of the replications
    ReplicatedMeans<-array(data=NA,dim=Iterations)
    
    # Create a for( ) loop that repeats the process 100 times.
    for (Counter in 1:Iterations) {
        
        # Make sure that you sample the same number of specimens for each species
        # as in the original dataset
        NewControl<-sample(Barrel,dim(Control)[[1]],replace=TRUE)
        NewTreatment<-sample(Barrel,dim(Treatment)[[1]],replace=TRUE)
        
        # Find the means 
        ControlMean<-mean(NewControl)
        TreatmentMean<-mean(NewTreatment)
        
        # Find the difference of the means and store it in the ReplicatedMeans array
        ReplicatedMeans[Counter]<-ControlMean-TreatmentMean
        
        }
        
    return(ReplicatedMeans)
    }
    
# Run the function for comparing weights
# Set the seed so we all get the same results
> set.seed(10)
> ReplicationResults<-compareWeights(Control,Treatment,100)

# Find how many of the replications showed a difference in the means 
# more extreme than the one we initially observed.
> length(which(ReplicationResults>0.371))
[1] 8
````

This means that approximately 8/100 (.08 or 8%) trials of the experiment would give us a difference between the control and treatment groups as large (or larger) than what we observed, even *if* there was no difference between the two groups.

Generally, as a completely arbitrary rule of thumb (with a complex and controversial history), scientists do not accept results of these sorts of tests where the outcome comes out to greater than 5% (i.e., 0.05).

Importantly, this sort of test does not tell us whether there is or is not an effect of the treatment on plant growth - i.e., whether the hypotheis is true or false. In fact, from a philosophical standpoint, it is impossible to estimate the probability whether a hypothesis is true or false. What we *can* estimate is the probability that we would observe the same result (or better) under  different conditions.

We will talk more about the philosophy of statistics in later lab assignments, but this is enough to give you a working idea of what can be achieved in R.
