# R Beginner Concepts Quiz

Complete this test by working through the problem sets and then uploading your answer sheet to your own markdown sheet in your GitHub Repository. If you need a quick tutorial on how to create this document, [click here](/GitTutorial.md). Submit a link to your answer sheet in Canvas by the due date: Tuesday, January 15 at 2pm.

## Loading in Data

We will be using several pre-built datasets that come with R. You can load them into R using the ````data( )```` function.

````R
# Data from the 1974 Motor Trends US magazine
> data(mtcars)

# The body temperature series of a beaver
> data(beaver1)

# Areas of the world's major landmasses
> data(islands)
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
> tail(beaver1)
    day time  temp activ
109 347  250 36.84     0
110 347  300 36.86     0
111 347  310 36.88     0
112 347  320 36.93     0
113 347  330 36.97     0
114 347  340 37.15     1

> head(islands)
      Africa   Antarctica         Asia    Australia Axel Heiberg 
       11506         5500        16988         2968           16 
````
**PROBLEM SET 1**

1. What **class** of object is ````mtcars````? What function did you use to find out?

2. Is ````islands```` defined as a **1-dimensional array** or a **vector**? How did you find out?

3. How would you convert the data.frame ````beaver1```` into a matrix?

4. What is the name of the 8th landmass in the ````islands```` dataset? How did you find out?

5. What function would you use if you wanted to combine all three datasets into a single object?

6. Does ````islands```` consist of numeric data? How did you find out?

7. Code **four** different ways to **subscript** the **2nd row** and **7th column** of ````mtcars```` using bracket notation. (Hint: You are looking to return the number 17.02)

8. How would you change the landmass values of "Iceland", "Kyushu", and "Tierra del Fuego" to 82, 21, and 10 in the ````islands```` dataset? (Hint: You will need to use **subscripts** and the ````<-```` operator).

9. Are there any times when the beaver in the ````beaver1```` dataset had a temperature lower than 36.30 degrees? How did you find out?

10. Take the sum of all elements in the column **temp** in the ````beaver1```` dataset and call this value **ValueA**. Take the sume of all elements in the row **Valiant** in the ````mtcars```` dataset and call this value **ValueB**. Take the sum of the first **7 elements** of the ````islands```` dataset and call this value **ValueC**. Divide **ValueC** by **ValueB** and add **ValueA**. What is your final answer? Show your code.
