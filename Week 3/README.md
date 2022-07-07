`Week 3` Regression Models
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
    -   🏁 Finish: Thursday, 07 July 2022

------------------------------------------------------------------------

#### Assignments & Deliverables

-   [🚀 Course Project 3
    Repository](https://github.com/AndersonUyekita/regression-models_course-project-3)
-   [📝 Quiz 3](./quiz-3_regression-models.md)

#### Slides

-   Module 3 – Multivariable Regression, Residuals, & Diagnostics
    -   03_01 GLMs
    -   03_02 Binary outcomes
    -   03_03 Count outcomes
    -   03_04 Olio

#### Description

> This week, we’ll build on last week’s introduction to multivariable
> regression with some examples and then cover residuals, diagnostics,
> variance inflation, and model comparison.

------------------------------------------------------------------------

## Class Notes

#### Multivariable regression

> We now extend linear regression so that our models can contain more
> variables. A natural first approach is to assume additive effects,
> basically extending our line to a plane, or generalized version of a
> plane as we add more variables. Multivariable regression represents
> one of the most widely used and successful methods in statistics.

> The interpretation of a multivariate regression coefficient is the
> expected change in the response per unit change in the regressor,
> holding all of the other regressors fixed.

##### Example

Multivariate using `Seatbelts` dataset.

``` r
# Loading Seatbelts dataset
library(datasets)

# Creating a object.
df <- as_tibble(datasets::Seatbelts)

# Fitting the model
fit <- lm(data = df, formula = DriversKilled ~ kms + PetrolPrice)

# Printing the coefficients results.
round(summary(fit)$coeff, 4)
```

    ##              Estimate Std. Error t value Pr(>|t|)
    ## (Intercept)  215.7461    14.6656 14.7110   0.0000
    ## kms           -0.0017     0.0006 -2.8469   0.0049
    ## PetrolPrice -643.7895   148.2896 -4.3414   0.0000

All parameters have **rejected** the
![H_0](https://latex.codecogs.com/png.image?%5Cdpi%7B110%7D&space;%5Cbg_white&space;H_0 "H_0")
hypothesis. However, it lacks interpretation because the expected number
of 215 drivers killed when the `kms` and `PetroPrice` are equal to zero
has no sense.

##### Adjusting the data

Creating a meaningful model.

``` r
# Loading Seatbelts dataset
library(datasets)

# Creating a object.
df <- as_tibble(datasets::Seatbelts)

# Centering the dataset.
df$kms_avg <- df$kms - mean(df$kms)
df$petrol_avg <- (df$PetrolPrice - mean(df$PetrolPrice))/sd(df$PetrolPrice)

# Adding new variables to the dataset.
kms_avg <- mean(df$kms)
petrol_avg <- mean(df$PetrolPrice)

# Fitting the model
fit <- lm(data = df, formula = DriversKilled ~ kms_avg + petrol_avg)

# Printing the coefficients results.
summary(fit)$coeff
```

    ##                  Estimate   Std. Error   t value      Pr(>|t|)
    ## (Intercept) 122.802083333 1.6628507162 73.850336 2.395106e-141
    ## kms_avg      -0.001749546 0.0006145401 -2.846919  4.902428e-03
    ## petrol_avg   -7.838673933 1.8055491157 -4.341435  2.304713e-05

-   The intercept of 122.8 is the estimated number of `DriversKilled`
    for the average `PetroPrice` and average `kms`;
-   The `PetroPrice` negative 7.9 means we expect less 7.9
    `DriversKilled` per **one standard deviation** change in
    `PetroPrice`, and;
-   Maintaining the other variables constant, the negative 0.017 in
    average `kms` means we expect less 0.0017 `DriversKilled` per `kms`,
    It is counter intuitive because increasing the `kms` reduces the
    drivers killed.

It is also possible to scale `kms` to analyze it in thousands.

``` r
# Loading Seatbelts dataset
library(datasets)

# Creating a object.
df <- as_tibble(datasets::Seatbelts)

# Centering the dataset.
df$kms_avg <- (df$kms - mean(df$kms))/1000
df$petrol_avg <- (df$PetrolPrice - mean(df$PetrolPrice))/sd(df$PetrolPrice)

# Adding new variables to the dataset.
kms_avg <- mean(df$kms)
petrol_avg <- mean(df$PetrolPrice)

# Fitting the model
fit <- lm(data = df, formula = DriversKilled ~ kms_avg + petrol_avg)

# Printing the coefficients results.
summary(fit)$coeff
```

    ##               Estimate Std. Error   t value      Pr(>|t|)
    ## (Intercept) 122.802083  1.6628507 73.850336 2.395106e-141
    ## kms_avg      -1.749546  0.6145401 -2.846919  4.902428e-03
    ## petrol_avg   -7.838674  1.8055491 -4.341435  2.304713e-05

-   After the scaling, we expect less 1.75 `DriversKilled` per 1000
    `kms`.
