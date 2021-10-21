# Modeling Ecological Gradients

Modeling ecological gradients using multivariate data analyses.

## Basic Concepts

Most traditional data analyses are **univariate** or **bivariate**. A univariate analysis assesses a single variable and a bivariate analysis compares two variables. For example, `hist( )` is a univariate analysis, a `t.test( )` is bivariate, and a scatter plot is bivariate.

Such analyses fall short when you want to anlyze the relationships among many different variables, i.e, a **multivariate** dataset. In fact, there are a whole host of problems associated with applying multiple bivariate anlyses to a multivariate dataset. In other words, you can't just perform a bivariate analysis on every possible pair of combinations in a multivariate dataset.

One way to solve this issue to use **multivariate** analyses. In this lab, we are going to explore a broad class of multivariate analyses known as **ordinations**. The driving principle behind these methods is to "*reduce the dimensionality*" of a multivariate dataset. In other words, we will take a dataset with many variables and summaries their relationships with only a few of those variables.

## Our First Multivariate Dataset

Instead of using the Paleobiology Database API, we will use an R package called `velociraptr` that is designed to downloand and manipulate PBDB data. So first, we need to install this package:

**STEP 1**

````R
install.packages("velociraptr")
library(velociraptr)
````

**STEP 2**

Download a dataset of Phanerozoic bivalve and gastropod fossils using the `downloadPBDB( )` function. This data includes ALL clams and snails from the Cambrian through the Pleistocene, so it will likely take a few minutes to download.

````R
DataPBDB <- downloadPBDB(Taxa=c("Bivalvia","Gastropoda"),StartInterval="Cambrian",StopInterval="Pleistocene")
````

If your download keeps timing out, you can split your download into chunks and then recombine them using the following script:

````R
dataPaleozoic <- downloadPBDB(Taxa = c("Bivalvia","Gastropoda"),
                             StartInterval = "Cambrian",
                             StopInterval = "Permian")

dataMesozoic <- downloadPBDB(Taxa = c("Bivalvia","Gastropoda"),
                             StartInterval = "Triassic",
                             StopInterval = "Cretaceous")

dataCenozoic <- downloadPBDB(Taxa = c("Bivalvia","Gastropoda"),
                             StartInterval = "Paleogene",
                             StopInterval = "Pleistocene")

dataPBDB <- rbind(dataPaleozoic,dataMesozoic,dataCenozoic)
````

Then, use `cleanTaxonomy( )` to remove any occurrences that are not properly resolved to the genus level.

````R
DataPBDB <- cleanTaxonomy(DataPBDB,"genus")
````

Next, use `downloadTime( )` to download a matrix of geologic epoch definitions and metadata. We will use this information to constain the ages of fossils from `DataPBDB`.

````R
Epochs <- downloadTime(Timescale="international epochs")
````

Finally, use `constrainAges( )` to remove any remaining fossils that are poorly constrained.

````R
DataPBDB <- constrainAges(DataPBDB,Epochs)
````

**STEP 3**

Let's turn our newly downloaded and cleaned PBDB data into a community matrix. A community matrix is one of the most fundamental data formats in ecology. In such a matrix, the rows represent different samples, the columns represent different taxa, and the cell valuess represent the abundance of the species in that sample. If you would like to read more about the theory behind community matrices, this is a good paper to start with: [Novak et al. 2016](https://www.annualreviews.org/doi/10.1146/annurev-ecolsys-032416-010215)

Here are a few things to remember about community matrices.

1. Samples are sometimes called sites or quadrats, but those are sub-discipline specific terms that should be avoided. Stick with samples because it is universally applicable.
2. The columns do not have to be species per se. Columns could be other levels of the Linnean Hierarchy (e.g., genera, families) or some other ecological grouping (e.g., different habits, different morphologies).
3. Since there is no such thing as a negative abundance, there should be no negative data in a Community Matrix.
4. Sometimes we may not have abundance data, in which case we can substitute presence-absence data - i.e, is the taxon present or absent in the sample. This is usually represented with a 0 for absent and a 1 for present.

Let's convert our PBDB dataset into a presence-absence dataset using the `presenceMatrix( )` function fo the PBDB package. This function requires that you define which column will count as samples. For now, let's use "`early_interval`" (i.e., geologic age) as the separator. Because we are using a large dataset, this set might take a few minutes.

