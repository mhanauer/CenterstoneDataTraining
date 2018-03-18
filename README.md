---
title: "Data Cleaning and Analysis in R"
output: html_document
---

```{r setup, include=FALSE}
knitr::opts_chunk$set(echo = TRUE)
```
First thing you want to do is download R and R Stuido
R PC: https://cran.r-project.org/bin/windows/base/
R Mac: https://cran.r-project.org/bin/macosx/
RStudio (choose PC or Mac version): https://www.rstudio.com/products/rstudio/download/#download 

Here is the location of the data set you will want to use: https://drive.google.com/open?id=1F4QVSPgvs20nnX6LU5FuIH4wC6Xnn4mR
```{r message=FALSE, warning=FALSE, include=FALSE}
setwd("~/Desktop")
set.seed(12345)
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

Then if you want to write the data set to a csv you can use the write csv function. You also will likely want to set the row.names to false, because if it is TRUE then it adds a new row that has a number for each row (i.e. row one gets a 1, row two gets a 2).
```{r warning=FALSE}
setwd("~/Desktop")
dat = read.csv("dat.csv", header = TRUE)
write.csv(dat, "dat.csv", row.names = FALSE)

```
Dealing with missing data (Creating some missing data ignore this)
```{r warning=FALSE}
dat$Ethnicity[100:200] = NA
dat$postScore[120:220] = NA
```
A lot of packages in R freak out if there is missing data, so it is probably best just to get rid of the missing values.  To get rid of all data points that have any missing value in some variable we can use the na.omit function in R.  

We can use the dim function to show us how many data points and variables that the data set has.  Therefore, by having dim before and after using the na.omit function, we can see how much data is missing before and after excluding the missing data.
```{r warning=FALSE}
dim(dat)
dat = na.omit(dat)
dim(dat)
```
Creating more data to merge (ignore this)
```{r warning=FALSE}
set.seed(12345)
income = c("high", "low")
dat1 = data.frame(id = 1:10000, income = sample(income, 10000, replace= TRUE))

```
Sometimes datasets are spread out and not all together and we want to merge them by some unique identifier usually id.  In this case we have a data set called dat1 which contains the unique id variable and a new variable called income.

We can use the merge function in R to merge data sets.  There are three kinds of merges right (y), left (x), and all.  For right this means that we match all the data in the right data set (the second data set you list).  So for right it will match all the ids in the right data set with all of the matching ids in the left data set and any id that is in the left data set that is not in the right data set will be dropped.  Both means that we include ids from both data sets even if they have no matching one and any missing data is left blank.  So in the both example below you will see some missing data for eth and the other variables, because some people in dat are missing those variables and not included in the dat1.
```{r warning=FALSE}
datLeft = merge(dat, dat1, all.x = TRUE)
datLeft

datRight = merge(dat, dat1, all.y = TRUE)
datRight

datAll = merge(dat, dat1, all = TRUE)
datAll[200:250,]
```
Sometimes you want to get a summary of the data set and each of the variables.  For simplicity, let us just use the original dat dataset and get the descriptives.  One way to get the descriptives is through the summary function.  For continuous variables, summary provides the mean, median, and range (min and max).  For dichotomous, nominal and ordinal outcomes, it provides the counts for each of the levels.
```{r warning=FALSE}
summary(dat)
```
One common statistic that we want to evaluate is the percentage change over time.  We can create a new variable in R that has the percentage change from pre to post.

We create this by using the $ for the dat set to create a new variable called PercentChange.  Then we say that PercentChange equals the percentage change formula which is (postScore-preScore)/preScore.  Then we use the round function to round the percentage change variable to two decimal points.
```{r warning=FALSE}
dat$PercentChange = round((dat$postScore-dat$preScore)/dat$preScore,2)
head(dat)
```
Sometimes we want to evaluate whether or not there was a statistically significant change between two time points.  To do that we would use a paired t-test to account for the correlation between the two time points. 

The results demonstrate that the p-value is well below .05 providing evidence that the average post score is statistically significantly larger than the pre scores.
```{r warning=FALSE}
t.test(dat$postScore, dat$preScore, paired = TRUE)
```
Although, because we have a large sample our data is likely normally distributed often times data are not and we need to use a test that does not make the assumption that data are normally distributed.  Therefore, we can use a nonparametric test like the Wilcoxon rank test to evaluate whether the post scores are larger than the pre scores.
```{r warning=FALSE}
wilcox.test(dat$postScore, dat$preScore, paired = TRUE)
```
