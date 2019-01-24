# Additional Morphometrics Visualizations

## pc.shape

You can access the coordiante data for each principal component. If you want to plot the shape difference between a refrence and target specimen, use `plotRefToTangent( )` 

````R



## mshape

The `mshape( )` function estimates the average landmark coordinates for a set of aligned specimens. 

````R
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
````

This function is useful for comparing the amount of variation in each PC. You can plot the max and min variation for each PC
