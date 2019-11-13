# Reliability and Validity Analysis
Daniel A. Gundersen, PhD<br/>DanielA_Gundersen@dfci.harvard.edu<br/>Survey and Data Management Core<br/>Dana-Farber Cancer Institute  
November 29, 2017  
## Overview/Agenda
1. Reliability - definition and common approaches
2. Validity - definition and common approaches

## Goals 
1. Familiarize you to how reliability and validity is commonly assessed for survey items
2. Conceptually describe and illustrate some commonly used statistics 

## Reliability - Definition
* Non-technical: degree to which a measurement produces the same value on the same sample when measured in the same manner repeatedly  

## Reliability - Definition
* Non-technical: degree to which a measurement produces the same value on the same sample when measured in the same manner repeatedly 

* Technical: Ratio of variance of true score to observed score (1)
      * True score: mean of independent identically conducted measurements
      * Observed score: 'True' score + measurement error
      
$$
\begin{align}
Reliability = \frac{\sigma^2_{true}}{\sigma^2_{true} + \sigma^2_{error}}
\end{align}
$$
(1) Chmura Kraemer H, Periyakoil VS, Noda A. Kappa coefficients in medical research. Statistics in medicine. 2002 Jul 30;21(14):2109-29.

## Reliability - Steps
1. Gather data on the same measure from N samples from your population
2. Conduct M repeated measurements on variable(s) of interest 
3. Calculate a reliability statistic
4. Interpret

## Reliability
* Who is the population? Your target of inference. 
      * Patients, Clinicians, Community members, tissues, etc.

* What are the 'M' independent replications?
      * $\ge2$ Repeated measure of same survey instrument on same samples (test-retest)
      * $\ge2$ independent raters/coders on same samples (inter-rater)
      * $\ge2$ independent data entry (inter-rater)
      * $\ge2$ tests/scales of the same construct developed the same approach (parallel forms)
      * $>2$  items of a test/scale (internal consistency)

## Reliability for categorical variables 
* Nominal:
      * A survey item measuring tobacco use status (current user vs. non-user)
      * Administered to same sample of 10 patients two times, 1 day apart.
      * M = 2; N=10


```r
tobacco_1 <- c("user", "user", "user", "non-user", "non-user", 
               "non-user", "user", "non-user", "non-user", "non-user")

tobacco_2 <- c("non-user", "user", "user", "user", "non-user", 
               "non-user", "user", "non-user", "non-user", "non-user")

table(tobacco_1, tobacco_2)
```

```
##           tobacco_2
## tobacco_1  non-user user
##   non-user        5    1
##   user            1    3
```

## Reliability for categorical variables 

```r
library(irr); library(tidyverse)
tobacco <- tibble(tobacco_1, tobacco_2)

tobacco_agree <- agree(tobacco)
print(tobacco_agree)
```

```
##  Percentage agreement (Tolerance=0)
## 
##  Subjects = 10 
##    Raters = 2 
##   %-agree = 80
```

* The measurement has good **consensus**, but do they have good agreement?

## Reliability for categorical variables 

```r
print(tobacco_agree)
```

```
##  Percentage agreement (Tolerance=0)
## 
##  Subjects = 10 
##    Raters = 2 
##   %-agree = 80
```

* Is 80% agreement good?
* What level of agreement would we expect by chance?

## Reliability for categorical variables 

```r
tob_tab <- prop.table(table(tobacco_1, tobacco_2))
```

* Is 80% agreement good?
* What level of agreement would we expect by chance?

$$
      Chance Agreement = P_{r1}*P_{c1} + P_{r2}*P_{c2}
$$


```r
chance_agree <- sum(tob_tab[1,1:2])*sum(tob_tab[1:2,1]) +
                sum(tob_tab[2,1:2])*sum(tob_tab[1:2,2])

print(chance_agree)
```

```
## [1] 0.52
```

## Reliability for categorical variables 

```r
tob_tab <- prop.table(table(tobacco_1, tobacco_2))
print(tob_tab)
```

```
##           tobacco_2
## tobacco_1  non-user user
##   non-user      0.5  0.1
##   user          0.1  0.3
```

* Is 80% agreement good?
* What level of agreement would we expect by chance?
* Cohen's Kappa is agreement above and beyond chance for M=2

$$
      Cohen's\ Kappa = \frac{P_o - P_c}{1 - P_c}
$$

* Theoretical range is -1 to 1

## Reliability for categorical variables 

```r
tob_kappa <- kappa2(tobacco)
print(tob_kappa)
```

