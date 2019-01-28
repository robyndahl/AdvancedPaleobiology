# R Advanced Concepts Quiz Answers

## A Mystical Rite of Passage

1. Write a function that returns the phrase "Hello, world."

````R
> greetWorld <- function(Message) {
+     print(Message)
+ }
> greetWorld("Hello, world.")
[1] "Hello, world."

# or

> greetWorld <- function(Message) {
+     return(Message)
+ }
> greetWorld("Hello, world.")
[1] "Hello, world."

# or

> greetWorld <- function() {
+     return("Hello, world.")
+ }
> greetWorld()
[1] "Hello, world."
````

## Problem Set

2. Load the `iris` dataset. Write a function that takes `iris` as its argument and returns three subsets of the data.frame, split by the three different types of species (saved as a single object).

````R
# An easy version
> IrisFunction <- function(iris) {
    Setosa <- iris[which(iris[,"Species"]=="setosa"),]
    Virginica <- iris[which(iris[,"Species"]=="virginica"),]
    Versicolor <- iris[which(iris[,"Species"]=="versicolor"),]
    return(list(Setosa,Virginica,Versicolor))
  }
> IrisFunction(iris)

# A difficult but more versatile version that will accept any dataset and 
# split by the unique categories in the column

> IrisFunction<-function(Dataset,Column) {
+     Categories<-unique(Dataset[ ,Column])
+     BlankList<-list()
+     for (i in 1:length(Categories)) {
+         BlankList[[i]]<-Dataset[which(Dataset[,Column]==Categories[i]),]
+     }
+     return(BlankList)
+ }
> IrisFunction(iris,"Species")
````

3. Write a function that takes `iris` as its argument. The function should, for each row, **add** `Sepal.Length` and `Petal.Length` *if* `Sepal.Width` is >3.1. It should **subtract** `Petal.Length` from `Sepal.Length` *if* `Sepal.Width` is <3.1. The answer should be returned as a vector.

````R
# Example using if/else

> IrisFunction <- function(iris) {
+     AnswerVector <- array(NA, dim = dim(iris) [[1]]) # Create an array to store the output
+     for (index in 1:dim(iris)[[1]]) { # Create a for( ) loop that runs through every row of the dataset
+         if (iris[index,"Sepal.Width"] > 3.1) { # Use an if statement to isolate rows were sepal width is > 3.1
+             AnswerVector[index] <- iris[index,"Petal.Length"] + iris[index,"Sepal.Length"] # Add petal length and sepal width
+             }
+         else if (iris[index,"Sepal.Width"] < 3.1) { # Use an else statement to isolate rows where sepal width is < 3.1
+             AnswerVector[index] <- iris[index,"Sepal.Length"] - iris[index,"Petal.Length"] # Subtract petal length and sepal width
+             }
+         }
+     return(AnswerVector)
+     }
> 
> IrisFunction(iris)
````

4. Load the `mtcars` dataset. Write a function that takes the number of cylinders as its argument. Have the function return the average miles per gallon (column `mpg`) for *all* cars with that number of cylinders (column `cyl`).

````R
> findMPG<-function(NumCylinders) {
+     CylinderSubset<-mtcars[which(mtcars[,"cyl"]==NumCylinders),] # Subset the mtcars dataset to just rows where cyl is equal to NumCylinders
+     Answer<-mean(CylinderSubset[,"mpg"]) # Find the mean of the mpg column in the subset
+     return(Answer)
+ }
>
> findMPG(6)
[1] 19.74286
> findMPG(4)
[1] 26.66364
````

5. Write a function that simulates 1,000,000 PowerBall drawings. A PowerBall drawing takes a randome sample of five numbers (without replacement) from 1-69, plus one powerball number ranging from 1-26. The function should return a single object recording all of your draws.

````R
> PowerballDraw<-function(NumDrawings) {
+     # Create a matrix to store the output
+     # You want 6 columns, one for each lottery number.
+     DrawsMatrix<-matrix(NA,nrow=NumDrawings,ncol=6)
+     # Create a for( ) loop that repeats the drawing process
+     for (draw in 1:NumDrawings) {
+         DrawsMatrix[draw,1:5]<-sample(c(1:69),5,replace=FALSE)
+         DrawsMatrix[draw,6]<-sample(c(1:26),1,replace=FALSE)
+     }
+     return(DrawsMatrix)
+ }
> PowerballDraw(1000000)
````

6. Write a function that takes a single set of lottery numbers (as a vector) as its argument. As before, write a function that simulates 1,000,000 PowerBall drawings. Have the function return a `TRUE` or `FALSE` value indicating if you won any of the drawings.

````R
> # Make a function that takes a vector of lottery numbers
> PowerballDraw<-function(MyNumbers) {
+     # Create a matrix to store the output
+     # You want 6 columns, one for each lottery number.
+     DrawsMatrix<-matrix(NA,nrow=1000000,ncol=6)
+     # Create a for( ) loop that repeats the drawing process
+     for (draw in 1:1000000) {
+         DrawsMatrix[draw,1:5]<-sample(c(1:69),5,replace=FALSE)
+         DrawsMatrix[draw,6]<-sample(c(1:26),1,replace=FALSE)
+     }
+     # Next test if any of the 1000000 draws is equal to MyNumbers
+     # Create a vector to store your tests in
+     TestForMatch<-array(NA,dim=1000000)
+     for (test in 1:1000000) {
+         TestForMatch[test]<-all(DrawsMatrix[test,]==MyNumbers)
+     }
+     # See if any of your tests got a postive result - i.e., TRUE
+     Result<-any(TestForMatch)
+     return(Result)
+ }
>
> # Create an array of lotter numbers called MyNumbers
> MyNumbers <- c(4,8,15,16,23,42)
>
> # Run the test
> PowerballDraw(MyNumbers)
[1] FALSE
````
