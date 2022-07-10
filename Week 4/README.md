`Week 4` Regression Models
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
-   📆 Week 4
    -   🚦 Start: Tuesday, 05 July 2022
    -   🏁 Finish: Sunday, 10 July 2022

------------------------------------------------------------------------

#### Assignments & Deliverables

-   [🚀 Course Project 4
    Repository](https://github.com/AndersonUyekita/regression-models_course-project-4)
-   [📝 Quiz 4](./quiz-4_regression-models.md)

#### Slides

-   Module 4 – Logistic Regression and Poisson Regression
    -   Logistic Regression
    -   Poisson Regression
    -   Hodgepodge

#### Description

> This week, we will work on generalized linear models, including binary
> outcomes and Poisson regression.

------------------------------------------------------------------------

## Class Notes

#### Generalized linear models (GLMs)

> The generalized linear model is family of models that includes linear
> models. By extending the family, it handles many of the issues with
> linear models, **but at the expense of some complexity and loss of
> some of the mathematical tidiness**. A GLM involves three components
>
> -   An exponential family model for the response.
> -   A systematic component via a linear predictor.
> -   A link function that connects the means of the response to the
>     linear predictor.
>
> The three most famous cases of GLMs are: linear models, binomial and
> binary regression and Poisson regression. We’ll go through the GLM
> model specification and likelihood for all three. For linear models,
> we’ve developed them previously. The next two modules will be devoted
> to binomial and Poisson regression. We’ll only focus on the most
> popular and useful link functions.

![Y_i \\sim N(\\mu_i, \\sigma^2)](https://latex.codecogs.com/png.image?%5Cdpi%7B110%7D&space;%5Cbg_white&space;Y_i%20%5Csim%20N%28%5Cmu_i%2C%20%5Csigma%5E2%29 "Y_i \sim N(\mu_i, \sigma^2)")

.
