# R Intermediate Concepts Quiz

1. What does the `replace =` argument of the `sample( )` function do?

If `replace = TRUE`, then sampled values will be returned back into the pool of potential values for the next draw.

If `replace = FALSE`, then sampled values will not be returned.

Think of it as drawing number balls for the lotter (like in Powerball). If `replace = TRUE`, then when you draw the first number ball, you record the number and then put it back into the pool so that it could potentially be selected again. If `replace = FALSE`, then you would not return the ball to the pool and it can only be selected once.

2. Using `as(MyMatrix,"numeric")` will not convert `MyMatrix` to numeric data! Can you use another property of logicals, other than `as( )`, that will convert the logicals of `MyMatrix` to 0's and 1's?

If you do any arithmetic on the matrix, it will convert from a logical to a numeric:

````R
> MyMatrix * 1
````

3. If you wanted to check if all the elements in a row are true, how would you do this?

You can use the `apply( )` function:

````R
> apply(MyMatrix, 1, all)
[1] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
````

4. How many times does the number 7 occur in `MyMatrix`?

Number 7 occurs nine times in the matrix. There are several ways to determine this:

````R
# Check how many times each number occurs
> table(MyMatrix)
MyMatrix
 1  2  3  4  5  6  7  8  9 10 
12  9  9  5 12  8  9 11  8 13

# Use the same method, but only ask about number 7
> table(MyMatrix)["7"]
7 
9 

# Or, use which( ) to ask how many cells contain number 7
> length(which(MyMatrix == 7)
[1] 9
````

5. How do you find the sum of each row?

````R
> apply(MyMatrix, 1, sum)
[1] 69 66 70 60 61 70 75 67
````

6. How do you find the product of each column?

````R
> apply(MyMatrix, 2, prod)
 [1]  100000    9600    6804  840000  241920 7741440   12600  403200   70000   38880
[11] 4408992 1008000
````

7. How would you change every instance of the number 1 to 11?

````R
> MyMatrix[which(MyMatrix == 1)] <- 11
> MyMatrix
     [,1] [,2] [,3] [,4] [,5] [,6] [,7] [,8] [,9] [,10] [,11] [,12]
[1,]   10    6    7    3   11    8   11    4    5     8     6    10
[2,]   11    2   11   10    6    9   10    3   10    11     8     5
[3,]    5    5    2    8    6    8   11   10    5     6     7     7
[4,]   11   11    3    5   10    8    5    7    2     3     6     9
[5,]   10    2    9    5    7    3    7    3    5     5     3     2
[6,]   10    8    3   10    4    8    9    2    2    11     9     4
[7,]    4   10   11    2    3   10    4    8    7     9     9     8
[8,]    5   11    6    7    8    7   11   10    2     6     9     5
````

8. How many values in `MyMatrix` are greater than 3 and less than 8?

````R
> length(which(MyMatrix>3 & MyMatrix<8))
[1] 34
````

9) How do you change the elements of column 12 into character data, while keeping columns 1- 11 as numeric data??

````R
# Change the matrix into a data frame
> MyFrame<-data.frame(MyMatrix)
# or
> MyFrame<-as.data.frame(MyMatrix)

# Redo the 12th row
> MyFrame[,12]<-as(MyFrame[,12],"character")
````

10) Find which rows of MyMatrix have a sum >70. Make a new version of MyMatrix where the 13th column is a set of TRUE and FALSE values denoting which rows have a sum >70. (Hint: What type of object allows you to store both logical and numeric data at once?)

````R
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
