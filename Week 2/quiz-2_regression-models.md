`Quiz 2` Regression Models
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
-   📆 Week 2
    -   🚦 Start: Tuesday, 05 July 2022
    -   🏁 Finish: Monday, 18 July 2022
-   🌎 Rpubs: [Interactive
    Document](https://rpubs.com/AndersonUyekita/quiz-2_regression-models)

------------------------------------------------------------------------

## Question 1

Consider the following data with x as the predictor and y as as the
outcome.

``` r
x <- c(0.61, 0.93, 0.83, 0.35, 0.54, 0.16, 0.91, 0.62, 0.62)
y <- c(0.67, 0.84, 0.6, 0.18, 0.85, 0.47, 1.1, 0.65, 0.36)
```

Give a P-value for the two sided hypothesis test of whether

-   [x] 0.05296
-   [ ] 2.325
-   [ ] 0.025
-   [ ] 0.391

**Answer**

``` r
# Creating a data frame.
df_q1 <- data.frame(x, y)

# Fitting a model.
fit_q1 <- lm(data = df_q1, formula = y ~ x)

# Printing the coefficients.
summary(fit_q1)$coeff
```

    ##              Estimate Std. Error   t value   Pr(>|t|)
    ## (Intercept) 0.1884572  0.2061290 0.9142681 0.39098029
    ## x           0.7224211  0.3106531 2.3254912 0.05296439

The `p-value` for
![\beta_1](https://latex.codecogs.com/png.image?%5Cdpi%7B110%7D&space;%5Cbg_white&space;%5Cbeta_1 "\beta_1")
is 0.05296.

## Question 2

Consider the previous problem, give the estimate of the residual
standard deviation.

-   [ ] 0.4358
-   [ ] 0.05296
-   [ ] 0.3552
-   [x] 0.223

**Answer**

``` r
summary(fit_q1)$sigma
```

    ## [1] 0.2229981

## Question 3

In the `mtcars` data set, fit a linear regression model of weight
(predictor) on mpg (outcome). Get a 95% confidence interval for the
expected mpg at the average weight. What is the lower endpoint?

-   [ ] -6.486
-   [ ] -4.00
-   [ ] 21.190
-   [x] 18.991

**Answer**

It is necessary to center the weight (subtracting the mean of each
element of wt)

``` r
# Creating the data frame
df_q3 <- data.frame(mpg = mtcars$mpg,
                    wt_c = mtcars$wt - mean(mtcars$wt))

# Fitting the model.
fit_q3 <- lm(data = df_q3, formula = mpg ~ wt_c)

# Calculating the Confidence Interval.
confint(object = fit_q3, level = 0.95)
```

    ##                 2.5 %    97.5 %
    ## (Intercept) 18.990982 21.190268
    ## wt_c        -6.486308 -4.202635

## Question 4

Refer to the previous question. Read the help file for `mtcars.` What is
the weight coefficient interpreted as?

-   [x] The estimated expected change in mpg per 1,000 lb increase in
    weight.
-   [ ] The estimated expected change in mpg per 1 lb increase in
    weight.
-   [ ] It can’t be interpreted without further information
-   [ ] The estimated 1,000 lb change in weight per 1 mpg increase.

**Answer**

    [, 6]   wt  Weight (1000 lbs)

The expected change in the response per unit change in the predictor.

## Question 5

Consider again the `mtcars` data set and a linear regression model with
mpg as predicted by weight (1,000 lbs). A new car is coming weighing
3000 pounds. Construct a 95% prediction interval for its mpg. What is
the upper endpoint?

-   [ ] 21.25
-   [ ] -5.77
-   [ ] 14.93
-   [x] 27.57

**Answer**

``` r
# Fitting a model.
fit_q5 <- lm(data = mtcars, formula = mpg ~ wt)

# Value to be predicted.
pred_q5 <- data.frame(wt = 3)

# Based on the model fitted, let's predict.
predict(object = fit_q5,
        newdata = pred_q5,
        interval = "prediction")
```

    ##        fit      lwr      upr
    ## 1 21.25171 14.92987 27.57355

## Question 6

Consider again the `mtcars` data set and a linear regression model with
mpg as predicted by weight (in 1,000 lbs). A “short” ton is defined as
2,000 lbs. Construct a 95% confidence interval for the expected change
in mpg per 1 short ton increase in weight. Give the lower endpoint.

-   [ ] -9.000
-   [x] -12.973
-   [ ] 4.2026
-   [ ] -6.486

**Answer**

``` r
# Fitting a model based on the given short definition.
fit_q6 <- lm(data = mtcars,
             formula = mpg ~ I(wt/2)) # converting wt values into "short" unit

# Printing the coefficients from short definition.
summary(fit_q6)$coeff
```

    ##              Estimate Std. Error   t value     Pr(>|t|)
    ## (Intercept)  37.28513   1.877627 19.857575 8.241799e-19
    ## I(wt/2)     -10.68894   1.118202 -9.559044 1.293959e-10

``` r
# Constructing a Confidence Interval of 95%
confint(object = fit_q6)
```

    ##                 2.5 %   97.5 %
    ## (Intercept)  33.45050 41.11975
    ## I(wt/2)     -12.97262 -8.40527

## Question 7

If my X from a linear regression is measured in centimeters and I
convert it to meters what would happen to the slope coefficient?

-   [x] It would get multiplied by 100.
-   [ ] It would get divided by 100
-   [ ] It would get divided by 10
-   [ ] It would get multiplied by 10

**Answer**

``` r
# Fitting a model in kg and ton
fit_q7_kg <- lm(data = mtcars, formula = mpg ~ wt)
fit_q7_ton <- lm(data = mtcars, formula = mpg ~ I(1000*wt))

# Printing the coefficients.
summary(fit_q7_kg)$coeff;
```

    ##              Estimate Std. Error   t value     Pr(>|t|)
    ## (Intercept) 37.285126   1.877627 19.857575 8.241799e-19
    ## wt          -5.344472   0.559101 -9.559044 1.293959e-10

``` r
summary(fit_q7_ton)$coeff
```

    ##                  Estimate  Std. Error   t value     Pr(>|t|)
    ## (Intercept)  37.285126167 1.877627337 19.857575 8.241799e-19
    ## I(1000 * wt) -0.005344472 0.000559101 -9.559044 1.293959e-10

From the above example, when `wt` is multiplied by 1000, the coefficient
is divided by 1000. So, if I converted `cm` to `m`, I will divide the by
100, and probably my coefficient will be divided by 100.

## Question 8

I have an outcome,
![Y](https://latex.codecogs.com/png.image?%5Cdpi%7B110%7D&space;%5Cbg_white&space;Y "Y"),
and a predictor,
![X](https://latex.codecogs.com/png.image?%5Cdpi%7B110%7D&space;%5Cbg_white&space;X "X")
and fit a linear regression model with
![Y = \beta_0 + \beta_1 \cdot X + \epsilon](https://latex.codecogs.com/png.image?%5Cdpi%7B110%7D&space;%5Cbg_white&space;Y%20%3D%20%5Cbeta_0%20%2B%20%5Cbeta_1%20%5Ccdot%20X%20%2B%20%5Cepsilon "Y = \beta_0 + \beta_1 \cdot X + \epsilon")
to obtain
![\hat \beta_0](https://latex.codecogs.com/png.image?%5Cdpi%7B110%7D&space;%5Cbg_white&space;%5Chat%20%5Cbeta_0 "\hat \beta_0")
and
![\hat \beta_1](https://latex.codecogs.com/png.image?%5Cdpi%7B110%7D&space;%5Cbg_white&space;%5Chat%20%5Cbeta_1 "\hat \beta_1").
What would be the consequence to the subsequent slope and intercept if I
were to refit the model with a new regressor,
![X + c](https://latex.codecogs.com/png.image?%5Cdpi%7B110%7D&space;%5Cbg_white&space;X%20%2B%20c "X + c")
for some constant,
![c](https://latex.codecogs.com/png.image?%5Cdpi%7B110%7D&space;%5Cbg_white&space;c "c")?

-   [x] The new intercept would be
    ![\hat \beta_0 - c \cdot \hat \beta_1](https://latex.codecogs.com/png.image?%5Cdpi%7B110%7D&space;%5Cbg_white&space;%5Chat%20%5Cbeta_0%20-%20c%20%5Ccdot%20%5Chat%20%5Cbeta_1 "\hat \beta_0 - c \cdot \hat \beta_1")
-   [ ] The new slope would be
    ![c \cdot \hat \beta_1](https://latex.codecogs.com/png.image?%5Cdpi%7B110%7D&space;%5Cbg_white&space;c%20%5Ccdot%20%5Chat%20%5Cbeta_1 "c \cdot \hat \beta_1")
-   [ ] The new intercept would be
    ![\hat \beta_0 + c \cdot \hat \beta_1](https://latex.codecogs.com/png.image?%5Cdpi%7B110%7D&space;%5Cbg_white&space;%5Chat%20%5Cbeta_0%20%2B%20c%20%5Ccdot%20%5Chat%20%5Cbeta_1 "\hat \beta_0 + c \cdot \hat \beta_1")
-   [ ] The new slope would be
    ![\hat \beta_0 + c](https://latex.codecogs.com/png.image?%5Cdpi%7B110%7D&space;%5Cbg_white&space;%5Chat%20%5Cbeta_0%20%2B%20c "\hat \beta_0 + c")

**Answer**

``` r
# Fitting a model subtracting 1 from all value of wt.
fit_q8_minus_2 <- lm(data = mtcars, formula = mpg  ~ I(wt - 2))
fit_q8_minus_1 <- lm(data = mtcars, formula = mpg  ~ I(wt - 1))
fit_q8 <- lm(data = mtcars, formula = mpg  ~ wt)
fit_q8_plus_1 <- lm(data = mtcars, formula = mpg  ~ I(wt + 1))
fit_q8_plus_2 <- lm(data = mtcars, formula = mpg  ~ I(wt + 2))

# Printing the coefficients.
summary(fit_q8_minus_2)$coeff;
```

    ##              Estimate Std. Error   t value     Pr(>|t|)
    ## (Intercept) 26.596183  0.8678067 30.647590 3.359471e-24
    ## I(wt - 2)   -5.344472  0.5591010 -9.559044 1.293959e-10

``` r
summary(fit_q8_minus_1)$coeff;
```

    ##              Estimate Std. Error   t value     Pr(>|t|)
    ## (Intercept) 31.940655   1.351552 23.632578 6.039935e-21
    ## I(wt - 1)   -5.344472   0.559101 -9.559044 1.293959e-10

``` r
summary(fit_q8)$coeff;
```

    ##              Estimate Std. Error   t value     Pr(>|t|)
    ## (Intercept) 37.285126   1.877627 19.857575 8.241799e-19
    ## wt          -5.344472   0.559101 -9.559044 1.293959e-10

``` r
summary(fit_q8_plus_1)$coeff
```

    ##              Estimate Std. Error   t value     Pr(>|t|)
    ## (Intercept) 42.629598   2.418567 17.625976 2.239703e-17
    ## I(wt + 1)   -5.344472   0.559101 -9.559044 1.293959e-10

``` r
summary(fit_q8_plus_2)$coeff
```

    ##              Estimate Std. Error   t value     Pr(>|t|)
    ## (Intercept) 47.974069   2.966249 16.173312 2.329891e-16
    ## I(wt + 2)   -5.344472   0.559101 -9.559044 1.293959e-10

The comparison:

| Condition |  Results  |     Delta      |
|:---------:|:---------:|:--------------:|
|    -2     | 26.596183 | 2 \* 5.344472  |
|    -1     | 31.940655 |    5.344472    |
| Baseline  | 37.285126 |                |
|    +1     | 42.629598 |   -5.344472    |
|    +2     | 47.974069 | -2 \* 5.344472 |

For each unit decreased in `wt`, there is a subtraction in the intercept
in
![\beta_1](https://latex.codecogs.com/png.image?%5Cdpi%7B110%7D&space;%5Cbg_white&space;%5Cbeta_1 "\beta_1")
magnitude. Thus:

![\text{New intercep}t = \text{Intercept} - c \cdot \beta_1](https://latex.codecogs.com/png.image?%5Cdpi%7B110%7D&space;%5Cbg_white&space;%5Ctext%7BNew%20intercep%7Dt%20%3D%20%5Ctext%7BIntercept%7D%20-%20c%20%5Ccdot%20%5Cbeta_1 "\text{New intercep}t = \text{Intercept} - c \cdot \beta_1")

In case of
![c](https://latex.codecogs.com/png.image?%5Cdpi%7B110%7D&space;%5Cbg_white&space;c "c")
equals to `2` and
![\beta_1](https://latex.codecogs.com/png.image?%5Cdpi%7B110%7D&space;%5Cbg_white&space;%5Cbeta_1 "\beta_1")
equals to `-5.344472`:

![\text{New Intercept} = 37.285126 - 2 \cdot (-5.344472) = 47.974069](https://latex.codecogs.com/png.image?%5Cdpi%7B110%7D&space;%5Cbg_white&space;%5Ctext%7BNew%20Intercept%7D%20%3D%2037.285126%20-%202%20%5Ccdot%20%28-5.344472%29%20%3D%2047.974069 "\text{New Intercept} = 37.285126 - 2 \cdot (-5.344472) = 47.974069")

## Question 9

Refer back to the mtcars data set with mpg as an outcome and weight (wt)
as the predictor. About what is the ratio of the the sum of the squared
errors,
![\sum\_{i=1}^{n}{(Y_i - \hat Y_1)^2}](https://latex.codecogs.com/png.image?%5Cdpi%7B110%7D&space;%5Cbg_white&space;%5Csum_%7Bi%3D1%7D%5E%7Bn%7D%7B%28Y_i%20-%20%5Chat%20Y_1%29%5E2%7D "\sum_{i=1}^{n}{(Y_i - \hat Y_1)^2}")
when comparing a model with just an intercept (denominator) to the model
with the intercept and slope (numerator)?

-   [ ] 0.50
-   [ ] 4.00
-   [x] 0.25
-   [ ] 0.75

**Answer**

``` r
# Baseline
fit_q9_baseline <- lm(data = mtcars, formula = mpg ~ 1)

# One regressor
fit_q9_wt <- lm(data = mtcars, formula = mpg ~ wt)

# Calculating the residuals
sse_baseline <- sum(fit_q9_baseline$residuals^2)
sse_wt <- sum(fit_q9_wt$residuals^2)

# Calculating the erros ratio
sse_wt/sse_baseline
```

    ## [1] 0.2471672

## Question 10

Do the residuals always have to sum to 0 in linear regression?

-   [ ] The residuals must always sum to zero.
-   [ ] The residuals never sum to zero.
-   [x] If an intercept is included, then they will sum to 0.
-   [ ] If an intercept is included, the residuals most likely won’t sum
    to zero.

**Answer**

``` r
# Fitting a model with and without a intercept.
fit_q10_with_intercept <- lm(data = mtcars, formula = mpg ~ wt)
fit_q10_without_intercept <- lm(data = mtcars, formula = mpg ~ wt -1)

# Calculating the residual summation.
print(paste("With Intercept:", sum(fit_q10_with_intercept$residuals)))
```

    ## [1] "With Intercept: -1.63757896132211e-15"

``` r
print(paste("Without Intercept:", sum(fit_q10_without_intercept$residuals)))
```

    ## [1] "Without Intercept: 98.1167155791475"

As expected, the least squared has fitted a line to minimize the
summation of the residuals.
