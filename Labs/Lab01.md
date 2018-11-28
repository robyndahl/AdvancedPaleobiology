# Begginer Concepts in R

Welcome to your first tutorial in the programming language R. We will work through this tutorial together in class, but you can always return to this page to review any of the concepts. This tutorial is based on the Beginner Concepts tutorial designed by Andrew Zaffos for his course teachPaleobiology. We will cover the same information in a different way, and you are welcome to use his page as a learning resource.

When you have completed this tutorial, you will be prepared to take the [Beginner Concepts Test](/Labs/Tests/BeginnerTest.md).

## Table of Contents

+ [What is R?](#what-is-r)
+ [The Most Basic R Concepts](#the-most-basic-r-concepts)
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

![R Studio Screenshot](/R_Studio.png)
