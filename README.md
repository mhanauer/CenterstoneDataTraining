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

You can read a data by using the read.csv package.  You 
```{r}
setwd("~/Desktop")
dat = read.csv("dat.csv", header = TRUE)
write.csv(dat, "data.csv", row.names = FALSE)

```
Dealing with missing data (Creating some missing data ignore this)
```{r}
dat$Ethnicity[100:200] = NULL
dat$postScore[120:220] = NULL
```
At lot of packages in R freak out if there is missing data, so it is probably best just to get rid of the missing values.  


However, if you are analysis only requires certain variables, only include those variables when you are going to omit missing variables

Merging

Descriptives (summary and describe)

paired t-test in R

make skewed data

show skewed data

non-parametric t-test

linear regression bivariate

multivariates regression 

logisitc

