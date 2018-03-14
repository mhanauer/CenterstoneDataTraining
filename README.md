---
title: "Test1"
output: html_document
---

```{r setup, include=FALSE}
knitr::opts_chunk$set(echo = TRUE)
```
Need to explain how to download R, RStudio, and RMarkdown.

Creating fake data.  For PC users set the working directory first.
```{r}
setwd("~/Desktop")
id = 1:10000
genderSamp = c("Male", "Female", "Other_Identity")
ethSamp = c("White", "African_American", "Asian", "Hispanic", "Other_Ethnic_Identity")
SESSamp = c("Very_Low", "Low", "Middle", "High")
set.seed(12345)
preScore = abs(rnorm(10000, 50, 10))
postScore = abs(rnorm(10000, 60, 10))
dat = data.frame(id, Gender = sample(genderSamp, 10000, replace = TRUE, prob = c(.40, .30, .30)), Ethnicity = sample(ethSamp, 10000, replace = TRUE, prob = c(rep(.2, 5))), SES = sample(SESSamp, 10000, replace = TRUE, prob = c(rep(.25, 4))), preScore = round(preScore, 0), postScore = round(postScore, 0)); dat

write.csv(dat, "dat.csv", row.names = FALSE)
```
Here we are going to load the data into R.  I placed my data on the desktop, so I can use the code below.  The easiest way to do this by setting the working directory to the desktop using, set working directory and then selecting the location.  

You can read a data set by using the read.csv package.  You need to put the name of the data set and then have a dot and csv (this package only works with csv's).  Header equals true means that the first row in the data set contains the variable names.  

Then if you want to write the data set to a csv you can use the write csv function. You also will likely want to set the row.names to false, because if it is TRUE then it adds a new row that has a number for each row (i.e. row one gets a 1, row two gets a 2)
```{r}
setwd("~/Desktop")
dat = read.csv("dat.csv", header = TRUE)
write.csv(dat, "dat.csv", row.names = FALSE)

```
Dealing with missing data (Creating some missing data ignore this)
```{r}
dat$Ethnicity[100:200] = NA
dat$postScore[120:220] = NA
```
At lot of packages in R freak out if there is missing data, so it is probably best just to get rid of the missing values.  To get rid of all data points that have any missing value in some variable using the na.omit function in R.  

We can use the dim function to show us how many data points and variables that the data set has.  
```{r}
dim(dat)
dat = na.omit(dat)
dim(dat)
```
Creating more data to merge (ignore this)
```{r}
set.seed(12345)
income = c("high", "low")
dat1 = data.frame(id = 1:10000, income = sample(income, 10000, replace= TRUE))

```
Sometimes data set are spread out and not all together and we want to merge them by some unique identifier.  In this case we have a data set called dat1 which contains the unique id variable and a new variable called income.

We can use the merge function in R to merge data sets.  There are three kinds of merges right, left, and all.  For right this means that we match all the data in the right data set (the second data set you list).  So for right it will match all the ids in the right data set with all of the matching ids in the left data set and any id that is in the left data data set that is not in the right data will be dropped.  Both means that we include ids from both data sets even if they have no matching one and any missing data is left blank.  So in the both example below you will see some missing data for eth and the other variables, because some people in dat are missing and not included in the dat1.
```{r}
datLeft = merge(dat, dat1, all.x = TRUE)
datLeft

datRight = merge(dat, dat1, all.y = TRUE)
datRight

datAll = merge(dat, dat1, all = TRUE)
datAll
```
Descriptives (summary and describe)

paired t-test in R

make skewed data

show skewed data

non-parametric t-test

linear regression bivariate

multivariates regression 

logisitc

