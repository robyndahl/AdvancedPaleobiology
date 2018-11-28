# Lab 01 - R Beginner Concepts

*This tutorial was designed by Andrew Zaffos for [teachPaleobiology](https://github.com/aazaff/teachPaleobiology), with some additions by Robyn Dahl for GEOL497Q/597Q*

In this tutorial, you will learn the most basic concept of using the programming language R. Once you have worked through the tuorial, complete the Beginner Concepts Test to ensure your mastery of the concepts addressed here.

## Table of Contents

+ [The most basic concepts of R](#the-most-basic-concepts-of-r)
+ [Why should you use R?](#why-should-you-use-r)
+ [R as a fancy scientific calculator](#r-as-a-fancy-calculator)
+ [Using functions for basic math](#using-functions-for-basic-math)
+ [Storing data in an array](#storing-data-in-an-array)
+ [The different types of data](#the-different-types-of-data)
+ Vectors and matrices as special arrays
+ Referencing elements of an array
+ Multiple data types in an array
+ On the proper names of things

## The Most Basic Concepts of R

Throughout this tutorial there will be references to **objects** and **functions**. These are the two fundamental units of the R programming language. R stores information as objects and uses functions to interact with the objects. The following quote (not mine) summarizes the relationship quite well.

“Everything that **exists** in R is an object.

Everything that you **do** in R is a function.”

The line between objects and functions can become somewhat blurred because all functions are stored as objects and all objects can only be interacted with via functions. Don't be discouraged by this complexity, just remember the above quote and you will be fine.

## Why should you use R?

If you are feeling overwhelmed by what you are learning or doing in R, never forget that everything you do in R is just a **function** designed for one of the four following purposed. Determine which step you are trying to perform, and proceed from there.

1. Mathematical Operations - using R as a glorified calculator
2. Storing Data - storing data in a format that allows you to perform a mathematical operation on it
3. Reshaping Data - changing the format of previously stored data so you can perform a different kind of operation on it
4. Visualizing Data - methods to make graphs or other visual representations of stored data

Although there are literally thousands of R function, you can accomplish anything as long as you know a handful of basic methods for each of these four steps. This beginner's tutorial will largely focus on Mathematical Operations and Storing Data.

### The First Rule of R-Club
***"Always talk about R!***

Spell things out for future readers of yoru programs as explicitly as possible. You want other programmers to easily recognize what you are trying to accomplish with a particular line of code. A quote that I quite like is:

"Always annotate your code, because the sucker trying to figure out what you did six months from now is going to be ***you***."

To do this, you should always leave **thorough comments** in your code. Comments in R are always preceded by the `#` symbol. Anything that follows a `#` symbol will not be executed by the software.

````R
> # 2 * 5; everything on this line is a comment an none of this will execute if you copy it into R
> 2 * 5 # Comments can also be placed after an expression. Whatever is in front of the # WILL execute.
````

### The Second Rule of R-Club
***"The computer is always right."***

As a beginner, you will often encounter errors. It is easy to become frustrated when the computer does not do what you want. However, remember that the computer ***"'tis not but clockwork"*** and can only do what you tell it to do. If you are getting an error, it is almost certainly *your* fault. Don't despair and don't get mad. Take a deep breath, think about what you are trying to do, and try again.

## R as a Fancy Calculator

If you have ever used a scientific calculator or a graphing calculator, then you already intuitively know all the basics of doing arithmetic in R. Yay! You've learned about 25% of R without doing anything! But, let's just start with a quick review:

````R
# Addition
> 2 + 2
[1] 4

# Subtraction
> 5 - 3
[1] 2

# Multiplication
> 3 * 4
[1] 12

# Division
> 1 / 5
[1] 0.2

# Exponents
> 10 ^ 2
[1] 100

# Modulus (i.e., the remainder)
> 4 %% 2
[1] 0

# Make sure you always pay attention to the order of operations (i.e., PEMDAS).
# Compare the following two expressions.

> (1 + 3) / (5 / 6)
[1] 4.8

> 1 + 3 / 5 / 6
[1] 1.1

# Also, be careful about those tricky negative signs!
> -5 ^ 2
[1] -25

> (-5) ^ 2
[1] 25
````

You can also perform what are **logical operations**. A **logical** returns a value of either `TRUE` or `FALSE`. Logicals are extraordinarily impportant to R.

````R
# Let's ask R if 0 is greater than 1
> 0 > 1
[1] FALSE

# What about if 5 is 1/2 of 10?
> 5 == (1/2 * 10)
[1] TRUE
````

Notice that we used `==` to ask if these two quantities are equal, rather than a single `=` sign. Most **operators** in R are straightforward, but there are some (like `==`) that are not intuitive. Here is a list of the basics. There are a few other operators in R, but we will worry about them later.

Operator Example | Operator Definition
---------------- | -------------------
x **>** y | Is x **greater than** y
x **>=** y | Is x **greater than or equal to** y
x **<** y | Is x **less than** y
x **<=** y | Is x **less than or equal to** y
x **==** y | Is x **equal to** y
x **!=** y | Is x **not equal to** y

## Using Functions for Basic Math

Of course, writing out the arithmetic every time wouldn't be any better than just using a calculator. The first benefit that R has over a calculator is that it many **functions** for arithmetic expressions that you can use as shortcuts.

````R
# For example, if I wanted to take the square root of a number, I could just write out the expression:
> 4 ^ (1/2)
[1] 2

# But, I can use the sqrt() function to do this as well. Note that all the functions have two parts.
# The function name - e.g., sqrt - and a set of function arguments.
# Arguments are objects that you want the function to evaluate - e.g., the number 4.
# Function arguments are always placed in parthentheses.
> sqrt(4)
[1] 2
````

Now you might be thinking, that doesn't really save any time compared to writing the expression. True, but functions will save you time on more complex expressions.

````R
# Let's try typing out 10!
> 10 * 9 * 8 * 7 * 6 * 5 * 4 * 3 * 2 * 1
[1] 3628800

# Compare that to the factorial function:
> factorial(10) # much clearer!
[1] 3628800
````

The second most important benefit of functions is that they explicitly say what your code is doing (remember the first rule of R!) Seeing `factorial(10)` makes it immediately obvious that you are calculating the factorial of ten.

## Storing Data in an Array

These simple functions probably still don't seem that impressive if you consider that most calculators already have buttons for square roots and factorials, etc. However, functions in R don't really shine until they are paired with stored data.

Let's consider a quadratic equation, `5x ^ 2 + 3x + 7`. Let's say we want to solve this equation for `x = 4`. That's easy to do in the way that we've already learned:

````R
# 5x ^ 2 + 3x + 7, where x = 4
> 5 * (4 ^ 2) + (3 * 4) + 7
[1] 99
````

But what if we want to solve this equation for multiple values of x? Let's try solving for x = 1, 2, 3, and 4. We can do this by telling R to perform the function on all four numbers at once using the `c( )` command.

````R
# The c( ) function tells R to treat the values inside the parentheses, separated by commas, as a single object.
> 5 * c(1,2,3,4) ^ 2 + 3 * c(1,2,3,4) + 7
[1] 15 33 61 99
````

But what if we had a very long equation and/or a very long list of x values that we want to solve for? Typing all those x values with `c( )` every time x appears in the equation could get very hard to read very quickly.

````R
# Typing things out every time would defeat the whole point of using computers.
> 9 * c(1,2,3,4,5,6,7,8,9,10) ^ 3 + 6 * c(1,2,3,4,5,6,7,8,9,10) ^ 2 + 8 * c(1,2,3,4,5,6,7,8,9,10) + 2
[1]   25  114  323  706 1317 2210 3439 5058 7121 9682
````

A better alternative involves learning how to store data as an **object**, and then plugging it into equations. The most fundamental type of data object in computer science is generally called an **array**. In R, there is an even more fundamental type of data object known as a **vector**. For now, let's talk about arrarys.

An array is essentially a set of values saved to your computer memory that is referenced by a name.

````R
# Here is an example of how to make an array named "MyArray"
> MyArray <- array(data = c(1,2,3,4), dim=4)

# Once we've created the array named MyArray, then typing MyArray will return the values in the array.
> MyArray
[1] 1 2 3 4

# Similarly, performing arithmetic on MyArray will apply that expression to all elements in the array.
> MyArray + 4
[1] 5 6 7 8

# You can input MyArray into a function, and the function will be applied to each element (number) in the array.
> factorial(MyArray)
[1]  1  2  6 24

> 5 * MyArray ^ 2 + 3 * MyArray + 7
[1] 15 33 61 99
````

## Differences Between Array (the Object) and array( ) (the Function)

Remember that everything that **exists** in R is an **object** and everything that you **do** in R is a **function**. In the above example, `MyArray` is an object (an array) that you created by using a function: `array( )`.

If you are ever interested in knowing whether an object is an **array** or a **function**, you can use the function `class( )`. All objects have a class.

````R
> MyArray <- array(data=c(1,2,3,4), dim = 4)
> class(MyArray)
[1] "array"

> class(array)
[1] "function"
````

Any time that you want to store the output of a function as an object, you use the `<-` operator, also known as the **assign** operator. In the `MyArray` example, `<-` tells R to store the output of the function on the right, `array( )`, under the name on the left - i.e., `MyArray`.

````R
# Here are some other examples:
> FirstObject <- sqrt(5)
> FirstObject
[1] 2.236068

# Note that an existing object can be used to make a new object.
> SecondObject <- factorial(FirstObject ^ 2)
> SecondObject
[1] 120
````
Let's talk some more about the `array( )` function, and about functions in general. Aside from the **symbolic** operations described above (e.g., `+`, `-`, `/`, `!=`, etc.), functions follow a simple format. They have a name (e.g, sqrt, factorial, array) that is followed by parentheses. Within these parentheses, you place one or more **arguments** that tell the function what to do.

````R
# Let's review the "MyArray" example again
> MyArray <- array(data = c(1,2,3,4), dim=4)
````

The `data=` is your way of telling it what you want stored in the array. In this case, it's the numbers one, two, three, and four, but you could substitute any list of values or even a single value.

````R
# Single value example
> SingleArray <- array(data=5,dim=1)
````

In other words data is the **argument** name, and you're telling it what values you want that argument to take. Another way of thinking about this is that you're telling R to *temporarily* create an **object** named **data** that the function `array( )` should use and then delete once the function completes. 

Similarly, `dim=` (short for dimensions) indicates how many values you want in the array. If you indicate more values to the array then you provide, R will simply repeat the values you did give it until the array is full.

````R
> MyArray <- array(data=c(1,2,3,4,5), dim=10)
> MyArray
[1] 1 2 3 4 5 1 2 3 4 5
````

Beware! This is really bad behavior and most computer languages would not allow it. The potential for introducing error into your calculation in this way isn't trivial, so be precise when defining your array size!

Perhaps the most interesting aspect of `dim=` is that you can also give it multiple values. Remember that when we want to treat multiple values as a single object, we use the `c( )` function.

````R
# Two dimensional array
> TwoArray <- array(data=c(0,1), dim=c(4,6))
> TwoArray
     [,1] [,2] [,3] [,4] [,5] [,6]
[1,]    0    0    0    0    0    0
[2,]    1    1    1    1    1    1
[3,]    0    0    0    0    0    0
[4,]    1    1    1    1    1    1
````

Essentially, you told R to make 6 arrays with 4 values and store them within a single object named **"TwoArray"**.

````R
# You can do this as many times as you'd like. For example, 2 sets of 6 sets of arrays with 4 values.
> ThreeArray < -array(data=c(0,1), dim=c(4,6,2))
> ThreeArray
, , 1

     [,1] [,2] [,3] [,4] [,5] [,6]
[1,]    0    0    0    0    0    0
[2,]    1    1    1    1    1    1
[3,]    0    0    0    0    0    0
[4,]    1    1    1    1    1    1

, , 2

     [,1] [,2] [,3] [,4] [,5] [,6]
[1,]    0    0    0    0    0    0
[2,]    1    1    1    1    1    1
[3,]    0    0    0    0    0    0
[4,]    1    1    1    1    1    1
````

We describe arrays based on the number of arrays referenced within them. A single array is a **1-dimensional array**. An array of arrays is a **2-dimensional array**. An array of arrays of arrays is a **3-dimensional array**, and so on.

Incidentally, if you ever want to check the dimensions of an array, you can use the `dim( )` function.

````R
# Check the dim of MyArray
> dim(MyArray)
[1] 4 6 2
````

## The Different Types of Data

What if we want to store something other than a number? There are a variety of data **types** in R, but there are only a few that you really need to know. Let's begin with the three most basic types:

Data Type | Definition
--------- | ----------
**logical** | **TRUE** or **FALSE**
**character** | letters, numbers, and symbols that act like letters
**numeric** | numbers that act like numbers

Type **logical** is fairly straightforward. It is simply a `TRUE` or `FALSE` value. Note, however, that R will convert `TRUE` to a `1` and `FALSE` to a `O` if you try to perform basic mathematically operations on an array of logical data.

````R
# Create an array of logical values
> MyLogical <- array(data=c(TRUE,TRUE,FALSE,TRUE,FALSE,TRUE), dim=6)
> MyLogical
[1]  TRUE  TRUE FALSE  TRUE FALSE  TRUE

# Multiply your 1-dimensional array of logicals by 3
> MyLogical * 3
[1] 3 3 0 3 0 3

# You can check what type of data you have using the typeof( ) function:
> typeof(MyLogical)
[1] "logical"

# Or, if you have a specific guess, you can use the is( ) function:
> is(MyLogical, "logical")
[1] TRUE

# If you want to see if ANY elements in your logical array are TRUE, use the any( ) function:
> any(MyLogical)
[1] TRUE

# If you want to see if ALL elements in your logical array are TRUE, use the all( ) function:
> all(MyLogical)
[1] FALSE
````

Type **character** is also fairly straightfoward. It is basically a way of "de-mathing" something. You tell R that something is meant to be a character by using quotation makrs `" "`:

````R
# Create an array of characters made of letters
> MyCharacters <- array(data = c("Bob","Loves","Lucy","Almost","As","Much","As","He","Loves","R"),dim=10)
> MyCharacters
[1] "Bob"    "Loves"  "Lucy"   "Almost" "As"     "Much"   "As"     "He"     "Loves"  "R"

# Check the type
> typeof(MyCharacters)
[1] "character"
````
