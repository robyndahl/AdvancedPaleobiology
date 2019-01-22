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

Next, I will demonstrate how to use the `geomorph` package to conduct a general procrustes analysis (GPA), plot the GPA, and then conduct and plot a principle components analysis (PCA). At each step, we will discuss what the analysis does and what the results tell us.

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

The resulting plot shows the same data, but it has been aligned and resized to show each landmark's **centroid** and all the individual landmark variation around that centroid point.

![FacesGPA](/Images/facesGPA.png)

After we have produced a PCA plot, we will discuss what the major axes of the plot show and decide as a group whether the results accurately pick up differences in each class member's facial features or whether other biases affect the outcome of the analysis.

Finally, we will discuss how PCA might be used in other paleontological contexts.
