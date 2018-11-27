# R Beginner Concepts Test

Complete this test by working through the problem sets and then uploading your answer sheet to your own GitHub Repository.

## Loading in Data

We will be using several pre-built datasets that come with R. You can load them into R using the ````data( )```` function.

````R
# Data from the 1974 Motor Trends US magazine
> data(mtcars)

# Measurements of the girth, height, and volume of felled black cherry trees
> data(trees)

# The average amount of precipitation (rainfall) in inches for U.S. cities
> data(precip)
````

Note that you would normally need to define the name of new objects using the ````<-```` operator, but the ````data( )```` function creates a new object for you automatically.

````R
# Take a quick look at the structure of each dataset
# You can use the head( ) function to only display the first few rows of each dataset
> head(mtcars)
                   mpg cyl disp  hp drat    wt  qsec vs am gear carb
Mazda RX4         21.0   6  160 110 3.90 2.620 16.46  0  1    4    4
Mazda RX4 Wag     21.0   6  160 110 3.90 2.875 17.02  0  1    4    4
Datsun 710        22.8   4  108  93 3.85 2.320 18.61  1  1    4    1
Hornet 4 Drive    21.4   6  258 110 3.08 3.215 19.44  1  0    3    1
Hornet Sportabout 18.7   8  360 175 3.15 3.440 17.02  0  0    3    2
Valiant           18.1   6  225 105 2.76 3.460 20.22  1  0    3    1

# You can also use the tail( ) function to display the last few rows of the dataset
> tail(trees)
   Girth Height Volume
26  17.3     81   55.4
27  17.5     82   55.7
28  17.9     80   58.3
29  18.0     80   51.5
30  18.0     80   51.0
31  20.6     87   77.0

> head(precip)
Mobile      Juneau     Phoenix Little Rock Los Angeles  Sacramento
  67.0        54.7         7.0        48.5        14.0        17.2
````
**PROBLEM SET 1**

1. What **class** of object is ````mtcars````? What function did you use to find out?

2. Is ````precip```` defined as a **1-dimensional array** or a **vector**? How did you find out?

3. How would you convert the data.frame ````trees```` into a matrix?

4. What is the name of the 14th city in the ````precip```` dataset?

5. What function would you use if you wanted to combine all three datasets into a single object?

6. Does ````precip```` consist of numeric data? How did you find out?

7. Code **four** different ways to **subscript** the **2nd row** and **7th column** of ````mtcars```` using bracket notation. (Hint: You are looking to return the number 17.02)

8. How would you change the precipitation values of "Juneau", "Phoenix", and "Sacramento" to 23, 46, and 12 in the ````precip```` dataset? (Hint: You will need to use **subscripts** and the ````<-```` operator).

9. Are there any trees in the ````trees```` dataset with more **girth** than **volume**? How did you find out?

10. Take the sum of all elements in the column **height** in the ````trees```` dataset and call this value **ValueA**. Take the sume of all elements in the row **Valiant** in the ````mtcars```` dataset and call this value **ValueB**. Take the sum of the first **8 elements** of the ````precip```` dataset and call this value **ValueC**. Divide **ValueC** by **ValueB** and add **ValueA**. What is your final answer? Show your code.
