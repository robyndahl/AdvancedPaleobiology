# Additional Morphometrics Visualizations

To get started, we need to load in some data. We will use the Faces dataset from my previous example. The datasets are available in this repository.

````R
> install.packages("geomorph")
> library(geomorph)
> lands <- readland.tps("https://raw.githubusercontent.com/robyndahl/AdvancedPaleobiology/master/Lectures/landmarks.tps", specID = "ID")
> specimens <- read.csv("https://raw.githubusercontent.com/robyndahl/AdvancedPaleobiology/master/Lectures/specimens.csv", header = TRUE)
> facesGPA <- gpagen(lands)
> facesPCA <- plotTangentSpace(facesGPA$coords)
````

## mshape( )

The `mshape( )` function estimates the average landmark coordinates for a set of aligned specimens. You can use `mshape( )` to create an average reference specimen, which can then be used to show specific shape variation.

````R
# view the coordinates created by mshape( )
> mshape(facesGPA$coords)
             [,1]         [,2]
 [1,]  0.13912822 -0.285952973
 [2,]  0.15374449 -0.069408491
 [3,]  0.07936724 -0.224496806
 [4,]  0.09158629 -0.105959895
 [5,]  0.10485204 -0.157994972
 [6,]  0.08660642 -0.156784702
 [7,]  0.07260133 -0.138867654
 [8,] -0.10600640 -0.096746050
 [9,] -0.09344878  0.002715881
[10,] -0.09832813  0.102225237
[11,] -0.24686581 -0.125423293
[12,] -0.22644902  0.008761989
[13,] -0.24617789  0.010031166
[14,] -0.29313599  0.011263435
[15,] -0.23592443  0.142561342
[16,]  0.08989407  0.083277766
[17,]  0.10035169  0.213301603
[18,]  0.11805269  0.147914546
[19,]  0.10103935  0.149130132
[20,]  0.08164440  0.156460926
[21,]  0.16469379  0.062365934
[22,]  0.16277445  0.271624877
attr(,"class")
[1] "mshape" "matrix"

# use mshape( ) to create a reference specimen
> ref <- mshape(facesGPA$coords)
````

## plotRefToTarget( )

You can access the coordiante data for each principal component. If you want to plot the shape difference between a refrence and target specimen, use `plotRefToTarget( )` 

````R
# to compare specimen 1 to the reference, with three different plot types
> plotRefToTarget(ref, facesGPA$coords[,,1], method = "TPS")
> plotRefToTarget(ref, facesGPA$coords[,,1], method = "vector")
> plotRefToTarget(ref, facesGPA$coords[,,1], method = "points")

# to view the minimum and maximum variation for a PC
> plotRefToTarget(ref, facesPCA$pc.shapes$PC1min, method = "TPS")
> plotRefToTarget(ref, facesPCA$pc.shapes$PC1max, method = "TPS")

# to view the range of variation for a PC, line three plots up in a row
> par(mfrow = c(1,3)
> plotRefToTarget(ref, facesPCA$pc.shapes$PC1min, method = "TPS")
> plotRefToTarget(ref, ref, method = "TPS")
> plotRefToTarget(ref, facesPCA$pc.shapes$PC1max, method = "TPS")
````
![PC1 min and max](/Images/plotRef.png)

## Use ggplot to plot the data

`ggplot2` is a powerful data visualization package that we will not discuss extensively in this course. I will, however, provide examples of how to use `ggplot2` to visualize our projects. You can also learn `ggplot2` and there rest of `tidyverse` on your own, with the help of the book *R for Data Science* (conviently available [here](https://r4ds.had.co.nz/)).

````R
> install.packages("tidyverse")
> library(tidyverse)

# ggplot requires all data sources to be data frames or tibbles, so we need convert our data
> facesTIB <- as.tibble(facesPCA$pc.scores)
> facesTIB
# A tibble: 27 x 26
       PC1      PC2      PC3      PC4      PC5      PC6      PC7      PC8
     <dbl>    <dbl>    <dbl>    <dbl>    <dbl>    <dbl>    <dbl>    <dbl>
 1 -0.0315  0.0184  -0.0402   7.76e-3 -1.94e-2  0.0321  -2.54e-2  1.86e-5
 2  0.0116 -0.0250  -0.0455   2.02e-2 -2.46e-2  0.0411  -2.68e-2 -2.80e-2
 3 -0.0328 -0.00610 -0.0267  -7.39e-4 -6.99e-3 -0.0119  -1.43e-2 -1.45e-2
 4 -0.0624  0.00560  0.0445  -4.15e-2 -1.40e-2  0.00977 -2.24e-2  1.22e-2
 5  0.0673  0.0128   0.0397  -5.44e-2 -6.07e-3  0.00572 -2.73e-3  4.87e-4
 6 -0.0638  0.00361  0.0491  -2.63e-2  5.89e-3 -0.0195  -2.95e-3 -3.22e-3
 7 -0.0397 -0.00280 -0.00718  1.20e-2  1.40e-2  0.0181   1.76e-2  3.55e-2
 8  0.0438 -0.0889  -0.0368   3.70e-2  2.12e-4  0.00646  2.71e-2  2.61e-2
 9 -0.0140  0.00281 -0.0879   7.64e-3  6.32e-2 -0.0259  -6.58e-4  3.31e-3
10 -0.0224 -0.0520   0.0387  -1.87e-2  2.72e-2  0.0153  -1.83e-2  7.13e-3
# ... with 17 more rows, and 18 more variables: PC9 <dbl>, PC10 <dbl>,
#   PC11 <dbl>, PC12 <dbl>, PC13 <dbl>, PC14 <dbl>, PC15 <dbl>, PC16 <dbl>,
#   PC17 <dbl>, PC18 <dbl>, PC19 <dbl>, PC20 <dbl>, PC21 <dbl>, PC22 <dbl>,
#   PC23 <dbl>, PC24 <dbl>, PC25 <dbl>, PC26 <dbl>

# add identifying information to facesTIB using mutate( )
> facesTIB <- mutate(facesTIB, Name = specimens$Name)
> facesTIB <- mutate(facesTIB, Expression = specimens$Expression)

# create the plot
> ggplot(facesTIB, aes(PC1, PC2)) +
        geom_point(aes(color = Name, shape = Expression)
````
 
