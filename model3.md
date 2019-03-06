model3
================
John Kennedy Mwangi
6 March 2019

Loading the data from csv's and load required libraries
=======================================================

``` r
modeldata <- read.csv('./modeldata3.csv', header=T)

library(caret)
```

    ## Loading required package: lattice

    ## Loading required package: ggplot2

``` r
library(rattle)
```

    ## Rattle: A free graphical interface for data science with R.
    ## Version 5.2.0 Copyright (c) 2006-2018 Togaware Pty Ltd.
    ## Type 'rattle()' to shake, rattle, and roll your data.

Check the dimensions and the structure of the loaded data
=========================================================

``` r
dim(modeldata)
```

    ## [1] 174140      9

``` r
str(modeldata)
```

    ## 'data.frame':    174140 obs. of  9 variables:
    ##  $ X             : int  1 2 3 4 5 6 7 8 9 10 ...
    ##  $ group_id      : Factor w/ 572 levels "010DEV","010PHP",..: 290 290 290 290 290 290 290 290 290 290 ...
    ##  $ rsvps_response: Factor w/ 3 levels "no","waitlist",..: 3 3 3 3 3 3 3 3 3 3 ...
    ##  $ rsvps_user_id : int  39980 34217 36559 30877 22290 65384 61274 20901 22295 52867 ...
    ##  $ status        : Factor w/ 5 levels "cancelled","past",..: 2 2 2 2 2 2 2 2 2 2 ...
    ##  $ venue_id      : int  68572 68572 68572 68572 68572 68572 68572 68572 68572 68572 ...
    ##  $ duration      : num  NA NA NA NA NA NA NA NA NA NA ...
    ##  $ day           : Factor w/ 7 levels "Friday","Monday",..: 5 5 5 5 5 5 5 5 5 5 ...
    ##  $ hour          : Factor w/ 62 levels "00:00","01:00",..: 47 47 47 47 47 47 47 47 47 47 ...

remove NAs
==========

``` r
modeldata <- modeldata[complete.cases(modeldata), ]
dim(modeldata)
```

    ## [1] 66434     9

``` r
str(modeldata)
```

    ## 'data.frame':    66434 obs. of  9 variables:
    ##  $ X             : int  61 62 63 64 65 66 67 68 69 70 ...
    ##  $ group_id      : Factor w/ 572 levels "010DEV","010PHP",..: 443 443 443 443 443 443 443 443 443 443 ...
    ##  $ rsvps_response: Factor w/ 3 levels "no","waitlist",..: 1 3 3 3 3 3 3 3 1 3 ...
    ##  $ rsvps_user_id : int  12382 31923 53504 48268 47022 26867 35154 66198 15711 44600 ...
    ##  $ status        : Factor w/ 5 levels "cancelled","past",..: 2 2 2 2 2 2 2 2 2 2 ...
    ##  $ venue_id      : int  67397 67397 67397 67397 67397 67397 67397 67397 67397 67397 ...
    ##  $ duration      : num  3 3 3 3 3 3 3 3 3 3 ...
    ##  $ day           : Factor w/ 7 levels "Friday","Monday",..: 5 5 5 5 5 5 5 5 5 5 ...
    ##  $ hour          : Factor w/ 62 levels "00:00","01:00",..: 51 51 51 51 51 51 51 51 51 51 ...

remove first column with row numbers
====================================

``` r
modeldata <- modeldata[, -1]
```

Check the dimensions and the structure of the loaded data
=========================================================

``` r
dim(modeldata)
```

    ## [1] 66434     8

``` r
str(modeldata)
```

    ## 'data.frame':    66434 obs. of  8 variables:
    ##  $ group_id      : Factor w/ 572 levels "010DEV","010PHP",..: 443 443 443 443 443 443 443 443 443 443 ...
    ##  $ rsvps_response: Factor w/ 3 levels "no","waitlist",..: 1 3 3 3 3 3 3 3 1 3 ...
    ##  $ rsvps_user_id : int  12382 31923 53504 48268 47022 26867 35154 66198 15711 44600 ...
    ##  $ status        : Factor w/ 5 levels "cancelled","past",..: 2 2 2 2 2 2 2 2 2 2 ...
    ##  $ venue_id      : int  67397 67397 67397 67397 67397 67397 67397 67397 67397 67397 ...
    ##  $ duration      : num  3 3 3 3 3 3 3 3 3 3 ...
    ##  $ day           : Factor w/ 7 levels "Friday","Monday",..: 5 5 5 5 5 5 5 5 5 5 ...
    ##  $ hour          : Factor w/ 62 levels "00:00","01:00",..: 51 51 51 51 51 51 51 51 51 51 ...

