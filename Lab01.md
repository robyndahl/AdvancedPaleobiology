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
*** "Always talk about R! ***

Spell things out for future readers of yoru programs as explicitly as possible. You want other programmers to easily recognize what you are trying to accomplish with a particular line of code. A quote that I quite like is:

"Always annotate your code, because the sucker trying to figure out what you did six months from now is going to be ***you***."

To do this, you should always leave **thorough comments** in your code. Comments in R are always preceded by the ````#```` symbol. Anything that follows a # symbol will not be executed by the software.
