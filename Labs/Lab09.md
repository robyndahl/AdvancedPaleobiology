# Calculating Stratigraphic Ranges

Calculating confidence intervals for the stratigraphic ranges of taxa.

## Basic Concepts

The easiest way to calculate the stratigraphic range of a fossil is to find the age of its oldest occurrence (sometimes called *First Occurrence*, **FO**) and its youngest occurrence (sometimes called *Last Occurrence*, **LO**).

Today we are going to exclusively focus on calculating confidence intervals for last occurrences (time of extinction), but the principles are the same for calculating confidence intervals on origination rates.

#### Step 1

Open R and load in the following modules of the beta version of the University of Wisconsion's [paleobiologyDatabase.R](https://github.com/aazaff/paleobiologyDatabase.R) package using the `source( )` function.

````R
> source("https://raw.githubusercontent.com/aazaff/paleobiologyDatabase.R/master/communityMatrix.R")
````

#### Step 2:

Download a global dataset of bivalve occurrences from the Cenozoic Eara using the ````downloadPBDB( )```` function

````R
# Download data from the Paleobiology Database
# This may take a couple of minutes.
DataPBDB <- downloadPBDB(Taxa=c("Bivalvia"),StartInterval="Cenozoic",StopInterval="Cenozoic")

# Clean up bad genus names
DataPBDB <- cleanRank(DataPBDB)
````

If your download fails for any reason, load the data directly using the following:

````R
# Download directly from this course's GitHub Repository
DataPBDB <- read.csv("https://github.com/robyndahl/AdvancedPaleobiology/blob/master/Labs/cenozoic.csv")

# Clean up bad genus names
DataPBDB <- cleanRank(DataPBDB)
````

There are four columns in `DataPBDB` relevant to the age of an organism: `early_interval`, `late_interval`, `max_ma`, and `min_ma`. Because we rarely have a precise date, we generally give the age of an occurrence as a range. This range can be expressed by interval names or by numbers.

