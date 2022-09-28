---
layout: post
title:  "Shiny Math Rocks and Monte Carlo"
date:   2022-09-20
author: Ellen Wight
description: A difference between means analysis using Monte Carlo simulation and D&D data from Critical Role
image: /assets/images/DnD.jpg
---

## Introduction to Monte Carlo
Monte Carlo is a simulation technique that uses repeated random trials to model the potential outcomes of an uncertain event and the probability of those outcomes, such as the expected sum from rolling two standard dice in a casino (and is in fact named after the well-known casino in Monaco). Given a known distribution or formula for a random variable X, Monte Carlo produces a large simulated dataset by randomly selecting potential values for X given some paramater(s) that we can use as a starting point. The applications for this technique are near infinite, from risk analysis in finance to studying physical processes to exponentially increasing the inference capabilities of Bayesian statistics (more on that in a minute).

How large of a sample is needed? Generally about 10,000 simulated values are used, which can be quickly illustrated when simulating the probabilities from a known distribution, such as rolling one die where the probability for each value is 1/6.

In mathematical notation: $X \sim Discrete Uniform(1,6)$  

Computationally, we can approximate the known probability distribution using the `purrr:rdunif(n, a, b)` function in R as our Monte Carlo simulation. Below are three graphs with varying sample sizes to show the approach to the "correct" probability for each side as the sample size increases, a principle known as the Law of Large Numbers.

# IMAGE

This isn't interesting for any analysis, of course, but it does show the improvement of the approximation and the intuitive nature of Monte Carlo, if we draw many values from a distribution, it makes sense that a high enough sample size would yield an approximate shape of that distribution. In application, however, this makes Monte Carlo useful for finding properties of distributions that can't be calculated mathematically. What if we wanted to simulate the average sum of many dice rolled, and not only that, but if certain players at a dice-rolling game, such as Dungeons & Dragons, were inexplicably better at rolling dice than others? I won't get into testing the validity of the Wil Wheaton Curse yet, but Monte Carlo can be used to settle a long-standing feud between two other players: Liam O'Brien and Sam Riegel.

# link to wil wheaton curse

## Data Description: D&D Feud

## Monte Carlo and Bayesian Inference
One application of this method is approximating the distribution of the difference between two population means, given those populations are independent. Why can't this be directly calculated computationally? Well, take the Gamma distribution as an example




## Reference Sites
For sources and more information:
[IBM Cloud Education (provides a comprehensive overview)](https://www.ibm.com/cloud/learn/monte-carlo-simulation)
[Investopedia (applications in risk and finance)]https://www.investopedia.com/terms/m/montecarlosimulation.asp
https://wires.onlinelibrary.wiley.com/doi/10.1002/wics.1314

