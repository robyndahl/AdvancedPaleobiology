# Morphometrics Demonstration

As part of our introduction to gemetric morphometrics, I will be demonstrating several concepts using a dataset derived from class selfies (based on an activity created by [David Polly](http://www.indiana.edu/~g562/)). 

## Step 1 - Selfies

Each student has submitted three selfies: neutral, smiling, frowning. The photos can be taken from the same or different angles:

![Faces Fig01](/Images/Faces_Fig1.png)

Since we have such a small class, I have added additional people into the mix: my wife and her two brothers, and my aunt. With these additions, we have people who are genetically related to each other in the set, which may or may not show up in our analysis (we'll see!).

If you want to practice collecting landmark data from the entire photoset, you can download them in a .zip file from Canvas.

## Step 2 - Landmark Data Collection

Before class, I have collected landmark data from the faces photoset. There are an endless number of facial landmarks that can be used, so I have opted for a scheme that uses 22 landmarks, simlar to this scheme:

![Facial Landmarks](https://docs.microsoft.com/en-us/azure/cognitive-services/face/images/landmarks.1.jpg) <img src="https://github.com/robyndahl/AdvancedPaleobiology/blob/master/Images/Landmarks.png?raw=true" width="250">

Though the landmark data has already been collected, I will demonstrate how to collect a set of landmark data in FIJI, export the data to an Excel spreadsheet, add labels, and save it as a .tps file. Detailed instructions on how to do this can be found in [Lab 05](/Labs/Lab05.md).

## Step 3 - Data Analysis in R

Next, I will demonstrate how to use the `geomorph` package to conduct a general procrustes analysis (GPA), plot the GPA, and then conduct and plot a principle components analysis (PCA). At each step, we will discuss what the analysis does and what the results tell us. If you want to read more about the `geomorph` package, do so [here](http://people.tamu.edu/~alawing/materials/ESSM689/Quick_Guide_to_Geomorph_v2.0.pdf).

First, we load the data into R:

````R
> library(geomorph)

# load landmark data from .tps file
> landmarks <- readland.tps("landmarks.tps", specID=TRUE)
````

At this point, we could visualize the raw data, but it wouldn't be very informative:

![Raw Landmark Data](/Images/rawlands.png)

We need to align the data using **general procrustes analysis**. To do this in `geomorph` and then to visualize the results:

````R
> facesGPA <- gpagen(landmarks)
|=================================================================================| 100%
>
> facesGPA

Call:
gpagen(A = landmarks) 



Generalized Procrustes Analysis
with Partial Procrustes Superimposition

22 fixed landmarks
0 semilandmarks (sliders)
2-dimensional landmarks
2 GPA iterations to converge


Consensus (mean) Configuration

                X            Y
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
>
> plot(facesGPA)
````

The resulting plot shows the same data, but it has been rotated, scaled, and centered to show each landmark's **centroid** and all the individual landmark variation around that centroid point. Think of the centroids as representing the **consensus shape** of the whole landmark dataset.

![FacesGPA](/Images/facesGPA.png)

From the GPA plot, we can instantly tell which landmarks had the most and least variation. But we still can't really do any analysis beyond a visual check, so we need to use **principal components analysis**. To do PCA in R:

````R
> facesPCA <- plotTangentSpace(facesGPA$coords)

> facesPCA$pc.summary
Importance of first k=26 (out of 27) components:
                           PC1     PC2    PC3     PC4     PC5     PC6     PC7     PC8
Standard deviation     0.05288 0.05162 0.0364 0.03527 0.03063 0.02179 0.01892 0.01517
Proportion of Variance 0.25997 0.24765 0.1231 0.11563 0.08722 0.04415 0.03326 0.02138
Cumulative Proportion  0.25997 0.50761 0.6308 0.74640 0.83362 0.87776 0.91102 0.93240
                           PC9    PC10     PC11     PC12     PC13     PC14    PC15
Standard deviation     0.01360 0.01058 0.009762 0.008011 0.007613 0.007058 0.00603
Proportion of Variance 0.01719 0.01041 0.008860 0.005970 0.005390 0.004630 0.00338
Cumulative Proportion  0.94959 0.95999 0.968850 0.974820 0.980200 0.984830 0.98821
                           PC16     PC17    PC18     PC19     PC20     PC21     PC22
Standard deviation     0.005579 0.005085 0.00445 0.004005 0.003153 0.002654 0.002239
Proportion of Variance 0.002890 0.002400 0.00184 0.001490 0.000920 0.000650 0.000470
Cumulative Proportion  0.991110 0.993510 0.99535 0.996840 0.997770 0.998420 0.998890
                           PC23     PC24     PC25     PC26
Standard deviation     0.002186 0.001783 0.001583 0.001226
Proportion of Variance 0.000440 0.000300 0.000230 0.000140
Cumulative Proportion  0.999330 0.999630 0.999860 1.000000
````

You may have noticed that using `plotTangentSpace( )` produced a plot. We will come back to that, but let's explore the `facesPCA$pc.summary` first.

## What are principal components?

Principal components analysis (PCA) is a method of analyzing variation in a data set. In paleontology, it is most often used to analyze variation in shape data. In this context, each principal component (PC) represents a different "shape variable" and provides a basis for further analysis. In PCA, the PCs are numbered from **most variability** to **least variability**.

In the `pc.summary` we created above, you can view the standard deviation, proportion of variance, and cumulative proportion of 26 of the 27 PCs for our faces data set. The **Proportion of Variance** indicates how much shape variation that PC accounts for. In our data set, PC1 accounts for 25.997% of shape variation. PC2 accounts for 24.765% (almost as much as PC1), and together, they acccount for 50.761%, which is indicated by the **Cumulative Proportion** of PC2.

David Polly (Indiana University) has a more detailed explanation of PCA [here](http://www.indiana.edu/~g562/Lectures/Lecture%204%20-%20Procrustes,%20PCA,%20and%20morphospace.pdf)

## Back To Our Analysis

Our intial use of `plotTangentSpace( )` produced a graph like this:

![Blank PCA](/Images/rawPCA.png)

The first thing you should always examine on a graph are the axes, to make sure you know what you're looking at. In this case, the x-axis is labeld **PC 1** and y-axis is labeld **PC 2**, so we're looking at a plot of PC1 vs PC2. This is a standard way to view PCA, because PC1 and PC2 usually account for most of the shape variation in a data set. The values on each axis show the percent variation along each axis.

The next thing you might notice are all the data (the black dots). These are our "specimens" (each photo we collected landmark data from), shown in the "shape space" of PC1 vs PC2. There are no labels or other information indicate which photo each dot represents, but we'll add that info next.

The plot also shows two shape grids, illustrating the minimum and maximum shapes on the two PCs plotted.

## Step 4 - Add More Information to PCA Plot

We can add additional information like labels or colors using a spreadsheet of data:

````R
> specimens <- read.csv("specimens.csv", header = TRUE)
> specimens
      Specimen     Name Expression
1   Specimen 1  Brandon    neutral
2   Specimen 2  Brandon    smiling
3   Specimen 3  Brandon   frowning
4   Specimen 4    Jenny    neutral
5   Specimen 5    Jenny    smiling
6   Specimen 6    Jenny   frowning
7   Specimen 7    Robyn    neutral
8   Specimen 8    Robyn    smiling
9    Specmen 9    Robyn   frowning
10 Specimen 10     Tina    neutral
11 Specimen 11     Tina    smiling
12 Specimen 12     Tina   frowning
13 Specimen 13    AlexF    smiling
14 Specimen 14    AlexF    neutral
15 Specimen 15    AlexF   frowning
16 Specimen 16     Bert    neutral
17 Specimen 17     Bert    smiling
18 Specimen 18     Bert   frowning
19 Specimen 19   Connor    neutral
20 Specimen 20   Connor    smiling
21 Specimen 21   Connor   frowning
22 Specimen 22    AlexH    neutral
23 Specimen 23    AlexH    smiling
24 Specimen 24    AlexH   frowning
25 Specimen 25 Victoria    neutral
26 Specimen 26 Victoria    smiling
27 Specimen 27 Victoria   frowning
````
To add labels or color, let's explore the `plotTangentSpace( )` function more closely. 

***FUNCTION:***
````R
plotTangentSpace(A, axis1 = 1, axis2 = 2, warpgrids = TRUE, mesh = NULL, label = FALSE, groups = NULL, verbose = FALSE)
````

***ARGUMENTS:***
Argument | Description
-------- | -----------
**A** | A 3D array containing landmark coordinates for a set of aligned specimens
**warpgrids** | A logical value indicating whether deformation grids for shapes along X-axis should be displayed
**mesh** | A mesh3d object to be warped to represent shape deformation along X-axis (when `warpgrids = TRUE`) as described as `warpRefMesh( )`
**axis1** | A value indicating which PC axis should be displayed as the X-axis (default = PC1)
**axis2** | A value indicating which PC axis should be displayed as the Y-axis (default = PC2)
**label** | A logical value indicating whether labels for each specimen should be displayed
**groups** | An optional factor vector specifying group identity for each specimen
**verbose** | A logical value indicating whether the output is basic or verbose
