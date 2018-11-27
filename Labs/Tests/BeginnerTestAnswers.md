# Beginner Concepts Test Answers

## 1. What **class** of objects is ````mtcars````? What function did you use to find out?

````mtcars```` is a data.frame. I used the following code to find out:

````R
> class(mtcars)
[1] "data.frame"
````

## 2. Is ````precip```` defined as a 1-dimensional array or a vector? How did you find out?

````precip```` is both a 1-dimensional array and a vector, because they are the same thing. I checked if ```precip``` was a vector using the following code:

````R
> is(precip, "vector")
[1] TRUE
````

## 3. How would you convert the data.frame ````trees```` into a matrix?

````R
> NewMatrix <- as(trees,"matrix")
> NewMatrix
      Girth Height Volume
 [1,]   8.3     70   10.3
 [2,]   8.6     65   10.3
 [3,]   8.8     63   10.2
 [4,]  10.5     72   16.4
 [5,]  10.7     81   18.8
 [6,]  10.8     83   19.7
 [7,]  11.0     66   15.6
 [8,]  11.0     75   18.2
 [9,]  11.1     80   22.6
[10,]  11.2     75   19.9
[11,]  11.3     79   24.2
[12,]  11.4     76   21.0
[13,]  11.4     76   21.4
[14,]  11.7     69   21.3
[15,]  12.0     75   19.1
[16,]  12.9     74   22.2
[17,]  12.9     85   33.8
[18,]  13.3     86   27.4
[19,]  13.7     71   25.7
[20,]  13.8     64   24.9
[21,]  14.0     78   34.5
[22,]  14.2     80   31.7
[23,]  14.5     74   36.3
[24,]  16.0     72   38.3
[25,]  16.3     77   42.6
[26,]  17.3     81   55.4
[27,]  17.5     82   55.7
[28,]  17.9     80   58.3
[29,]  18.0     80   51.5
[30,]  18.0     80   51.0
[31,]  20.6     87   77.0
````

## 4. What is the name of the 14th city in the ````precip```` dataset?

````R
> precip[14]
Atlanta 
   48.3
````

## 5. What function would you use if you wanted to combine all three datasets into a single object?

````R
> MyList <- list(precip,trees,mtcars)
````

I am not showing the resulting list because it's too long to show here.

## 6. Does precip consist of numeric data? How did you find out?

````R
> is(precip, "numeric")
[1] TRUE
````

## 7. Code four different ways to subscript the 2nd row and 7th column of ````mtcars```` using bracket notation.

````R
> mtcars[2,7]
[1] 17.02

> mtcars[2,"qsec"]
[1] 17.02

> mtcars["Mazda RX4 Wag",7]
[1] 17.02

> mtcars["Maxda RX4 Wag","qsec"]
[1] 17.02
````

## 8. How would you convert the precipitation values of Juneau, Phoenix and Sacramento to 23, 46, and 12 in the ````precip```` dataset?

````R
> precip[c("Juneau","Phoenix","Sacramento")] <- c(23,46,12)
````

## 9. Are there any trees in the ````trees```` dataset with more girth than volume? How did you find out?

No, there are none. I used the following code to find out:

````R
> any(trees[,"Girth"] > trees[,"Volume"])
[1] FALSE
````

## 10. Take the sum of all elements in column height of the trees dataset, call this value ValueA. Take the sum of all elements in row Valiant of the mtcars dataset, call this value ValueB. Take the sum of the first 8 elements of the precip dataset, call this value ValueC. Divide ValueC by ValueB and add ValueA. What is your final answer?

The answer is 2356.633. I used the following code:

````R
> ValueA <- sum(trees[,"Height"])
> ValueB <- sum(mtcars["Valiant",])
> ValueC <- sum(precip[1:8])
> (ValueC / ValueB) + ValueA
[1] 2356.633
````
