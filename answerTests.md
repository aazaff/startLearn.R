# Introdution

Possible answers to the questions in the [beginnerTest](), [intermediateTest](), [advancedTest](), and [expertTest]() questions. There are many ways to achieve the same end in R. What is important isn't that you used the exact same code, but that you produce the same outputs.

## Beginner Test

1. What class of object is ````mtcars````? What function did you use to find out?

````
> class(mtcars)
[1] "data.frame"
````

2. Is ````precip```` defined as a 1-dimensional array or a vector? How did you find out?

````
> is(precip,"vector")
[1] TRUE

> length(precip)
[1] 70

> names(precip)
````

3. How would you convert the data.frame ````trees```` into a matrix?

````
# You can use various versions of the as( ) function
> NewMatrix<-as(trees,"matrix")
> NewMatrix<-as.matrix(trees)
> NewMatrix<-data.matrix(trees)\

# Or you could create a new matrix/array
> NewMatrix<-matrix(c(trees[,"Girth"],trees[,"Height"],"trees[,"Volume"]),nrow=nrow(trees),ncol=ncol(trees))
> NewMatrix<-array(c(trees[,"Girth"],trees[,"Height"],"trees[,"Volume"]),dim=c(nrow(trees),ncol(trees)))
````

4. What is the name of the 14th city in the ````precip```` dataset?

````
> precip[14]
````

5. What function would you use if you wanted to combine all three data sets into a single object?

````
> MyList<-list(precip,trees,mtcars)
````

6. Does precip consist of **numeric** data? How did you find out?

````
> is(precip,"numeric")
[1] TRUE

# Remember that double is equivalent to numeric
> typeof(precip)
[1] "double"
`````

7. Code four different ways to subscript the 2nd row and 7th column of ````mtcars```` using bracket notation - i.e., 17.02.

`````
> mtcars[2,7]
[1] 17.02

> mtcars["Mazda RX4 Wag",7]
[1] 17.02

> mtcars["Mazda RX4 Wag","qsec"]
[1] 17.02

> mtcars["Mazda RX4 Wag",][7]
               qsec
Mazda RX4 Wag 17.02

> mtcars[,"qsec"][2]
[1] 17.02
````

8. How would you change the precipitation values of "Juneau", "Phoenix", and "Sacramento" to 23, 46, and 12 in the precip dataset. (Hint: You will need to use subscripts and the <- operator).

````
> precip[c("Juneau","Phoenix","Sacramento")]<-c(23,46,12)
# or
> precip[which(names(precip)=="Juneau" | names(precip)=="Phoenix" | names(precip)=="Sacramento")]<-c(23,46,12)
````

9. Are there **any** ````trees```` in the ````trees```` dataset with more girth than volume? How did you find out?

````
> any(trees[,"Girth"]>trees[,"Volume"])
[1] FALSE
````

10. Take the sum of all elements in column height of the trees dataset, call this value A. Take the sum of all elements in row Valiant of the mtcars dataset, call this value B. Take the sum of the first 8 elements of the precip dataset, call this value C. Divide C by B and add A. What is your final answer?

````
> A<-sum(trees[,"Height"])
> B<-sum(mtcars["Valiant",])
> C<-sum(precip[1:8])
> (C/B)+A
[1] 2356.633
````

## Intermediate Test

1. What does the REPLACE= argument of the sample( ) function do?

IF TRUE: Return sampled values back to the pool of potential values for the next draw.

IF FALSE: Do not return sampled values back to the pool of potential values for the next draw.

2. Using as(MyMatrix,"numeric") will not convert MyMatrix to numeric data! Can you think of a property of logicals that you can use to convert the logicals to 0's and 1's other than the as( ) function?

````
> MyMatrix*1
````

3. If you wanted to check if all of the elements in each row are true, how would you do this?

````
> apply(MyMatrix,1,all)
[1] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
````

4. How many times does the number 7 occur in MyMatrix?

````
> table(MyMatrix)
 1  2  3  4  5  6  7  8  9 10 
 9 10  7  5  6  6 16 10 13 14 
 
> table(MyMatrix)["7"]
 7 
16 

> length(which(MyMatrix==7))
[1] 16
````

5. How do you find the sum of each column?

````
> apply(MyMatrix,1,sum)

> for (i in 1:dim(MyMatrix)[[1]]) {
      print(sum(MyMatrix[i,]))
      }
````

6. How do you find the product of each column?

````
> apply(MyMatrix,2,prod)

> for (i in 1:dim(MyMatrix)[[2]]) {
      print(sum(MyMatrix[,i]))
      }
````

7. How would you change every instance of the number 10 to 12?

````
> MyMatrix[which(MyMatrix==10)]<-12
````

8. How many values in MyMatrix are greater than 3 and less than 8?

````
> length(which(MyMatrix>3 & Mymatrix<8))
````

9. How do you change the elements of column 12 into character data, while keeping columns 1- 11 as numeric data??

````
# Change the matrix into a data frame
> MyFrame<-data.frame(MyMatrix)
# or
> MyFrame<-as.data.frame(MyMatrix)

# Redo the 12th row
> MyFrame[,12]<-as(MyFrame[,12],"character")
````

10. Find which rows of MyMatrix have a sum >70. Make a new version of MyMatrix where the 13th column is a set of TRUE and FALSE values denoting which rows have a sum >70. (Hint: What type of object allows you to store both logical and numeric data at once?)

````
# Find the row sums (see previous questions)
> Sums<-apply(MyMatrix,1,sum)

# Find which of those sums are > 70
> Sums>70
[1] FALSE  TRUE FALSE FALSE  TRUE  TRUE  TRUE  TRUE

# Make a new matrix with 13 columns, then convert it to a data frame
> NewMatrix<-matrix(NA,nrow=8,ncol=13)
> NewFrame<-data.frame(NewMatrix)

# Put the old data from MyMatrix into the new matrix
> NewFrame[,1:12]<-MyMatrix

# Put the TRUE/FALSE vector into the 13th column
> NewFrame[,13]<-Sums>70
> NewFrame
  X1 X2 X3 X4 X5 X6 X7 X8 X9 X10 X11 X12   X13
1  8  4  7  9  3  2  8  2  9   2   3   5 FALSE
2  8  6  8 10  5 10  7  9  1  10   8  10  TRUE
3  1  6  1  6  9  5  5  1  8   4  10   1 FALSE
4  7  1  5 10  2  2  3  7  7   2   9  10 FALSE
5  9  6  7  5  9  9  7  1  4   3  10   2  TRUE
6  1  2  9  9  2 10  8 10 10   4   7   3  TRUE
7  9  9  1  2 10  6  6  9  7  10   8   8  TRUE
8  8  3  7  7  7 10  7  7  7   7   4   3  TRUE
````

## Advanced Test

1. Write a function that returns the phrase "Hello, World."

````
> greetWorld<-function(Message) {
      print(Message)
      }
> greetWorld("Hello, World.")
[1] "Hello, World."

# or

> greetWorld<-function(Message) {
      return(Message)
      }
> greetWorld("Hello, World.")
[1] "Hello, World."

# or 

> greetWorld<-function( ) {
      return("Hello, World.")
      }
> greetWorld()
[1] "Hello, World."
````

2. Load the iris dataset we used in the earlier tests. Write a function that takes iris as its argument, and returns three subsets of the data.frame split by the three different types of species (saved as a single object).

````
# An easy version
IrisFunction<-function(iris) {
  Setosa<-iris[which(iris[,"Species"]=="setosa"),]
  Virginica<-iris[which(iris[,"Species"]=="virginica"),]
  Versicolor<-iris[which(iris[,"Species"]=="versicolor"),]
  return(list(Setosa,Virginica,Versicolor)
  }

# A difficult, but more versatile version that will accept any dataset (not just iris) 
# and split it by the unique categories in the column
IrisFunction<-function(Dataset,Column) {
  Categories<-unique(Dataset[ ,Column])
  BlankList<-list()
  for (i in 1:length(Categories)) {
      BlankList[[i]]<-Dataset[which(Dataset[,Column]==Categories[i]),]
      }
  return(BlankList)
  }
> IrisFunction(iris,"Species")
````

3. Write a function that takes ````iris```` as its argument. The function should, for each row, add Sepal.Length and Petal.Length if Sepal.Width is > 3.1. It should substract Petal.Length from Sepal.Length if Sepal.Width is <3.1. The answer should be returned as a vector.

````
# Using if/else version
# Roughly how I intended for you to do it.
IrisFunction<-function(iris) {
  # Create an array to store the ouptut
  AnswerVector<-array(NA,dim=dim(iris)[[1]])
  # Create a for( ) loop that runs through every row of hte iris dataset
  for (index in 1:dim(iris)[[1]]) {
    # Use an if statement to isolate rows where Sepal.width is >3.1
    if (iris[index,"Sepal.Width"]>3.1) {
      # Add Petal Length and Sepal width
      AnswerVector[index]<-iris[index,"Petal.Length"]+iris[index,"Sepal.Length"]
       }
    # Use an else if statement to isolate rows where Sepal.width is >3.1
    else if (iris[index,"Sepal.Width"]<3.1) {
      # Subtract Petal Length and Sepal Length
      AnswerVector[index]<-iris[index,"Sepal.Length"]-iris[index,"Petal.Length"]
      }
    }
  return(AnswerVector)
  }
````

3. Load the ````mtcars```` dataset we used in the earlier tests. Write a function that takes a number of cylinders as its argument. Have the function return the average miles per gallon (column mpg) for all cars with that many cylinder (column cyl).

````
findMPG<-function(NumCylinders) {
  # Subset the mtcars dataset to just rows where cyl is equal to NumCylinders
  CylinderSubset<-mtcars[which(mtcars[,"cyl"]==NumCylinders),]
  # Find the mean of the mpg column in the subset
  Answer<-mean(CylinderSubset[,"mpg"])
  return(Answer)
  }
````

4. Write a function that simulates 1,000,000 powerball drawings. A powerball drawing takes a random sample of 5 numbers (without replacement) from 1 through 69, plus one powerball number ranging from 1 through 26. The function should return a single object recording all of your draws.

````
PowerballDraw<-function(NumDrawings) {
  # Create a matrix to store the output
  # You want 6 columns, one for each lottery number.
  DrawsMatrix<-matrix(NA,nrow=NumDrawings,ncol=6)
  # Create a for( ) loop that repeats the drawing process
  for (draw in 1:NumDrawings) {
    DrawsMatrix[draw,1:5]<-sample(c(1:69),5,replace=FALSE)
    DrawsMatrix[draw,6]<-sample(c(1:26),1,replace=FALSE)
    }
  return(DrawsMatrix)
  }
  
> PowerballDraw(1000000)
````

5. Write a function that take a single set of lottery numbers (as a vector) as its argument. As before, write a function that simulates 1,000,000 powerball drawings. Have the function return a TRUE or FALSE value if you won any of the drawings.

````
# Make a function that takes a vector of lottery numbers
PowerballDraw<-function(MyNumbers) {
  # Create a matrix to store the output
  # You want 6 columns, one for each lottery number.
  DrawsMatrix<-matrix(NA,nrow=1000000,ncol=6)
  # Create a for( ) loop that repeats the drawing process
  for (draw in 1:1000000) {
    DrawsMatrix[draw,1:5]<-sample(c(1:69),5,replace=FALSE)
    DrawsMatrix[draw,6]<-sample(c(1:26),1,replace=FALSE)
    }
  # Next test if any of the 1000000 draws is equal to MyNumbers
  # Create a vector to store your tests in
  TestForMatch<-array(NA,dim=1000000)
  for (test in 1:1000000) {
    TestForMatch[test]<-all(DrawsMatrix[test,]==MyNumbers)
    }
  # See if any of your tests got a postive result - i.e., TRUE
  Result<-any(TestForMatch)
  return(Result)
  }
````

1. What is the mean, median, and standard deviation of precip?
````
> mean(precip)
[1] 34.91571
> median(precip)
[1] 36.6
> sd(precip)
[1] 13.33442
````

2. Is precip best visualized using a ````barplot( )```` or ````hist( )````? Why?

````hist( )````, because precip is continuous data.

3. Generate a vector of random numbers drawn from a normal distribution with the same mean, standard deviation, and number of elements as in the precip dataset. Name this vector RandomNormal.

````
RandomNormal<-rnorm(length(precip),mean(precip),sd(precip))
````

4. Write a function that tests, based on the means of each distribution, whether it is likely that RandomNormal and precip were drawn from the same underlying distribution.

````
comparePrecip<-function(precip,RandomNormal,Iterations=100) {
    # Find the current difference in means between precip and RandomNormal
    MeanDifference<-mean(precip)-mean(RandomNormal)
    
    # Create a hypothetical sampling distribution named barrel 
    # using BOTH the observed Control and Treatment
    Barrel<-c(precip,RandomNormal)

    # Create a 1-dimensional array to store the output of the replications
    ReplicatedMeans<-array(data=NA,dim=Iterations)

    # Create a for( ) loop that repeats the process 100 times.
    for (Counter in 1:Iterations) {

        # Make sure that you sample the same number of specimens for each species
        # as in the original dataset
        NewPrecip<-sample(Barrel,length(precip),replace=TRUE)
        NewRandomNormal<-sample(Barrel,length(RandomNormal),replace=TRUE)

        # Find the means 
        PrecipMean<-mean(NewPrecip)
        RandomMean<-mean(NewRandomNormal)

        # Find the difference of the means and store it in the ReplicatedMeans array
        ReplicatedMeans[Counter]<-RandomMean-PrecipMean

        }
        
    # Find the percentage of differences in Replicated means that are more extreme than what you observe.
    Percentage<-length(which(ReplicationResults>MeanDifference))/Iterations
    return(Percentage)
    }
````

5. Create a density( ) plot of precip and RandomNormal. Is the test you performed above (question 4) a good or bad indicator of whether the two distributions are identical? Why or why not?

Depends, they do both have similar peaks (i.e., medians, modes, and means). However, notice that precip has a second smaller peak around a value of 15, whereas RandomNormal only has the one peak.
