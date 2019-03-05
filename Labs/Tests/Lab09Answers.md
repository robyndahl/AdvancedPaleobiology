# Calculating Stratigraphic Ranges

## Problem Set 1

1. What do the `max_ma` and `min_ma` columns of `DataPBDB` represent? If you do not intuitively know, you can always check the Paleobiology Database API documentation.

`max_ma` defines the oldest time boundary for data included in the dataset. In other words, taxa are not inlcuded if they are older than this number (given in millions of years).

`min_ma` defines the youngest time boundary for data included in the dataset. In other words, taxa are not included if they are younger than this number (given in millions of years).

2. What is oldest age of each genus? (Hint: Use the tapply( ) and max( ) functions we've used in previous labs.) Show the code you would use to find out.

````R
> tapply(DataPBDB$max_ma, DataPBDB$genus, max)
````

3. What is the youngest age of each genus? (Hint: Use the tapply( ) and min( ) functions we've used in previous labs.) Show the code you would use to find out.

````R
> tapply(DataPBDB$max_ma, DataPBDB$genus, min)
````

4. Find which genus has the most occurrences in the dataset (Hint: Use the table( ) function!). What code did you use?

This is one, two-step way to answer this question. You could also write a function that combines the steps.

````R
> max(table(DataPBDB$genus))
[1] 2103
> which(table(DataPBDB$genus) == 2103)
Anadara 
     44
````

5. What is the stratigraphic range of this taxon (i.e., your answer to question 4). Show your code.

````R
# First, separate out the data for genus Anadara:
> Anadara <- DataPBDB[which(DataPBDB$genus == "Anadara"),]

# Then, determine the maximum and minimum age occurrences
> max(Anadara$max_ma)
[1] 66
> min(Anadara$min_ma)
[1] 0
````
The data show that Anadara occurrences span the entire Cenozoic (66 Ma - 0 Ma).

## Problem Set 2

6. Qualitatively describe what is happening in the following lines of code. A good answer should identify what the different arguments are for each function, and what they are used for.

````R
> PaleoLng <- na.omit(Lucina$paleolng)
> mean(sample(PaleoLng, length(PaleoLng), replace=TRUE))
````

7. Plot a [kernel density](https://github.com/aazaff/startLearn.R/blob/master/expertConcepts.md#describing-distributions-with-statistics) graph of `ResampledMeans`. Show your code. Does the distribution look approximately Gaussian? Explain why you think it does or does not.

![Lab09_Fig1](https://github.com/robyndahl/AdvPaleoQuizzes/blob/master/Lab09_Fig1.png)

Yes, the plot does look Gaussian, because it forms a bell curve. The highest density is around the mean and it drops off as you move away from the mean.

8. Find the mean of `ResampledMeans`, is it similar to the mean of the original data?

````R
> mean(ResampledMeans)
[1] 24.27991
````
It was very close. The original mean was 24.30135, only slightly higher than the mean of means.

9. Sort `ResampledMeans` from lowest to highest. [Hint: We learned how to sort a vector in [Lab 6](/LabExercise6.md#problem-set-2)].

````R
> OrderedMeans <- sort(ResampledMeans)
````

10. Now that you have sorted `ResampledMeans`, what is the 2.5th percentile of ResampledMeans and what is the 97.5th percentile of Resampled means. If you do not know what a percentile is, or how to calculate it, you can use google. Show your code.

````R
> quantile(OrderedMeans, c(.025, .975))
    2.5%    97.5% 
21.71473 26.69090
````

11. Incidentally, these numbers (your answer to question 5) are the lower and upper confidence interval of the mean! Qualitatively explain why this is the case.

95% of the data falls within 21.71473 and 26.69090. 5% of the data is either lower or higher than that.

## Problem Set 3

12. Based on the confidence intervals given above, do you think it likely or unlikely that *Lucina* is still alive?

Yes, likely...

13. Find the extinction confidence interval for the genus *Dallarca*. Show your code.

````R
> Dallarca <- subset(DataPBDB, DataPBDB$genus == "Dallarca")
> estimateExtinction(Dallarca$min_ma, 0.95)
Earliest   Latest 
 2.58800 -3.88749 
````

14. A pure reading of the fossil record says that *Dallarca* went extinct at the end of the Pliocene Epoch. Based on its confidence interval, do you think it is possible that *Dallarca* is still extant (alive)?

The confidence interval suggests that it could still be alive today, but the likelihood of a genus with a robust fossil record until the end of the Pliocene still existing today without any noted occurrences is low. It would have to have left no fossil record through the Pleistocene and have no known living representatives.

15. In this case, should we trust the confidence interval or a pure reading of the fossil record? Explain your reasoning.

## Problem Set 4

16.

17.

## Problem Set 5

18. How many occurrences are in `DataPBDB`? How many are in `ExtantData`? How many occurrences are lost by limiting our analysis to only extant bivalves?

````R
> dim(DataPBDB)
[1] 71160    28
> dim(ExtantData)
[1] 62055    28

> 71160 - 62055
[1] 9105
````

19. How many `unique( )` genera were in `DataPBDB` and `ExtantData`, respectively. Using this information, what percentage of Cenozoic bivalves in the PBDB are still extant today?

````R
> length(unique(DataPBDB$genus))
[1] 1075
> length(unique(ExtantData$genus))
[1] 544

> 544 / 1075
[1] 0.5060465
````

50.6% of Cenozoic bivalves are still extant today.

20. Find the stratigraphic range of fossil occurrences for each genus in the `ExtantData` dataset.

You can write a function that returns a data frame with the relevant information:

````R
> Range <- function(ExtantData) {
+     max <- tapply(ExtantData$max_ma, ExtantData$genus, max)
+     min <- tapply(ExtantData$min_ma, ExtantData$genus, min)
+     NewFrame <- data.frame(max,min)
+     return(NewFrame)
+ }
> Range(ExtantData)
                   max     min
Abra           56.0000  0.0000
Acanthocardia  66.0000  0.0000
Acar           66.0000  0.0000
Acesta         66.0000  0.0117
Acharax        48.6000  2.5880
Acila          66.0000  0.0117
````

21. Using your answer to question 3, find which genera in `ExtantData` are not extant according to the PBDB - i.e., do not have a minimum min_age of zero. Show your code.

````R
> RangeTable <- Range(ExtantData)
> NotExtant <- RangeTable[which(RangeTable[,"min"] != 0),]
> head(NotExtant)
               max    min
Acesta       66.00 0.0117
Acharax      48.60 2.5880
Acila        66.00 0.0117
Acorylus     11.62 0.1260
Acrosterigma 23.03 0.0117
Adamussium   28.10 0.7810
````

22. Calculate the confidence interval for the extinction of the following genera (careful with your spelling!): *Scrobicularia*, *Meiocardia*, *Dimya*, *Digitaria*, *Cuspidaria*, *Arctica*, *Aloides*, *Kurtiella*, *Gouldia*, and *Acrosterigma*. Show your code. What percentage of these taxa have confidence intervals indicating that the taxon might still be extant?

First, write a function that subsets the data by chosen genus and then uses the `extimateExtinction` function:

````R
Extinction <- function(DataPBDB, genus) {
+     genusSubset <- DataPBDB[which(DataPBDB[,"genus"] == genus),]
+     return(estimateExtinction(genusSubset$min_ma, 0.95))
+ }
````

Then use the function to estimate extinction for the relevant genera:

````R
> Extinction(DataPBDB, "Scrobicularia")
 Earliest    Latest 
  0.01170 -34.70966 

> Extinction(DataPBDB, "Meiocardia")
 Earliest    Latest 
 0.011700 -3.937808 

> Extinction(DataPBDB, "Dimya")
 Earliest    Latest 
 0.781000 -2.054688 

> Extinction(DataPBDB, "Digitaria")
 Earliest    Latest 
 0.781000 -3.761154 

> Extinction(DataPBDB, "Cuspidaria")
 Earliest    Latest 
2.5880000 0.8802009 

> Extinction(DataPBDB, "Arctica")
 Earliest    Latest 
 0.011700 -1.696099 

> Extinction(DataPBDB, "Aloides")
Earliest   Latest 
   5.333     -Inf 

> Extinction(DataPBDB, "Kurtiella")
 Earliest    Latest 
   0.0117 -189.9883 

> Extinction(DataPBDB, "Gouldia")
 Earliest    Latest 
 0.011700 -2.047386 

> Extinction(DataPBDB, "Acrosterigma")
 Earliest    Latest 
 0.011700 -3.481128 
````