```
##  Cohen's Kappa for 2 Raters (Weights: unweighted)
## 
##  Subjects = 10 
##    Raters = 2 
##     Kappa = 0.583 
## 
##         z = 1.84 
##   p-value = 0.0651
```

* Some rules of thumb/guidelines exist for what constitutes good agreement(1)
* Best interpretation is rooted in your field.  

(1) Landis JR, Koch GG. The measurement of observer agreement for categorical data. biometrics. 1977 Mar 1:159-74.

## Reliability for categorical variables {.smaller}
* Kappa Paradox
* With badly skewed distributions, consensus can be high while agreement is low

```
##           tobacco_2
## tobacco_1  non-user user
##   non-user        8    1
##   user            1    0
```

```
##  Percentage agreement (Tolerance=0)
## 
##  Subjects = 10 
##    Raters = 2 
##   %-agree = 80
```

```
##  Cohen's Kappa for 2 Raters (Weights: unweighted)
## 
##  Subjects = 10 
##    Raters = 2 
##     Kappa = -0.111 
## 
##         z = -0.351 
##   p-value = 0.725
```

* Recommended to report consensus with agreement

## Reliability for categorical variables 
* Ordinal Variables have opportunity for *degrees* of disagreement on a given sample member

## Reliability for categorical variables 
* Ordinal Variables have opportunity for *degrees* of disagreement on a given sample member


```r
library(forcats)
tobacco_1 <- as.ordered(c("everyday", "someday", "everyday", "someday", "someday", 
               "non-user", "non-user", "non-user", "non-user", "non-user"))
tobacco_1 <- fct_relevel(tobacco_1, "everyday", after = 2)

tobacco_2 <- as.ordered(c("non-user", "everyday", "everyday", "someday", "someday",
               "someday", "non-user", "non-user", "non-user", "non-user"))
tobacco_2 <- fct_relevel(tobacco_2, "everyday", after = 2)

table(tobacco_1, tobacco_2)
```

```
##           tobacco_2
## tobacco_1  non-user someday everyday
##   non-user        4       1        0
##   someday         0       2        1
##   everyday        1       0        1
```

## Reliability for categorical variables 
* Ordinal Variables have opportunity for *degrees* of disagreement
* Greater distance of non- rating can be penalized by weighting
* You can specify any weighting matrix, common ones are linear and quadratic


```r
kappas <- NULL
kappas[1] <- kappa2(tibble(tobacco_1, tobacco_2), weight = "squared")$value
kappas[2] <- kappa2(tibble(tobacco_1, tobacco_2), weight = "equal")$value
kappas[3] <- kappa2(tibble(tobacco_1, tobacco_2))$value

weights <- c("Squared", "Linear", "Unweighted")
a <- data.frame(weights, kappas)
knitr::kable(a, digits = 3)
```



weights       kappas
-----------  -------
Squared        0.388
Linear         0.459
Unweighted     0.516

## Reliability for categorical variables
* Fleiss' Kappa extends this concept to M>2(1)
* Different versions of ICC for different designs (2)

