---
layout: post
title:  "A Brief Overview of Monte Carlo Approximation"
date:   2022-10-09
author: Ellen Wight
description: FOR WRTG 316, NOT MY STAT 386 BLOG TUTORIAL
image: /assets/images/DnD.jpg
---

## Definition
Monte Carlo approximation, named after the casino in Monaco, is a computer simulation technique that uses repeated random trials to model the potential outcomes of an uncertain event. By comparing frequencies of said outcomes to total trials, it also models probabilities. For example, consider the expected sum X from rolling two standard dice while gambling in the method's namesake. Given the known distribution or formula for X (the sum of two dice, each with an equal chance of rolling a 1-6), Monte Carlo produces a large simulated dataset by randomly selecting potential values for X from all possibilities.

<img src="https://github.com/emwight/stat386-projects/raw/main/assets/images/Roll_sum.png" height="400" align="middle"/>

## Theory
How large of a sample is needed? The rule of thumb is generally 10,000 to fulfill the Law of Large Numbers (a theorem in statistics: as sample size from a population increases, the mean of that sample approaches the true population mean). Unsurprisingly, this technique has therefore become more applicable with the advent of computers and programming languages that can quickly generate "random samples" (really algorithmic approximations of randomness, but it's close enough for simulation purposes). As a quick illustration of the importance of a large sample size, consider the probability of rolling just one standard die. In R, we can approximate the known probability distribution using the `purrr:rdunif(n, 1, 6)` function as our Monte Carlo simulation with sample size n. Below are three graphs with increasing sample sizes to show the computational approach to the mathematically true probabilities.

<img src="https://github.com/emwight/stat386-projects/raw/main/assets/images/rolls.png" height="400" align="middle"/>

This isn't interesting for any analysis, of course; it simply visualizes the intuitive nature of Monte Carlo. If we draw many values from a known distribution with set parameters, it follows that a high enough sample size would yield its approximate shape, center, and spread. By combining distribution simulations, however, Monte Carlo becomes useful for approximating what can't be calculated mathematically. 

## Applications in Statistics
Monte Carlo has become especially useful in the field of Bayesian analysis. Compared to the traditional Frequentist approach to statistics, which considers population parameters as fixed values, Bayesian analysis states that population parameters are unknown random variables that follow a probability distribution estimated by both prior knowledge and collected data. For example, the proportion of students at a university taking a certain class could fall anywhere between 0 and 1. A Bayesian approach would estimate the expected proportion by combining prior knowledge about the popularity and difficulty of the class with data collected from a random sample of students. Monte Carlo allows for combinations of those "posterior" distributions, vastly expanding the analytic capabilities of the approach.

### Combining Distributions (Addition, etc.)
As visualized in the first graph exemplifying Monte Carlo, the sum of two distributions isn't necessarily the sum of the parameters of the same distribution class. In other words, the sum of two Uniform distributions, each ranging from A=1 to B=6, doesn't yield a Uniform distribution that goes between A=2 (1+1) and B=12. This can be mathematically proven, and there are cases where a known, parameterized distribution can be mathematically calculated from these combinations, but often a Monte Carlo approximation is the best method. From the simulated values, we can approximate the expected value of a population. Going back to the first example, the average approximated sum was 7.05, which is only 0.5 off from the true sum that would be expected to have the highest frequency.

### Predictive Distributions

### Multiple Population Parameters


## Other Applications

## Sources

[Britannica (Law of Large Numbers)](https://www.britannica.com/science/law-of-large-numbers)

[IBM Cloud Education (comprehensive overview of Monte Carlo)](https://www.ibm.com/cloud/learn/monte-carlo-simulation)

[Investopedia (Monte Carlo applications in risk and finance)](https://www.investopedia.com/terms/m/montecarlosimulation.asp)

Kroese, D. P., Brereton, T., Taimre, T., and Botev, Z. I. (2014). "Why the Monte Carlo method is so important today," _WIREs Computational Statistics (Vol 6)_ [online], 6, 386-392, DOI: [10.1002/wics.1314](https://doi.org/10.1002/wics.1314). Available at https://wires.onlinelibrary.wiley.com/doi/10.1002/wics.1314
Provides a definition, uses, applications outside of pure statistics, and the potential future of the Monte Carlo simulation.
