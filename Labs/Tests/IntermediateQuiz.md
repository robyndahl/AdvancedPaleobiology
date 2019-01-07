# R Intermediate Concepts Quiz

The following quiz is designed to test the concepts covered in the [R Intermediate Concepts] tutorial created by Andrew Zaffos. For the WWU Advanced Paleobiology Course, you should answer these questions in a Markdown file in your personal repository, then submit a link to that file on the course Canvas site.

## A Logical Matrix

We are going to make our own dataset for this exercise using random data. Normally, because the dat is random, we would not be able to see the same results. However, if we use the `set.seed( )` function, we will all generate the same "random" results.

````R
# First we need to set the seed:
> set.seed(222)

# To create an 8 x 12 matrix of random logica data, we need to have 96 elements (8 * 12)
> MatrixElements <- sample(c(TRUE,FALSE), size=96, replace=TRUE)

# Make a 2-dimensional array populated with the MatrixElements vector
> MyMatrix <- array(data=MatrixElements, dim=c(8,12))
> MyMatrix
      [,1]  [,2]  [,3]  [,4]  [,5]  [,6]  [,7]  [,8]  [,9] [,10] [,11] [,12]
[1,] FALSE FALSE FALSE  TRUE  TRUE FALSE  TRUE  TRUE  TRUE FALSE FALSE FALSE
[2,]  TRUE  TRUE  TRUE FALSE FALSE FALSE FALSE  TRUE FALSE  TRUE FALSE  TRUE
[3,]  TRUE  TRUE  TRUE FALSE FALSE FALSE  TRUE FALSE  TRUE FALSE FALSE FALSE
[4,]  TRUE  TRUE  TRUE  TRUE FALSE FALSE  TRUE FALSE  TRUE  TRUE FALSE FALSE
[5,] FALSE  TRUE FALSE  TRUE FALSE  TRUE FALSE  TRUE  TRUE  TRUE  TRUE  TRUE
[6,] FALSE FALSE  TRUE FALSE  TRUE FALSE FALSE  TRUE  TRUE  TRUE FALSE  TRUE
[7,]  TRUE FALSE  TRUE  TRUE  TRUE FALSE  TRUE FALSE FALSE FALSE FALSE FALSE
[8,]  TRUE  TRUE FALSE FALSE FALSE FALSE  TRUE FALSE  TRUE FALSE FALSE  TRUE
````

**Section 1 Questions**

1. What does the `replace =` argument of the `sample( )` function do?

2. Using `as(MyMatrix,"numeric")` will not convert `MyMatrix` to a numeric data! Can you use another property of logicals other than `as( )` that will convert the logicals of `MyMatrix` to 0's and 1's?

3. If you wanted to check if **all** the elements in each row are true, how would you do this?

## A Numeric Matrix

Let's create a numeric matrix next:

````R
# First we need to set the seed
> set.seed(222)

# Then we will randomly sample a vector of numeric data
> MatrixElements<-sample(c(1,2,3,4,5,6,7,8,9,10), size = 96, replace=TRUE)
> MyMatrix<-matrix(MatrixElements,8,12)

> MyMatrix
     [,1] [,2] [,3] [,4] [,5] [,6] [,7] [,8] [,9] [,10] [,11] [,12]
[1,]   10    6    7    3    1    8    1    4    5     8     6    10
[2,]    1    2    1   10    6    9   10    3   10     1     8     5
[3,]    5    5    2    8    6    8    1   10    5     6     7     7
[4,]    1    1    3    5   10    8    5    7    2     3     6     9
[5,]   10    2    9    5    7    3    7    3    5     5     3     2
[6,]   10    8    3   10    4    8    9    2    2     1     9     4
[7,]    4   10    1    2    3   10    4    8    7     9     9     8
[8,]    5    1    6    7    8    7    1   10    2     6     9     5
````

**Section 2 Questions**

4. How many times does the number 7 occur in `MyMatrix`?

5. How do you find the sum of each column?

6. How do you find the product of each column?

7. How can you change every instance of the number 1 to 11?

8. How many values in `MyMatrix` are greater than 3 and less than 8?

9. How can you change the elements of column 12 into **character data**, while keeping columns 1-11 as numeric data?

10. Find which rows of `MyMatrix` have a sum >70. Make a *new* version of `MyMatrix` where the 13th column is a set of `TRUE` and `FALSE` values denoting which rows have a sum >70. (**Hint:** What type of object allows you to store both logical and numeric data at once?)