choose random rows for sampling
===============================

``` r
library(dplyr)
```

    ## 
    ## Attaching package: 'dplyr'

    ## The following objects are masked from 'package:stats':
    ## 
    ##     filter, lag

    ## The following objects are masked from 'package:base':
    ## 
    ##     intersect, setdiff, setequal, union

``` r
set.seed(12345)
modeldata <- sample_n(modeldata, 5000)
dim(modeldata)
```

    ## [1] 5000    8

``` r
str(modeldata)
```

    ## 'data.frame':    5000 obs. of  8 variables:
    ##  $ group_id      : Factor w/ 572 levels "010DEV","010PHP",..: 332 75 290 504 423 540 290 364 338 423 ...
    ##  $ rsvps_response: Factor w/ 3 levels "no","waitlist",..: 3 3 3 2 1 1 3 3 3 3 ...
    ##  $ rsvps_user_id : int  38977 10037 56663 57895 10108 17139 29478 18342 32355 63639 ...
    ##  $ status        : Factor w/ 5 levels "cancelled","past",..: 5 2 2 2 2 2 2 2 2 2 ...
    ##  $ venue_id      : int  67551 68278 68572 68323 68779 67346 68572 68721 67905 68779 ...
    ##  $ duration      : num  31 45.5 6 4 3 3 5.25 3 5 3 ...
    ##  $ day           : Factor w/ 7 levels "Friday","Monday",..: 3 1 1 6 5 7 1 7 2 2 ...
    ##  $ hour          : Factor w/ 62 levels "00:00","01:00",..: 21 53 47 45 51 49 47 41 45 49 ...

Partition the modeldata into two datasets for training and testing the models
=============================================================================

``` r
set.seed(12345)
trainPartition <- createDataPartition(modeldata$rsvps_response, p = 0.7, list = FALSE)
Traindata  <- modeldata[trainPartition, ]
Testdata <- modeldata[-trainPartition, ]
```

Check the dimensions and the structure of the tarining and testing data
=======================================================================

``` r
dim(Traindata)
```

    ## [1] 3502    8

``` r
dim(Testdata)
```

    ## [1] 1498    8

Further Partition the Traindata into two datasets
=================================================

``` r
set.seed(12345)
trainPartition <- createDataPartition(Traindata$rsvps_response, p = 0.7, list = FALSE)
Traindata1  <- Traindata[trainPartition, ]
Traindata2 <- Traindata[-trainPartition, ]
```

Check the dimensions and the structure of the tarining and testing data
=======================================================================

``` r
dim(Traindata1)
```

    ## [1] 2452    8

``` r
dim(Traindata2)
```

    ## [1] 1050    8

``` r
str(Traindata1)
```

    ## 'data.frame':    2452 obs. of  8 variables:
    ##  $ group_id      : Factor w/ 572 levels "010DEV","010PHP",..: 332 75 504 423 443 227 75 76 342 239 ...
    ##  $ rsvps_response: Factor w/ 3 levels "no","waitlist",..: 3 3 2 3 3 3 3 3 3 3 ...
    ##  $ rsvps_user_id : int  38977 10037 57895 63639 14978 62808 18303 34316 14440 28469 ...
    ##  $ status        : Factor w/ 5 levels "cancelled","past",..: 5 2 2 2 2 2 2 2 2 5 ...
    ##  $ venue_id      : int  67551 68278 68323 68779 67397 68716 67927 68072 68601 68712 ...
    ##  $ duration      : num  31 45.5 4 3 3 9 2 3 2.5 3 ...
    ##  $ day           : Factor w/ 7 levels "Friday","Monday",..: 3 1 6 2 5 3 7 6 6 5 ...
    ##  $ hour          : Factor w/ 62 levels "00:00","01:00",..: 21 53 45 49 51 13 26 49 47 49 ...

### Use Traindata1 to create models with Random Forest and Classification Trees

