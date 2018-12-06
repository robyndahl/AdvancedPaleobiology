# Beginner Concepts in R

Welcome to your first tutorial in the programming language R. We will work through this tutorial together in class, but you can always return to this page to review any of the concepts. This tutorial is based on the Beginner Concepts tutorial designed by Andrew Zaffos for his course teachPaleobiology. We will cover the same information in a different way, and you are welcome to use his page as a learning resource.

When you have completed this tutorial, you will be prepared to take the [Beginner Concepts Test](/Labs/Tests/BeginnerTest.md).

## Table of Contents

+ [What is R?](#what-is-r)
+ [The Most Basic R Concepts](#the-most-basic-r-concepts)
+ [Why Should You Use R?](#why-should-you-use-r)
+ [R as a Fancy Calculator](#r-as-a-fancy-calculator)
+ [Using Functions for Basic Math](#using-functions-for-basic-math)
+ [Storing Data in an Array](#storing-data-in-an-array)
+ [The Different Types of Data](#the-different-types-of-data)
+ [Vectors and Matrices as Special Arrays](#vectors-and-matrices-as-special-arrays)
+ [Referencing Elements of an Array](#referencing-elements-of-an-array)
+ [Multiple Data Types in an Array](#multiple-data-types-in-an-array)
+ [On the Proper Names of Things](#on-the-proper-names-of-things)

## What is R?

*Direct from the [R website](https://www.r-project.org):*

### Introduction to R

R is a language and environment for statistical computing and graphics. It is a GNU project which is similar to the S language and environment which was developed at Bell Laboratories (formerly AT&T, now Lucent Technologies) by John Chambers and colleagues. R can be considered as a different implementation of S. There are some important differences, but much code written for S runs unaltered under R.

R provides a wide variety of statistical (linear and nonlinear modelling, classical statistical tests, time-series analysis, classification, clustering, …) and graphical techniques, and is highly extensible. The S language is often the vehicle of choice for research in statistical methodology, and R provides an Open Source route to participation in that activity.

One of R’s strengths is the ease with which well-designed publication-quality plots can be produced, including mathematical symbols and formulae where needed. Great care has been taken over the defaults for the minor design choices in graphics, but the user retains full control.

R is available as Free Software under the terms of the Free Software Foundation’s GNU General Public License in source code form. It compiles and runs on a wide variety of UNIX platforms and similar systems (including FreeBSD and Linux), Windows and MacOS.

### The R Environment

R is an integrated suite of software facilities for data manipulation, calculation and graphical display. It includes

+ an effective data handling and storage facility,
+ a suite of operators for calculations on arrays, in particular matrices,
+ a large, coherent, integrated collection of intermediate tools for data analysis,
+ graphical facilities for data analysis and display either on-screen or on hardcopy, and
+ a well-developed, simple and effective programming language which includes conditionals, loops, user-defined recursive functions and input and output facilities.

The term “environment” is intended to characterize it as a fully planned and coherent system, rather than an incremental accretion of very specific and inflexible tools, as is frequently the case with other data analysis software.

### How We Will Use R in GEOL 497Q/597Q

R is one of the favorite programming languages for paleontologists, because statistics is the most commonly used type of math in paleontology. In our Advanced Paleobiology course, we will learn enough R to be able to conduct the most common types of staticial analyses used in paleontology. We will also use R for data visualization.

## The Most Basic R Concepts

Let's get started! Open R Studio. It should look something like this (though the colors may be different; you can choose the color scheme!):

![R Studio Screenshot](/Images/R_Studio.png)

You will be working in the panel on the left-hand side (**Consol**) but we will sometimes refer to information or other material that will appear in some of the other panels, like the **Environment** and **Plots** panels on the right-hand side.

Type something on the command line `>`

What happened? It likely returned an error, something like this:

````R
> hello, world
Error: object "hello, world" not found
````

Ok, so clearly we are not using the correct language. We'll need to start with some basic terminology. Specifically, let's talk about **objects** and **functions**. These are the two fundamental units of the R programming language. R stores information as objects and uses functions to interact with those objects. The following quote sums this up quite well:

*Everything that* ***exists*** *in R is an object.*

*Everything that you* ***do*** *in R is a function.*

So by typing `hello, world` in the command line, you were requesting R find an object called "hello, world". But no such object exists, because we have not defined any objects yet.

## Why Should You Use R?

If you're confused about why we're doing any of this, just remember that everything we do in R is just a **function** designed for one of the following four purposes. Determine which step you are trying to perform and then proceed from there.

1. **Mathematical Operations** - using R as a fancy calculator
2. **Storing Data** - storing data in a format that allows you to perform a mathematical operation on it
3. **Reshaping Data** - changing the format of previously stored data so that you can perform a different kind of operation on it
4. **Visualizing Data** - methods to make graphs or other visual representations of stored data

Although there are literally thousands of R functions, so long as you know a handful of basic methods for each of these four steps you can accomplish anything. This beginner's tutorial will largely focus on Mathematical Operations and Storing Data.

## R as a Fancy Calculator

If you have ever used a scientific or graphing calculator, then you already intuitively know all the basics of doing arithmetic in R. Yay! You've learned about 25% of R without even trying! But let's do a quick review anyway.

How do you think you would ask R to perform addition? Try asking it to add two and two.

Did it work?

````R
> 2+2
[1] 4
````
Great! A couple of notes:

*Spaces mean nothing in R*. You can type `> 2+2` or `> 2 + 2` and R will return the same answer. You can add as many spaces as you want and they will never impact the answer that R returns.

*We'll talk about that* `[1]` *later*. The answer that R returned was obviously 4, but why was it preceded by that `[1]`? We'll discuss this a little bit later in the tutorial.

Try out some more arithmetic. Addition, subtraction, multiplication, and division should be pretty straightforward, but there are some other operators and things to keep in mind. Use the following table to review different operators, and test them out in R.

Operator | Description | Example
-------- | ----------- | -------
x + y | addition | `2 + 2`
x - y | subtraction | `4 - 1`
x * y | multiplication | `2 * 3`
x / y | division | `6 \ 2`
x ^ y | exponents | `10 ^ 2`
x %% y | modulus (the remainder) | `4 %% 2`

Remember to pay attention to the order of operations! `(1 + 3)/(5 / 6)` will return a different value than `1+3/5/6`.

You can also perform what are called **logical operations**. A **logical** returns a value that is either `TRUE` or `FALSE`. Logicals are extraordinally important in R.

Try to figure out how you would ask R if 0 is greater than 1.

Did it work?

````R
> 0 > 1
[1] FALSE

# Let's ask if 5 is 1/2 of 10
> 5 == (1/2 * 10)
[1] TRUE
````

Notice that we used `==` to ask if these two questions are equal, rather than a single `=` sign. Most **operators** in R are straightforward, but there are some (like `==`) that are not intuitive. Here is a list of the basics. There are a few other operators in R, but we will worry about them later.

**Operator Example** | **Operator Definition**
-------------------- | -----------------------
x > y | is x greater than y
x >= y | is x greater than or equal to y
x < y | is x less than y
x <= y | is x less than or equal to y
x == y | is x equal to y
x != y | is x not equal to y

## Using Functions for Basic Math

Of course, writing out the artithmetic every time wouldn't be any better than just using a calculator. The first benefit that R has over a calculator is that it has many **functions** for arithmetic expressions that you can use as shortcuts.

````R
# For example, if I wanted to take the square root of a number
# I could just write out the expression:
> 4 ^ (1/2)
[1] 2

# Or I could use the sqrt( ) function
> sqrt(4)
[1] 2
````
Note that all functions have two parts. The function name `sqrt( )` and a set of function arguments that go inside the parentheses. Arguments are objects that you want the function to evaluate.

Now, you might be thinking, that doesn't really save any time compared to writing out the expression. True, but functions will save you more time on complex expressions. For example, try typing out the factorial of 10:

````R
> 10 * 9 * 8 * 7 * 6 * 5 * 4 * 3 * 2 * 1
[1] 3628800

# Instead of typing all those numbers, you can just use the factorial function:
> factorial(10)
[1] 3628800
````

The second most important benefit of functions is that they explicitly say what your code is doing. Seeing `factorial(10)` makes it immediately clear that you are calculating the factorial of ten.

## Storing Data in an Array

These simple functions probably still don't seem that impressive if you consider that most calculators already have buttons for square roots, factorials, etc. However, functions in R don't really shing unitl they are paired with stored data.

Let's consider the quadratic equation `5x^2+3x+7`. Let's say we want to solve this equation for `x=4`. That's easy to do in the way that we've already learned:

````R
> 5 * (4 ^ 2) + (3 * 4) + 7
[1] 99
````

But what if we wanted to solve this equation for multiple values of `x`? Let's try solving for `x = 1,2,3,4`. We cna do this by telling R to perform the function on all four numbers at once using the `c( )` command. `c( )` concatenates all the values within the parentheses into a vector.

````R
> 5 * c(1,2,3,4) ^ 2 + 3 * c(1,2,3,4) + 7
[1] 15 33 61 99
````

What if we wanted to solve a very long equation with a very long list of x-values? Typing out all of the x-values with `c( )` every time x appears in the equation could get very hard to read very quickly:

````R
# Typing things out every time woudl defeat the whole point of using computers.
> 9*c(1,2,3,4,5,6,7,8,9,10)^3+6*c(1,2,3,4,5,6,7,8,9,10)^2+8*c(1,2,3,4,5,6,7,8,9,10)+2
[1]   25  114  323  706 1317 2210 3439 5058 7121 9682
````

A better alternative involves learning how to store data as an **object**, and then plugging that object into equations. The most fundamental type of data object in computer science is generally called an **array**. In R, there is an even more fundamental type of data object known as a **vector**. For now, let's talk about arrays.

An array is essentially a set of values saved to your computer memory that is referenced by a name. Here is an example of how to make an array:

````R
# Create an array named "MyArray"
> MyArray <- array(data = c(1,2,3,4), dim = 4)

# View your array by typing the name
> MyArray
[1] 1 2 3 4

# You can perform arithmetic on your array
> MyArray + 4
[1] 5 6 7 8

# You can input your array into a function, and the function will be applied to each element in the array
> factorial(MyArray)
[1]  1  2  6 24

> 5 * MyArray ^ 2 + 3 * MyArray + 7
[1] 15 33 61 99
````

## Differences Between Array (the object) and array( ) (the function)

