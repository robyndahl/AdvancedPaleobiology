# Intermediate Concepts

## Table of Contents

+ [Subscripting and subsetting with logicals](#subscripting-and-subsetting-with-logicals)
+ [Rewriting elements using logical subscripts](#rewriting-elements-using-logical-subscripts)
+ Direct subsetting with functionals

## Subscripting and Subsetting with Logicals

Perhaps the biggest benefit of ````[ ]```` notation is that we can perform complex subscripting operations within them. The most powerful of these is the ````which( )```` function, which finds the **index** (aka, the position) of ````TRUE```` values in a logical array. In other words, ````which( )```` is shrot for the phrase *which of the elements in this array are TRUE values*.

````R
# Create a vector of logical values, where the first element and the fifth element are TRUE
> MyLogical <- c(TRUE,FALSE,FALSE,FALSE,TRUE)
> MyLogical
[1]  TRUE FALSE FALSE FALSE  TRUE

# Use which( ) to find which elements of MyLogical are TRUE
> which(MyLogical)
[1] 1 5
````

Now, you might ask, how does this useful? Well, now that you have the index positions, you can reference those elements of the array directly:

````R
# Display the elements of MyLogical that are TRUE
> MyLogical[which(MyLogical)]
[1] TRUE TRUE
````

This isn't very impressive since it's circular. We asked which elements had a value of ````TRUE````, so of course the values of those elements is ````TRUE````. But, what if we don't start out with logical data?

````R
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

````R
# Find numbers that are greater than 5 AND less than 9
> MyVector[which(MyVector > 5 & MyVector < 9])
[1] 6 6 7

# Find numbers that are greater than 5 or equal 3
> MyVector[which(MyVector > 5 | MyVector == 3)]
[1] 6 6 3 7 9 3
````

## Rewriting Elements Using Logical Subscripts