``` r
library(randomForest)
```

    ## randomForest 4.6-14

    ## Type rfNews() to see new features/changes/bug fixes.

    ## 
    ## Attaching package: 'randomForest'

    ## The following object is masked from 'package:dplyr':
    ## 
    ##     combine

    ## The following object is masked from 'package:rattle':
    ## 
    ##     importance

    ## The following object is masked from 'package:ggplot2':
    ## 
    ##     margin

``` r
controlRandom <- trainControl(method="none", number=3, verboseIter=FALSE)

RandomModel <- train(rsvps_response~., data=Traindata1,  method="rf", trControl=controlRandom, verbose=FALSE)
```

``` r
library(rpart)
Classificationmodel <- train(rsvps_response~., data=Traindata1, method="rpart", trControl=controlRandom )
```

### Test the models on Traindata2

``` r
library(randomForest)
predictTrainRand <- predict(RandomModel, newdata=Traindata2)
```

``` r
library(rpart)

predictTrainClass <- predict(Classificationmodel, newdata=Traindata2)
```

### Obtain confusionmatrices for the two models and compare

``` r
library(randomForest)
confMatRandom <- confusionMatrix(predictTrainRand, Traindata2$rsvps_response)
confMatRandom
```

    ## Confusion Matrix and Statistics
    ## 
    ##           Reference
    ## Prediction  no waitlist yes
    ##   no        56        4  91
    ##   waitlist   0        0   1
    ##   yes      246        9 643
    ## 
    ## Overall Statistics
    ##                                           
    ##                Accuracy : 0.6657          
    ##                  95% CI : (0.6363, 0.6942)
    ##     No Information Rate : 0.7             
    ##     P-Value [Acc > NIR] : 0.9926          
    ##                                           
    ##                   Kappa : 0.0713          
    ##  Mcnemar's Test P-Value : <2e-16          
    ## 
    ## Statistics by Class:
    ## 
    ##                      Class: no Class: waitlist Class: yes
    ## Sensitivity            0.18543       0.0000000     0.8748
    ## Specificity            0.87299       0.9990357     0.1905
    ## Pos Pred Value         0.37086       0.0000000     0.7160
    ## Neg Pred Value         0.72636       0.9876072     0.3947
    ## Prevalence             0.28762       0.0123810     0.7000
    ## Detection Rate         0.05333       0.0000000     0.6124
    ## Detection Prevalence   0.14381       0.0009524     0.8552
    ## Balanced Accuracy      0.52921       0.4995178     0.5327

``` r
library(rpart)

confMatClass <- confusionMatrix(predictTrainClass, Traindata2$rsvps_response)
confMatClass 
```

    ## Confusion Matrix and Statistics
    ## 
    ##           Reference
    ## Prediction  no waitlist yes
    ##   no         0        0   0
    ##   waitlist   0        0   0
    ##   yes      302       13 735
    ## 
    ## Overall Statistics
    ##                                           
    ##                Accuracy : 0.7             
    ##                  95% CI : (0.6713, 0.7276)
    ##     No Information Rate : 0.7             
    ##     P-Value [Acc > NIR] : 0.5152          
    ##                                           
    ##                   Kappa : 0               
    ##  Mcnemar's Test P-Value : NA              
    ## 
    ## Statistics by Class:
    ## 
    ##                      Class: no Class: waitlist Class: yes
    ## Sensitivity             0.0000         0.00000        1.0
    ## Specificity             1.0000         1.00000        0.0
    ## Pos Pred Value             NaN             NaN        0.7
    ## Neg Pred Value          0.7124         0.98762        NaN
    ## Prevalence              0.2876         0.01238        0.7
    ## Detection Rate          0.0000         0.00000        0.7
    ## Detection Prevalence    0.0000         0.00000        1.0
    ## Balanced Accuracy       0.5000         0.50000        0.5

Test the random forest model on the initial test data
=====================================================

