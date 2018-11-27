# Lab 01 - R Beginner Concepts

*This tutorial was designed for [teachPaleobiology](https://github.com/aazaff/teachPaleobiology)*

In this tutorial, you will learn the most basic concept of using the programming language R. Once you have worked through the tuorial, complete the Beginner Concepts Test to ensure your mastery of the concepts addressed here.

## Table of Contents

+ The most basic concepts of R
+ Why should you use R?
+ R as a fancy scientific calculator
+ Using functions for basic math
+ Storing data in an array
+ The different types of data
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

To do this, you should always leave **thorough comments** in your code. Comments in R are always preceded by the ````#```` symbol. Anything that follows a ````#```` symbol will not be executed by the software.

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

You can also perform what are **logical operations**. A **logical** returns a value of either ````TRUE```` or ````FALSE````. Logicals are extraordinarily impportant to R.

````R
# Let's ask R if 0 is greater than 1
> 0 > 1
[1] FALSE

# What about if 5 is 1/2 of 10?
> 5 == (1/2 * 10)
[1] TRUE
````

Notice that we used ````==```` to ask if these two quantities are equal, rather than a single ````=```` sign. Most **operators** in R are straightforward, but there are some (like ````==````) that are not intuitive. Here is a list of the basics. There are a few other operators in R, but we will worry about them later.

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

The second most important benefit of functions is that they explicitly say what your code is doing (remember the first rule of R!) Seeing ````factorial(10)```` makes it immediately obvious that you are calculating the factorial of ten.

## Storing Data in an Array

These simple functions probably still don't seem that impressive if you consider that most calculators already have buttons for square roots and factorials, etc. However, functions in R don't really shine until they are paired with stored data.

Let's consider a quadratic equation, ````5x ^ 2 + 3x + 7````. Let's say we want to solve this equation for ````x = 4````. That's easy to do in the way that we've already learned:

````R
# 5x ^ 2 + 3x + 7, where x = 4
> 5 * (4 ^ 2) + (3 * 4) + 7
[1] 99
````