1. Fleiss, J. L. (1971) "Measuring nominal scale agreement among many raters." Psychological Bulletin, Vol. 76, No. 5 pp. 378-382
2. Revelle, W. (in prep) Classical Test Theory and the Measurement of
Reliability. In. An introduction to psychometric theory with applications in R. Springer. (Working Draft Accessed on November 29, 2017 from: http://www.personality-project.org/r/book/Chapter7.pdf)

## Reliability for interval and continuous variables
* Example:
      * Weight in kilograms measured on a scale two times, one hour apart
      * M=2, N=1000


```r
library(MASS)
set.seed(1981)
wts.df <- mvrnorm(1000, mu=c(75,70), Sigma=matrix(c(5,4, 
                                                4,5),2,2))
wts.df <- as_tibble(wts.df)
plot(wts.df$V1, wts.df$V2)
```

![](reliability-validity-analysis_files/figure-slidy/unnamed-chunk-11-1.png)<!-- -->

## Reliability for interval and continuous variables


```r
cor(wts.df)
```

```
##           V1        V2
## V1 1.0000000 0.8043473
## V2 0.8043473 1.0000000
```

```r
mean_diff <- mean(wts.df$V1)-mean(wts.df$V2)
```

* Strong positive correlation
* Difference in mean of 5
* The scales are consistent, but do they have good agreement?

## Reliability for interval and continuous variables
* Correlation indicate the two scales are highly consistent, but they differ by roughly 5, on average.


```r
icc(wts.df, model = "twoway", type = "agreement")
```

```
##  Single Score Intraclass Correlation
## 
##    Model: twoway 
##    Type : agreement 
## 
##    Subjects = 1000 
##      Raters = 2 
##    ICC(A,1) = 0.228
## 
##  F-Test, H0: r0 = 0 ; H1: r0 > 0 
## F(999,1.46) = 9.22 , p = 0.166 
## 
##  95%-Confidence Interval for ICC Population Values:
##   -0.035 < ICC < 0.579
```

* By factoring in this difference in means, the scales are found to have much lower agreement

## Validity of survey items - Definition:
* Non-technical: Degree to which a variable measured what it purports to measure
* Technical: 'Proportion of observed variance that reflects variance in the construct it intended to measure'(1)

(1) Chmura Kraemer H, Periyakoil VS, Noda A. Kappa coefficients in medical research. Statistics in medicine. 2002 Jul 30;21(14):2109-29.

## Validity of survey items - Definition:
* Validity:
      * Does self-report correlate or agree with gold-standard bio-measure?
      * In absence of gold standard, does it correlate or agree with other existing measurements?

## Validity of survey items
* Common scenario is to (wish to) measure via self-report data that are expensive or logistically difficult to collect via bio- or clinical-measurement

* A key difference from reliability analysis is 'gold standard' is a *different* variable
      * This may affect which statisics are reasonable to use
      * Agreement not always best to use
      * Correlations/association measures commonly used

## Validity of survey items 
* Example:
      * Is self-reported smoking a valid measure of cigarette smoking?
      * Survey measure: Do  you currently smoke everyday, some days, or not at all\
            * Everyday or some days = smoker; not at all = non-smoker
      * Bio-marker:\
            * Smoker >= 50 ng/ml; Non-smoker < 50 ng/ml


```r
smoker_bio <- as.factor(c("smoker", "smoker", "smoker", "non-smoker", "non-smoker",
                "non-smoker", "non-smoker", "non-smoker", "non-smoker", "non-smoker"))
smoker_self <- as.factor(c("non-smoker", "smoker", "smoker", "non-smoker", "non-smoker",
                "non-smoker", "non-smoker", "non-smoker", "non-smoker", "non-smoker"))
table(smoker_self, smoker_bio)
```

```
##             smoker_bio
## smoker_self  non-smoker smoker
##   non-smoker          7      1
##   smoker              0      2
```
* Real example:
Wong SL, Shields M, Leatherdale S, Malaison E, Hammond D. Assessment of validity of self-reported smoking status. Health reports. 2012 Mar 1;23(1):D1.

## Validity of survey items
* Validity may be asked as:
      * What proportion of true smokers (bio measure) are classified as smokers based on self-report (sensitivity)?
      * What proportion of true non-smokers (bio measure) are classified as non-smokers via self-report? (specificity)
      * What proportion of those classified as smokers via self-report are truly smokers? (positive predictive value)
      * What proportion of those classified as non-smokers via self-report are truly non-smokers? (positive predictive value)
      
## Validity of survey items     

```r
library(caret)
smoker_sensitivity <- sensitivity(smoker_self, smoker_bio)
smoker_specificity <- specificity(smoker_self, smoker_bio)
smoker_ppv <- posPredValue(smoker_self, smoker_bio)
smoker_npv <- negPredValue(smoker_self, smoker_bio)

smoker_valid <- data.frame(smoker_sensitivity, smoker_specificity,
                           smoker_ppv, smoker_npv)

knitr::kable(smoker_valid, digits = 2, 
             col.names = c("Sensitivity", "Specificity", "PPV", "NPV"), align = "c")
```



 Sensitivity    Specificity    PPV     NPV 
-------------  -------------  ------  -----
      1            0.67        0.88     1  

## Validity of survey items 
* Which statistic to emphasize in interpretation depends on consequence of misclassification
      * If missclassifying a true positive case has large consequence, sensitivity should be emphasized
      * If missclassifying a negative case has large consequence, sensitivity should be emphasized
      
## Resources:
* Chmura Kraemer H, Periyakoil VS, Noda A. Kappa coefficients in medical research. Statistics in medicine. 2002 Jul 30;21(14):2109-29.
* Fleiss, J. L. (1971) "Measuring nominal scale agreement among many raters." Psychological Bulletin, Vol. 76, No. 5 pp. 378-382
* Landis JR, Koch GG. The measurement of observer agreement for categorical data. biometrics. 1977 Mar 1:159-74.
* Revelle, W. (in prep) Classical Test Theory and the Measurement of
Reliability. In. An introduction to psychometric theory with applications in R. Springer. (Working Draft Accessed on November 29, 2017 from: http://www.personality-project.org/r/book/Chapter7.pdf)
