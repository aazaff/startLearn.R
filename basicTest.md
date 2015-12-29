## Introduction

The following are a set of self-test questions designed to complement the [startLearn.R](https://github.com/aazaff/startLearn.R/blob/master/README.md) tutorial. These exercises are only meant to be attempted **after** you have finished reading the startLearn.R tutorial.

## Loading in Data

We will be using several pre-built in datasets that come with R for this exercise. You can load them into R using the **data( )** function.

````
# Data from the 1974 Motor Trend US magazine. 
> data(mtcars)

# Measurements of the girth, height and volume of felled black cherry trees.
> data(trees)
  
# The average amount of precipitation (rainfall) in inches for U.S. cities.
> data(precip)
````
  
Note that you would normally expect to define the name of new objects using the **<-** operator, but the **data( )** function creates a new object for you automoatically.

````
# Take a quick look at the structure of each dataset.
# You can use the head( ) function to only display the first few rows of the dataset.
> head(mtcars)
                   mpg cyl disp  hp drat    wt  qsec vs am gear carb
Mazda RX4         21.0   6  160 110 3.90 2.620 16.46  0  1    4    4
Mazda RX4 Wag     21.0   6  160 110 3.90 2.875 17.02  0  1    4    4
Datsun 710        22.8   4  108  93 3.85 2.320 18.61  1  1    4    1
Hornet 4 Drive    21.4   6  258 110 3.08 3.215 19.44  1  0    3    1
Hornet Sportabout 18.7   8  360 175 3.15 3.440 17.02  0  0    3    2
Valiant           18.1   6  225 105 2.76 3.460 20.22  1  0    3    1

# you can also use the tail( ) function to only display the last few rows of the dataset
> tail(trees)
   Girth Height Volume
26  17.3     81   55.4
27  17.5     82   55.7
28  17.9     80   58.3
29  18.0     80   51.5
30  18.0     80   51.0
31  20.6     87   77.0

> head(precip)
 Mobile      Juneau     Phoenix Little Rock Los Angeles  Sacramento 
   67.0        54.7         7.0        48.5        14.0        17.2 
````

#### Problem Set 1
1. What **class** of objects is **mtcars**? What function did you use to find out?

2. Is **precip** defined as a **1-dimensional array** or a **vector**? How did you find out?

3. Assuming you wanted to convert **precip** from a **vector** into an **array**, or from an **array** into a **vector** - what function would you use?

4. What are the dimensions of **mtcars**, **trees**, and **precip**? What function(s) did you use to find out?

5. What function would you use if you wanted to combine all three objects into a single object?

6. Does **precip** consist of numeric data? What are two different ways you could find out?

7. Code **four** different ways to **subscript** the **2nd row** and **7th column** of **mtcars** using bracket notation - i.e., 17.02.

8. How would you change the names of the **first five elements** of **precip** to "Alabama", "Alaska", "Arizona", "Arkansas", and "California". (Hint: You will need to use **subscripts**).

9. Are there any trees in the **trees** dataset with more **girth** than **volume**? How did you find out?

10. What is the total sum of all elements in **trees**? What is the sum of each row of **trees**?
