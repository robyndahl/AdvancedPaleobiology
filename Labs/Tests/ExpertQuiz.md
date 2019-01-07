# R Expert Concepts Quiz

The following quiz is designed to test the concepts covered in the R Expert Concepts tutorial created by Andrew Zaffos. For the WWU Advanced Paleobiology Course, you should answer these questions in a Markdown file in your personal repository, then submit a link to that file on the course Canvas site by the due date: Tuesday, January 29 @ 11:59pm

## Problem Set

Load the `precip` dataset.

1. What is the **mean**, **median**, and **standard deviation** of `precip`?

2. Is `precip` best visualized using a `barplot( )` or `hist( )`? Why?

3. Generate a vector of random numbers drawn from a normal distribution with the same mean, standard deviation, and number of elements as in the `precip` dataset. Name this vector `RandomNormal`.

4. Write a function that tests, based on the means of each distribution, whether it is likely that `RandomNormal` and `precip` were drawn from the same underlying distribution.

5. Create a `density( )` plot of `precip` and `RandomNormal`. Is the test you preformed above in Question 4 a good or bad indicator of whether the two distributions are identical? Why or why not?
