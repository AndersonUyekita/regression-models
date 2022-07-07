`Week 2` Regression Models
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
-   📆 Week 2
    -   🚦 Start: Tuesday, 05 July 2022
    -   🏁 Finish: Wednesday, 06 July 2022

------------------------------------------------------------------------

#### Assignments & Deliverables

-   [🚀 Course Project 2
    Repository](https://github.com/AndersonUyekita/regression-models_course-project-2)
-   [📝 Quiz 2](./quiz-2_regression-models.md)

#### Slides

-   Module 2 – Linear Regression & Multivariable Regression
    -   02_01 Multivariate regression
    -   02_02 Multivariate examples
    -   02_03 Adjustment
    -   02_04 Residual variation and diagnostics
    -   02_05 Multiple variables

#### Description

> This week, we will work through the remainder of linear regression and
> then turn to the first part of multivariable regression.

------------------------------------------------------------------------

## Class Notes

#### Statistical linear regression models

> Up to this point, we’ve only considered estimation. Estimation is
> useful, but we also need to know **how to extend our estimates to a
> population**. This is the process of statistical inference. Our
> approach to statistical inference will be through a statistical model.
> At the bare minimum, we need a few distributional assumptions on the
> errors. However, we’ll focus on full model assumptions under
> Gaussianity.

For this lecture we are going to use the `diamond` dataset from `UsingR`
package.

##### Case 1 – Without Centering Data

``` r
# Loading Diamond dataset in Environment.
data("diamond")

# Printing the structure
str(diamond)
```

    ## 'data.frame':    48 obs. of  2 variables:
    ##  $ carat: num  0.17 0.16 0.17 0.18 0.25 0.16 0.15 0.19 0.21 0.15 ...
    ##  $ price: int  355 328 350 325 642 342 322 485 483 323 ...

Now that we know the variables’ names let’s create a linear regression
to explain the price (in SIN units – Singapore Dollar) by the diamond
carats.

``` r
# Calculating the linear regression with intercept.
#
# lm(formula = 'What we want to explain' ~ 'What we think is a good estimator' )
#
# If we do not want intercept just add -1
#
# lm(formula = 'What we want to explain' ~ 'What we think is a good estimator' - 1 ) # With no intercept
#
fit <- lm(data = diamond, formula = price ~ carat)

# Printing results.
fit$coefficients
```

    ## (Intercept)       carat 
    ##   -259.6259   3721.0249

**Explanation/Interpretation**

> We estimate an expected 3721.02 SIN dollar increase in price for every
> carat increase in mass of diamond, and. The intercept -259.63 the
> expected price of a 0 carat diamond.

**WARNING**

It is hard to believe in paying 259.63 SIN for a diamond with 0 carats.

##### Case 2 – Centering Data

At this time, we are “centering” the dataset on creating an
interpretable model. We only need to center the carat variable.

``` r
# We do not have problem with price variable.
price <- diamond$price

# Centering carat variable.
carat_c <- diamond$carat - mean(diamond$carat)

# Fitting a new linear model
fit2 <- lm(formula = price ~ carat_c, data = data.frame("prince" = price, "carat_c" = carat_c))

# Printing the coefficients
fit2$coefficients
```

    ## (Intercept)     carat_c 
    ##    500.0833   3721.0249

**Explanation/Interpretation**

> -   Notice the estimated slope didn’t change at all.
>     -   We estimate an expected 3721.02 SIN dollar increase in price
>         for every carat increase in mass of diamond, and;
> -   Thus 500.1 SIN is the expected price for the averae sized diamond
>     of the data (0.2042 carats).

#### Predicting based on a fitted model

**Using model `fit`**

``` r
# Sample of 3 diamonds with x carats.
newx <- c(0.16, 0.27, 0.34)

# Calculating using predict.
predict(fit, newdata = data.frame(carat = newx))
```

    ##         1         2         3 
    ##  335.7381  745.0508 1005.5225

**Using model `fit2`**

``` r
# Centering the carat new sample.
newx2 <- c(0.16, 0.27, 0.34) - mean(diamond$carat)

# Calculating "manually".
fit2$coefficients[1] + fit2$coefficients[2]*newx2
```

    ## [1]  335.7381  745.0508 1005.5225

Now using `predict()`.

``` r
predict(fit2, newdata = data.frame(carat_c = newx2))
```

    ##         1         2         3 
    ##  335.7381  745.0508 1005.5225

It is important to keep the `newdata` column name in accordance with the
`data` used in `lm()`.

#### Residuals

> **Residuals represent variation left unexplained by our model.** We
> emphasize the difference between residuals and errors. The errors
> unobservable true errors from the known coefficients, while residuals
> are the observable errors from the estimated coefficients. In a sense,
> the residuals are estimates of the errors.

My model:

![Y_i = \\beta_O + \\beta_1 \\cdot X_i + \\epsilon_i](https://latex.codecogs.com/png.image?%5Cdpi%7B110%7D&space;%5Cbg_white&space;Y_i%20%3D%20%5Cbeta_O%20%2B%20%5Cbeta_1%20%5Ccdot%20X_i%20%2B%20%5Cepsilon_i "Y_i = \beta_O + \beta_1 \cdot X_i + \epsilon_i")

Where:

-   ![\\epsilon_i](https://latex.codecogs.com/png.image?%5Cdpi%7B110%7D&space;%5Cbg_white&space;%5Cepsilon_i "\epsilon_i")
    is the error in
    ![i](https://latex.codecogs.com/png.image?%5Cdpi%7B110%7D&space;%5Cbg_white&space;i "i")
    observation.

Assuming the notation
![\\hat Y_i](https://latex.codecogs.com/png.image?%5Cdpi%7B110%7D&space;%5Cbg_white&space;%5Chat%20Y_i "\hat Y_i")
to the predicted value to
![i](https://latex.codecogs.com/png.image?%5Cdpi%7B110%7D&space;%5Cbg_white&space;i "i")
observation.

![\\epsilon_i = Y_i - \\hat Y_i](https://latex.codecogs.com/png.image?%5Cdpi%7B110%7D&space;%5Cbg_white&space;%5Cepsilon_i%20%3D%20Y_i%20-%20%5Chat%20Y_i "\epsilon_i = Y_i - \hat Y_i")

So
![\\epsilon_i \\thicksim N(0,\\sigma^2)](https://latex.codecogs.com/png.image?%5Cdpi%7B110%7D&space;%5Cbg_white&space;%5Cepsilon_i%20%5Cthicksim%20N%280%2C%5Csigma%5E2%29 "\epsilon_i \thicksim N(0,\sigma^2)").
