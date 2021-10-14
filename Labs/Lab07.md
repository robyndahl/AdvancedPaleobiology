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

**Exercise Questions Part 1**

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

Occurrences are the number of collections that contain a taxon. Since the size and definition of collections is variable, the meaning of occurrences is also somewhat imprecise.

Therefore, as we progress in this course, you will see that oftentimes the first step of any data analysis project using the PBDB is to reorganize occurrences into a more sensible and standardized format. We will discuss occurrences more when discussing how to download data.

**Exercise Questions Part 2**

Return to the PBDB splash page, and select the **Navigator** tool. This tool is the best way to visualize the age and location of collections in the PBDB. Take a few minutes to orient yourself to the navigator page. Explore the timescale along the bottom, or search for different fossil groups using the search bar at the top. Zoom in closer to different continents, or view the continents in different paleogeographic orientations.

Once you have familiarized yourself with the page, search for the genus *Abra*. *Abra* is a genus of bivalve.

1. Zoom in so that you can see from Texas to Florida and from Florida to New York. Some of the occurrences are orange and others are yellow. What is the significance of the different colors?

2. Zoom back out. Add an additioanl filter to the search bar, the Ypresian stage. The Ypresian is a time interval ranging from 47.8-56.0 million years ago. In what countries are there Ypresian occurrences of *Abra*?

3. Clear the *Abra* and Ypresian filters from the search. Look for the genus *Ambonychia* (another genus of bivalve). Within the United States, find the city with the most occurrences of *Ambonychia*. What is the name of this city?

4. What age (Period) are most *Ambonychia* occurrences?

Add your answer to Question 4 as an additional filter. Click the little icon on South America breaking away from Africa on the left side of the screen. This icon rotates the continents back to their position in the specified time-period. **Note that it requires you to set a specific time period as a filter**.

5. During this time period, were most occurrences of *Ambonychia* arrayed parallel or perpendicular to the equator?

6. Click on the little insect icon on the left side of the screen. This brings up taxonomic information about the target taxon. What order does *Ambonychia* belong to?

## Downloading Data

You can download the data displayed in your Navigator window using the little arrow icon on the left side of the screen, but its options are limited.

To customize the data that you want, use the more detailed download form. To find this form, return to the splash page and click **Download Data**. This download form uses the PBDB API. Once you are more familiar with the PBDB and R, you will be able to download data direclty into R using the API, and you will no longer have to use to the Navigator or the download form.

![Lab07_Fig3](/Images/Lab07_Fig3.png)

From the download form, we can download all collections of both *Abra* and *Ambonychia* as a tab-separated file. To do this:

+ Select `Collections`
+ Select `Tab-separated values (tsv)`
+ Enter `Abra, Ambonychia` into the taxon textbar
+ Click the `Test` button in the middle of the page. This open a new window that displays your data. Close that window to return to the download form, which should now show a blue URL describing your data request above the `Test` and `Download` buttons

The URL should look like this:

