# Morphometrics Demonstration

As part of our introduction to gemetric morphometrics, I will be demonstrating several concepts using a dataset derived from class selfies (based on an activity created by [David Polly](http://www.indiana.edu/~g562/)). 

## Step 1 - Selfies

Each student has submitted three selfies: neutral, smiling, frowning. The photos can be taken from the same or different angles:

![Faces Fig01](/Images/Faces_Fig1.png)

## Step 2 - Landmark Data Collection

Before class, I will have collected landmark data from the faces photoset. There are an endless number of facial landmarks that can be used, so I have opted for a scheme that uses 27 landmarks:

![Facial Landmarks](https://docs.microsoft.com/en-us/azure/cognitive-services/face/images/landmarks.1.jpg)

Though the landmark data has already been collected, I will demonstrate how to collect a set of landmark data in FIJI, export the data to an Excel spreadsheet, add labels, and save it as a .tps file. Detailed instructions on how to do this can be found in [Lab 05](/Labs/Lab05.md).

## Step 3 - Data Analysis in R

Next, I will demonstrate how to use the `geomorph` package to conduct a general procrustes analysis (GPA), plot the GPA, and then conduct and plot a principle components analysis (PCA). At each step, we will discuss what the analysis does and what the results tell us.

After we have produced a PCA plot, we will discuss what the major axes of the plot show and decide as a group whether the results accurately pick up differences in each class member's facial features or whether other biases affect the outcome of the analysis.

Finally, we will discuss how PCA might be used in other paleontological contexts.
