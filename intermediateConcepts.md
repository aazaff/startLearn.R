# Intermediate Concepts

This section covers intermediate R concepts, and is meant to be read through after completeing the [beginnerConcepts](https://github.com/aazaff/startLearn.R/blob/master/beginnerConcepts.md) tutorial and the [beginnerTest](https://github.com/aazaff/startLearn.R/blob/master/beginnerTest.md) exercise.

## Table of Contents

+ [Subscripting and subsetting with logicals](#subscripting-and-subsetting-with-logicals)
+ [Rewriting elements using logical subscripts](#rewriting-elements-using-logical-subscripts)
+ [Logical subsetting with functionals](#logical=subsetting-data-with-functionals)
+ [Direct subsetting with functionals](#direct-subsetting-with-functionals)

## Subscripting and subsetting with logicals

Perhaps the biggest benefit of ````[ ]```` notation is that we can perform complex subscripting operations within them. The most powerful of these is the ````which( )```` function, which finds the **index** (a.k.a., the position) of ````TRUE```` values in a logical array. In other words ````which( )```` is short for the phrase: *which of the elements in this array are TRUE values*.

````
# Create a vector of logical values, where the first element and fifth element are TRUE
> MyLogical<-c(TRUE,FALSE,FALSE,FALSE,TRUE)
> MyLogical
[1]  TRUE FALSE FALSE FALSE  TRUE

# Use which to find which elements of MyLogical are TRUE
> which(MyLogical)
[1] 1 5
````

Now, you might ask, how does this help? Well now that you you have the index positions, you can reference those elements of the array directly.

````
# Display the elements of MyLogical that are TRUE
> MyLogical[which(MyLogical)]
[1] TRUE TRUE
````

This isn't very impressive since it is circular. We asked which elements had a value of ````TRUE````, so of course the values of those elements is ````TRUE````. But, what if we don't start out with logical data?

````
# What if we want to see all the elements in array that are greater than 5 and what those elements are?
> MyVector<-c(2,6,4,5,6,1,3,4,7,9,3)
> MyVector
[1] 2 3 4 5 6 1 3 4 7 9 3

# We merely need to convert our numeric data into logical using the appropriate logical operator.
> MyLogical<-MyVector > 5
> MyLogical
[1] FALSE FALSE FALSE FALSE  TRUE FALSE FALSE FALSE  TRUE  TRUE FALSE

# Find which elements of MyLogical are TRUE and display them.
> MyVector[which(MyLogical)]
[1] 6 6 7 9

# We can do all of this in a single step.
> MyVector[which(MyVector > 5)]
[1] 6 6 7 9
````

You can combine logical statements using the ````&```` (and) and ````|```` (or) operators.

````
# Find numbers that are greater than 5 AND less than 9
> MyVector[which(MyVector > 5 & MyVector < 9)]
[1] 6 6 7

# Find numbers that are greater than 5 or equal 3
> MyVector[which(MyVector > 5 | MyVector == 3)]
[1] 6 6 3 7 9 3
````

## Rewriting elements using logical subscripts

The true power of ````which( )```` doesn't become apparent until you want to start **rewriting** elements of a data object. 

````
# Let's make a practice data frame
> MyArray<-array(data=c(5,6,4,5,5,6),dim=6)
> MyArray
[1] 5 6 4 5 5 6

# Let's imagine that we know the third element of the array, the number 4, should actually be a 7.
> MyArray[3]<-7
> MyArray
[1] 5 6 7 5 5 6

# Remember that you cannot mix types!
> MyArray[3]<-"seven"
> MyArray
[1] "5"     "6"     "seven" "5"     "5"     "6"  

# R automatically coerced the numbers to characters.
````

That's fairly straightforward, but what if we want to overwrite multiple elements in an array?

````
# Create an array
> MyArray<-array(data=c(5,6,4,5,5,6),dim=6)
> MyArray
[1] 5 6 4 5 5 6

# Change all values of 5 to 9
> MyArray[which(MyArray==5)]<-9
> MyArray
[1] 9 6 4 9 9 6
````

We can also perform logicals on two and three-dimensional arrays, but it can be somewhat more complicated. Let's take a look at the ````WorldPhones```` matrix. [WorldPhones](https://stat.ethz.ch/R-manual/R-devel/library/datasets/html/WorldPhones.html) is an example dataset that comes preloaded with all versions of R.

Each row represents a different year. Each column represents a different country. The value of each cell represents how many phones were present in that country that year.

````
# We can load any of R's example datasets using the data( ) function.
> data(WorldPhones)

# Check if WorldPhones is a matrix
> is(WorldPhones,"matrix")
[1] TRUE

# Take a gander at WorldPhones
> WorldPhones
     N.Amer Europe Asia S.Amer Oceania Africa Mid.Amer
1951  45939  21574 2876   1815    1646     89      555
1956  60423  29990 4708   2568    2366   1411      733
1957  64721  32510 5230   2695    2526   1546      773
1958  68484  35218 6662   2845    2691   1663      836
1959  71799  37598 6856   3000    2868   1769      911
1960  76036  40341 8220   3145    3054   1905     1008
1961  79831  43173 9053   3338    3224   2005     1076
````

Let's say that we recently learned that because of an error in the original study, any value indicating less than 2,000 or greater than 77,000 phones is unreliable. To reflect our uncertainty, we want to change all instances <2,000 or >77000 to **NA**. **NA** is R's way of saying that there is no data.

````
# Let's attempt to do some logical subsetting with which() on the WorldPhones matrix.
> which(WorldPhones < 2000 | WorldPhones > 77000)
[1]  7 22 29 36 37 38 39 40 41 43 44 45 46 47 48 49

# Convert those values to NA
> WorldPhones[which(WorldPhones < 2000 | WorldPhones > 77000)]<-NA
> WorldPhones
     N.Amer Europe Asia S.Amer Oceania Africa Mid.Amer
1951  45939  21574 2876     NA      NA     NA       NA
1956  60423  29990 4708   2568    2366     NA       NA
1957  64721  32510 5230   2695    2526     NA       NA
1958  68484  35218 6662   2845    2691     NA       NA
1959  71799  37598 6856   3000    2868     NA       NA
1960  76036  40341 8220   3145    3054     NA       NA
1961     NA  43173 9053   3338    3224   2005       NA
````

What if we are only concerned with years where there were more than 118,000 phones worldwide? Logically, the first step would be to find the total number (sum) of phones for each year (row). 

````
# Reload the data
> data(WorldPhones)

# Find the sum of the WorldPhones matrix.
> sum(WorldPhones)
[1] 805303
````

The ````sum( )```` function doesn't give us what we want! It sums all elements in an object, not each row. What we need is a way to *apply* the ````sum( )```` function to each individual row of the matrix. Luckily, there is an aptly named function, ````apply( )````, that we can use. 

````
# Find the sum of each row in WorldPhones with apply( )
> apply(WorldPhones,1,sum)
 1951   1956   1957   1958   1959   1960   1961 
74494 102199 110001 118399 124801 133709 141700 

# The 1 indicates the dimension of the matrix you want summed. We could also do the second dimension (columns).
> apply(WorldPhones,2,sum)
N.Amer   Europe     Asia   S.Amer  Oceania   Africa Mid.Amer 
467233   240404    43605    19406    18375    10388     5892 
````

Notice that apply returned a vector of sums for each row or column. We can perform a logical operation on that vector, and use the logical vector to define the rows we want from the matrix.

````
# Isolate which rows have more than 118,000 phones
> which(apply(WorldPhones,1,sum) > 118000)
1958 1959 1960 1961 
   4    5    6    7
   
# Reference only those rows, 
> WorldPhones[which(apply(WorldPhones,1,sum) > 118000), ]
     N.Amer Europe Asia S.Amer Oceania Africa Mid.Amer
1958  68484  35218 6662   2845    2691   1663      836
1959  71799  37598 6856   3000    2868   1769      911
1960  76036  40341 8220   3145    3054   1905     1008
1961  79831  43173 9053   3338    3224   2005     1076
````

The ````apply( )```` function is a special type of function called a **functional**. Functionals are an extremely versatile and important tool in your aRsenal. 

Each functional is characterized by two features. First, the kinds of objects that it will accept. Second, the kind of object it returns. This second requirement implicitly restricts the kinds of functions that the **functional** will accept. For example, ````apply( )```` only returns a vector. If you use a function that returns values incompatible with a vector, then apply cannot work.

Functionals | Accepted Object | Returned Object | Example Formula
-------- | -------- | -------- | --------
````apply( )```` | Array | Vector | apply(object, dimension, function)
````sapply( )```` | Vector or List | Vector | sapply(object, function)
````lapply( )```` | Vector or List | List | lapply(object, function)

You will learn more about using **functionals** in concert with more complex functions in the [advancedConcepts.R]() tutorial. In the mean time, here are some useful [functions](https://github.com/aazaff/startLearn.R/blob/master/summaryFunctions.md) that go well with these three basic functionals.

## Direct subsetting with functionals

Sometimes you can avoid logical subscripting all together by subsetting your data directly with a functional. Let's load in the ````iris```` dataset, another (quite famous) example dataset that comes with all versions of R.

Iris is a **data.frame** consisting of different sepal and petal measurements of different iris flowers. It is a data.frame because, in addition to the numeric measurements, there is a column of non-numeric data that denotes which of three different species the specimen belonged to: *Iris setosa*, *Iris virginica*, and *Iris versicolor*.   

````
# Let's load in and take a look at the data
> data(iris)

# Take a look at the first five rows.
> iris[1:5,]
  Sepal.Length Sepal.Width Petal.Length Petal.Width Species
1          5.1         3.5          1.4         0.2  setosa
2          4.9         3.0          1.4         0.2  setosa
3          4.7         3.2          1.3         0.2  setosa
4          4.6         3.1          1.5         0.2  setosa
5          5.0         3.6          1.4         0.2  setosa
````

What if we wanted to find which **species** has the largest **sepal length**? Using what we've already learned, we could use ````which( )```` to subset the dataset by species. We could then find the ````max( )```` sepal length of each species. Let's try it.

````
# Subset the iris data.frame into three separate data frames by species using which( ). 
# Beware of upper and lower case!
> Setosa<-iris[which(iris[,"Species"]=="setosa"),]
> Virginica<-iris[which(iris[,"Species"]=="virginica"),]
> Versicolor<-iris[which(iris[,"Species"]=="versicolor"),]

# Find the maximum of sepal length of each new data frame.
> max(Setosa[,"Sepal.Length"])
[1] 5.8
> max(Virginica[,"Sepal.Length"])
[1] 7.9 
> max(Versicolor[,"Sepal.Length"])
[1] 7
````

Nice! We were able to figure out that a specimen of *Iris virginica* had the longest sepal length! But, we can actually do this even faster us another functional called ````tapply( )````.

The ````tapply( )```` function takes a two-dimensional array, splits it into subsets, and then applies a function to a specific **column** in the subset.

````
# Find the maximum sepal length of each species
> tapply(iris[,"Sepal.Length"],iris[,"Species"],max)
    setosa versicolor  virginica 
       5.8        7.0        7.9
````

Although this might seem like a trivial improvement at the moment, don't forget that you might be working on a data set with hundreds, thousands, or hundreds of thousands of species. Consider, that the first way we calculated this would take two-hundred thousand lines of code for a data frame with 100,000 species. It only takes *one* line with ````tapply( )````!

#### The fifth rule of R-Club
***"The computer is the robot, you are the useR."***

Automating boring and repetitive tasks is the whole reason that we invented computers in the first place! If it seems like an operation will take an inordinate amount of code, consider taking a break and formulating a more elegant solution. I assure you, one exists.

Don't worry, we will cover automatic repetition (iterating) in the [advancedConcepts]() tutorial.
