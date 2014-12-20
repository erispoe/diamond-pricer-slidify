---
title       : Diamond pricer
subtitle    : Tells you the expected price of your diamond
author      : Thomas Favre-Bulle
job         : 
framework   : io2012        # {io2012, html5slides, shower, dzslides, ...}
highlighter : highlight.js  # {highlight.js, prettify, highlight}
hitheme     : tomorrow      # 
widgets     : []            # {mathjax, quiz, bootstrap}
mode        : selfcontained # {standalone, draft}
knit        : slidify::knit2slides
---

## The diamond pricer app

* Diamond pricer uses the ggplot2 diamonds dataset, containing data about almost 54,000 diamonds to predict the price of a diamond given some values entered by the user.

* The use can enter a limited number of informations about the diamond and get an estimate given these informations.

* Additionnaly, the user can see the expected price of his diamond is the overall distribution od diamonds, to understand if the diamond is exceptionnally expensive, or average.

--- .class #id 

## What makes the cost of a diamond

* An examination of a model of price regressed against all other predictors in the dataset shows that different variables have vastly different influence on the price.

* Carat seems to be the most influent variable, but the level of clarity and the color are also important.


```
##                 Estimate Std. Error      t value      Pr(>|t|)
## (Intercept)  5753.761857 396.629824   14.5066294  1.352581e-47
## carat       11256.978307  48.627509  231.4940348  0.000000e+00
## cut.L         584.457278  22.478150   26.0011290 3.958257e-148
## cut.Q        -301.908158  17.993919  -16.7783441  5.082665e-63
## cut.C         148.034703  15.483328    9.5609097  1.214344e-21
## cut^4         -20.793893  12.376508   -1.6801098  9.294175e-02
## color.L     -1952.160010  17.341767 -112.5698421  0.000000e+00
## color.Q      -672.053621  15.776995  -42.5970601  0.000000e+00
## color.C      -165.282926  14.724927  -11.2247022  3.323306e-29
## color^4        38.195186  13.526539    2.8237221  4.748693e-03
## color^5       -95.792932  12.776114   -7.4978145  6.588192e-14
## color^6       -48.466440  11.613917   -4.1731348  3.009074e-05
## clarity.L    4097.431318  30.258596  135.4137965  0.000000e+00
## clarity.Q   -1925.004097  28.227228  -68.1967102  0.000000e+00
## clarity.C     982.204550  24.151516   40.6684433  0.000000e+00
## clarity^4    -364.918493  19.285011  -18.9223900  1.352616e-79
## clarity^5     233.563110  15.751700   14.8278029  1.213580e-49
## clarity^6       6.883492  13.715100    0.5018915  6.157459e-01
## clarity^7      90.639737  12.103482    7.4887321  7.059940e-14
## depth         -63.806100   4.534554  -14.0710870  6.867782e-45
## table         -26.474085   2.911655   -9.0924516  1.000214e-19
## x           -1008.261098  32.897748  -30.6483316 1.601160e-204
## y               9.608886  19.332896    0.4970226  6.191751e-01
## z             -50.118891  33.486301   -1.4966983  1.344776e-01
```

--- .class #id 

## The information we can do without

* If we evaluate the marginal contribution if each variable to a regression model (the difference in variance explained r-squared between a model with all the predictors and a model without the variable), we see that each variable contributes to very little marginaly.

* This is likely because some variables are correlated, and therefore the absence of a variable is well compensated by its correlates.

* This justifies our choice to propose a prodiction engine where some variables can be omitted.


```
##   variable contribution
## 1    carat 7.972277e-02
## 2      cut 1.005939e-03
## 3    color 1.989818e-02
## 4  clarity 4.158935e-02
## 5        x 1.397386e-03
## 6        y 3.674980e-07
## 7        z 3.332509e-06
## 8    depth 2.945493e-04
## 9    table 1.229886e-04
```


--- .class #id 

## Put a price with limited information

* The original dataset includes 9 variables, but you don't necessiraly know everything about the diamond you want to estimate. Diamond pricer allows you to select only the variables that you know, and recalculates a model in real-time to predict with these variables only.

* There are 362880 possible models.

* The app uses linear models, which are relatively fast to compute. Therefore, it makes more sense to compute the model corresponding to the user's selected variables on the fly than to precompute and store them all.
