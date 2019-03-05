## Problem Set III (from Lab 8B)

1. Download a dataset from the Paleobiology Database that includes all Ordovician-age animals (i.e., *Animalia*). Name the object `Ordovician`. What code did you use?

Use the `downloadPBDB` function in the `velociraptr` package:

````R
Ordovician <- velociraptr::downloadPBDB(Taxa = "Animalia", StartInterval = "Ordovician", StopInterval = "Ordovician")
````

2. Clean up the poorly resolved genus names. What function and/or code did you use?

Use the `cleanTaxonomy` function in the `velociraptr` package:

````R
Ordovician <- velociraptr::cleanTaxonomy(Ordovician,"genus")
````

3. Turn you object `Ordovician` into a **community matrix** of the samples by genera, where the samples are different `geoplate` codes. Geoplate codes denote different paleocontinents, so by doing this, your community matrix will list which genera were present on which paleocontinents during the Ordovician. Cull this matrix so that each sample has a minimum of 25 taxa and each taxon occurs in at least two samples. Show your code.

Use the `presenceMatrix` function in the `velociraptr` package:

````R
Ordovician <- velociraptr::presenceMatrix(Ordovician, Rows = "geoplate", Columns = "genus")
````

Then use the `cullMatrix` function:

````R
Ordovician <- velociraptr::cullMatrix(Ordovician, Rarity = 2, Richness = 25)
````

4. Preform a DCA on your new community matrix. Analyze your DCA with a plot. Do you think that the orientation of samples along either axis 1 or axis 2 is related to the average latitude or longitude of each plate in question? Explain how you determined your answer, and shwo your code. *Hint: information about the paleolatitude and paleolongitude of different geoplates is included in the data you originally downloaded (i.e., the object `Ordovician`)*

````R
> OrdovicianDCA <- vegan::decorana(Ordovician)
> plot(OrdovicianDCA, display = "sites")
> OrdovicianDCA

Call:
vegan::decorana(veg = Ordovician) 

Detrended correspondence analysis with 26 segments.
Rescaling of axes with 4 iterations.

                  DCA1   DCA2   DCA3   DCA4
Eigenvalues     0.3446 0.2422 0.1967 0.1948
Decorana values 0.3501 0.2467 0.1852 0.1502
Axis lengths    4.0836 5.8066 2.8025 3.8765
````
