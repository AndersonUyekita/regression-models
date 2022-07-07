`Week 1` Regression Models
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
-   📆 Week 1
    -   🚦 Start: Tuesday, 05 July 2022
    -   🏁 Finish: Wednesday, 06 July 2022

------------------------------------------------------------------------

#### Assignments & Deliverables

-   [🚀 Course Project 1
    Repository](https://github.com/AndersonUyekita/regression-models_course-project-1)
-   [📝 Quiz 1](./quiz-1_regression-models.md)

#### Slides

-   Module 1 – Least Squares and Linear Regression
    -   01_01 Introduction
    -   01_02 Notation
    -   01_03 Ordinary least squares
    -   01_04 Regression to the mean
    -   01_05 Linear regression
    -   01_06 Residuals
    -   01_07 Regression inference

#### Description

> This week, we focus on least squares and linear regression.

------------------------------------------------------------------------

## Class Notes

### Regression

> Regression models are the workhorse of data science. They are the most
> well described, practical and theoretically understood models in
> statistics. A data scientist well versed in regression models will be
> able to solve an incredible array of problems.
>
> Perhaps the key insight for regression models is that they produce
> highly interpretable model fits. This is unlike machine learning
> algorithms, which often **sacrifice interpretability** for **improved
> prediction performance or automation**. These are, of course, valuable
> attributes in their own rights. However, the benefit of simplicity,
> **parsimony and interpretability** offered by regression models (and
> their close generalizations) should make them a first tool of choice
> for any practical problem.

> A ball hog is a derisive term for a basketball player who handles the
> ball exclusively to the point of impairing the team. Despite not being
> a violation of the rules of basketball, “ball-hogging” is generally
> considered unacceptable playing behavior at all levels of basketball
> competition. – [Wikipedia](https://en.wikipedia.org/wiki/Ball_hog)

**Summary**

> -   Prediction e.g.: to use the parent’s heights to predict children’s
>     heights.
> -   Modeling e.g.: to try to find a parsimonious, easily described
>     mean relationship between parental and child heights.
> -   Covariation e.g.: to investigate the variation in child heights
>     that appears unrelated to parental heights (residual variation)
>     and to quantify what impact genotype information has beyond
>     parental height in explaining child height.

#### Defining the middle

``` r
# Loading the UsingR package
library(UsingR)

# Loading the Galton dataset to environment.
data("galton")

# Plotting the data.
ggplot(data = reshape::melt(galton), aes(x = value, fill = variable)) + 
    geom_histogram(colour = 'black', bindwidth = 1) + 
    facet_grid(. ~ variable)
```

![](README_files/figure-gfm/unnamed-chunk-1-1.png)<!-- -->

The middle is a given
![\\mu](https://latex.codecogs.com/png.image?%5Cdpi%7B110%7D&space;%5Cbg_white&space;%5Cmu "\mu")
which minimizes the summation:

![\\sum\_{i=1}^{n}(Y\_{i} - \\mu)^2](https://latex.codecogs.com/png.image?%5Cdpi%7B110%7D&space;%5Cbg_white&space;%5Csum_%7Bi%3D1%7D%5E%7Bn%7D%28Y_%7Bi%7D%20-%20%5Cmu%29%5E2 "\sum_{i=1}^{n}(Y_{i} - \mu)^2")

-   This is physical center of mass of the histogram.

![\\text{Least Squares}=\\sum\_{i=1}^{n}(Y\_{i} - \\bar Y)^2](https://latex.codecogs.com/png.image?%5Cdpi%7B110%7D&space;%5Cbg_white&space;%5Ctext%7BLeast%20Squares%7D%3D%5Csum_%7Bi%3D1%7D%5E%7Bn%7D%28Y_%7Bi%7D%20-%20%5Cbar%20Y%29%5E2 "\text{Least Squares}=\sum_{i=1}^{n}(Y_{i} - \bar Y)^2")

Where

-   ![\\bar Y](https://latex.codecogs.com/png.image?%5Cdpi%7B110%7D&space;%5Cbg_white&space;%5Cbar%20Y "\bar Y"):
    Mean of
    ![Y_n](https://latex.codecogs.com/png.image?%5Cdpi%7B110%7D&space;%5Cbg_white&space;Y_n "Y_n")

> **Minimizing the average (or sum of the)** squared errors seems like a
> reasonable strategy, though of course there are others.

Strategy is minimize the Error Summation:

![\\text{Minimize} \\sum\_{i=1}^{n}\\Big( Y\_{i} - \\bar Y \\Big)^2](https://latex.codecogs.com/png.image?%5Cdpi%7B110%7D&space;%5Cbg_white&space;%5Ctext%7BMinimize%7D%20%5Csum_%7Bi%3D1%7D%5E%7Bn%7D%5CBig%28%20Y_%7Bi%7D%20-%20%5Cbar%20Y%20%5CBig%29%5E2 "\text{Minimize} \sum_{i=1}^{n}\Big( Y_{i} - \bar Y \Big)^2")

or the Average of error:

![\\text{Minimize} \\frac{\\sum\_{i=1}^{n}\\Big( Y\_{i} - \\bar Y \\Big)^2}{n}](https://latex.codecogs.com/png.image?%5Cdpi%7B110%7D&space;%5Cbg_white&space;%5Ctext%7BMinimize%7D%20%5Cfrac%7B%5Csum_%7Bi%3D1%7D%5E%7Bn%7D%5CBig%28%20Y_%7Bi%7D%20-%20%5Cbar%20Y%20%5CBig%29%5E2%7D%7Bn%7D "\text{Minimize} \frac{\sum_{i=1}^{n}\Big( Y_{i} - \bar Y \Big)^2}{n}")

The Average will be the Error Summation divided by n, which is a
constant. So it will not change the minimization results.

#### Regression through the origin

Considering a line crossing the origin and minimizing the squared error.

![\\sum\_{i=1}^n{\\Big( Y_i - \\underbrace{X_i \\cdot \\beta}\_{\\text{Estimated Functoin}} \\Big)^2}](https://latex.codecogs.com/png.image?%5Cdpi%7B110%7D&space;%5Cbg_white&space;%5Csum_%7Bi%3D1%7D%5En%7B%5CBig%28%20Y_i%20-%20%5Cunderbrace%7BX_i%20%5Ccdot%20%5Cbeta%7D_%7B%5Ctext%7BEstimated%20Functoin%7D%7D%20%5CBig%29%5E2%7D "\sum_{i=1}^n{\Big( Y_i - \underbrace{X_i \cdot \beta}_{\text{Estimated Functoin}} \Big)^2}")

-   Generally is it a bad practice.

#### Centering Data

This procedure moves the origin (0,0) to be on the mass center of the
data. So doing it, the intercept should be zero.

Explaining the `lm` function:

![lm(data = galton, I(\\underbrace{child - mean(child)}\_{\\text{centering in y-axis}}) \\sim I(\\underbrace{parent - mean(parent)}\_{\\text{centering in x-axis}}) \\underbrace{- 1}\_{\\text{get rid of the intercept}})](https://latex.codecogs.com/png.image?%5Cdpi%7B110%7D&space;%5Cbg_white&space;lm%28data%20%3D%20galton%2C%20I%28%5Cunderbrace%7Bchild%20-%20mean%28child%29%7D_%7B%5Ctext%7Bcentering%20in%20y-axis%7D%7D%29%20%5Csim%20I%28%5Cunderbrace%7Bparent%20-%20mean%28parent%29%7D_%7B%5Ctext%7Bcentering%20in%20x-axis%7D%7D%29%20%5Cunderbrace%7B-%201%7D_%7B%5Ctext%7Bget%20rid%20of%20the%20intercept%7D%7D%29 "lm(data = galton, I(\underbrace{child - mean(child)}_{\text{centering in y-axis}}) \sim I(\underbrace{parent - mean(parent)}_{\text{centering in x-axis}}) \underbrace{- 1}_{\text{get rid of the intercept}})")

The `I()` means “as is”, so the `child - mean(child)` will be evaluate
as is.

``` r
# Regression through the origin.
lm(data = galton, I(child - mean(child)) ~ I(parent - mean(parent)) - 1)
```

    ## 
    ## Call:
    ## lm(formula = I(child - mean(child)) ~ I(parent - mean(parent)) - 
    ##     1, data = galton)
    ## 
    ## Coefficients:
    ## I(parent - mean(parent))  
    ##                   0.6463

The coefficient is related to a linear regression centered at the mass
center of the dataset.

#### Regression with intercept

> **Ordinary least squares** (OLS) is the workhorse of statistics. It
> gives a way of taking complicated outcomes and explaining behavior
> (such as trends) using linearity. The simplest application of OLS is
> fitting a line through some data. In the next few lectures, we cover
> the basics of linear least squares.

Possible function which explains the child’s Height.

![\\underbrace{\\text{Child's Height}}\_{Y} = \\beta_0 + \\underbrace{\\text{Parent's Height}}\_{X} \\cdot \\beta_1](https://latex.codecogs.com/png.image?%5Cdpi%7B110%7D&space;%5Cbg_white&space;%5Cunderbrace%7B%5Ctext%7BChild%27s%20Height%7D%7D_%7BY%7D%20%3D%20%5Cbeta_0%20%2B%20%5Cunderbrace%7B%5Ctext%7BParent%27s%20Height%7D%7D_%7BX%7D%20%5Ccdot%20%5Cbeta_1 "\underbrace{\text{Child's Height}}_{Y} = \beta_0 + \underbrace{\text{Parent's Height}}_{X} \cdot \beta_1")

I want to minimize:

![\\sum\_{i=1}^n{ \\Big(Y_i-(\\underbrace{\\beta_0+\\beta_1 \\cdot X_i}\_{\\text{Estimated Function}})\\Big)^2 }](https://latex.codecogs.com/png.image?%5Cdpi%7B110%7D&space;%5Cbg_white&space;%5Csum_%7Bi%3D1%7D%5En%7B%20%5CBig%28Y_i-%28%5Cunderbrace%7B%5Cbeta_0%2B%5Cbeta_1%20%5Ccdot%20X_i%7D_%7B%5Ctext%7BEstimated%20Function%7D%7D%29%5CBig%29%5E2%20%7D "\sum_{i=1}^n{ \Big(Y_i-(\underbrace{\beta_0+\beta_1 \cdot X_i}_{\text{Estimated Function}})\Big)^2 }")

``` r
# Assign the values to variables y and x.
y <- galton$child
x <- galton$parent

# Calculating the theoretical values.
beta1 <- cor(y,x) * sd(y)/sd(x)
beta0 <- mean(y) - beta1 * mean(x)

# Printing.
beta0; beta1;
```

    ## [1] 23.94153

    ## [1] 0.6462906

As you can see, the slope
(![\\beta1](https://latex.codecogs.com/png.image?%5Cdpi%7B110%7D&space;%5Cbg_white&space;%5Cbeta1 "\beta1"))
is equal to the centered data!! Centering the data have just shifted the
data and has changed only the intercept.

Now, using the `lm` function **with** intercept.

``` r
# Calculating the coefficients with intercept.
lm(y ~ x)
```

    ## 
    ## Call:
    ## lm(formula = y ~ x)
    ## 
    ## Coefficients:
    ## (Intercept)            x  
    ##     23.9415       0.6463

#### Regression to the mean

> Here is a fundamental question. Why is it that the children of tall
> parents tend to be tall, but not as tall as their parents? Why do
> children of short parents tend to be short, but not as short as their
> parents? Conversely, why do parents of very short children, tend to be
> short, but not a short as their child? And the same with parents of
> very tall children?
>
> We can try this with anything that is measured with error. Why do the
> best performing athletes this year tend to do a little worse the
> following? Why do the best performers on hard exams always do a little
> worse on the next hard exam?
>
> These phenomena are all examples of so-called regression to the mean.
> Regression to the mean, was invented by Francis Galton in the paper
> “Regression towards mediocrity in hereditary stature” The Journal of
> the Anthropological Institute of Great Britain and Ireland , Vol. 15,
> (1886). The idea served as a foundation for the discovery of linear
> regression.
