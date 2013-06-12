Cleaning the Bird Data
===============================




In the Ordway bird data, many of the species are listed in multiple forms.

```r
data(OrdwayBirdsOrig)
counts <- groupBy(OrdwayBirdsOrig, by = SpeciesName)
```


Some examples:

```
##                SpeciesName count
## 14        Baltimore Oriole   206
## 15            Bank Swallow    21
## 16            Barn Swallow    23
## 17         Batimore Oriole     1
## 18    Bay-breasted Warbler     2
## 19   Blac-capped Chickadee     1
## 20 Black and White Warbler     9
## 21 Black-and-white Warbler     1
## 22     Black-billed Cookoo     1
## 23     Black-billed Cuckoo    15
## 24  Black-capeed Chickadee     1
## 25  Black-capped Chicakdee     1
## 26  Black-capped chickadee   187
## 27  Black-capped Chickadee  1110
## 28  Black-Capped Chickadee    13
```


How to correct these, deleting the questionable or bogus species names and fixing others as needed?

This process requires some human judgment.  It's difficult to implement that on the computer, but it's straightforward, with the right relational operations, to make it easy for human judgment to be applied to the data.

### Steps

#### 1. Get a list of all the species names in the uncleaned data:


```r
snames <- with(data = OrdwayBirdsOrig, unique(SpeciesName))
```


#### 2. Write these out as a data table with room for the corrected names:

```r
write.csv(file = "NamesForCleaning.csv", data.frame(SpeciesName = snames, CorrectedName = NA), 
    row.names = FALSE)
```


#### 3. Edit the data table to insert the corrected names.

When there is no sensible correction, leave the `NA` entry.

Here, we'll publish the data table to Google Docs to allow us to "crowd-source" the corrections.  Follow this link to [Google Docs](https://docs.google.com/spreadsheet/ccc?key=0Am13enSalO74dDZjYlg3LXhVam9ZVDdtZGdvMzNPSGc&usp=sharing)

We'll do a bit of editing in the workshop.  With 15 people, it should take only a few minutes.

#### 4. Join the Corrected Data to the Birds

You can access the data table with this command:

```r
newNames <- fetchGoogle("https://docs.google.com/spreadsheet/pub?key=0Am13enSalO74dDZjYlg3LXhVam9ZVDdtZGdvMzNPSGc&single=true&gid=0&output=csv")
```

```
## Warning: package 'RCurl' was built under R version 2.15.2
```


Then, to combine the original data with the new names:

```r
fixedBirds <- join(OrdwayBirdsOrig, newNames)
```

```
## Joining by: SpeciesName
```


### Question:

After correction:
* How many distinct species are there?

```
