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
