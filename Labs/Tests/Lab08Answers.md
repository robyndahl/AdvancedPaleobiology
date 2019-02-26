# Modeling Ecological Gradients Using Multivariate Data

## Problem Set 1

1. How many unique genera are in the Miocene, Early Jurassic, Late Cretaceous, and Pennsylvanian epochs (not total, each)? What code did you use to find out?

You could write a function to determine the number of taxa in each epoch:

````R
> TaxaPerEpoch <- function(PresencePBDB) {
+     BlankList <- apply(PresencePBDB,1,sum)
+     return(as.matrix(BlankList))
+ }
> TaxaPerEpoch(PresencePBDB)
                  [,1]
Pennsylvanian      121
Early Ordovician    26
Middle Ordovician   38
Late Ordovician     67
Llandovery          78
Wenlock             79
Ludlow              92
Pridoli             73
Mississippian      133
Early Devonian     111
Middle Devonian    118
Late Devonian       83
Cisuralian         104
Late Jurassic      196
Eocene             654
Late Cretaceous    396
Early Cretaceous   291
Middle Jurassic    185
Paleocene          504
Oligocene          567
Miocene            653
Early Jurassic     177
Pleistocene        571
Guadalupian        143
Middle Triassic    124
Late Triassic      182
Pliocene           615
Early Triassic      72
Lopingian          107
````

Or, you could ask for the specific epochs of interest individually:

````R
> length(which(PresencePBDB["Miocene",] == 1)
[1] 653
> length(which(PresencePBDB["Early Jurassic",] == 1)
[1] 177
> length(which(PresencePBDB["Late Cretaceous",] == 1)
[1] 396
> length(which(PresencePBDB["Pennsylvanian",] == 1)
[1] 121
````

2. How many geologic epochs in general are in this dataset? What code did you use to find out?

There are 29 epochs

````R
> # Check the dimensions of the community matrix
> # The first number represents rows
> dim(PresencePBDB)
[1]  29 942
````

3. Which epochs contain specimens of the genus *Mytilus*? What code did you use to find out?

````R
> # Basic approach
> which(PresencePBDB[,"Mytilus"] == 1)
         Pridoli   Early Devonian    Late Jurassic           Eocene  Late Cretaceous Early Cretaceous 
               8               10               14               15               16               17 
 Middle Jurassic        Paleocene        Oligocene          Miocene   Early Jurassic      Pleistocene 
              18               19               20               21               22               23 
 Middle Triassic    Late Triassic         Pliocene   Early Triassic 
              25               26               27               28
              
> # A more complex approach
> MytilusEpochs <- function(PresencePBDB) {
+  AnswerMatrix <- as.matrix(which(PresencePBDB[,"Mytilus"] == 1))
+  return(AnswerMatrix)
+ }
> MytilusEpochs(PresencePBDB)
                 [,1]
Pridoli             8
Early Devonian     10
Late Jurassic      14
Eocene             15
Late Cretaceous    16
Early Cretaceous   17
Middle Jurassic    18
Paleocene          19
Oligocene          20
Miocene            21
Early Jurassic     22
Pleistocene        23
Middle Triassic    25
Late Triassic      26
Pliocene           27
Early Triassic     28
````

4. Look at the epochs in the geologic timescale. Using your answer from Q3, in which epochs can we infer that *Mytilus* was present, even though we have no record of them in the PBDB? How did you deduce this?

*Mytilus* first appearance is in the Pridoli and last appearance is in the Pleistocene, so we can assume that it has been present through that entire time slice, even though there is no record from the Middle Devonian, Late Devonian, Mississippian, Pennsylvanian, Cisuralian, Guadalupian, or Lopingian.

## Problem Set II

1. Using your own R code, find the Jaccard similarity of the Pleistocene and Miocene "samples" in your `PresencePBDB` matrix. It is possible to code this entirely using only functions discussed in the R Tutorial.

````R
# Find the number of taxa present only in the Miocene
> MioceneOnly <- length(which(PresencePBDB["Miocene",] == 1 & PresencePBDB["Pleistocene",] == 0))
> MioceneOnly
[1] 96

# Then find the number of taxa present only in the Pleistocene
> PleistoceneOnly <- length(which(PresencePBDB["Pleistocene",] == 1 & PresencePBDB["Miocene",] == 0))
> PleistoceneOnly
[1] 14

# Then find the number of taxa that are present in both epochs
> SharedTaxa <- length(which(PresencePBDB["Pleistocene",] == 1 & PresencePBDB["Miocene",] == 1))
> SharedTaxa
[1] 557

# Finally, conduct the Jaccard similarity test
> Jaccard <- SharedTaxa / (SharedTaxa + MioceneOnly + PleistoceneOnly)
> Jaccard
[1] 0.8350825
````

2. How would you convert your similarity index into distance?

````R
> Distance <- 1 - Jaccard
> Distance
[1] 0.1649175
````

3. Again, calculate the jaccard distance of the "Miocene" and "Pleistocene" samples of PresencePBDB, but this time use the `vegdist( )` function. This should be an identical answer to what you got in question 2. Hint: You will have to change one of the default settings of the function

````R
# Turn the info returned by vegdist( ) into a more useful object, a matrix
> Jaccard <- as.matrix(vegdist(PresencePBDB, method = "jaccard"))

# Find the location in the matrix at the intersection of the Miocene and Pleistocene
> Jaccard["Miocene","Pleistocene"]
[1] 0.1649175
````

4. Using the `vegdist( )` function, calculate the Jaccard distances for all of the following epochs in PresencePBDB: Pleistocene, Pliocene, Miocene, Oligocene, Eocene, Paleocene. What code did you use? Which two epochs are the most dissimilar?

First, I created a function that separated out the specific data that we want to use:

````R
> CenozoicData <- function(PresencePBDB) {
+     Paleocene <- PresencePBDB["Paleocene",]
+     Eocene <- PresencePBDB["Eocene",]
+     Oligocene <- PresencePBDB["Oligocene",]
+     Miocene <- PresencePBDB["Miocene",]
+     Pliocene <- PresencePBDB["Pliocene",]
+     Pleistocene <- PresencePBDB["Pleistocene",] 
+     NewFrame <- data.frame(Paleocene,Eocene,Oligocene,Miocene,Pliocene,Pleistocene)
+     return(t(NewFrame))
+ }
````

Then I used that function and `vegdist( )`:

````R
> Cenozoic <- CenozoicData(PresencePBDB)
> vegdist(Cenozoic)
             Paleocene     Eocene  Oligocene    Miocene   Pliocene
Eocene      0.19170984                                            
Oligocene   0.25490196 0.10237510                                 
Miocene     0.19101124 0.04667177 0.08852459                      
Pliocene    0.23324397 0.07643814 0.11167513 0.04731861           
Pleistocene 0.28000000 0.12000000 0.15992970 0.08986928 0.06913997
````
From that analysis, you can see that the Paleocene and the Pleistocene are the most dissimilar, because that comparison has the highest dissimilarity number (0.28)