``` r
library(randomForest)
predictTestData <- predict(RandomModel, newdata=Testdata)
predictTestData
```

    ##    [1] no       yes      yes      yes      yes      no       yes     
    ##    [8] yes      yes      no       no       yes      yes      yes     
    ##   [15] yes      yes      yes      yes      yes      yes      yes     
    ##   [22] yes      yes      yes      yes      yes      yes      yes     
    ##   [29] yes      yes      yes      yes      yes      yes      yes     
    ##   [36] yes      yes      yes      yes      yes      yes      yes     
    ##   [43] yes      yes      yes      no       yes      yes      yes     
    ##   [50] yes      yes      yes      yes      no       yes      yes     
    ##   [57] yes      yes      yes      yes      yes      yes      yes     
    ##   [64] yes      yes      yes      waitlist no       no       yes     
    ##   [71] yes      yes      yes      yes      no       yes      yes     
    ##   [78] yes      yes      no       yes      yes      yes      yes     
    ##   [85] yes      yes      yes      yes      yes      yes      no      
    ##   [92] yes      yes      no       yes      yes      yes      yes     
    ##   [99] no       yes      yes      no       no       yes      yes     
    ##  [106] yes      yes      yes      no       yes      yes      yes     
    ##  [113] yes      no       yes      no       no       yes      yes     
    ##  [120] yes      yes      yes      yes      yes      yes      no      
    ##  [127] yes      yes      no       yes      yes      yes      yes     
    ##  [134] yes      yes      no       yes      no       yes      no      
    ##  [141] yes      yes      yes      yes      yes      yes      no      
    ##  [148] yes      yes      yes      yes      yes      yes      yes     
    ##  [155] yes      yes      yes      yes      no       yes      no      
    ##  [162] yes      yes      yes      yes      yes      yes      yes     
    ##  [169] yes      yes      yes      yes      no       yes      no      
    ##  [176] yes      yes      yes      yes      yes      yes      yes     
    ##  [183] yes      yes      yes      yes      yes      yes      yes     
    ##  [190] yes      yes      yes      yes      yes      no       yes     
    ##  [197] yes      no       yes      yes      yes      no       no      
    ##  [204] yes      yes      yes      yes      no       yes      yes     
    ##  [211] yes      yes      no       yes      no       yes      yes     
    ##  [218] yes      yes      yes      yes      no       no       yes     
    ##  [225] yes      yes      yes      yes      yes      yes      yes     
    ##  [232] yes      yes      yes      yes      yes      no       yes     
    ##  [239] yes      yes      yes      no       no       yes      yes     
    ##  [246] yes      no       yes      yes      yes      yes      yes     
    ##  [253] yes      yes      yes      yes      yes      yes      yes     
    ##  [260] yes      yes      yes      yes      no       yes      yes     
    ##  [267] yes      yes      yes      yes      yes      no       yes     
    ##  [274] yes      yes      yes      yes      yes      yes      yes     
    ##  [281] yes      yes      yes      yes      yes      yes      yes     
    ##  [288] yes      no       yes      yes      no       yes      yes     
    ##  [295] yes      yes      yes      no       yes      yes      yes     
    ##  [302] yes      yes      yes      yes      yes      yes      yes     
    ##  [309] yes      yes      yes      yes      yes      yes      no      
    ##  [316] no       yes      yes      yes      yes      yes      yes     
    ##  [323] yes      no       no       yes      yes      yes      no      
    ##  [330] yes      yes      yes      yes      yes      yes      yes     
    ##  [337] yes      yes      yes      yes      yes      yes      yes     
    ##  [344] yes      yes      no       yes      yes      yes      yes     
    ##  [351] yes      no       yes      yes      yes      yes      yes     
    ##  [358] yes      yes      yes      yes      yes      yes      yes     
    ##  [365] yes      yes      yes      yes      yes      yes      yes     
    ##  [372] yes      yes      yes      yes      yes      yes      yes     
    ##  [379] yes      no       yes      yes      yes      no       yes     
    ##  [386] yes      yes      yes      yes      yes      yes      yes     
    ##  [393] yes      no       yes      yes      yes      yes      yes     
    ##  [400] yes      yes      yes      yes      yes      yes      yes     
    ##  [407] yes      yes      yes      no       no       yes      yes     
    ##  [414] no       yes      yes      yes      yes      yes      yes     
    ##  [421] yes      yes      yes      yes      yes      no       yes     
    ##  [428] yes      yes      no       yes      no       no       yes     
    ##  [435] yes      yes      no       yes      no       yes      no      
    ##  [442] yes      no       yes      yes      yes      yes      yes     
    ##  [449] yes      yes      waitlist yes      no       yes      yes     
    ##  [456] yes      yes      no       yes      yes      yes      yes     
    ##  [463] no       yes      yes      yes      yes      yes      yes     
    ##  [470] yes      yes      yes      yes      no       yes      yes     
    ##  [477] no       yes      yes      yes      yes      yes      yes     
    ##  [484] yes      yes      yes      yes      yes      yes      yes     
    ##  [491] yes      yes      no       yes      yes      no       yes     
    ##  [498] yes      yes      yes      yes      no       yes      yes     
    ##  [505] yes      no       yes      yes      yes      yes      yes     
    ##  [512] no       yes      yes      yes      yes      yes      no      
    ##  [519] yes      yes      yes      yes      no       yes      yes     
    ##  [526] yes      yes      yes      yes      yes      yes      yes     
    ##  [533] yes      yes      yes      no       yes      yes      yes     
    ##  [540] yes      yes      yes      yes      yes      yes      yes     
    ##  [547] yes      yes      yes      yes      yes      yes      yes     
    ##  [554] yes      yes      yes      yes      no       yes      no      
    ##  [561] yes      yes      yes      yes      yes      yes      yes     
    ##  [568] no       yes      yes      yes      yes      yes      yes     
    ##  [575] yes      yes      yes      yes      yes      no       no      
    ##  [582] yes      no       yes      yes      yes      no       yes     
    ##  [589] yes      yes      yes      yes      yes      no       yes     
    ##  [596] yes      no       yes      yes      yes      yes      yes     
    ##  [603] yes      yes      yes      no       yes      yes      no      
    ##  [610] yes      yes      yes      yes      yes      yes      yes     
    ##  [617] yes      yes      yes      yes      yes      yes      yes     
    ##  [624] yes      no       no       yes      no       yes      no      
    ##  [631] yes      yes      no       yes      yes      yes      no      
    ##  [638] yes      yes      yes      yes      no       yes      yes     
    ##  [645] yes      yes      yes      no       yes      yes      yes     
    ##  [652] yes      yes      yes      yes      yes      no       yes     
    ##  [659] no       yes      yes      yes      yes      yes      yes     
    ##  [666] yes      no       yes      yes      yes      yes      yes     
    ##  [673] yes      yes      yes      yes      yes      yes      yes     
    ##  [680] yes      yes      yes      yes      yes      yes      yes     
    ##  [687] no       yes      no       yes      yes      yes      yes     
    ##  [694] no       yes      no       yes      yes      yes      yes     
    ##  [701] yes      no       yes      no       yes      yes      no      
    ##  [708] yes      no       yes      yes      yes      yes      no      
    ##  [715] no       yes      no       yes      yes      yes      yes     
    ##  [722] yes      yes      no       yes      no       yes      yes     
    ##  [729] yes      no       yes      yes      yes      yes      no      
    ##  [736] yes      yes      yes      yes      yes      yes      yes     
    ##  [743] yes      yes      yes      yes      no       yes      yes     
    ##  [750] yes      yes      yes      yes      no       yes      yes     
    ##  [757] yes      no       yes      no       yes      yes      yes     
    ##  [764] no       no       yes      yes      yes      yes      yes     
    ##  [771] no       yes      yes      yes      yes      yes      yes     
    ##  [778] yes      yes      yes      yes      yes      yes      yes     
    ##  [785] no       yes      no       yes      yes      yes      yes     
    ##  [792] yes      yes      yes      yes      yes      yes      yes     
    ##  [799] yes      yes      yes      yes      yes      yes      no      
    ##  [806] yes      yes      no       yes      yes      yes      yes     
    ##  [813] no       yes      yes      yes      yes      yes      yes     
    ##  [820] yes      yes      no       yes      yes      yes      yes     
    ##  [827] yes      yes      yes      no       yes      yes      no      
    ##  [834] yes      yes      yes      no       no       yes      yes     
    ##  [841] yes      yes      yes      yes      yes      yes      yes     
    ##  [848] yes      no       no       yes      yes      no       yes     
    ##  [855] yes      no       no       yes      yes      yes      yes     
    ##  [862] yes      yes      yes      yes      yes      no       yes     
    ##  [869] yes      no       no       yes      yes      yes      yes     
    ##  [876] no       yes      yes      yes      no       yes      yes     
    ##  [883] yes      yes      yes      yes      yes      yes      yes     
    ##  [890] yes      yes      no       yes      yes      yes      yes     
    ##  [897] yes      no       yes      yes      yes      yes      yes     
    ##  [904] yes      yes      yes      yes      yes      yes      no      
    ##  [911] yes      yes      no       yes      yes      yes      yes     
    ##  [918] yes      yes      yes      yes      no       yes      yes     
    ##  [925] yes      no       yes      yes      yes      no       yes     
    ##  [932] yes      yes      yes      yes      yes      yes      yes     
    ##  [939] no       yes      yes      no       yes      yes      yes     
    ##  [946] yes      yes      yes      yes      no       no       no      
    ##  [953] no       yes      yes      yes      yes      yes      yes     
    ##  [960] yes      yes      yes      yes      yes      waitlist no      
    ##  [967] yes      yes      yes      yes      yes      yes      yes     
    ##  [974] yes      yes      yes      yes      yes      no       yes     
    ##  [981] yes      yes      yes      yes      yes      yes      yes     
    ##  [988] yes      no       yes      yes      yes      yes      yes     
    ##  [995] yes      yes      yes      yes      no       yes      yes     
    ## [1002] yes      yes      yes      yes      no       yes      yes     
    ## [1009] yes      yes      yes      yes      yes      yes      yes     
    ## [1016] yes      no       yes      yes      yes      yes      no      
    ## [1023] yes      yes      no       yes      no       yes      yes     
    ## [1030] yes      yes      yes      no       yes      no       no      
    ## [1037] no       yes      yes      yes      no       yes      yes     
    ## [1044] yes      yes      yes      yes      yes      yes      yes     
    ## [1051] no       yes      yes      no       yes      yes      yes     
    ## [1058] yes      yes      yes      yes      yes      no       yes     
    ## [1065] no       yes      yes      yes      yes      yes      yes     
    ## [1072] yes      yes      yes      yes      yes      yes      yes     
    ## [1079] yes      yes      no       yes      no       yes      yes     
    ## [1086] yes      yes      yes      yes      yes      yes      yes     
    ## [1093] yes      yes      yes      yes      yes      yes      yes     
    ## [1100] yes      no       yes      yes      yes      yes      yes     
    ## [1107] yes      yes      yes      yes      yes      yes      yes     
    ## [1114] yes      yes      yes      yes      yes      no       yes     
    ## [1121] yes      yes      yes      yes      yes      yes      yes     
    ## [1128] yes      yes      yes      yes      no       yes      yes     
    ## [1135] no       yes      yes      yes      yes      yes      yes     
    ## [1142] yes      yes      yes      no       no       yes      no      
    ## [1149] yes      yes      yes      yes      yes      yes      yes     
    ## [1156] yes      yes      yes      no       no       no       yes     
    ## [1163] yes      yes      no       yes      yes      yes      yes     
    ## [1170] no       yes      no       yes      no       yes      waitlist
    ## [1177] no       no       no       yes      yes      yes      yes     
    ## [1184] no       yes      yes      yes      yes      yes      yes     
    ## [1191] no       yes      yes      yes      yes      yes      no      
    ## [1198] yes      no       yes      yes      yes      yes      yes     
    ## [1205] yes      yes      yes      yes      yes      yes      yes     
    ## [1212] yes      yes      yes      yes      yes      yes      yes     
    ## [1219] yes      yes      yes      yes      yes      yes      yes     
    ## [1226] yes      no       yes      yes      yes      yes      yes     
    ## [1233] yes      yes      yes      yes      yes      yes      yes     
    ## [1240] yes      yes      yes      yes      no       yes      yes     
    ## [1247] yes      yes      yes      yes      yes      yes      yes     
    ## [1254] no       no       yes      yes      yes      no       yes     
    ## [1261] yes      yes      yes      yes      yes      yes      yes     
    ## [1268] yes      yes      yes      yes      no       yes      yes     
    ## [1275] yes      yes      yes      no       yes      yes      yes     
    ## [1282] yes      yes      yes      yes      yes      yes      yes     
    ## [1289] yes      yes      yes      yes      yes      yes      no      
    ## [1296] yes      yes      yes      yes      yes      yes      yes     
    ## [1303] yes      yes      no       yes      yes      yes      yes     
    ## [1310] yes      no       yes      no       yes      yes      yes     
    ## [1317] yes      yes      yes      yes      yes      yes      yes     
    ## [1324] yes      yes      yes      no       yes      yes      yes     
    ## [1331] yes      yes      no       yes      yes      yes      yes     
    ## [1338] yes      yes      yes      yes      yes      yes      yes     
    ## [1345] yes      yes      yes      yes      no       yes      yes     
    ## [1352] yes      yes      yes      yes      yes      yes      yes     
    ## [1359] yes      yes      yes      yes      no       yes      yes     
    ## [1366] yes      yes      yes      no       yes      yes      yes     
    ## [1373] yes      yes      yes      yes      yes      yes      yes     
    ## [1380] yes      yes      yes      no       yes      yes      yes     
    ## [1387] yes      yes      yes      yes      yes      yes      yes     
    ## [1394] yes      yes      yes      no       yes      yes      yes     
    ## [1401] no       yes      yes      yes      yes      yes      yes     
    ## [1408] yes      yes      yes      yes      yes      yes      yes     
    ## [1415] yes      yes      yes      yes      yes      yes      yes     
    ## [1422] yes      yes      yes      yes      yes      yes      yes     
    ## [1429] yes      yes      yes      yes      no       yes      yes     
    ## [1436] yes      yes      yes      yes      yes      yes      no      
    ## [1443] yes      yes      yes      yes      yes      yes      yes     
    ## [1450] no       yes      yes      yes      yes      yes      yes     
    ## [1457] yes      yes      yes      yes      yes      yes      yes     
    ## [1464] yes      yes      yes      yes      yes      yes      yes     
    ## [1471] yes      no       yes      yes      no       no       no      
    ## [1478] yes      yes      yes      yes      no       yes      yes     
    ## [1485] yes      yes      yes      yes      yes      yes      no      
    ## [1492] yes      yes      yes      yes      no       yes      yes     
    ## Levels: no waitlist yes

