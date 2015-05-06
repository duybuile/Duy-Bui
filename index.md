---
title       : Salary prediction
subtitle    : Predict human wages based on their status
author      : Duy Bui
job         : Data Scientist
framework   : io2012        # {io2012, html5slides, shower, dzslides, ...}
highlighter : highlight.js  # {highlight.js, prettify, highlight}
hitheme     : tomorrow      # 
widgets     : [mathjax]     # {mathjax, quiz, bootstrap}
mode        : selfcontained # {standalone, draft}
knit        : slidify::knit2slides
---

## Problem Overview
### Advantages
- Salary prediction is useful for tax companies to detect any frauds within tax payments. 
- Meaningful for demographic purposes

### Idea
- Create a trustful and confident prediction model having a high accuracy in predicting somebody's salary based on his/her personal information. We predict whether income exceeds $50K/yr based on census data

--- .class #id 

## Data pre-processs
### Data source
- UCI Database: http://archive.ics.uci.edu/ml/datasets/Adult 
- Data was collected by Barry Becker from the 1994 (32561 instances, 14 attributes)
- Attributes: age, workclass, fnlwgt, education, education-num, marital-status, occupation, race, sex, capital-gain, capital-loss, hours-per-week, native-country.


```r
adultData <- read.table("adult.data", header = FALSE, sep = ",", strip.white = TRUE)
adultName <- read.csv("adult.name.csv", header = FALSE, sep = ",", stringsAsFactors = FALSE)
names(adultData) <- adultName[, 1]
```

### Data selection
It is ideal to use the whole dataset as training set. However, the training session will consume much time which causes users' impatience.  The data therefore was reduced to only containing 7 attributes. The selection is entirely based on developers' preferences.

--- .class #id 

## Machine Learning method

There are numerous machine learning methods, however "Recusive Partitioning" was chosen in this model due to its light-weight and quickness. When it comes to speed testing, "Recusive Partitioning" always out-performs other machine learning methods such as "Random Forest", or "Boosting", while its model accuracy within this data set is sufficiently acceptable with more than 80%.


```r
trainIndex = createDataPartition(adultData$salary, p=0.70, list=FALSE)
training = adultData[ trainIndex, ];testing = adultData[-trainIndex, ]
set.seed(33833)
modFit <- train(salary ~ ., method = "rpart", data=training)
confusionMatrix(testing$salary, predict(modFit, newdata = testing))$overall
```

```
FALSE       Accuracy          Kappa  AccuracyLower  AccuracyUpper   AccuracyNull 
FALSE   8.070229e-01   3.680702e-01   7.990546e-01   8.148077e-01   8.754095e-01 
FALSE AccuracyPValue  McnemarPValue 
FALSE   1.000000e+00  2.215460e-150
```

--- .class #id 

## Summary
### Result
- Application https://duybuile.shinyapps.io/Assignment1 
- High accuracy machine learning model

### Conclusion
- Practice of machine learning, data production
- Tools: shinyapps, slidify
