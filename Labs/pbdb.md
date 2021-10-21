# Modeling Ecological Gradients

Modeling ecological gradients using multivariate data analyses.

## Basic Concepts

Most traditional data analyses are **univariate** or **bivariate**. A univariate analysis assesses a single variable and a bivariate analysis compares two variables. For example, `hist( )` is a univariate analysis, a `t.test( )` is bivariate, and a scatter plot is bivariate.

Such analyses fall shore when you want to anlyze the relationships among many different variables, i.e, a **multivariate** dataset. In fact, there are a whole host of problems associated with applying multiple bivariate anlyses to a multivariate dataset. In other words, you can't just perform a bivariate analysis on every possible pair of combinations in a multivariate dataset.

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
DataPBDB <- velociraptr::downloadPBDB(Taxa=c("Bivalvia","Gastropoda"),StartInterval="Cambrian",StopInterval="Pleistocene")
````

Then, use `cleanTaxonomy( )` to remove any occurrences that are not properly resolved to the genus level.

````R
DataPBDB <- velociraptr::cleanTaxonomy(DataPBDB,"genus")
````

Next, use `downloadTime( )` to download a matrix of geologic epoch definitions and metadata. We will use this information to constain the ages of fossils from `DataPBDB`.

````R
Epochs <- velociraptr::downloadTime(Timescale="international epochs")
````

Finally, use `constrainAges( )` to remove any remaining fossils that are poorly constrained.

````R
DataPBDB <- velociraptr::constrainAges(DataPBDB,Epochs)
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
PresencePBDB <- velociraptr::presenceMatrix(DataPBDB,Rows="early_interval",Columns="genus")
````

Next, we need to use `cullMatrix` to clean up this new matri and remove depauperate samples and rare taxa. We will set it so that a sample needs at least 24 reported taxa for us to consider it reliable, and each taxon must occur in at least 5 samples. These are common minimums for sample sizes in ordination analysis.

````R
PresencePBDB <- velociraptr::cullMatrix(PresencePBDB,Rarity=5,Richness=24)
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
