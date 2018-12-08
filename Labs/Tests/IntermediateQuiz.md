# R Intermediate Concepts Quiz

The following quiz is designed to test the concepts covered in the [R Intermediate Concepts] tutorial created by Andrew Zaffos. For the WWU Advanced Paleobiology Course, you should answer these questions in a Markdown file in your personal repository.

## A Logical Matrix

We are going to make our own dataset for this exercise using random data. Normally, because the dat is random, we would not be able to see the same results. However, if we use the `set.seed( )` function, we will all generate the same "random" results.

````R
# First we need to set the seed:
> set.seed(541)

# To create an 8 x 12 matrix of random logica data, we need to have 96 elements (8 * 12)
> MatrixElements <- sample(c(TRUE,FALSE), size=96, replace=TRUE)

# Make a 2-dimensional array populated with the MatrixElements vector
> MyMatrix <- array(data=MatrixElements, dim=c(8,12))
> MyMatrix
      [,1]  [,2]  [,3]  [,4]  [,5]  [,6]  [,7]  [,8]  [,9] [,10] [,11] [,12]
[1,] FALSE  TRUE  TRUE  TRUE FALSE FALSE FALSE  TRUE  TRUE  TRUE  TRUE FALSE
[2,] FALSE FALSE FALSE  TRUE FALSE  TRUE  TRUE  TRUE  TRUE FALSE FALSE  TRUE
[3,] FALSE FALSE  TRUE  TRUE  TRUE  TRUE FALSE FALSE  TRUE  TRUE  TRUE  TRUE
[4,]  TRUE FALSE FALSE  TRUE  TRUE FALSE  TRUE  TRUE  TRUE  TRUE FALSE  TRUE
[5,]  TRUE  TRUE FALSE  TRUE  TRUE  TRUE FALSE  TRUE FALSE  TRUE FALSE FALSE
[6,]  TRUE  TRUE FALSE  TRUE FALSE FALSE  TRUE  TRUE  TRUE  TRUE FALSE FALSE
[7,]  TRUE FALSE  TRUE FALSE  TRUE FALSE FALSE  TRUE  TRUE  TRUE  TRUE FALSE
[8,] FALSE FALSE  TRUE  TRUE FALSE  TRUE  TRUE  TRUE FALSE  TRUE  TRUE FALSE
````

**Section 1 Questions**

1. What does the `replace =` argument of the `sample( )` function do?

2. Using `as(MyMatrix,"numeric")` will not convert `MyMatrix` to a numeric data! Can you use another property of logicals other than `as( )` that will convert the logicals of `MyMatrix` to 0's and 1's?

3. If you wanted to check if **all** the elements in each row are true, how would you do this?

## A Numeric Matrix

Let's look at airline passenger data. This dataset shows the number of airline passengers (in 1000's), from 1949 to 1960.

````R
# Load the AirPassengers dataset
> data(AirPassengers)
> AirPassengers
     Jan Feb Mar Apr May Jun Jul Aug Sep Oct Nov Dec
1949 112 118 132 129 121 135 148 148 136 119 104 118
1950 115 126 141 135 125 149 170 170 158 133 114 140
1951 145 150 178 163 172 178 199 199 184 162 146 166
1952 171 180 193 181 183 218 230 242 209 191 172 194
1953 196 196 236 235 229 243 264 272 237 211 180 201
1954 204 188 235 227 234 264 302 293 259 229 203 229
1955 242 233 267 269 270 315 364 347 312 274 237 278
1956 284 277 317 313 318 374 413 405 355 306 271 306
1957 315 301 356 348 355 422 465 467 404 347 305 336
1958 340 318 362 348 363 435 491 505 404 359 310 337
1959 360 342 406 396 420 472 548 559 463 407 362 405
1960 417 391 419 461 472 535 622 606 508 461 390 432
````

**Section 2 Questions**

4. How would you find the annual total of passengers for each year?

5. How would you determine the month and year in which the **most** number of people flew?

6. How would you determine the month and year in which the **least** number of people flew?
