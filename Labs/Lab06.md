# Introduction to the Building Phylogenies

This lab builds on the morphometrics activity that we have already complete in [Lab 05](/Labs/Lab05.md). It was originaly created by [Andrew Zaffos](https://github.com/aazaff/teachPaleobiology/blob/master/LabExercise2.md) and edited for this course.

Part 1 should be quick and easy based on our previous lab. Part 2 explores how PCA can be combined with phylogenetic data to better understand evolutionary relationships.

## Part 1

Fossil species are recognized and classified on the basis of morphology. This introduces subjectivity into the process of classification because all species display some range of morphological variability and the line between intra- and interspecific variability is often a judgment call.

#### Step 1

Click [here](https://github.com/aazaff/paleobiologyWebsite/blob/master/Lab2Figures/Figure1.pdf) to see illustrations of 25 ammonoid specimens. Your job will be to group them into species, keeping in mind the potential for intraspecific variability, ontogenetic change, and sexual dimorphism.

#### Step 2

We are going to open the [ammonite_classify.csv](https://raw.githubusercontent.com/paleobiodb/teachPaleobiology/master/Lab2Figures/ammon_classify.csv) file in R. CSV stands for **comma separated file**, it is one of the most common file formats used in R for storing two-dimensional arrays - i.e., matrices or data frames. You can also view, create, and edit them using a spreadsheet program like Microsoft Excel.

It is a data file containing shell coiling measurements traditionally used in ammonoid classification, including diameter (D), the ratio of umbilical width U to diameter (U/D), and the ratio of shell width W to diameter (W/D). The file also contains the stratigraphic position (relative geologic age) of each specimen.

<a href="url"><img src="https://raw.githubusercontent.com/aazaff/paleobiologyWebsite/master/Lab2Figures/Figure2.png" align="center" height="150" width="200" ></a>

To load the CSV file into R you will use the ````read.csv( )```` function.

````R
# First record the URL of the file as a character string - i.e., use quotes
URL<-"https://raw.githubusercontent.com/paleobiodb/teachPaleobiology/master/Lab2Figures/ammon_classify.csv"

# Save the CSV File as an object named Ammonites
# We will also use the row.names argument to tell R that the first column of the CSV file
# should be used as the rownames.
Ammonites<-read.csv(URL,row.names=1)
````

#### Step 3

Your basic workflow should to qualitatively (i.e., visually) decide which specimens you think belong to different species. Then you should subset the Ammonites array into several arrays based on your hypothesized species groupings. Using the techniques from the [expertConcepts](https://github.com/aazaff/startLearn.R/blob/master/expertConcepts.md) R tutorial (that we didn't actually complete in class), you can visualize, describe, or statistically test the different size and shape distributions of your hypothesized species. We can do this together in class if you are not sure how to on your own.

You will find the following articles extremely helpful, particularly for question 4: [An introducton to ammonites](http://www.ukfossils.co.uk/guides/ammonites.html) and [Shell Anatomy and Diversity](https://en.wikipedia.org/wiki/Ammonoidea#Shell_anatomy_and_diversity).

#### Part I Questions

1. Your identifications (how many species do you recognize in the group, and which specimens belong to which species). Explain how and why you came to this conclusion.

2. The morphological features you used to distinguish each species, including whatever combination of qualitative or quantitative traits you think are important.
 
3. The nature of ontogenetic change, if any, in the species. Explain your reasoning.

4. The possibility of sexual dimorphism as a cause of morphological differences and how you evaluated that possibility.

## Part II

We are going to perform a simple landmark analysis in R on pre-measured landmark data.

#### Step 1

We will use the `geomorph` package again, so load it using `library(geomorph)`

#### Step 2

The geomorph package comes with several pre-loaded example datasets. We will use two in this activity: `hummingbirds` and `plethodon`. Plethodons are a type of salamander. Load the data using:

````R
> data(hummingbirds)
> data(plethodon)
````

You should be able to tell from the R Studio global environment window that `hummingbirds` and `plethodon` are both lists. If you click on either in the global enivironment window, it will open the dataset in the upper left corner of the screen. If you want to view an individual object in either list, you can use the `$` syntax that we learned in Lab 05:

````R
> hummingbirds$curvepts
      before slide after
 [1,]      1    11    12
 [2,]     11    12    13
 [3,]     13    14    15
 [4,]      7    15    14
 [5,]     12    13    14
 [6,]      1    16    17
 [7,]     16    17    18
 [8,]     17    18    19
 [9,]     18    19    20
[10,]     10    20    19
[11,]      2    21    22
[12,]     21    22    23
[13,]     22    23    24
[14,]     23    24    25
[15,]      8    25    24
>
> plethodon$species
 [1] Jord  Jord  Jord  Jord  Jord  Jord  Jord  Jord  Jord  Jord  Teyah Teyah Teyah
[14] Teyah Teyah Teyah Teyah Teyah Teyah Teyah Jord  Jord  Jord  Jord  Jord  Jord 
[27] Jord  Jord  Jord  Jord  Teyah Teyah Teyah Teyah Teyah Teyah Teyah Teyah Teyah
[40] Teyah
Levels: Jord Teyah
````

#### Part II Questions - Subsection 1

1. Each element of the ````plethodon```` list has a name. What are they?

2. What **class** is each object?

3. Whare are the dimensions of the first object in the plethodon list?

Based on the name, class, and structure of the first object in the ````plethodon```` list, we can surmise that it is a 3-dimensional array of morphometric landmark data. The first column represents x coordinates, the second column records y coordinates, and the third dimension represents different specimens. 

This is the dataset we need to conduct a landmark analysis.

#### Step 3

The first thing we need to do in a landmark analysis is a **procrustes** mathematical transformation. It takes its name from the ancient greek legend of Procrustes (Προκρούστης). Procrustes was a serial killer who would invite unsuspecting travellers to stay the night at his inn. If the travellers were too short for the bed, Procrustes would cruelly stretch out the travellers' bodies to fit the bed. If they were too tall for the bed, Procrustes would amputate their limbs until they fit.

<a href="url"><img src="https://github.com/aazaff/teachPaleobiology/blob/master/Lab2Figures/Procrustes.jpg" align="center" height="300" width="400" ></a>

Remember that landmark analysis is concerned with **shape** and not **size**. We therefore apply that the procrustes transformation to shrink or enlargen the data (specifically known as scaling) in such a way that we elminate size as a factor between specimens, but still maintain appropriate information about shape - i.e., the *relative* distance of the landmarks.

The mathematics behind this is fairly complex, but luckily there is a funciton in the geomorph packages that will do this for us. Let's perform a procrustes transformation on the````plethodon```` landmarks using the ````gpagen( )````  function.

````R
> ProcrustesPlethodon<-gpagen(plethodon$land)
````

#### Step 4

The next step we need to do is a **principle components analysis** (PCA) on the transformed data. PCA is one of the most common forms of **multivariate**. A large part of this class will be about various forms of multivariate analyses. We will go more into the underlying theory of multivariate analyses - in general - in later labs. [For now, we'll just explain the basic idea of PCA](https://github.com/aazaff/teachPaleobiology/blob/master/PCATutorial.md).

Because it can be used for a variety of purposes, not just morphometrics, there are several packages and functions for running a PCA in R. However, we will stick with the function built into the geomorph package, ````plotTangentSpace( )````.

````R
# plotTangentSpace both runs the PCA & plots it simultaneously. Other packages
# that we will use later in the semester will not do both of these things simulateneously
# so don't get confused. We are also going to turn of the warpgrids and verbose features,
# because we do not need them for our puproses.

> plotTangentSpace(ProcrustesPlethodon$coords, warpgrids = FALSE, verbose = FALSE)
````

<a href="url"><img src="https://github.com/aazaff/teachPaleobiology/blob/master/Lab2Figures/Plethodon.png" align="center" height="500" width="500" ></a>

#### Step 5

Now that you have plotted the PCA, it should be obvious that the specimens cluster into three distinct morphologic groups based on their morphology - presumably this means that there are three different species in the data set. Though, remember, there can be other sources of morphologic variation.

#### Part II Questions - Subsection 2

1. Use the hummingbird dataset. Which object in the list records the landmark data?

2. Perform a procrustes on the landmark data.

3. Perform a PCA on the hummingbird data.

4. How many "species" of hummingbird are there?

## Part III

Examine the following cladogram and answer the questions.

<a href="url"><img src="https://github.com/aazaff/teachPaleobiology/blob/master/Lab2Figures/Figure3.png" align="center" height="300" width="650" ></a>

#### Part III Questions

1.	What is the synapomorphy of the clade containing species D and E?

2.	What is a plesiomorphic character of that clade?

3.	What is the synapomorphy of the clade containing species A and B?

4.	Which taxa have a sulfurous odor?

5.	What character distinguishes species D from species E?

6.	Are adorable eyelashes a synapomorphy or an autapomorphy? 

7.	Traditionally, the five taxa are grouped into three families. Determine if each family is monophyletic, paraphyletic, or polyphyletic.

Family | Species
--------- | ----------
Family 1 | Contains species A. 
Family 2 | Contains species B and C.
Family 3 | contains species D and E. 

8.	More recently, species A has been grouped in a family with species D and E. Is this advisable? Why or why not?

9.	Determine if the following groups are monophyletic, paraphyletic, or polyphyletic. 

Group | Species
--------- | ----------
Group 1 | Species A, B, C. 
Group 2 | Species C, D, E. 
Group 3 | Species C and D. 
Group 4 | Species A and B. 
Group 5 | Species B, D, E.
