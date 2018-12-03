# Lab 05 - Introduction to Morphometrics

**Acknowledgement:** This lab is adapted from activities in David Polly's [Geometric Morphometrics](http://www.indiana.edu/~g562/) course

## Table of Contents

+ [Pre-lab reading assignment](#pre-lab-reading-assignment)
+ [Part 1: Osteostracans Landmark Data Collection](#part-1-osteostracans-landmark-data-collection)
+ Part 2: Data Analysis
+ Part 3: Conclusions

## Pre-Lab Reading Assignment

Before getting started on this lab, please read [Webster & Sheets (2010) *A Practical Introduction to Landmark-Based Geometric Morphometrics*](https://geosci.uchicago.edu/~mwebster/Webster_and_Sheets_2010.pdf), which will provide an overview of the applications of morphometrics in paleontology and an introduction to the methods we will practice in this lab.

## Part 1: Osteostracans Landmark Data Collection

From the [*Tree of Life*](http://tolweb.org/Osteostraci)

The Osteostraci, or osteostracans, are a major clade (about 200 species) of fossil, armored jawless vertebrates which lived from the Early Silurian (about 430 million years) to the Late Devonian (about 370 million years). Most of them have a characteristic horseshoe-shaped head, which consists of a massive endoskeletal skull, covered with a shield of dermal bone. On the dorsal surface of the head are the closely-sat eyes, a pineal foramen, and a medium, keyhole-shaped nasophypophysial openening. In addition, there are peculiar "fields" (in fact, depressions of the braincase, covered with loose platelets of dermal bone), which have been regarded as either sense organs or electric organs. The mouth and gill openings are, like the Galeaspida, situated on the ventral side of the head. Osteostracans also have large, pad-shaped paired fins. Most osteostracans are about 20 to 40 cm in total length, but some species could be extremely small (about 4 cm in length). The largest species was about one meter in length.

**Step 1:** Download the osteostracan skull photo set from Canvas. This .zip file contains skull diagrams of 30 different Osteostracan species, which you will be collecting landmark data from. Save this folder on your desktop. Spend a few minutes reviewing the diagrams so that you are familiar with the range of variation among species.

**Step 2:** Use [FIJI](https://fiji.sc/) to collect your landmark data. But first, some things to remember about collecting landmarks (from [David Polly](http://www.indiana.edu/~g562/Handouts/Collecting%20Landmarks.pdf):

+ Each image must have the same number of landmarks
+ The landmarks on each image image must be collected in the same order
+ Landmarks are ordinarily placed on homologous points, points that can be replicated from object to object based on common morphology, common function, or common geometry
+ You may have to flip some images so that they are not reversed left to right (e.g., if most of your images show the right side, flip left side images so that they mimic the right side)

To collect your landmark data, open the first image in the folder in FIJI. Use the multipoint collector tool to place landmarks on the skull. You can use the image below as a guide or decide on your own landmark positions When you have placed all of your points, use CTRL-M to record them in the measurement window (a spreadsheet that will populate with all of your data as you collect from each image). When you are done with your first image, use CTRL-SHIFT-O to open the next image. Repeat until you have collected landmark data from all 30 images.

**Step 3:**
