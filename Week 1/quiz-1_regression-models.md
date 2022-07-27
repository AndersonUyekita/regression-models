`Quiz 1` Regression Models
================

-   👨🏻‍💻 Author: Anderson H Uyekita
-   📚 Specialization: <a
    href="https://www.coursera.org/specializations/data-science-statistics-machine-learning"
    target="_blank" rel="noopener">Data Science: Statistics and Machine
    Learning Specialization</a>
-   📖 Course:
    <a href="https://www.coursera.org/learn/regression-models"
    target="_blank" rel="noopener">Regression Models</a>
    -   🧑‍🏫 Instructor: Brian Caffo
-   📆 Week 1
    -   🚦 Start: Tuesday, 05 July 2022
    -   🏁 Finish: Monday, 18 July 2022
-   🌎 Rpubs: [Interactive
    Document](https://rpubs.com/AndersonUyekita/quiz-1_regression-models)

------------------------------------------------------------------------

## Question 1

Consider the data set given below

``` r
x <- c(0.18, -1.54, 0.42, 0.95)
```

And weights given by

``` r
w <- c(2, 1, 3, 1)
```

Give the value of
![\mu](https://latex.codecogs.com/png.image?%5Cdpi%7B110%7D&space;%5Cbg_white&space;%5Cmu "\mu")
that minimizes the least squares equation

![\sum\_{i = 1}^{n}{w_i \cdot (x_i - \mu)^2}](https://latex.codecogs.com/png.image?%5Cdpi%7B110%7D&space;%5Cbg_white&space;%5Csum_%7Bi%20%3D%201%7D%5E%7Bn%7D%7Bw_i%20%5Ccdot%20%28x_i%20-%20%5Cmu%29%5E2%7D "\sum_{i = 1}^{n}{w_i \cdot (x_i - \mu)^2}")

-   [ ] 0.300
-   [x] 0.1471
-   [ ] 1.077
-   [ ] 0.0025

**Answer**

The
![\mu](https://latex.codecogs.com/png.image?%5Cdpi%7B110%7D&space;%5Cbg_white&space;%5Cmu "\mu")
value that minimizes the given function should be the mean. For this
reason, I will calculate the weighted average.

![\mu = \frac{\sum{x_i \cdot w_i}}{\sum{w_i}}](https://latex.codecogs.com/png.image?%5Cdpi%7B110%7D&space;%5Cbg_white&space;%5Cmu%20%3D%20%5Cfrac%7B%5Csum%7Bx_i%20%5Ccdot%20w_i%7D%7D%7B%5Csum%7Bw_i%7D%7D "\mu = \frac{\sum{x_i \cdot w_i}}{\sum{w_i}}")

``` r
# Calculating the weighted average.
sum(x * w)/sum(w)
```

    ## [1] 0.1471429

## Question 2

Consider the following data set:

``` r
x <- c(0.8, 0.47, 0.51, 0.73, 0.36, 0.58, 0.57, 0.85, 0.44, 0.42)
y <- c(1.39, 0.72, 1.55, 0.48, 1.19, -1.59, 1.23, -0.65, 1.49, 0.05)
```

Fit the regression through the origin and get the slope treating y as
the outcome and x as the regressor.

*(Hint, do not center the data since we want regression through the
origin, not through the means of the data.)*

-   [ ] 0.59915
-   [ ] -0.04462
-   [x] 0.8263
-   [ ] -1.713

**Answer**

Based on the `lm()` function, it is necessary to set the regression
without Interceptor.

``` r
# Creating a data frame.
df_q2 <- data.frame(x, y)

# Fitting a linear regression model without interceptor (using -1).
fit_q2 <- lm(data = df_q2, formula = y ~ x - 1)

# Printing the coefficients
summary(fit_q2)$coeff
```

    ##    Estimate Std. Error t value Pr(>|t|)
    ## x 0.8262517  0.5816544 1.42052  0.18916

## Question 3

Do `data(mtcars)` from the datasets package and fit the regression model
with **mpg** as the outcome and **weight** as the predictor. Give the
slope coefficient.

-   [ ] 30.2851
-   [ ] 0.5591
-   [ ] -9.559
-   [x] -5.344

**Answer**

The model is quite simple:

``` r
# Fitting a model using mpg and weight.
fit_q3 <- lm(data = mtcars, formula = mpg ~ wt)

# Printing the coefficients
summary(fit_q3)$coeff
```

    ##              Estimate Std. Error   t value     Pr(>|t|)
    ## (Intercept) 37.285126   1.877627 19.857575 8.241799e-19
    ## wt          -5.344472   0.559101 -9.559044 1.293959e-10

## Question 4

Consider data with an outcome (Y) and a predictor (X). The standard
deviation of the predictor is one half that of the outcome. The
correlation between the two variables is `0.5`. What value would the
slope coefficient for the regression model with
![Y](https://latex.codecogs.com/png.image?%5Cdpi%7B110%7D&space;%5Cbg_white&space;Y "Y")
as the outcome and
![X](https://latex.codecogs.com/png.image?%5Cdpi%7B110%7D&space;%5Cbg_white&space;X "X")
as the predictor?

-   [ ] 3
-   [x] 1
-   [ ] 4
-   [ ] 0.25

**Answer**

From the given formula (1) to calculate the
![\beta_1](https://latex.codecogs.com/png.image?%5Cdpi%7B110%7D&space;%5Cbg_white&space;%5Cbeta_1 "\beta_1").

![\beta_1 = \frac{Cov(Y,X)}{(sd(X))^2}](https://latex.codecogs.com/png.image?%5Cdpi%7B110%7D&space;%5Cbg_white&space;%5Cbeta_1%20%3D%20%5Cfrac%7BCov%28Y%2CX%29%7D%7B%28sd%28X%29%29%5E2%7D "\beta_1 = \frac{Cov(Y,X)}{(sd(X))^2}")

The Correlation is given by the follow formula (2):

![Cor(Y,X) = \frac{Cov(Y,X)}{sd(X) \cdot sd(Y)}](https://latex.codecogs.com/png.image?%5Cdpi%7B110%7D&space;%5Cbg_white&space;Cor%28Y%2CX%29%20%3D%20%5Cfrac%7BCov%28Y%2CX%29%7D%7Bsd%28X%29%20%5Ccdot%20sd%28Y%29%7D "Cor(Y,X) = \frac{Cov(Y,X)}{sd(X) \cdot sd(Y)}")

Using the formula (2) in (1):

![\beta_1 = Cor(X,Y) \cdot \frac{sd(Y)}{sd(X)}](https://latex.codecogs.com/png.image?%5Cdpi%7B110%7D&space;%5Cbg_white&space;%5Cbeta_1%20%3D%20Cor%28X%2CY%29%20%5Ccdot%20%5Cfrac%7Bsd%28Y%29%7D%7Bsd%28X%29%7D "\beta_1 = Cor(X,Y) \cdot \frac{sd(Y)}{sd(X)}")

``` r
# Creating the variables
cor_x_y <- 0.5
sdy_sdx <- 2

# Calculating the beta_1
beta_1 <- cor_x_y * sdy_sdx

# Printing beta_1
beta_1
```

    ## [1] 1

## Question 5

Students were given two hard tests and scores were normalized to have
empirical mean 0 and variance 1. The correlation between the scores on
the two tests was 0.4. What would be the expected score on Quiz 2 for a
student who had a normalized score of 1.5 on Quiz 1?

-   [ ] 0.16
-   [ ] 0.4
-   [x] 0.6
-   [ ] 1.0

**Answer**

In this case of normalized data, the slope will equal the correlation.
Due to the mean equal to zero, the intercept should be zero.

![y = \underbrace{\beta_0}\_{\text{Should be zero}} + \underbrace{\beta_1}\_{\text{Should be equal to Cor(X,Y)}} \cdot x](https://latex.codecogs.com/png.image?%5Cdpi%7B110%7D&space;%5Cbg_white&space;y%20%3D%20%5Cunderbrace%7B%5Cbeta_0%7D_%7B%5Ctext%7BShould%20be%20zero%7D%7D%20%2B%20%5Cunderbrace%7B%5Cbeta_1%7D_%7B%5Ctext%7BShould%20be%20equal%20to%20Cor%28X%2CY%29%7D%7D%20%5Ccdot%20x "y = \underbrace{\beta_0}_{\text{Should be zero}} + \underbrace{\beta_1}_{\text{Should be equal to Cor(X,Y)}} \cdot x")

``` r
# Creating the regression as a function.
q6 <- function(x) {
    
    beta_0 <- 0
    beta_1 <- 0.4
    
    return(beta_0 + beta_1 * x)
    }

# Calculating the value to 1.5.
q6(1.5)
```

    ## [1] 0.6

## Question 6

Consider the data given by the following

``` r
x <- c(8.58, 10.46, 9.01, 9.64, 8.86)
```

What is the value of the first measurement if x were normalized (to have
mean 0 and variance 1)?

-   [ ] 8.86
-   [x] -0.9719
-   [ ] 8.58
-   [ ] 9.31

**Answer**

``` r
# Normalizing th x vector.
x_norm <- (x - mean(x))/sd(x)

# Printing the first element
x_norm[1]
```

    ## [1] -0.9718658

## Question 7

Consider the following data set (used above as well). What is the
intercept for fitting the model with x as the predictor and y as the
outcome?

``` r
x <- c(0.8, 0.47, 0.51, 0.73, 0.36, 0.58, 0.57, 0.85, 0.44, 0.42)
y <- c(1.39, 0.72, 1.55, 0.48, 1.19, -1.59, 1.23, -0.65, 1.49, 0.05)
```

-   [ ] 2.105
-   [x] 1.567
-   [ ] 1.252
-   [ ] -1.713

**Answer**

``` r
# Creating a data frame.
df_q7 <- data.frame(x,y)


# Fitting a model with intercept
fit_q7 <- lm(data = df_q7, formula = y ~ x)

# Printing the coefficients
summary(fit_q7)$coeff
```

    ##              Estimate Std. Error    t value  Pr(>|t|)
    ## (Intercept)  1.567461   1.252107  1.2518582 0.2459827
    ## x           -1.712846   2.105259 -0.8136034 0.4394144

## Question 8

You know that both the predictor and response have mean 0. What can be
said about the intercept when you fit a linear regression?

-   [ ] Nothing about the intercept can be said from the information
    given.
-   [x] It must be identically 0.
-   [ ] It is undefined as you have to divide by zero.
-   [ ] It must be exactly one.

**Answer**

``` r
# Using the same values of Question 7
x <- c(0.8, 0.47, 0.51, 0.73, 0.36, 0.58, 0.57, 0.85, 0.44, 0.42)
y <- c(1.39, 0.72, 1.55, 0.48, 1.19, -1.59, 1.23, -0.65, 1.49, 0.05)

# Centering the data to have mean zero.
x_c <- x - mean(x)
y_c <- y - mean(y)

# Creating a data frame
df_q8 <- data.frame(x_c, y_c)

# Fitting a model
fit_q8 <- lm(data = df_q8, formula = y_c ~ x_c)

# Printing the coefficients
summary(fit_q8)$coeff
```

    ##                  Estimate Std. Error       t value  Pr(>|t|)
    ## (Intercept)  1.002843e-16  0.3355297  2.988834e-16 1.0000000
    ## x_c         -1.712846e+00  2.1052592 -8.136034e-01 0.4394144

## Question 9

Consider the data given by

``` r
x <- c(0.8, 0.47, 0.51, 0.73, 0.36, 0.58, 0.57, 0.85, 0.44, 0.42)
```

What value minimizes the sum of the squared distances between these
points and itself?

-   [ ] 0.36
-   [ ] 0.8
-   [x] 0.573
-   [ ] 0.44

**Answer**

The answer is similar to question 1, except this data has no weight. So,
the value which minimizes the Squared Distances should be the mean of x.

``` r
# Calculating the average.
mean(x)
```

    ## [1] 0.573

## Question 10

Let the slope having fit Y as the outcome and X as the predictor be
denoted as
![\beta_1](https://latex.codecogs.com/png.image?%5Cdpi%7B110%7D&space;%5Cbg_white&space;%5Cbeta_1 "\beta_1").
Let the slope from fitting X as the outcome and Y as the predictor be
denoted as
![\gamma_1](https://latex.codecogs.com/png.image?%5Cdpi%7B110%7D&space;%5Cbg_white&space;%5Cgamma_1 "\gamma_1").
Suppose that you divide
![\beta_1](https://latex.codecogs.com/png.image?%5Cdpi%7B110%7D&space;%5Cbg_white&space;%5Cbeta_1 "\beta_1")
by
![\gamma_1](https://latex.codecogs.com/png.image?%5Cdpi%7B110%7D&space;%5Cbg_white&space;%5Cgamma_1 "\gamma_1");
in other words consider
![\frac{\beta_1}{\gamma_1}](https://latex.codecogs.com/png.image?%5Cdpi%7B110%7D&space;%5Cbg_white&space;%5Cfrac%7B%5Cbeta_1%7D%7B%5Cgamma_1%7D "\frac{\beta_1}{\gamma_1}").
What is this ratio always equal to?

-   [x]
    ![Var(Y)/Var(X)](https://latex.codecogs.com/png.image?%5Cdpi%7B110%7D&space;%5Cbg_white&space;Var%28Y%29%2FVar%28X%29 "Var(Y)/Var(X)")
-   [ ]
    ![2 \cdot sd(X)/sd(Y)](https://latex.codecogs.com/png.image?%5Cdpi%7B110%7D&space;%5Cbg_white&space;2%20%5Ccdot%20sd%28X%29%2Fsd%28Y%29 "2 \cdot sd(X)/sd(Y)")
-   [ ] 1
-   [ ]
    ![Cor(Y,X)](https://latex.codecogs.com/png.image?%5Cdpi%7B110%7D&space;%5Cbg_white&space;Cor%28Y%2CX%29 "Cor(Y,X)")

**Answer**

Given
![\beta_1](https://latex.codecogs.com/png.image?%5Cdpi%7B110%7D&space;%5Cbg_white&space;%5Cbeta_1 "\beta_1"):

![\beta_1 = Cor(Y,X) \cdot \frac{sd(Y)}{sd(X)}](https://latex.codecogs.com/png.image?%5Cdpi%7B110%7D&space;%5Cbg_white&space;%5Cbeta_1%20%3D%20Cor%28Y%2CX%29%20%5Ccdot%20%5Cfrac%7Bsd%28Y%29%7D%7Bsd%28X%29%7D "\beta_1 = Cor(Y,X) \cdot \frac{sd(Y)}{sd(X)}")

Given
![\gamma_1](https://latex.codecogs.com/png.image?%5Cdpi%7B110%7D&space;%5Cbg_white&space;%5Cgamma_1 "\gamma_1"):

![\gamma_1 = Cor(Y,X) \cdot \frac{sd(Y)}{sd(X)}](https://latex.codecogs.com/png.image?%5Cdpi%7B110%7D&space;%5Cbg_white&space;%5Cgamma_1%20%3D%20Cor%28Y%2CX%29%20%5Ccdot%20%5Cfrac%7Bsd%28Y%29%7D%7Bsd%28X%29%7D "\gamma_1 = Cor(Y,X) \cdot \frac{sd(Y)}{sd(X)}")

Calculating the
![\frac{\beta_1}{\gamma_1}](https://latex.codecogs.com/png.image?%5Cdpi%7B110%7D&space;%5Cbg_white&space;%5Cfrac%7B%5Cbeta_1%7D%7B%5Cgamma_1%7D "\frac{\beta_1}{\gamma_1}"):

![\frac{\beta_1}{\gamma_1} = \frac{Cor(Y,X) \cdot \frac{sd(Y)}{sd(X)}}{Cor(Y,X) \cdot \frac{sd(Y)}{sd(X)}}=\Big( \frac{sd(Y)}{sd(X)} \Big)^2 = \frac{Var(Y)}{Var(X)}](https://latex.codecogs.com/png.image?%5Cdpi%7B110%7D&space;%5Cbg_white&space;%5Cfrac%7B%5Cbeta_1%7D%7B%5Cgamma_1%7D%20%3D%20%5Cfrac%7BCor%28Y%2CX%29%20%5Ccdot%20%5Cfrac%7Bsd%28Y%29%7D%7Bsd%28X%29%7D%7D%7BCor%28Y%2CX%29%20%5Ccdot%20%5Cfrac%7Bsd%28Y%29%7D%7Bsd%28X%29%7D%7D%3D%5CBig%28%20%5Cfrac%7Bsd%28Y%29%7D%7Bsd%28X%29%7D%20%5CBig%29%5E2%20%3D%20%5Cfrac%7BVar%28Y%29%7D%7BVar%28X%29%7D "\frac{\beta_1}{\gamma_1} = \frac{Cor(Y,X) \cdot \frac{sd(Y)}{sd(X)}}{Cor(Y,X) \cdot \frac{sd(Y)}{sd(X)}}=\Big( \frac{sd(Y)}{sd(X)} \Big)^2 = \frac{Var(Y)}{Var(X)}")
