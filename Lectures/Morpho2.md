# Additional Morphometrics Visualizations

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
![PC1 min and max](\Images\plotref.png)
