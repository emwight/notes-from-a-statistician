---
layout: post
title:  "Shiny Math Rocks and Monte Carlo"
date:   2022-09-20
author: Ellen Wight
description: A Bayesian difference between means analysis using Monte Carlo simulation and D&D data from Critical Role
image: /assets/images/DnD.jpg
---

_note: this blog assumes a baseline knowledge of Bayesian thinking and/or Bayesian analysis for inference on means of counts (Poisson-Gamma)_

## Introduction to Monte Carlo
Monte Carlo is a simulation technique that uses repeated random trials to model the potential outcomes of an uncertain event and the probability of those outcomes, such as the expected sum from rolling two standard dice in a casino (and is in fact named after the well-known casino in Monaco). Given a known distribution or formula for a random variable X, Monte Carlo produces a large simulated dataset by randomly selecting potential values for X given some paramater(s) that we can use as a starting point. The applications for this technique are near infinite, from risk analysis in finance to studying physical processes to exponentially increasing the inferential capabilities of Bayesian statistics.

How large of a sample is needed? Generally about 10,000 simulated values are used to fulfill the Law of Large Numbers. As a quick illustration, consider the probability of rolling one standard die represented as a discrete uniform distribution. In R, we can approximate the known probability distribution using the `purrr:rdunif(n, 1, 6)` function as our Monte Carlo simulation with sample size n. Below are three graphs with increasing sample sizes to show the computational approach to the mathematically true probabilities.

![Law of Large Numbers](/assets/images/rolls.png)

This isn't interesting for any analysis, of course; it simply visualizes the intuitive nature of Monte Carlo. If we draw many values from a distribution, it follows that a high enough sample size would yield an approximate shape of that distribution. By combining distribution simulations, however, Monte Carlo becomes useful for approximating what can't be calculated mathematically. What if we wanted to simulate not only the average sum of many dice rolled, but also if certain players at a dice-rolling game, such as Dungeons & Dragons, were inexplicably better at rolling dice than others? I won't get into testing the validity of the legendary [Wil Wheaton Curse](http://folklore.usc.edu/dungeons-and-dragons-superstition-wil-wheaton-dice-curse/) yet, but Monte Carlo can be used to settle a long-standing feud between two other players: Liam O'Brien and Sam Riegel.

## Data Description: D&D Feud
Back in 2019, two players (mortal enemies$^1$) from the popular D&D show _Critical Role_ campaigned for the position of president of the company D&D Beyond. While neither player was declared the u. . . I, however, suggest a rematch: who builds characters that deal the most damage on average? Both Sam and Liam have played rogues and one character in a full-casting class (wizard and bard). As such, the deciding factor, besides chance, is how much each player builds their character to deal direct damage to enemies.

To model the probability of one player rolling better than another on average, I will use a Bayesian analysis of the Poisson-Gamma conjugate prior. Normally, sums or averages of dice rolls suggest a Normal distribution, but considering the varying number of dice and type of dice rolled, I will treat the data points as counts of damage dealt (measured in hit points) for each total damage roll. The Gamma distribution then models the average damage dealt, which generally will be on the lower end with some really high rolls as characters level up.

With a $\lambda \sim \text{Gamma}(1,0.1)$ prior for both Liam and Sam, I update the distribution on $\lambda$ with the likelihood of observing the data I obtained from [critrolestats](https://www.critrolestats.com), I constructed a posterior distribution for each player:


![Posteriors](/assets/images/posterior.png)

# Image + background, character class balance, why using Pois-Gamma (count if doing a sum? -> count of HP damage dealt since type of die and number of dice rolled vary a lot, so Pois for data and Gamma represents expected sum of rolls)

# Quick explanation of Pois-Gamma, with graphs of Prior and Posterior

# CITE CRITROLESTATS WITH LINK
# IMAGE EXPLAINING BAYESIAN ANALYSIS PRIOR -> POSTERIOR
# LINK TO REST OF CODE
# ADD FOOTNOTE ABOUT THE MORTAL ENEMIES THING LOL

## Difference between Population Means

# Explain why MC needed, basic algorithm

1. Draw a value from the posterior distribution of population 1
2. Draw a value from the posterior distribution of population 2
3. Calculate the difference between the two
4. Repeat many times

With the element-wise function capabilities that R has for vectors, this becomes very simple computationally:

# R Code Here

## The Victor of O'Brien v. Riegel

![Posteriors](/assets/images/mc_diff.png)

# Graphs, credible interval, analysis of how players construct their characters (without going too much into lore or stats lol)

# greater implications of this silly example


## Monte Carlo Reference Sites
For sources and more information:

[IBM Cloud Education (provides a comprehensive overview)](https://www.ibm.com/cloud/learn/monte-carlo-simulation)

[Investopedia (applications in risk and finance)](https://www.investopedia.com/terms/m/montecarlosimulation.asp)

Kroese, D. P., Brereton, T., Taimre, T., and Botev, Z. I. (2014). "Why the Monte Carlo method is so important today," _WIREs Computational Statistics (Vol 6)_ [online], 6, 386-392, DOI: [10.1002/wics.1314](https://doi.org/10.1002/wics.1314). Available at https://wires.onlinelibrary.wiley.com/doi/10.1002/wics.1314