1. What do the max_ma and min_ma columns of `DataPBDB` represent? If you do not intuitively know, you can always check the [Paleobiology Database API documentation](https://paleobiodb.org/data1.2/occs/list_doc.html).

2. What is oldest age of each genus? Use the following code to find out:

````R
tapply(DataPBDB$max_ma, DataPBDB$genus, max)
````

3. What is the youngest age of each genus? Use the following code to find out:

````R
tapply(DataPBDB$max_ma, DataPBDB$genus, min)
````

4. Use the following code to find which genus has the most occurrences in the dataset:

````R
# a simple way to do this requires two steps
> max(table(DataPBDB$genus))
[1] 2103
> which(table(DataPBDB$genus) == 2103)
````

5. With the information we collected in Question 4, we can determine the stratigraphic range of that taxon using the following:

````R
# First, separate out the data for genus Anadara:
> Anadara <- DataPBDB[which(DataPBDB$genus == "Anadara"),]

# Then, determine the maximum and minimum age occurrences
> max(Anadara$max_ma)
[1] 66
> min(Anadara$min_ma)
[1] 0
````

## Confidence intervals

In statistics we like to measure uncertainty. We often do this with something called a **confidence interval**. Google defines a confidence interval as, "a range of values so defined that there is a specified probability that the value of a parameter lies within it." Under this definition, a 95% confidence interval ranging from 0-10, means that there is a 95% probability that the true value of the parameter we are measuring lies somewhere between 0 and 10.

This definition/interpretation of confidence intervals has received ***extensive criticism*** in recent years, though you may still see it presented in some textbooks that way. The criticism stems, partly, from a broader debate between two different statistical philosophies about the nature of probability. We will not dive into this debate, but I want you to be aware that *many* statisticians have a deep disapproval of the definition given above. Nevertheless, it is the one we will use moving forward for this lab exercise.

##### Step 1

Let's find the average paleolatitude of the genus *Lucina*.

````R
# Subset the data so that we get the genus *Lucina*
> Lucina <- subset(DataPBDB, DataPBDB$genus == "Lucina")

# Isolate the paleolatitude data, while also omitting any entries with missing data (NA)
> PaleoLat <- na.omit(Lucina$paleolat)

# Find the mean paleolat of all Lucina occurrences.
> OriginalMean <- mean(PaleoLat)
> OriginalMean
[1] 25.41262
````

##### Step 2

Let's put a 95% confidence interval around this value. We want to see if we randomly resampled from this underlying distribution, what the distribution of potential means would be.

````R
# Set a seed so that we get the same result
set.seed(100)

# A randomly resample the occurrences of Lucina, and find the mean latitude of our random resample
> NewMean <- mean(sample(PaleoLat, length(PaleoLat), replace = TRUE))
> NewMean
[1] 24.81061
````

Notice that we got a lower mean, 24.81061, this time compared to our original mean, 25.41262.

##### Step 3

Now, remember the [Law of Large Numbers](https://github.com/aazaff/startLearn.R/blob/master/expertConcepts.md#the-law-of-large-numbers). We need to repeat this process many times to converge on a long term solution. For that we'll need to use a `for(  )` loop.

````R
# Create a vector from 1 to 1,000, this is how many times we will repeat the resampling procedure
Repeat <- 1:1000

# Create a blank array to store our answers in.
> ResampledMeans <- array(NA, dim = length(Repeat))

# Use a for( ) loop to repeat the procedure
for (counter in Repeat) {
  ResampledMeans[counter] <- mean(sample(PaleoLat, length(PaleoLat), replace = TRUE))
  }
  
# Take a peak at what Resampled Means looks like, the numbers should be the same. If not, go back and re-set your
# seed, and try again from that step onwards.
head(ResampledMeans)
[1] 24.54456 24.72732 25.01327 26.51116 26.06427 26.33383
````

#### Problem Set 2

6. Qualitatively describe what is happening in the following lines of code. A good answer should identify what the different arguments are for each function, and what they are used for.

````R
PaleoLng <- na.omit(Lucina$paleolng)
mean(sample(PaleoLng, length(PaleoLng), replace=TRUE))
````

7. Plot a [kernel density](https://github.com/aazaff/startLearn.R/blob/master/expertConcepts.md#describing-distributions-with-statistics) graph of `ResampledMeans` using the code below. Does the distribution look approximately Gaussian? Explain why you think it does or does not.

````R
plot(density(ResampledMeans)
````

8. Find the mean of `ResampledMeans`, is it similar to the mean of the original data?

9. Sort `ResampledMeans` from lowest to highest using the following code:

````R
OrderedMeans <- sort(ResampledMeans)
`````

Now that you have sorted `ResampledMeans`, what is the 2.5th percentile of ResampledMeans and what is the 97.5th percentile of Resampled means. If you do not know what a percentile is, or how to calculate it, you can use google. Hint: use the `quantile( )` function.

10. Incidentally, these numbers (your answer to question 5) are the lower and upper confidence interval of the mean! Qualitatively explain why this is the case.

#### Step 1

Load in the following function I wrote for you!

````R
# From Marshall's (1990) adaptation of Strauss and Sadler.
estimateExtinction <- function(OccurrenceAges, ConfidenceLevel=.95)  {
  # Find the number of unique "Horizons"
  NumOccurrences<-length(unique(OccurrenceAges))-1
  Alpha<-((1-ConfidenceLevel)^(-1/NumOccurrences))-1
  Lower<-min(OccurrenceAges)
  Upper<-min(OccurrenceAges)-(Alpha*10)
  return(setNames(c(Lower,Upper),c("Earliest","Latest")))
  }
````

#### Step 1

Let's find the estimated extinction date for the genus *Lucina* using the `estimateExtinction( )` function.

````R
estimateExtinction(Lucina[,"min_ma"],0.95)
 Earliest    Latest 
 0.000000 -1.173424
````

#### Problem Set 3

12. Based on the confidence intervals given above, do you think it likely or unlikely that *Lucina* is still alive?

13. Find the extinction confidence interval for the genus *Dallarca*. Show your code.

14. A pure reading of the fossil record says that *Dallarca* went extinct at the end of the Pliocene Epoch. Based on its confidence interval, do you think it is possible that *Dallarca* is still extant (alive)?

15. In this case, should we trust the confidence interval or a pure reading of the fossil record? Explain your reasoning.

## Non-Uniform Recovery

The problem with the `estimateExtinction(  )` function is that it makes an important assumption that is unlikely to be true. 

It assumes randomly distributed fossils. In other words, the fossil occurrences of taxa are randomly distributed in time, and the likelihood of preservation does not change up or down a stratigraphic section.

For this reason, paleobiologists have developed more sophisticated versions of confidence interval analysis. However, for simplicity, we will stick with the Strauss and Sadler (1987) method. 

#### Problem Set 4

16. State one ecological reason why this assumption is unlikey to be true.

17. State one geological reason why this assumpiton is unlikely to be true.

## Testing Strauss and Sadler 

Let's take a list of bivalve genera that we know to be extant today, but are extinct according to the Paleobiology Database.

````R
# Load in a csv of extant bivalves
ExtantBivalves <- read.csv("https://raw.githubusercontent.com/paleobiodb/teachPaleobiology/master/Lab7Figures/ExtantBivalves.csv",row.names=1,header=TRUE)

# Subset DataPBDB to find only occurrences of ExtantBivalves
ExtantData <- subset(DataPBDB,DataPBDB$genus%in%ExtantBivalves$Extant==TRUE)
````

#### Problem Set 5

18. How many occurrences are in `DataPBDB`. How many are in `ExtantData`? How many occurrences were lost by limiting our anaysis to only extant bivalves?

19. How many `unique(  )` genera were in `DataPBDB` and `ExtantData`, respectively. Using this information, what percentage of Cenozoic bivalves in the PBDB are still extant today.

20. Find the stratigraphic range of fossil occurrences for each genus in the `ExtantData` dataset. (NOTE: I am providing the answer for this question because it requires writing a function, a skill we have not learned in this course)

````R
Range <- function(ExtantData) {
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
Acharax        56.0000  0.0117
Acila          66.0000  0.0000
````

21. Using your answer to question 20, find which genera in `ExtantData` are not extant according to the PBDB - i.e., do not have a minimum min_age of zero. Show your code.

22. Use the function below to calculate the confidence interval for the extinction of the following genera (careful with your spelling!): *Scrobicularia*, *Meiocardia*, *Dimya*, *Digitaria*, *Cuspidaria*, *Arctica*, *Aloides*, *Kurtiella*, *Gouldia*, and *Acrosterigma*. What percentage of these taxa have confidence intervals indicating that the taxon might still be extant?

````R
Extinction <- function(DataPBDB, genus) {
+     genusSubset <- DataPBDB[which(DataPBDB[,"genus"] == genus),]
+     return(estimateExtinction(genusSubset$min_ma, 0.95))
+ }
````
