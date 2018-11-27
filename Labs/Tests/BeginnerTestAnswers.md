# Beginner Concepts Test Answers

## 1. What **class** of objects is ````mtcars````? What function did you use to find out?

````mtcars```` is a data.frame. I used the following code to find out:

````R
> class(mtcars)
[1] "data.frame"
````

## Is ````precip```` defined as a 1-dimensional array or a vector? How did you find out?

````precip```` is both a 1-dimensional array and a vector, because they are the same thing. I checked if ```precip``` was a vector using the following code:

````R
> is(precip, "vector")
[1] TRUE
