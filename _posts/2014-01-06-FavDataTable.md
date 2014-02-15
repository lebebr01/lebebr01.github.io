---
layout: post
title: Two of my favorite data.table features
tags: [R, data.table]
---

When I started to use the *data.table* package I was primarily using it to aggregate.  I had read about *data.table* and its blazing speed compared to the other options from base or the *plyr* package especially with large amounts of data.  As an example, I remember calculating averages or percentages while at Saint Paul Public Schools and while the calculations were running would walk away for 5 minutes to wait for them to finish.  When using *data.table* to do the same calculations I didn't need to wait 5 minutes to see the calculated values.

The speed of *data.table* is publicized widely, however there are two features found in *data.table* that are not talked about as frequently that I use very often.

***************

## Add aggregated variables to the raw data file
The ability to add aggregated variables to the raw data file can be very helpful in numerous data situations.  At Saint Paul Public Schools I used this feature to give differing levels of data to external clients requesting data.  I also used this feature when creating graphics.  Outside of the district world, this feature is extremely helpful when fitting linear mixed models with *lme4* or *nlme*.  Adding aggregated variables is needed if you want to add variables at any of the cluster levels (unless you calculate them on the fly with the **I()** command).  Instead of creating a set of aggregated variables in a new data frame and merging back in, *data.table* allows you to do it one one command.  Here is a small example:


``` r
# generate a small dataset
set.seed(1234)
smalldat <- data.frame(group1 = rep(1:2, each = 5), 
                       group2 = rep(c('a','b'), times = 5), 
                       x = rnorm(10))

# convert to data.frame to data.table
library(data.table)
smalldat <- data.table(smalldat)

# convert aggregated variable into raw data file
smalldat[, aggGroup1 := mean(x), by = group1]
```

#### Output 
``` r
##     group1 group2       x aggGroup1
##  1:      1      a -1.2071   -0.3524
##  2:      1      b  0.2774   -0.3524
##  3:      1      a  1.0844   -0.3524
##  4:      1      b -2.3457   -0.3524
##  5:      1      a  0.4291   -0.3524
##  6:      2      b  0.5061   -0.4140
##  7:      2      a -0.5747   -0.4140
##  8:      2      b -0.5466   -0.4140
##  9:      2      a -0.5645   -0.4140
## 10:      2      b -0.8900   -0.4140
```

``` r
# aggregate with 2 variables
smalldat[, aggGroup1.2 := mean(x), by = list(group1, group2)]
```

#### Output
``` r
##     group1 group2       x aggGroup1 aggGroup1.2
##  1:      1      a -1.2071   -0.3524      0.1022
##  2:      1      b  0.2774   -0.3524     -1.0341
##  3:      1      a  1.0844   -0.3524      0.1022
##  4:      1      b -2.3457   -0.3524     -1.0341
##  5:      1      a  0.4291   -0.3524      0.1022
##  6:      2      b  0.5061   -0.4140     -0.3102
##  7:      2      a -0.5747   -0.4140     -0.5696
##  8:      2      b -0.5466   -0.4140     -0.3102
##  9:      2      a -0.5645   -0.4140     -0.5696
## 10:      2      b -0.8900   -0.4140     -0.3102
```

The key part of the syntax is the **:=**, which tells *data.table* to compute an aggregated variable and merge it back into the original data.  This is very compact syntax to create aggregated variables that are automatically placed within the original data.  The only drawback is the inability to create more than one aggregated variable at a time.  If I wanted to created the mean and the median of x for each level of the variable *group1*, I would have to write two commands.  If a lot of variables need to be aggregated in this fashion, it may be more concise to create an aggregated data set and merge back into the original.  Below is an example of what I mean by aggregate then merge back:


``` r
library(plyr)

# create aggregated data
aggdat1 <- ddply(smalldat, .(group1), summarize,
                 aggGroup1plyr = mean(x))
aggdat12 <- ddply(smalldat, .(group1, group2), summarize, 
                aggGroup1.1plyr = mean(x))

# join back into data
smalldat <- join(smalldat, aggdat1, by = 'group1')
smalldat <- join(smalldat, aggdat12, by = c('group1', 'group2'))

# print data
smalldat
```

#### Output
``` r
##     group1 group2       x aggGroup1 aggGroup1.2 aggGroup1plyr
##  1:      1      a -1.2071   -0.3524      0.1022       -0.3524
##  2:      1      b  0.2774   -0.3524     -1.0341       -0.3524
##  3:      1      a  1.0844   -0.3524      0.1022       -0.3524
##  4:      1      b -2.3457   -0.3524     -1.0341       -0.3524
##  5:      1      a  0.4291   -0.3524      0.1022       -0.3524
##  6:      2      b  0.5061   -0.4140     -0.3102       -0.4140
##  7:      2      a -0.5747   -0.4140     -0.5696       -0.4140
##  8:      2      b -0.5466   -0.4140     -0.3102       -0.4140
##  9:      2      a -0.5645   -0.4140     -0.5696       -0.4140
## 10:      2      b -0.8900   -0.4140     -0.3102       -0.4140
##     aggGroup1.1plyr
##  1:          0.1022
##  2:         -1.0341
##  3:          0.1022
##  4:         -1.0341
##  5:          0.1022
##  6:         -0.3102
##  7:         -0.5696
##  8:         -0.3102
##  9:         -0.5696
## 10:         -0.3102
```


The above code using plyr likely won't take longer for R to run, however it does take more time to write out the code.

*************

## Removing duplicate observations
For most situations, using *data.table* has become my go to way to remove duplicate observations.  This is especially useful when it does not matter which value of the variables you want to keep in the final data set (e.g. when values of the variables are repeated).  The ability of *data.table* to create keyed values makes it extremely easy to remove duplicate observations based on those keyed variables.

Using the dataset created above:

``` r
# Set keys - this sorts the data based on these values
setkeyv(smalldat, c('group1','group2'))

# keep unique observations (I also remove the variable x)
uniqdat <- subset(unique(smalldat), select = -x)

# print data
uniqdat
```

``` r
##    group1 group2 aggGroup1 aggGroup1.2 aggGroup1plyr aggGroup1.1plyr
## 1:      1      a   -0.3524      0.1022       -0.3524          0.1022
## 2:      1      b   -0.3524     -1.0341       -0.3524         -1.0341
## 3:      2      a   -0.4140     -0.5696       -0.4140         -0.5696
## 4:      2      b   -0.4140     -0.3102       -0.4140         -0.3102
``` 


The code above first sets two keys for the data.table.  The key acts as an identifier and the data are automatically sorted based on the key variables.  This is one of the reasons why the *data.table* package can be so fast at doing many of its tasks.  Then unique observations are kept.  The *unique* function in the *data.table* package is similar to the same function in the base package, but when keys are defined for data.table, the *unique* function automatically selects unique observations based on those key variables.  

For more complicated cases, I tend to use a combination of *order* and *duplicated* from base R, however for easy cases where observations are repeated on the variables I want to keep, this is a quick and easy way to remove duplicate observations.