# Modeling Ecological Gradients, Pt. 2

This lab picks up where Lab 8A stopped, so you must have completed that lab before starting this one.

## Correspondence Analysis

Correspondence analysis is a type of reciprocal averaging. There are three primary varieties of correspondence analysis: correspondence analysis, detrended correspondence analysis, and canonical (constrained) corresponded analysis. Here we will examine the difference between correspondence analysis and detrended correspondence analysis.

**STEP 1: LOADING AND CLEANING DATA**

Rename your `PresencePBDB` community matrix as `PostCambrian`

**STEP 2: INITIAL CORRESPONDENCE ANALYSIS**

Use the following code to perform and plot a basic correspondence analysis on `PostCambrian`. Note that this requires you to use the `vegan` package, so make sure that you have that loaded before you begin.

````R
# Run a correspondence analysis using the decorana( ) function of vegan
# ira = 1 tells it to run a basic correspondence analysis rather than detrended correspondence analysis
PostCambrianCA<-vegan::decorana(PostCambrian,ira=1)

# Plot the inferred samples (sites).
# If you want to see the taxa, use display="species"
plot(PostCambrianCA,display="sites")
````

Your final product should look like this:

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
PostCambrianSpecies<-scores(PostCambrianCA,display="species")

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
PostCambrianSamples<-scores(PostCambrianCA,display="sites")

# Now that we know the [x,y] values of each point, we can plot them.
plot(x=PostCambrianSamples[,"RA1"],y=PostCambrianSamples[,"RA2"])
````

It certainly works, but it is a lot uglier than what the `plot.decorana` method came up with. Here are a few things that we could do to improve the plot:

````R
plot(x=PostCambrianSamples[,"RA1"],y=PostCambrianSamples[,"RA2"],pch=16,las=1,xlab="Gradient Axis 1",ylab="Gradient Axis 2",cex=2)
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
PostCambrianDCA<-vegan::decorana(PostCambrian,ira=0)

# Plot the DCA
plot(PostCambrianDCA,display="sites")
````

Your final product should look like this:

You will notice that the arch effect is gone! This is good, but the DCA is suffering from a new problem known as the **Wedge** effect. You can envision the wedge effect by taking a piece of paper and twisting it, such that axis 1 is preserved reasonably undistorted, but the second axis of variation is expressed on DCA axis 2 on one end and on DCA axis 3 at the opposite end. This produces a pattern consisting of a tapering of sample points in axis 1-2 space and an opposing wedge in axis 1-3 space.

Here, let's contrast our above DCA plot by plotting DCA Axis 1 and DCA Axis 3. Do you see another wedge running in the opposite direction?

````R
# Use the choices= argument to pick which ordination axes you want plotted.
plot(PostCambrianDCA,display="sites",choices=c(1,3))
````

Your new plot should look like this:

## Multi-dimensional Scaling

Because correspondence analysis suffers from the arch and detrended correspondence analysis can suffer from the wedge, many ecologists favour a completely different technique known as non-metric multi-dimensional scaling (NMDS). The underlying theory behind NMDS is quite different from correspondence analysis. If you want to learn more there is a good introduction by [Steven M. Holland](http://strata.uga.edu/software/pdf/mdsTutorial.pdf).

However, if you just want the highlights, here is what you need to know.

+ Most ordination methods result in a single unique solution. In contrast, NMDS is a brute force technique that iteratively seeks a solution via repeated trial and error, which means running the NMDS again will likely result in a somewhat different ordination.
+ NMDS, unlike correspondence analysis, is not based on weighted-averaging, but on the ecological distances (e.g., jaccard) of samples, similar to polar ordination.
+ The species scores presented by NMDS are just the weighted-average of the final NMDS sample scores.

Here is how you run an NMDS in R using the `vegan` package.

````R
PostCambrianNMDS<-metaMDS(PostCambrian)

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

It's the Arch Effect again, just like in correspondence analysis!!! Despite the fact that many ecologists like to praise NMDS over CA and DCA, the differences between them are exaggerated. The reason for this is that all three are unreliable when there isn't a strong gradient to pick up in the first place.

In these data, there is a very strong "time" gradient that is picked up in the first axis of all four ordination (polar, corresondence, detrended, and multidimensional) techniques that we've used so far. However, there is no secondary gradient (or tertiary etc.) that is obvious in these data, hence why all of the methods make up these (ecologically) meaningless wedges and arches along the second, third, and so on axes.

Of the ordination methods covered here, I recommend sticking with DCA. It is fairly robust and easy to interpret, though *watch out for those wedges*!

## Problem Set

1. Download a dataset from the Paleobiology Database that includes all Ordovician-age animals (i.e., *Animalia*). Name the object `Ordovician`. What code did you use?

2. Clean up the poorly resolved genus names. What function and/or code did you use?

3. Turn you object `Ordovician` into a **community matrix** of the samples by genera, where the samples are different `geoplate` codes. Geoplate codes denote different paleocontinents, so by doing this, your community matrix will list which genera were present on which paleocontinents during the Ordovician. Cull this matrix so that each sample has a minimum of 25 taxa and each taxon occurs in at least two samples. Show your code.

4. Preform a DCA on your new community matrix. Analyze your DCA with a plot. Do you think that the orientation of samples along either axis 1 or axis 2 is related to the average latitude or longitude of each plate in question? Explain how you determined your answer, and shwo your code. *Hint: information about the paleolatitude and paleolongitude of different geoplates is included in the data you originally downloaded (i.e., the object `Ordovician`)*
