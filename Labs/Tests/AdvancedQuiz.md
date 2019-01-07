# R Advanced Concepts Quiz


Complete this test by working through the problem sets and then uploading your answer sheet to your own markdown sheet in your GitHub Repository. Submit a link to your answer sheet in Canvas by the due date: **Thursday, Jan 24 @ 11:59pm**

## A Mystical Rite of Passage

The only way to learn a new programming language is by writing programs in it. The first program (function) to write is the same for all languages. You must print the words "Hello, world." *This is a sacred rite of passage. Cherish it!*

1. Write a function that returns the phrase "Hello, world."

## Problem Set

2. Load the `iris` dataset. Write a function that takes `iris` as its argument and returns three subsets of the data.frame, split by the three different types of species (saved as a single object).

3. Write a function that takes `iris` as its argument. The function should, for each row, **add** `Sepal.Length` and `Petal.Length` *if* `Sepal.Width` is >3.1. It should **subtract** `Petal.Length` from `Sepal.Length` *if* `Sepal.Width` is <3.1. The answer should be returned as a vector.

4. Load the `mtcars` dataset. Write a function that takes the number of cylinders as its argument. Have the function return the average miles per gallon (column `mpg`) for *all* cars with that number of cylinders (column `cyl`).

5. Write a function that simulates 1,000,000 PowerBall drawings. A PowerBall drawing takes a randome sample of five numbers (without replacement) from 1-69, plus one powerball number ranging from 1-26. The function should return a single object recording all of your draws.

6. Write a function that takes a single set of lottery numbers (as a vector) as its argument. As before, write a function that simulates 1,000,000 PowerBall drawings. Have the function return a `TRUE` or `FALSE` value indicating if you won any of the drawings.
