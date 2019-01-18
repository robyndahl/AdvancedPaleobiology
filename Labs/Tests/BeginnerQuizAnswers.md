# R Beginner Concepts Quiz

1. What **class** of objects is `mtcars`? What function did you use to find out?

`mtcars` is a data.frame. I used:
````R
> class(mtcars)
[1] "data.frame"
````

2. Is `islands` defined as a **1-dimensional array** or a **vector**? How did you find out?

`islands` is a vector, but vectors are also 1-dimensional arrays, so it is technically both. I check using:
````R
> is(islands,"vector")
[1] TRUE
````

3. How would you convert the data.frame `beaver1` into a matrix?

There are several options:
````R
> BeaverMatrix <- as(beaver1,"matrix")
> BeaverMatrix <- as.matrix(beaver1)
> BeaverMatrix <- data.matrix(beaver1)
````

4. What is the name of the 8th landmass in the `islands` dataset? How did you find out?

Borneo is the 8th landmass in the `islands` dataset. I used:
````R
> islands[8]
Borneo
   280
````

5. What function would you use if you wanted to combine all three datasets into a single object?

I would combine them into a list, like this:
````R
> NewList <- list(mtcars,beaver1,islands)
````

6. Does `islands` consist of **numeric** data? How did you find out?

Yes, `islands` is numeric. To find out, I used:
````R
> is(islands,"numeric")
[1] TRUE
````

7. Code **four** different ways to **subscript** the 2nd row and 7th column of `mtcars` using bracket notation. Hint: you are looking to return the number 17.02.

````R
> mtcars[2,7]
[1] 17.02

> mtcars[2,"qsec"]
[1] 17.02

> mtcars["Mazda RX4 Wag",7]
[1] 17.02

> mtcars["Mazda RX4 Wag","qsec"]
[1] 17.02
````

8. How would you change the landmass values of "Iceland", "Kyushu", and "Tierra del Fuego" to 82, 21, and 10 in the `islands` dataset? (Hint: You will need to use subscripts and the `<-` operator).

````R
> islands[c("Iceland","Kyushu","Tierra del Fuego")] <- c(82,21,10)
````

9. Are there any times when the beaver in the `beaver1` dataset had a temperature lower than 36.30 degrees? How did you find out?

No. I used:
````R
> any(beaver1[,"temp"] < 36.30)
[1] FALSE
````

10. Take the sum of all elements in the column **temp** in the `beaver1` dataset and call this value **ValueA**. Take the sume of all elements in the row **Valiant** in the `mtcars` dataset and call this value **ValueB**. Take the sum of the first **7 elements** of the `islands` dataset and call this value **ValueC**. Divide **ValueC** by **ValueB** and add **ValueA**. What is your final answer? Show your code.

The final answer is 4298.739. I used:
````R
> ValueA <- sum(beaver1[,"temp"])
> ValueB <- sum(mtcars["Valiant",])
> ValueC <- sum(islands[1:7])

> (ValueC / ValueB) + ValueA
[1] 4298.739
````
