---
title: Retrieving the Coefficient of Determination Value in R
date: "2018-08-13T08:00:00.000Z"
template: "post"
draft: false
slug: "/posts/get-r2-value-in-r"
category: "R"
tags:
  - "R"
  - "Statistics"
description: "Sometimes we want to return just the value of the coefficient of determination for a linear regression model, instead of the entire summary."
---
Sometimes we want to return just the value of R<sup>2</sup> for a linear regression model, instead of the entire summary. One use-case for extracting the value of R<sup>2</sup> from a model summary is including the R<sup>2</sup> value as inline code directly in the text of an R-Markdown file, such as in an automated report.

If your model or dataset changes, the R<sup>2</sup> value in the R-Markdown file will change according, which will help with reproducibility and minimize human error.

We'll create a simple linear regression model with the `mtcars` dataset, which comes from the 1974 *Motor Trend* US magazines and includes recordings of fuel consumption along with some attributes of the car. Let's specifically look at the relationship of miles per gallons and the number of cylinders of the engine, which we'll assign our model to `mpg.mod`.

``` r
mpg.mod <- lm(mpg ~ cyl, data = mtcars)
```

Running the command `summary(mpg.mod)` will show you a table that looks like this:

``` r
Call:
  lm(formula = mpg ~ cyl, data = mtcars)

Residuals:
  Min      1Q  Median      3Q     Max
-4.9814 -2.1185  0.2217  1.0717  7.5186

Coefficients:
  Estimate Std. Error t value Pr(>|t|)
(Intercept)  37.8846     2.0738   18.27  < 2e-16 ***
cyl          -2.8758     0.3224   -8.92 6.11e-10 ***
---
  Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

Residual standard error: 3.206 on 30 degrees of freedom
Multiple R-squared:  0.7262,	Adjusted R-squared:  0.7171
F-statistic: 79.56 on 1 and 30 DF,  p-value: 6.113e-10
```

To get just the R<sup>2</sup> value, evaluate this expression and assign it to a variable:

```r
r2 <- summary(mpg.mod)$r.squared
r2
```

Or for the adjusted R<sup>2</sup> value:

```r
adj.r2 <- summary(mpg.mod)$r.squared
adj.r2
```

Bringing it all together, below is how you could use the variable within an R-Markdown document using the `knitr` package to export the document with the evaluated embedded R code. The dynamically generated document can be exported as a PDF file, HTML, Microsoft Word, and many other formats.

You can put the code for your model in an R code block at the top of your R-markdown file and then call the R<sup>2</sup> variable as an inline code block within your text.

```r
{r setup, echo=FALSE, include=FALSE}
library(knitr)
knitr::opts_chunk$set(echo = TRUE)

mpg.mod <- lm(mpg ~ cyl, data = mtcars)
r2 <- summary(mpg.mod)$r.squared
adj.r2 <- summary(mpg.mod)$adj.r.squared
```

```md
The R-squared value for our linear model is `r r2`.
The adjusted R-squared is `r adj.r2`.
```

Using the `knitr` package will give you file with the following text:

*Originally published on [Quora](https://www.quora.com/How-do-I-calculate-the-coefficient-of-determination-r-2-in-R).*