````R
PresencePBDB <- presenceMatrix(DataPBDB,Rows="early_interval",Columns="genus")
````

Next, we need to use `cullMatrix` to clean up this new matri and remove depauperate samples and rare taxa. We will set it so that a sample needs at least 24 reported taxa for us to consider it reliable, and each taxon must occur in at least 5 samples. These are common minimums for sample sizes in ordination analysis.

````R
PresencePBDB <- cullMatrix(PresencePBDB,Rarity=5,Richness=24)
````

**PROBLEM SET I**

1. How many genera are in the Miocene, Early Jurassic, Late Cretaceous, and Pennsylvanian epochs (not total, but in each epoch)? What code did you use to find out?

2. How many geologic epochs are included in this dataset? What code did you use to find out?

3. Which epochs contain specimens of the genus *Mytilus* (a type of mussel)? What code did you use to find out?

4. Look at the epochs in the [geologic timescale](https://en.wikipedia.org/wiki/Geologic_time_scale#Table_of_geologic_time). Using your answer to question 3, in which epochs can we infer that *Mytilus* was present, even though we have no record of them in the PBDB? How did you deduce this?

## Basic Similarity Indices

Many ordination techniques are based on the use of **similarity indices**. Such indices generally range from 0 to 1. In a **similarity index**, zero indicates complete dissimilarity and 1 indicates complete similarity. In a **dissimilarity** or **distance index**, zero indicates complete similarity and 1 indicates complete dissimilarity.

**PROBLEM SET II**

The **Jaccard Index** is the simiplest similarity index. It is the intersection of two samples divided by the union of two samples. In other words, the number of genera shared between two samples, divided by the total number of (unique) genera in both samples. Or, put an even simpler way, it is the percentage of genera shared between two samples.

1. Using your own custom R code, find the Jaccard similarity of the Pleistocene and Miocene "samples" in your `PresencePBDB` matrix. It is possible to code this entirely using only functions discussed in the R Tutorial.

2. How can you convert your similarity index to a distance?

3. Install and load the `vegan` package. Read the help file for the `vegdist( )` function, then use `vegdist( )` to calculate the Jaccard distance of the Miocene and Pleistocene samples from the `PresencePBDB` matrix. Your answer should be identical to your answer to Question 2.

4. Using the `vegdist( )` function, calculate the Jaccard distances for all of the following epochs in `PresencePBDB`: Pleistocene, Pliocene, Miocene, Oligocene, Eocene, Paleocene. What code did you use? Which two epochs are the most dissimilar?

## Correspondence Analysis

Correspondence analysis is a type of reciprocal averaging. There are three primary varieties of correspondence analysis: correspondence analysis, detrended correspondence analysis, and canonical (constrained) corresponded analysis. Here we will examine the difference between correspondence analysis and detrended correspondence analysis.

**STEP 1: LOADING AND CLEANING DATA**

Rename your `PresencePBDB` community matrix as `PostCambrian`

**STEP 2: INITIAL CORRESPONDENCE ANALYSIS**

Use the following code to perform and plot a basic correspondence analysis on `PostCambrian`. Note that this requires you to use the `vegan` package, so make sure that you have that loaded before you begin.

````R
# Run a correspondence analysis using the decorana( ) function of vegan
# ira = 1 tells it to run a basic correspondence analysis rather than detrended correspondence analysis
PostCambrianCA <- decorana(PostCambrian,ira=1)

# Plot the inferred samples (sites).
# If you want to see the taxa, use display="species"
plot(PostCambrianCA,display="sites")
````

Your final product should look like this:

![Lab 8 Fig 1](/Images/Lab08_Fig1.png)

**STEP 3: ADVANCED PLOTTING**

You may also wish to make a more complex/detailed plot. R has a dizzying variety of plotting functions, but in this lab we will only need two of the basic functions.

The first function is `plot( )`, which you have used earlier in this lab and elsewhere in the course. It is a very versatile and easy to use function because it has many **methods** attached to it. Putting the technical definition of a method aside, a method is a set of pre-programmed settings or templates, that will make a different type of plot depending on what kind of data you give the function.

Check out all the different methods of plotting using:

````R
> methods(plot)
````

The **generic** version of plot (i.e., the version of plot that does not use any specified methods) produces a basic ***scatter plot***. The ordination plots we used above an example of a scatter plot. What if we wanted more customization than the basic `plot.decorana` method/template given to us?

````R
# Use the scores( ) function if you want to see/use the numerical values of the inferred gradient scores
# Note that by scores we mean gradient values - e.g., a temperature of 5 degrees or a depth of 10m
PostCambrianSpecies <- scores(PostCambrianCA,display="species")

# This shows the weighted average of all species abundances along each inferred gradient axis.
# i.e., The weight-average of Amphiscapha is 5.22 along axis 1, and -3.799 along axis 2.
head(PostCambrianSpecies)
                    RA1       RA2       RA3        RA4
Amphiscapha    5.221689 -3.799051 -2.998689 1.27944155
Donaldina      5.460634 -1.936823 -1.439545 0.77634443
Euphemites     5.472197 -2.781106 -2.433943 0.04989632
Glabrocingulum 4.270209 -3.789545 -1.794858 1.37530975
Palaeostylus   5.322415 -3.151507 -2.536464 0.60251234
Meekospira     5.322415 -3.151507 -2.536464 0.60251234

# You can also do this for sample ("sites") scores as well.
PostCambrianSamples <- scores(PostCambrianCA,display="sites")

# Now that we know the [x,y] values of each point, we can plot them.
plot(x=PostCambrianSamples[,"RA1"],y=PostCambrianSamples[,"RA2"])
````

![Lab 8 Fig 2](/Images/Lab08_Fig2.png)

It certainly works, but it is a lot uglier than what the `plot.decorana` method came up with. Here are a few things that we could do to improve the plot:

````R
plot(x=PostCambrianSamples[,"RA1"],y=PostCambrianSamples[,"RA2"],
  pch=16,las=1,xlab="Gradient Axis 1",ylab="Gradient Axis 2",cex=2)
````

Plotting Arguments | Description
----- | -----
````pch=```` | Dictates the symbol used for the points - e.g., ````pch = 16```` gives a solid circle, ````pch = 15```` gives a solid square. Try out different numbers.
````cex=```` | Dictates the size of the points, the larger cex value the larger the points. Try out different values.
````xlab=```` | Dictates the x-axis label, takes a **character** string.
````ylab=```` | Dictates the y-axis label, takes a **character** string.
````las=```` | Rotates the y-axis or x-axis tick marks. Play around with it.
````col=```` | The col function sets the color of the points. This will take either a [hexcode](http://www.color-hex.com/) as a character string, ````col="#010101"```` or a [named color](http://www.stat.columbia.edu/~tzheng/files/Rcolor.pdf) in R, ````col="red"````

There are a great many other arguments that can be given to the ````plot( )```` function, but we will discuss those as we need them.

Although this plot is prettier than what we had before, it would probably be better to have text names for the points, stating which point is which epoch (i.e., like in the ````plot.decorana```` method). To do this we can use the ````text( )```` function. Importantly, you cannot use ````text( )```` without making a ````plot( )```` first.

````R
# Create plot empty of points (but scaled to the data) by adding the type="n" argument.
plot(x=PostCambrianSamples[,"RA1"],y=PostCambrianSamples[,"RA2"],pch=16,las=1,xlab="Gradient Axis 1",ylab="Gradient Axis 2",type="n")

# The text( ) function takes [x,y] coordinates just like plot
# You also give it a labels= defintion, which states what text you want shown at those coordinates
# In this case, the rownames of the PostCambrianSamples matrix - i.e., the epoch names
text(x=PostCambrianSamples[,"RA1"],y=PostCambrianSamples[,"RA2"],labels=dimnames(PostCambrianSamples)[[1]])

# You can also give the text function many of the arguments that you give to plot( )
# Like cex= to increase the size or col= to change the color
text(x=PostCambrianSamples[,"RA1"],y=PostCambrianSamples[,"RA2"],labels=dimnames(PostCambrianSamples)[[1]],cex=1.5,col="dodgerblue3")
````

Coloring is a very powerful visual aid. We might want to subset the data into different groupings, and color those grouping differently to look for clusters in the ordination space. Let's try splitting the dataset into three parts: Cenozoic Epochs (1-66 mys ago), Mesozoic Epochs (66-252 mys), and the Paleozoic (252-541 mys), and plot each with a different color.

````R
# Create plot empty of points (but scaled to the data) by adding the type="n" argument.
plot(x=PostCambrianSamples[,"RA1"],y=PostCambrianSamples[,"RA2"],pch=16,las=1,xlab="Gradient Axis 1",ylab="Gradient Axis 2",type="n")

# Separate out the epochs
Cenozoic<-PostCambrianSamples[c("Pleistocene","Pliocene","Miocene","Oligocene","Eocene","Paleocene"),]
Mesozoic<-PostCambrianSamples[c("Late Cretaceous","Early Cretaceous","Late Jurassic","Early Jurassic","Late Triassic","Middle Triassic","Early Triassic"),]
Paleozoic<-PostCambrianSamples[c("Lopingian","Guadalupian","Cisuralian","Pennsylvanian","Mississippian","Late Devonian","Middle Devonian","Early Devonian","Pridoli","Ludlow","Wenlock","Llandovery","Late Ordovician","Middle Ordovician","Early Ordovician"),]

# Plot Cenozoic in gold
text(x=Cenozoic[,"RA1"],y=Cenozoic[,"RA2"],labels=dimnames(Cenozoic)[[1]],col="gold")
# Plot Mesozoic in blue
text(x=Mesozoic[,"RA1"],y=Mesozoic[,"RA2"],labels=dimnames(Mesozoic)[[1]],col="blue")
# Plot Paleozoic in dark green
text(x=Paleozoic[,"RA1"],y=Paleozoic[,"RA2"],labels=dimnames(Paleozoic)[[1]],col="darkgreen")
````
Your plot should look like this:

![Lab 8 Fig 3](/Images/Lab08_Fig3.png)

**STEP 4: ANALYSIS**

There are a few things you should notice about the above graph. First, the first axis (i.e, the horizontal axis, the x-axis) of the ordination has ordered the samples in terms of their age. On the far right of the x-axis is the Ordovician (the oldest epoch) and on the far left is the Pleistocene (the youngest epoch). Therefore, we can infer that *time* is the primary gradient.

However, you may have also noticed two other patterns in the data. 

First, when the second axis (i.e., the vertical axis, the y-axis) is taken into account, an interesting **Arch** shape is apparent. This arch does not represent a true ecological phenomenon, per se, but is actually a mathematical artefact of the correspondence analysis method. Brocard et al. (2013) describe the formation of the arch thusly,

> Long environmental gradients often support a succession of species. [Since species tend to have unimodal distributions along gradients], a long gradient may encompass sites that, at both ends of the gradient, have no species in common; thus, their distance reaches a maximum value (or their simi-larity is 0). But if one looks at either side of the succession, contiguous sites continue to grow more different from each other. Therefore, instead of a linear trend, the gradient is represented on a pair of CA axes as an arch.

In other words, the Pleistocene and Early Ordovician have no species in common, thus they are on opposite ends of the first axis. However, they do share something in common, which is that they become progressively dissimilar from epochs further away from them. For this reason, the correspondence analysis plots them on the same end of the second axis, and the epochs that are at the midpoint between them (i.e., the Permo-Trissic boundary) on the other end of the second axis. This is not helpful information (in fact, it is just a geometrically warped restatement of the information conveyed in the first axis) and we want to eliminate it for something more useful.

The other thing you may have noticed is **compression** towards the ends of the gradient. Meaning that most of the Cenozoic epochs (i.e., Pleistocene, Pliocene, Miocene, Oligocene, and Eocene) are overlain closely on top of each other. So much so that you probably have a hard time reading their text. This is also an artefact of correspondence analysis, and not necessarily an indication that those epochs are more similar to each other, than say the (more legible) Early and Late Cretaceous epochs.

For these reasons, it is rare for people to still use correspondence analysis (reciprocal averaging). Though some scientists argue you should at least try correspondence analysis before turning to another technique.

## Detrended Correspondence Analysis

The Arch effect and compression are both ameliorated by detrended correspondence analysis (DCA). DCA divides the first axis into a number of smaller segments. Within each segment it recalculates the second axis scores such that they have an average of zero.

You can perform a DCA in R using the `decorana( )` function of the `vegan` package.

````R
# Peform a DCA on the Post Cambrian Dataset
# ira = 0 is the default, so you do not need to put that part in.
PostCambrianDCA <- decorana(PostCambrian,ira=0)

# Plot the DCA
plot(PostCambrianDCA,display="sites")
````

Your plot should look like this:

![Lab 8 Fig 4](/Images/Lab08_Fig4.png)

You will notice that the arch effect is gone! This is good, but the DCA is suffering from a new problem known as the **Wedge** effect. You can envision the wedge effect by taking a piece of paper and twisting it, such that axis 1 is preserved reasonably undistorted, but the second axis of variation is expressed on DCA axis 2 on one end and on DCA axis 3 at the opposite end. This produces a pattern consisting of a tapering of sample points in axis 1-2 space and an opposing wedge in axis 1-3 space.

Here, let's contrast our above DCA plot by plotting DCA Axis 1 and DCA Axis 3. Do you see another wedge running in the opposite direction?

````R
# Use the choices= argument to pick which ordination axes you want plotted.
plot(PostCambrianDCA,display="sites",choices=c(1,3))
````

Your new plot should look like this:

![Lab 8 Fig 5](/Images/Lab08_Fig5.png)

## Multi-dimensional Scaling

Because correspondence analysis suffers from the arch and detrended correspondence analysis can suffer from the wedge, many ecologists favour a completely different technique known as non-metric multi-dimensional scaling (NMDS). The underlying theory behind NMDS is quite different from correspondence analysis. If you want to learn more there is a good introduction by [Steven M. Holland](http://strata.uga.edu/software/pdf/mdsTutorial.pdf).

However, if you just want the highlights, here is what you need to know.

+ Most ordination methods result in a single unique solution. In contrast, NMDS is a brute force technique that iteratively seeks a solution via repeated trial and error, which means running the NMDS again will likely result in a somewhat different ordination.
+ NMDS, unlike correspondence analysis, is not based on weighted-averaging, but on the ecological distances (e.g., jaccard) of samples, similar to polar ordination.
+ The species scores presented by NMDS are just the weighted-average of the final NMDS sample scores.

Here is how you run an NMDS in R using the `vegan` package.

````R
PostCambrianNMDS <- metaMDS(PostCambrian)

# The plotting defaults for metaMDS output is not as good as for decorana( )
# We have to do some graphical fiddling.

# Create a blank plot by setting type= to "n" (for nothing)
plot(PostCambrianNMDS,display="sites",type="n")

# Use the text( ) function, to fill in our blank plot with the names of the samples
# Importantly, text( ) will not work if there is not already an open plot,
# hence why we needed to make the blank plot first. Notice that we did not define the coordinates for
# text( ). This is because by giving the text( ) function an NMDS output, 
# it knows to use the text.metaMDS method/template.
text(PostCambrianNMDS,display="sites")
````

Your plot should look like this:

![Lab 8 Fig 6](/Images/Lab08_Fig6.png)

It's the Arch Effect again, just like in correspondence analysis!!! Despite the fact that many ecologists like to praise NMDS over CA and DCA, the differences between them are exaggerated. The reason for this is that all three are unreliable when there isn't a strong gradient to pick up in the first place.

In these data, there is a very strong "time" gradient that is picked up in the first axis of all four ordination (polar, corresondence, detrended, and multidimensional) techniques that we've used so far. However, there is no secondary gradient (or tertiary etc.) that is obvious in these data, hence why all of the methods make up these (ecologically) meaningless wedges and arches along the second, third, and so on axes.

Of the ordination methods covered here, I recommend sticking with DCA. It is fairly robust and easy to interpret, though *watch out for those wedges*!

## Problem Set

1. Download a dataset from the Paleobiology Database that includes all Ordovician-age animals (i.e., *Animalia*). Name the object `Ordovician`. What code did you use?

2. Clean up the poorly resolved genus names. What function and/or code did you use?

3. Turn you object `Ordovician` into a **community matrix** of the samples by genera, where the samples are different `geoplate` codes. Geoplate codes denote different paleocontinents, so by doing this, your community matrix will list which genera were present on which paleocontinents during the Ordovician. Cull this matrix so that each sample has a minimum of 25 taxa and each taxon occurs in at least two samples. Show your code.

4. Preform a DCA on your new community matrix. Analyze your DCA with a plot. Do you think that the orientation of samples along either axis 1 or axis 2 is related to the average latitude or longitude of each plate in question? Explain how you determined your answer, and shwo your code. *Hint: information about the paleolatitude and paleolongitude of different geoplates is included in the data you originally downloaded (i.e., the object `Ordovician`)*
