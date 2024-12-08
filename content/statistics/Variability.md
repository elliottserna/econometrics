# Statistics: Measures of Variability

## Definitions

**Measures of Variability:** Statistical measures that describe the spread or dispersion of a dataset. 

## Overview

Measures of variability provide insight into how much the values in a dataset differ from each other and from the mean. Understanding variability is essential for interpreting data and making informed conclusions.

The three most common measures of variability are:

1. **Range**
2. **Variance**
3. **Standard Deviation**

## Range

The **range** is the simplest measure of variability, defined as the difference between the maximum and minimum values in a dataset.

### Formula:

$$
\text{Range} = \text{Max} - \text{Min}
$$

### Example:

For the dataset $\{4, 8, 6, 5, 3\}$:

- Max = 8
- Min = 3

$$
\text{Range} = 8 - 3 = 5
$$

## Variance

**Variance** quantifies the average squared deviation of each data point from the mean. It provides a measure of how much the values in a dataset differ from the mean.

### Formula:

For a population variance ($\sigma^2$):

$$
\sigma^2 = \frac{1}{N} \sum_{i=1}^{N} (x_i - \mu)^2
$$

For a sample variance ($s^2$):

$$
s^2 = \frac{1}{n - 1} \sum_{i=1}^{n} (x_i - \bar{x})^2
$$

Where:
- $N$ = total number of observations in the population
- $n$ = total number of observations in the sample
- $x_i$ = each individual value in the dataset
- $\mu$ = population mean
- $\bar{x}$ = sample mean

### Example:

For the dataset $\{4, 8, 6, 5, 3\}$, first calculate the mean:

$$
\bar{x} = \frac{4 + 8 + 6 + 5 + 3}{5} = 5.2
$$

Then, calculate the variance:

$$
s^2 = \frac{1}{5 - 1} \left((4 - 5.2)^2 + (8 - 5.2)^2 + (6 - 5.2)^2 + (5 - 5.2)^2 + (3 - 5.2)^2\right)
$$

$$
= \frac{1}{4} \left((-1.2)^2 + (2.8)^2 + (0.8)^2 + (-0.2)^2 + (-2.2)^2\right)
$$

$$
= \frac{1}{4} \left(1.44 + 7.84 + 0.64 + 0.04 + 4.84\right) = \frac{1}{4} \times 14.8 = 3.7
$$

## Standard Deviation

The **standard deviation** is the square root of the variance and provides a measure of variability in the same units as the original data. It is widely used because it is more interpretable than variance.

### Formula:

For a population standard deviation ($\sigma$):

$$
\sigma = \sqrt{\sigma^2} = \sqrt{\frac{1}{N} \sum_{i=1}^{N} (x_i - \mu)^2}
$$

For a sample standard deviation ($s$):

$$
s = \sqrt{s^2} = \sqrt{\frac{1}{n - 1} \sum_{i=1}^{n} (x_i - \bar{x})^2}
$$

### Example:

Continuing from the previous example:

$$
s = \sqrt{3.7} \approx 1.92
$$

## Comparison of Measures

- **Range**: Simple to calculate but can be affected by outliers, making it less reliable as a measure of variability.
- **Variance**: Provides a comprehensive view of data spread, but the squared units can make interpretation challenging.
- **Standard Deviation**: Offers a clearer interpretation of variability, as it is in the same unit as the data. 

## Multivariate Measures

In many cases, we're interested in measuring how multiple variables move together. In these multivariate contexts, we build on the formula for variance to construct two new measures: **covariance** and **Pearson's correlation coefficient**.

### Covariance

To understand covariance, let’s first revisit the concept of **variance**. Variance measures the spread or dispersion of a single variable by calculating how far each data point deviates from the mean. The formula for the variance of a variable $X$ is:

$$
\mathrm{Var}(X) = s^2 =\frac{1}{n-1} \sum_{i=1}^{n}(x_i - \bar{x})^2
$$

This formula tells us how much the values of $X$ vary from their mean, but it only applies to a single variable. **Covariance** extends this concept to two variables, $X$ and $Y$, to measure how they change together. If $X$ and $Y$ increase together, the covariance will be positive; if one increases while the other decreases, the covariance will be negative.

We construct the covariance by replacing the squared deviation term in the variance formula with the product of the deviations of $X$ and $Y$ from their respective means:

$$
\mathrm{Cov}(X, Y) = s_{XY} = \frac{1}{n-1} \sum_{i=1}^{n}(x_i - \bar{x})(y_i - \bar{y})
$$

This formula tells us the average product of the deviations of $X$ and $Y$ from their means. Like variance, the covariance is influenced by the units of the variables, so the magnitude can be difficult to interpret directly. A positive covariance indicates that $X$ and $Y$ tend to increase together, while a negative covariance indicates that they move in opposite directions.

### Correlation

While covariance provides a measure of how two variables move together, its magnitude depends on the scales of the variables, making it hard to compare across different pairs of variables. **Pearson’s correlation coefficient**, denoted as $r$, standardizes this relationship, providing a dimensionless measure that always lies between -1 and 1.

The formula for Pearson’s correlation coefficient is:

$$
\mathrm{Corr}(X, Y) = r = \frac{\sum (x_i - \bar{x})(y_i - \bar{y})}{\sqrt{\sum (x_i - \bar{x})^2 \sum (y_i - \bar{y})^2}} = \frac{\mathrm{Cov}(X,Y)}{\sigma_X \sigma_Y}
$$

In this formula:
- The numerator is the covariance between $X$ and $Y$.
- The denominator is the product of the standard deviations of $X$ and $Y$, which adjusts for the scale of the variables.

The correlation coefficient $r$ provides a normalized measure of the linear relationship between $X$ and $Y$:
- $r = 1$ indicates a perfect positive linear relationship.
- $r = -1$ indicates a perfect negative linear relationship.
- $r = 0$ suggests no linear relationship.

Because the correlation coefficient is standardized, it allows us to easily compare the strength and direction of linear relationships across different pairs of variables, regardless of their units or scales.