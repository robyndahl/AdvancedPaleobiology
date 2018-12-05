# Introduction to the Paleobiology Database

*This lab is based on the [Lab Exercise 3](https://github.com/aazaff/teachPaleobiology/blob/master/LabExercise3.md) for [teachPaleobiology](https://github.com/aazaff/teachPaleobiology)

## Table of Contents

+ [Instructions](#instructions)
+ [Finding the Paleobiology Database](#finding-the-paleobiology-database)
+ [References](#references)
+ [Collections](#collections)
+ [Occurrences](#occurrences)
+ [Downloading Data](#downloading-data)
+ [Paleobiology Database API](#paleobiology-database-api)
+ [Writing Your Own API Function in R](#writing-your-own-api-function-in-r)
+ [Morphologic Measurements](#morphologic-measurements)

## Instructions

Complete the following lab exercise and submit your answers as a GitHub markdown file to your personal GitHub repository by February, 14, 2019. Post a link to the file in the Lab 07 Assignment on Canvas.

## Finding the Paleobiology Database

The URL for the Paleobiology Database (PDBD) is [www.paleobiodb.org](https://paleobiodb.org/). Open the PBDB in another web brower window. The first thing you should see is the **SPLASH** page:

![Lab07_Fig1](/Images/Lab07_Fig1.png)

The PBDB splash page has a lot of information packed into it. At the bottom of the screen, you will see some basic stats on the types and quantity of data located in the database.

**Data Type** | **Definition**
------------- | --------------
**References** | Scientific articles, books, monographs, or other data sources
**Taxa** | A taxon (plural: taxa) is a group of one or more populations of an organism or organisms seen by taxonomists to form a unit
**Opinions** | Different opinions on the correct taxonomic name/identification of different fossil taxa
**Collections** | A group (collection) of fossil taxa at a specific location
**Occurences** | An individual observation of a taxon at a specific location
**Scientists** | The number of scientists that are *officially* involved in the PBDB initiative

## References

All data in the PBDB can ultimately be traced back to one or more references. You can search through references in the PBDB using the **Search** menu at the top of the spash page. Select `Search > Published references` to navigate to the reference search form:

![Lab07_Fig2](/Images/Lab07_Fig2.png)

**Exercise 1 Questions**

Let's use the folowing paper as an example (which you can find on Canvas):

Holland, S. M. and M. E. Patzkowsky. 2007. Gradient ecology of a biotic invasion: Biofacies of the type Cincinnatian series (Upper Ordovician), Cincinnati, Ohio Region, USA. Palaios, 22:392-407.

Use the reference tool to look up collections associated with this paper, then answer the following questions.

1. How many collections are associated with this reference?
2. What is the reference ID number for the article?

Once you have answered the above questions, click the `view collections` hyperlink at the bottom of the page to see all the collections associated with this study. Click to view **Collection No. 72438**, then answer the following questions:

3. The first two taxa in the taxonomic list are bryozoans, and the third taxon listed is *Rafinesquina alternata*. Next to the taxonomic name is the citation (Conrad 1830). What is the *class, family, genus* and *species* of the fourth taxon in the taxonomic list?
4. In what county was the data collected?
5. What age (Period) is the data from?
6. What is the name of the geologic formation that the data was collected from?

## Collections

Collections are useful for getting additional information about the age, location, and geologic content of collected fossils. They are, however, generally a poor tool for data analysis. This is because there is no standard operational definition of a collection in the PBDB.

For example, collection no. **72438** from the previous example represents a single sample from the study by Holland & Patzkowsky. In that study, a sample represents a single *bedding plane* (i.e., the top of a single rock layer) between 100 cm<sup>2</sup> and 1600 cm<sup>2</sup> in size.

In contrast, collection no. **91240** represents a single sample in a study by Ivany et al. (2009), Ref#30540. In that study, a sample was defined as an entire rock outcrop (multiple beds), generally several square meters in extent.

If you blindly compared these two collections, you would be making an apples to oranges comparison.

## Occurrences
