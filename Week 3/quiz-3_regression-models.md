`Quiz 3` Regression Models
================

-   👨🏻‍💻 Author: Anderson H Uyekita
-   📚 Specialization: <a
    href="https://www.coursera.org/specializations/data-science-foundations-r"
    target="_blank" rel="noopener">Data Science: Foundations using R
    Specialization</a>
-   📖 Course:
    <a href="https://www.coursera.org/learn/regression-models"
    target="_blank" rel="noopener">Regression Models</a>
    -   🧑‍🏫 Instructor: Brian Caffo
-   📆 Week 3
    -   🚦 Start: Tuesday, 05 July 2022
    -   🏁 Finish: Monday, 18 July 2022
-   🌎 Rpubs: [Interactive
    Document](https://rpubs.com/AndersonUyekita/quiz-3_regression-models)

------------------------------------------------------------------------

## Question 1

Consider the `mtcars` data set. Fit a model with mpg as the outcome that
includes number of cylinders as a factor variable and weight as
confounder. Give the adjusted estimate for the expected change in mpg
comparing 8 cylinders to 4.

-   [ ] 33.991
-   [x] -6.071
-   [ ] -3.206
-   [ ] -4.256

**Answer**

``` r
# Fitting a model using wt and cyl.
fit_q1 <- lm(data = mtcars, formula = mpg ~ wt + factor(cyl))

# Printing the coefficients.
summary(fit_q1)$coeff
```

    ##               Estimate Std. Error   t value     Pr(>|t|)
    ## (Intercept)  33.990794  1.8877934 18.005569 6.257246e-17
    ## wt           -3.205613  0.7538957 -4.252065 2.130435e-04
    ## factor(cyl)6 -4.255582  1.3860728 -3.070244 4.717834e-03
    ## factor(cyl)8 -6.070860  1.6522878 -3.674214 9.991893e-04

The interpretation of `cyl` coefficients is based on cyl 4, the
baseline. So the coefficient -6.07 is in comparison to the cyl 4.

## Question 2

Consider the `mtcars` data set. Fit a model with mpg as the outcome that
includes number of cylinders as a factor variable and weight as a
possible confounding variable. Compare the effect of 8 versus 4
cylinders on mpg for the adjusted and unadjusted by weight models. Here,
adjusted means including the weight variable as a term in the regression
model and unadjusted means the model without weight included. What can
be said about the effect comparing 8 and 4 cylinders after looking at
models with and without weight included?

-   [ ] Holding weight constant, cylinder appears to have more of an
    impact on mpg than if weight is disregarded.
-   [x] Holding weight constant, cylinder appears to have less of an
    impact on mpg than if weight is disregarded.
-   [ ] Within a given weight, 8 cylinder vehicles have an expected 12
    mpg drop in fuel efficiency.
-   [ ] Including or excluding weight does not appear to change anything
    regarding the estimated impact of number of cylinders on mpg.

**Answer**

``` r
# Printing the coefficients.
summary(lm(data = mtcars, formula = mpg ~ factor(cyl)))$coeff;
```

    ##                Estimate Std. Error   t value     Pr(>|t|)
    ## (Intercept)   26.663636  0.9718008 27.437347 2.688358e-22
    ## factor(cyl)6  -6.920779  1.5583482 -4.441099 1.194696e-04
    ## factor(cyl)8 -11.563636  1.2986235 -8.904534 8.568209e-10

``` r
summary(lm(data = mtcars, formula = mpg ~ wt + factor(cyl)))$coeff
```

    ##               Estimate Std. Error   t value     Pr(>|t|)
    ## (Intercept)  33.990794  1.8877934 18.005569 6.257246e-17
    ## wt           -3.205613  0.7538957 -4.252065 2.130435e-04
    ## factor(cyl)6 -4.255582  1.3860728 -3.070244 4.717834e-03
    ## factor(cyl)8 -6.070860  1.6522878 -3.674214 9.991893e-04

Comparing the two fitted models, when wt has disregarded the influence
in cyl in greater.

## Question 3

Consider the `mtcars` data set. Fit a model with mpg as the outcome that
considers number of cylinders as a factor variable and weight as
confounder. Now fit a second model with mpg as the outcome model that
considers the interaction between number of cylinders (as a factor
variable) and weight. Give the P-value for the likelihood ratio test
comparing the two models and suggest a model using 0.05 as a type I
error rate significance benchmark.

-   [ ] The P-value is small (less than 0.05). So, according to our
    criterion, we reject, which suggests that the interaction term is
    necessary
-   [ ] The P-value is small (less than 0.05). Thus it is surely true
    that there is an interaction term in the true model.
-   [ ] The P-value is small (less than 0.05). So, according to our
    criterion, we reject, which suggests that the interaction term is
    not necessary.
-   [ ] The P-value is larger than 0.05. So, according to our criterion,
    we would fail to reject, which suggests that the interaction terms
    is necessary.
-   [ ] The P-value is small (less than 0.05). Thus it is surely true
    that there is no interaction term in the true model.
-   [x] The P-value is larger than 0.05. So, according to our criterion,
    we would fail to reject, which suggests that the interaction terms
    may not be necessary.

**Answer**

``` r
fit_q3_1 <- lm(data = mtcars, formula = mpg ~ factor(cyl) + wt)
fit_q3_2 <- lm(data = mtcars, formula = mpg ~ factor(cyl) * wt)

round(summary(fit_q3_1)$coeff,4); round(summary(fit_q3_2)$coeff,4)
```

    ##              Estimate Std. Error t value Pr(>|t|)
    ## (Intercept)   33.9908     1.8878 18.0056   0.0000
    ## factor(cyl)6  -4.2556     1.3861 -3.0702   0.0047
    ## factor(cyl)8  -6.0709     1.6523 -3.6742   0.0010
    ## wt            -3.2056     0.7539 -4.2521   0.0002

    ##                 Estimate Std. Error t value Pr(>|t|)
    ## (Intercept)      39.5712     3.1939 12.3895   0.0000
    ## factor(cyl)6    -11.1624     9.3553 -1.1932   0.2436
    ## factor(cyl)8    -15.7032     4.8395 -3.2448   0.0032
    ## wt               -5.6470     1.3595 -4.1538   0.0003
    ## factor(cyl)6:wt   2.8669     3.1173  0.9197   0.3662
    ## factor(cyl)8:wt   3.4546     1.6273  2.1229   0.0434

``` r
anova(fit_q3_1, fit_q3_2)
```

    ## Analysis of Variance Table
    ## 
    ## Model 1: mpg ~ factor(cyl) + wt
    ## Model 2: mpg ~ factor(cyl) * wt
    ##   Res.Df    RSS Df Sum of Sq      F Pr(>F)
    ## 1     28 183.06                           
    ## 2     26 155.89  2     27.17 2.2658 0.1239

## Question 4

Consider the `mtcars` data set. Fit a model with mpg as the outcome that
includes number of cylinders as a factor variable and weight inlcuded in
the model as

    lm(mpg ~ I(wt * 0.5) + factor(cyl), data = mtcars)

How is the `wt` coefficient interpretted?

-   [ ] The estimated expected change in MPG per one ton increase in
    weight.
-   [ ] The estimated expected change in MPG per half ton increase in
    weight for the average number of cylinders.
-   [ ] The estimated expected change in MPG per half ton increase in
    weight for for a specific number of cylinders (4, 6, 8).
-   [ ] The estimated expected change in MPG per half ton increase in
    weight.
-   [x] The estimated expected change in MPG per one ton increase in
    weight for a specific number of cylinders (4, 6, 8).

**Answer**

``` r
summary(lm(mpg ~ I(wt * 0.5) + factor(cyl), data = mtcars))$coeff;
```

    ##               Estimate Std. Error   t value     Pr(>|t|)
    ## (Intercept)  33.990794   1.887793 18.005569 6.257246e-17
    ## I(wt * 0.5)  -6.411227   1.507791 -4.252065 2.130435e-04
    ## factor(cyl)6 -4.255582   1.386073 -3.070244 4.717834e-03
    ## factor(cyl)8 -6.070860   1.652288 -3.674214 9.991893e-04

``` r
summary(lm(mpg ~ wt + factor(cyl), data = mtcars))$coeff;
```

    ##               Estimate Std. Error   t value     Pr(>|t|)
    ## (Intercept)  33.990794  1.8877934 18.005569 6.257246e-17
    ## wt           -3.205613  0.7538957 -4.252065 2.130435e-04
    ## factor(cyl)6 -4.255582  1.3860728 -3.070244 4.717834e-03
    ## factor(cyl)8 -6.070860  1.6522878 -3.674214 9.991893e-04

The `0.5` will not affect the interpretation or change the results of
the final model.

![mpg = \\beta_0 + \\beta_1 \\cdot wt + \\beta_2 \\cdot cyl_6 + \\beta_3 \\cdot cyl_8](https://latex.codecogs.com/png.image?%5Cdpi%7B110%7D&space;%5Cbg_white&space;mpg%20%3D%20%5Cbeta_0%20%2B%20%5Cbeta_1%20%5Ccdot%20wt%20%2B%20%5Cbeta_2%20%5Ccdot%20cyl_6%20%2B%20%5Cbeta_3%20%5Ccdot%20cyl_8 "mpg = \beta_0 + \beta_1 \cdot wt + \beta_2 \cdot cyl_6 + \beta_3 \cdot cyl_8")

For model 1:

![mpg = 33.99 -6.41 \\cdot wt \\cdot 0.5 -4.25 \\cdot cyl_6 -6.07 \\cdot cyl_8](https://latex.codecogs.com/png.image?%5Cdpi%7B110%7D&space;%5Cbg_white&space;mpg%20%3D%2033.99%20-6.41%20%5Ccdot%20wt%20%5Ccdot%200.5%20-4.25%20%5Ccdot%20cyl_6%20-6.07%20%5Ccdot%20cyl_8 "mpg = 33.99 -6.41 \cdot wt \cdot 0.5 -4.25 \cdot cyl_6 -6.07 \cdot cyl_8")

For model 2:

![mpg = 33.99 -3.206 \\cdot wt -4.25 \\cdot cyl_6 -6.07 \\cdot cyl_8](https://latex.codecogs.com/png.image?%5Cdpi%7B110%7D&space;%5Cbg_white&space;mpg%20%3D%2033.99%20-3.206%20%5Ccdot%20wt%20-4.25%20%5Ccdot%20cyl_6%20-6.07%20%5Ccdot%20cyl_8 "mpg = 33.99 -3.206 \cdot wt -4.25 \cdot cyl_6 -6.07 \cdot cyl_8")

In the end, both models has the same equation.

## Question 5

Consider the following data set

``` r
x <- c(0.586, 0.166, -0.042, -0.614, 11.72)
y <- c(0.549, -0.026, -0.127, -0.751, 1.344)
```

Give the hat diagonal for the most influential point

-   [ ] 0.2287
-   [ ] 0.2025
-   [x] 0.9946
-   [ ] 0.2804

**Answer**

``` r
max(influence(lm(y ~ x))$hat)
```

    ## [1] 0.9945734

## Question 6

Consider the following data set

``` r
x <- c(0.586, 0.166, -0.042, -0.614, 11.72)
y <- c(0.549, -0.026, -0.127, -0.751, 1.344)
```

Give the slope dfbeta for the point with the highest hat value.

-   [x] -134
-   [ ] 0.673
-   [ ] -.00134
-   [ ] -0.378

**Answer**

``` r
influence.measures(lm(y ~ x))
```

    ## Influence measures of
    ##   lm(formula = y ~ x) :
    ## 
    ##    dfb.1_     dfb.x     dffit cov.r   cook.d   hat inf
    ## 1  1.0621 -3.78e-01    1.0679 0.341 2.93e-01 0.229   *
    ## 2  0.0675 -2.86e-02    0.0675 2.934 3.39e-03 0.244    
    ## 3 -0.0174  7.92e-03   -0.0174 3.007 2.26e-04 0.253   *
    ## 4 -1.2496  6.73e-01   -1.2557 0.342 3.91e-01 0.280   *
    ## 5  0.2043 -1.34e+02 -149.7204 0.107 2.70e+02 0.995   *

## Question 7

Consider a regression relationship between Y and X with and without
adjustment for a third variable Z. Which of the following is true about
comparing the regression coefficient between Y and X with and without
adjustment for Z.

-   [ ] For the the coefficient to change sign, there must be a
    significant interaction term.
-   [x] It is possible for the coefficient to reverse sign after
    adjustment. For example, it can be strongly significant and positive
    before adjustment and strongly significant and negative after
    adjustment.
-   [ ] Adjusting for another variable can only attenuate the
    coefficient toward zero. It can’t materially change sign.
-   [ ] The coefficient can’t change sign after adjustment, except for
    slight numerical pathological cases.