Test the classification model on the initial test data
======================================================

``` r
library(randomForest)
predictTestData2 <- predict(Classificationmodel, newdata=Testdata)
predictTestData2
```

    ##    [1] yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes
    ##   [18] yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes
    ##   [35] yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes
    ##   [52] yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes
    ##   [69] yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes
    ##   [86] yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes
    ##  [103] yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes
    ##  [120] yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes
    ##  [137] yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes
    ##  [154] yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes
    ##  [171] yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes
    ##  [188] yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes
    ##  [205] yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes
    ##  [222] yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes
    ##  [239] yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes
    ##  [256] yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes
    ##  [273] yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes
    ##  [290] yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes
    ##  [307] yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes
    ##  [324] yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes
    ##  [341] yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes
    ##  [358] yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes
    ##  [375] yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes
    ##  [392] yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes
    ##  [409] yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes
    ##  [426] yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes
    ##  [443] yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes
    ##  [460] yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes
    ##  [477] yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes
    ##  [494] yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes
    ##  [511] yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes
    ##  [528] yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes
    ##  [545] yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes
    ##  [562] yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes
    ##  [579] yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes
    ##  [596] yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes
    ##  [613] yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes
    ##  [630] yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes
    ##  [647] yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes
    ##  [664] yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes
    ##  [681] yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes
    ##  [698] yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes
    ##  [715] yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes
    ##  [732] yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes
    ##  [749] yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes
    ##  [766] yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes
    ##  [783] yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes
    ##  [800] yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes
    ##  [817] yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes
    ##  [834] yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes
    ##  [851] yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes
    ##  [868] yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes
    ##  [885] yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes
    ##  [902] yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes
    ##  [919] yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes
    ##  [936] yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes
    ##  [953] yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes
    ##  [970] yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes
    ##  [987] yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes
    ## [1004] yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes
    ## [1021] yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes
    ## [1038] yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes
    ## [1055] yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes
    ## [1072] yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes
    ## [1089] yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes
    ## [1106] yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes
    ## [1123] yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes
    ## [1140] yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes
    ## [1157] yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes
    ## [1174] yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes
    ## [1191] yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes
    ## [1208] yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes
    ## [1225] yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes
    ## [1242] yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes
    ## [1259] yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes
    ## [1276] yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes
    ## [1293] yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes
    ## [1310] yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes
    ## [1327] yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes
    ## [1344] yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes
    ## [1361] yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes
    ## [1378] yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes
    ## [1395] yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes
    ## [1412] yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes
    ## [1429] yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes
    ## [1446] yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes
    ## [1463] yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes
    ## [1480] yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes yes
    ## [1497] yes yes
    ## Levels: no waitlist yes
