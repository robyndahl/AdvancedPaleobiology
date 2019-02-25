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
> DataPBDB <- downloadPBDB(Taxa=c("Bivalvia"),StartInterval="Cenozoic",StopInterval="Cenozoic")

# Clean up bad genus names
> DataPBDB<-cleanRank(DataPBDB)
````

#### Problem Set 1

There are four columns in `DataPBDB` relevant to the age of an organism: `early_interval`, `late_interval`, `max_ma`, and `min_ma`. Because we rarely have a precise date, we generally give the age of an occurrence as a range. This range can be expressed by interval names or by numbers.

1) What do the max_ma and min_ma columns of `DataPBDB` represent? If you do not intuitively know, you can always check the [Paleobiology Database API documentation](https://paleobiodb.org/data1.2/occs/list_doc.html).

2) What is oldest age of each genus? [[Hint](https://github.com/aazaff/startLearn.R/blob/master/intermediateConcepts.md#direct-subsetting-with-functionals): Use the `tapply(  )` and `max(  )` functions we've used in previous labs]. Show the code you would use to find out.

3) What is the youngest age of each genus? [[Hint](https://github.com/aazaff/startLearn.R/blob/master/intermediateConcepts.md#direct-subsetting-with-functionals): Use the `tapply(  )` and `min(  )` functions we've used in previous labs]. Show the code you would use to find out.

4) Find which genus has the most occurrences in the dataset [Hint: Use the `table( )` function!]. What code did you use?

5) What is the stratigraphic range of this taxon (i.e., your answer to question 4). Show your code.

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
[1] 24.30135
````

##### Step 2

Let's put a 95% confidence interval around this value. We want to see if we randomly resampled from this underlying distribution, what the distribution of potential means would be.

````R
# Set a seed so that we get the same result
set.seed(100)

# A randomly resample the occurrences of Lucina, and find the mean latitude of our random resample
> NewMean <- mean(sample(PaleoLat, length(PaleoLat), replace = TRUE))
> NewMean
[1] 23.51157
````

Notice that we got a lower mean, 23.51157, this time compared to our original mean, 24.30135.

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
[1] 24.46657 23.55738 26.87615 23.41481 26.23879 21.94562
````

#### Problem Set 2

1) Qualitatively describe what is happening in the following line of code. A good answer should identify what the different arguments are for each function, and what they are used for.

````R
mean(sample(Lucina[,"paleolng"],length(Lucina[,"paleolng"]),replace=TRUE))
````

2) Plot a [kernel density](https://github.com/aazaff/startLearn.R/blob/master/expertConcepts.md#describing-distributions-with-statistics) graph of ````ResampledMeans````. Show your code. Does the distribution look approximately Gaussian? Explain why you think it does or does not.

3) Find the mean of ````ResampledMeans````, is it similar to the mean of the original data?

4) Sort ````ResampledMeans```` from lowest to highest. [Hint: We learned how to sort a vector in [Lab 6](/LabExercise6.md#problem-set-2)].

5) Now that you have sorted ````ResampledMeans````, what is the 2.5th percentile of ResampledMeans and what is the 97.5th percentile of Resampled means. If you do not know what a percentile is, or how to calculate it, you can use google. Show your code. 

6) Incidentally, these numbers (your answer to question 5) are the lower and upper confidence interval of the mean! Qualitatively explain why this is the case.
