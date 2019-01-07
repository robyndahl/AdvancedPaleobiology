# Lab 05 - Introduction to Morphometrics

**Acknowledgement:** This lab is adapted from activities in David Polly's [Geometric Morphometrics](http://www.indiana.edu/~g562/) course

## Table of Contents

+ [Pre-lab reading assignment](#pre-lab-reading-assignment)
+ [Part 1: Osteostracans Landmark Data Collection](#part-1-osteostracans-landmark-data-collection)
+ [Part 2: Data Analysis](#part-2-data-analysis)
+ [Part 3: Conclusions](#conclusions)

## Pre-Lab Reading Assignment

Before getting started on this lab, please read [Webster & Sheets (2010) *A Practical Introduction to Landmark-Based Geometric Morphometrics*](https://geosci.uchicago.edu/~mwebster/Webster_and_Sheets_2010.pdf), which will provide an overview of the applications of morphometrics in paleontology and an introduction to the methods we will practice in this lab.

## Part 1: Osteostracans Landmark Data Collection

From the [*Tree of Life*](http://tolweb.org/Osteostraci)

The Osteostraci, or osteostracans, are a major clade (about 200 species) of fossil, armored jawless vertebrates which lived from the Early Silurian (about 430 million years) to the Late Devonian (about 370 million years). Most of them have a characteristic horseshoe-shaped head, which consists of a massive endoskeletal skull, covered with a shield of dermal bone. On the dorsal surface of the head are the closely-sat eyes, a pineal foramen, and a medium, keyhole-shaped nasophypophysial openening. In addition, there are peculiar "fields" (in fact, depressions of the braincase, covered with loose platelets of dermal bone), which have been regarded as either sense organs or electric organs. The mouth and gill openings are, like the Galeaspida, situated on the ventral side of the head. Osteostracans also have large, pad-shaped paired fins. Most osteostracans are about 20 to 40 cm in total length, but some species could be extremely small (about 4 cm in length). The largest species was about one meter in length.

**Step 1:** Download the osteostracan skull photo set from Canvas. This `.zip` file contains skull diagrams of 30 different Osteostracan species, which you will be collecting landmark data from. Save this folder on your desktop. Spend a few minutes reviewing the diagrams so that you are familiar with the range of variation among species.

**Step 2:** Use [FIJI](https://fiji.sc/) to collect your landmark data. But first, some things to remember about collecting landmarks (from [David Polly](http://www.indiana.edu/~g562/Handouts/Collecting%20Landmarks.pdf):

+ Each image must have the same number of landmarks
+ The landmarks on each image image must be collected in the same order
+ Landmarks are ordinarily placed on homologous points, points that can be replicated from object to object based on common morphology, common function, or common geometry
+ You may have to flip some images so that they are not reversed left to right (e.g., if most of your images show the right side, flip left side images so that they mimic the right side)

To collect your landmark data, open the first image in the folder in FIJI. Use the multipoint collector tool to place landmarks on the skull. You can use the image below as a guide or decide on your own landmark positions When you have placed all of your points, use CTRL-M to record them in the measurement window (a spreadsheet that will populate with all of your data as you collect from each image). FIJI will collected more information than we are interested in, and you don't need to pay any attention to the first four columns on the sheet (`Area, Mean, Min, Max`). We are only interested in the cartesian coordinates of our landmarks (`X, Y`). When you are done with your first image, use CTRL-SHIFT-O to open the next image. Repeat until you have collected landmark data from all 30 images.

![Lab05_Fig1](/Images/Lab05_Fig1.png)

**Step 3:** When you have collected landmark data for all of the images, copy and paste the data from the FIJI measurements into Excel. Remember, we don't care about the first five columns of data, so you can ignore those columns.

We need to format this spreadsheet so that it can be saved as a `.tps` file that can be used by the `geomorph` R package. To do this, insert blank rows before and after the block of rows for each specimen. Above the x-coordinate, enter the text `LM=` followed by the number of landmarks you collected for the specimen (e.g., `LM=13`). Below the x-coordinate, enter the text `ID=` follwed by the taxon name (e.g., `ID=Steogosaurus`). Your spreadsheet should end up looking something like this:

![Lab05_Fig2](/Images/Lab05_Fig2.png)

To save this as a `.tps` file, highlight the two columns your coordinate data (columns F & G in the example picture) and copy. In Word, use `Paste Special` to paste the data as **unformatted text**. Some programs don't like tab characters, which are inserted by default when you paste from Excel. Remove them with `Edit > Find > Advanced Replace` and `Replace`. To do this, click the arrow in the lower left to show options. Click the "replace" tab at the top to open both Find and Replace input bars. Enter `^t` in the "Find What" bar and enter a single space in the "Replace With" bar. Click `Replace All`.

Save your file as **plain text** in the r directory on the computer you are working on.

## Part 2: Data Analysis

**Step 4:** Now we will use the `geomorph` R package to analyze our landmark data. You may want to review Adams & Otarola-Castillo (2013) *geomorph: an R package for the collection and analysis of geometric mophometric shape data* and Sherratt (2014) *Quick Guide to geomorph v2.0* before starting.

Open R Studio and complete the following:

````R
# install geomorph
> install.packages("geomorph")

# load geomorph
> library("geomorph")

# load your .tps file
> osteostracans <- readland.tps("osteostracans.tps", specID = "ID")

# view your .tps file
> osteostracans

# check the number of dimensions in your data
> dim(osteostracans)
> [1] 13  2 29 # osteostracans has 3 dimensions
````
Great, now your data is loaded and ready to be analyzed! The first analysis we want to conduct is a **Generalized Procrustes Analysis** or GPA. Here is an GPA explainer from Sherratt (2014):

*Generalized Procrustes Analysis (GPA: Gower 1975; Rohlf and Slice 1990) is the primary means by which shape variables are obtained from landmark data. GPA translates all specimens to the origin, scales them to unit-centroid size, and optimally rotates them (using a least-squares criterion) until the coordinates of corresponding points align as closely as possible. The resulting aligned Procrustes coordinates represent the shape of each specimen, and are found in a curved space related to Kendall's shape space (Kendall 1984). Typically, these are projected into a linear tangent space yielding Kendall's tangent space coordinates (Dyrden and Mardia 1993; Rohlf 1999), which are used for subsequent multivariate analyses.*

To conduct a GPA on your data, complete the following:

````R
# pick a name for your new function, something descriptive like "osteoGPA"
> osteoGPA <- gpagen(osteostracans)

# take a look at the GPA results
> osteoGPA

# plot the GPA
> plot(osteoGPA)
````
Examine the plot. What is information does it visualize?

**Step 5.** Now we will use `geomorph` to conduct a **Principal Components Analysis** or PCA. For a quick explanation of PCA, we can turn to Foote & Miller:

**Ordination of Specimens** One of the main uses of multivariate analysis is the facilitate visual inspection of data. In a bivariate plot, it is easy to see which specimens are most similar, how specimens differ, how the data trend, and so on. To do the same with multivariate data requires an **ordination** -- a representation of the position of the specimens relative to on another. One of the most widely employed methods to achieve this goal is PCA. In the figure above, Figure 3.12c shows the same hypothetical data as figure 3.12a. The points have simply been rotated so that the major and minor axes running through the data in Figure 3.12a are now in the same direction as the new x and y axes of Figure 3.12c. The direction of the major axis is the direction of maximal dispersion in the data and defines the first principal component. There is still residual variation around the axis, indicated by the minor axis that is perpendicular to the first axis. The minor axis defines the second principal component.

The method of principal components extends to any number of dimensions. Each successive axis is always perpendicular to all the previous ones, and it runs in the direction of maximal remaining dispersion around the previous axes. The position of each specimen along a particular principal-component axis is referred to as its score on that axis. The length of each axis tells how much variance in the data is acounted for by the corresponding principal component; it is expressed by a number called the eigenvalue.

To conduct and plot PCA in R:

````R
> osteoPCA <- plotTangenSpace(osteoGPA$coords, label = TRUE)

# View a summary of variance associated with each PC axis using $pc.summary
> osteoPCA$pc.summary

# View the shape variables as principal component scores using $pc.scores
> osteoPCA$pc.scores
````