[https://paleobiodb.org/data1.2/colls/list.tsv?datainfo&rowcount&base_name=Abra,Ambonychia](https://paleobiodb.org/data1.2/colls/list.tsv?datainfo&rowcount&base_name=Abra,Ambonychia)

**Exercise Questions Part 3**

For the following questions, generate the appropriate URL for the following data queries:

1. All occurrences of *Ambonychia* in the Lexington Limestone as a JSON

2. All occurrences of mammals present in the Paleocene through Oligocene epochs as a csv

3. All opinions on the order Testudines in the Mesozoic

4. All collections of Aves, Marsupialia, and Sirenia in the United States as a csv

5. All occurrences of the gastropod genus *Ficus* as a csv

The next set of problems is free form, in that you can find the answers using any of the PBDB tools discussed so far.

6. What family does the genus *Gastrocopta* belong to?

7. There is only one occurrence of *Isoetes* in Portugal. What age is it?

8. What is the age of the oldest occurrence of *Gastrocopta*?

9. There is only one occurrence of *Tiktaalik* in the PBDB. Was that occurrence located in the tropics or the extratropics when that organism was alive?

10. There are two occurrences of *Namacalathus* in Siberia. What geologic formations are they found in?

## Paleobiology Database API

The acronym API stands for Application Programming Interface. Technical definitions aside, it is a way for users to access data stored in an online database through web addresses (URLs). Companies that store a lot of data (e.g., Google, Twitter, Facebook) make API's available so that 3rd party developers can use their data to make applications. For example, if you've ever played a Facebook game (e.g., Candy Crush, Farmville), those programs were accessing information about you and your friends through the API.

The best way to think about using an API is to imagine it as a map to all the data stored online. You need to use this map to give the computer directions on how to find the particular data you want and access it. When we give directions to a location in the real world, we generally do so in two ways. We either give geographic coordinates (i.e., latitude, longitude, elevation) that specify the destination, or a set of routes to get somewhere (e.g., Take I-90 S to Everett, then US-2 W to Leavenworth).

When we access data in R via **subscripting** `Object[ ]`, we are using a coordinate system to point out the data in our object. In contrast, when we access data through an API, we are defining a **route**. In fact, route is the formal terminology. Depending on the size of an API, there may be dozens of routes, which may feel overwhelming at first. However, remember that a car map has thousands or hundreds of thousands of roads, most of which you will never travel on, but you still need to know how to use the map to get where you're going. It's the same with an API.

If you want to read more about APIs and see some non-paleontological example of how to use them, check out this blog post from the Pew Research Center: [Using APIs to collect website data](https://medium.com/pew-research-center-decoded/using-apis-to-collect-website-data-b7fc340d59e3)

Let's deconstruct a specific API **query** (i.e., URL):

![Lab07_Fig4](/Images/Lab07_Fig4.png)

The first half of the query, before the **?**, is pretty straightforward because there are only a few possible variations. However, the **parameters** that come afterwards can become quite cumbersome because there are many varieties of them, and many of them will change depending on what type of data you are using (i.e., collections vs. occurrences). You will need to use the documentation to see a full list of parameters.

+ [Occurrence Parameters](https://paleobiodb.org/data1.2/occs/list_doc.html)
+ [Collections Parameters](https://paleobiodb.org/data1.2/colls/list_doc.html)
+ [References Parameters](https://paleobiodb.org/data1.2/refs/list_doc.html)
+ [Opinions Parameters](https://paleobiodb.org/data1.2/opinions/list_doc.html)
+ [Specimens Parameters](https://paleobiodb.org/data1.2/specs/list_doc.html)

**Exercise Questions Part 4**

````
https://paleobiodb.org/data1.2/colls/list.csv?base_name=Mammut&interval=Pliocene
````

1. Download the data from the URL above into R. What are its dimensions?

2. Did the above call use the occurrences, collections, references, opinions, or specimens route?

3. What genus is being called for? What is its colloquial name? What age was the call limited to?

4. Look through the service doumentation for the appropriate route (based on your answer to Question 2). Find out how to extend the age search range from the Miocene Epoch through the Pleistocene Epoch. Give the new data query URL.

5. What URL would you use to show the paleocoordinates (i.e., paleolatitude and paleolongitude) of each data point?

## Writing Your Own API Function in R

Wouldn't it be really convenient if, instead of typing out the URL every time, you could just write an R function that takes a specific taxon name and interval and downloads the data directly into R?

Specifically, your final question for this lab is the write a function in R that will take as its arguments a **taxon name** and an **interval** and download all fossil occurrences from the PBDB as a csv.

Your final product should look like this:

````R
# Download all instances of the genus Abra from the Pleistocene interval
AbraData <- downloadPBDB(taxon="Abra",interval="Pleistocene")

# Your output should look like this
AbraData[1:6,1:6]
  occurrence_no record_type reid_no flags collection_no identified_name
1         94761         occ      NA    NA          7108   Abra aequalis
2        256368         occ      NA    NA         20604   Abra aequalis
3        256386         occ      NA    NA         20606   Abra aequalis
4        425385         occ      NA    NA         41501   Abra aequalis
5        427341         occ      NA    NA         41705   Abra aequalis
6        427901         occ      NA    NA         41740   Abra aequalis

# Download all instances of the genus Tyrannosaurus from the Mesozoic
TRexData <- downloadPBDB(taxon="Tyrannosaurus",interval="Mesozoic")

# Your output shoud look like this
TRexData[1:6,1:6]
  occurrence_no record_type reid_no flags collection_no       identified_name
1        139292         occ   22878    NA         11917     Tyrannosaurus rex
2        139293         occ      NA    NA         11918 Tyrannosaurus cf. rex
3        219998         occ      NA    NA         22654     Tyrannosaurus rex
4        220009         occ      NA    NA         22657     Tyrannosaurus rex
5        280101         occ      NA    NA         26760     Tyrannosaurus rex
6        291021         occ      NA    NA         14640 Tyrannosaurus cf. rex
````

In order to achieve this, you will need to use the `paste( )` function. Here are some examples of the paste function. See if you can figure out how it fits into the problem.

````R
# Example 1
paste("We","Love","R",sep=" ")
[1] "We Love R"

# Example 2
paste("We","Love","R",sep="")
[1] "WeLoveR"

# Example 3
paste("We","Love","R",sep="!")
[1] "We!Love!R"

# Example 4
LoveR <- c("We Love R")
HateR <- c("We Hate R")
paste(LoveR,HateR,sep=" >> ")
[1] "We Love R >> We Hate R"
````
**Exercise Questions Part 5**

1. Write an R function that will take a taxonomic name (as a character string) and an interval (as a character string) as its argument, and will download all fossil occurrences in R. See above.
