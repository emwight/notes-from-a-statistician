---
layout: post
title:  "A Brief Overview of Monte Carlo Approximation"
date:   2022-10-09
author: Ellen Wight
description: A technical description of the computer simulation technique
image: /assets/images/lines.png
---

## Definition
Monte Carlo approximation is a computer simulation technique that uses repeated random trials to model the potential outcomes of an uncertain event. By comparing frequencies of said outcomes to total trials, it also models probabilities. For example, consider the expected sum from rolling two standard dice while gambling in the method's namesake, the casino in Monaco. Given a known distribution or formula (the sum of two dice, each with an equal chance of rolling a 1-6), Monte Carlo produces a large, simulated dataset by randomly selecting potential sums.

<img src="https://github.com/emwight/notes-from-a-statistician/raw/main/assets/images/Roll_sum.png" height="400" align="middle"/>

## Theory
How large of a sample is needed? The rule of thumb is generally 10,000 to fulfill the Law of Large Numbers (a theorem in statistics: as sample size from a population increases, the mean of that sample approaches the true population mean).  To illustrate the importance of a large sample size, consider the probability of rolling just one standard die. Below are three graphs with increasing sample sizes to show the computational approach to the mathematically true probabilities.

<img src="https://github.com/emwight/notes-from-a-statistician/raw/main/assets/images/rolls.png" height="400" align="middle"/>

This isn't interesting for any analysis, but it visualizes the intuitive nature of Monte Carlo. If we draw many values from a known distribution with set parameters, it follows that a high enough sample size would yield its approximate shape, center, and spread. By combining simulations, however, Monte Carlo becomes useful for approximating distributions that can't be calculated mathematically. What if we wanted to simulate not only the average sum of many dice rolled, but also if certain players at a dice-rolling game, such as Dungeons & Dragons, were inexplicably better at rolling dice than others? While collecting data by having players roll against each other hundreds of times could work, Monte Carlo simulation is efficient, easy, and versatile.

## Basic Applications in Bayesian Statistics
First, a brief word about Bayesian statistics, which is one way to supply a distribution to a simulation. Compared to the traditional Frequentist approach to statistics, which considers population parameters as fixed values, Bayesian analysis states that population parameters are unknown random variables that follow a probability distribution estimated by both prior knowledge and collected data. For example, the sum of a roll for damage in D&D could fall anywhere from one to infinity (technically). A basic analysis involves three components:

  1. Prior: This is a distribution for the population that uses our prior knowledge about that population, for example watching or playing D&D gives a prior expectation for how high dice rolls could be on average.
  2. Data and Likelihood: The likelihood is a distribution on the data we collect to update our prior knowledge. It is the likelihood of getting the data given a specific value for the population parameter, for example the probability of rolling a five from the sum of two dice.
  3. Posterior: By combining the prior and likelihood, we calculate a posterior distribution, or a distribution for the population that's now updated by our observed data. For example, after incorporating a dataset of dice rolls, we have a better idea of average dice rolls than we did before.
 
Monte Carlo allows for combinations of those posterior distributions, vastly expanding the analytic capabilities of Bayesian statistics.

### Combining Distributions (Addition, etc.)
The sum of two distributions isn't just the sum of the parameters used in the same distribution type &mdash; just look at the previous example of the sum of two dice rolls. The sum of two Uniform distributions, each ranging from A=1 to B=6, doesn't yield a Uniform distribution that goes between A=2 and B=12. In the case where a direct mathematical derivation isn't possible, Monte Carlo provides an approximation of the shape, spread, and center of the distribution of interest by sampling from each posterior distribution and taking its sum, difference, etc. 

### Predictive Distributions
A second issue that Monte Carlo solves is the question of average vs. individual: we may know the average sum of two dice (in a casino winnings or losses on average), but often we want to know what the next roll will be. Monte Carlo can predict that next roll by simulating new data based on the known likelihood. First, random parameter values are drawn from the posterior to create sample populations. Second, one sample or predicted "data point" is drawn from the likelihood using each simulated population parameter. The result is a distribution on the individual occurrences of data points, independent of the distribution on the population parameter.

## Monte Carlo and D&D
As a brief demonstration of the applicability of Monte Carlo, let's look at an example difference between average counts and a predictive distribution. For our D&D players, let's look at rolls for damage to enemies from Liam O'Brien and Sam Riegel from the show _Critical Role_. Combining prior knowledge and data from [CritRoleStats](https://www.critrolestats.com/), Bayesian analysis first produces these two distinct posterior distributions:

<img src="https://github.com/emwight/notes-from-a-statistician/raw/main/assets/images/posterior.png" height="400"/>

Liam clearly rolls higher than Sam, but by how much on average? [Using R and Monte Carlo](https://emwight.github.io/stat386-projects/2022/09/28/Shiny-Math-Rocks-and-Monte-Carlo.html), we simulate the difference between average damage roll and produce a single posterior distribution:

<img src="https://github.com/emwight/notes-from-a-statistician/raw/main/assets/images/mc_diff.png" height="400"/>

The two distinct posteriors are centered about three points apart from each other, which is reflected in this difference between average counts. We also get additional information on the spread of the average difference. Finally, if we wanted to determine the probability of Liam "winning" against Sam from a single damage roll, we would simulate a predictive distribution:

<img src="https://github.com/emwight/notes-from-a-statistician/raw/main/assets/images/predictive.png" height="400"/>

If Liam and Sam were to each roll one time for damage, Liam is predicted to win about 62% of the time. Overall, he'll win more often, which agrees with the average difference between counts determined earlier, but there's more room for individual variation.

## Other Applications
This brief introduction to Monte Carlo only explained a couple of basic applications for the technique. Other uses include:
  - Markov Chain Monte Carlo (MCMC): this more complicated subcategory combines the iterative sampling of Monte Carlo and the probability space of Markov Chains, which creates a sequential "chain" of sampled values. Specific adaptations include Gibbs sampling and Metropolis random walk.
  - Optimization: Monte Carlo can quickly simulate potential outcomes, which can efficiently determine the best outcome out of a specific sample space. In industrial research, it optimizes efficiency, use of materials, scheduling, and more.
  - Physical Process Modeling: In physics, chemistry, and biology, Monte Carlo simulates processes and analyzes expected behavior of new materials given limited data or simply theoretical formulas.
  - Finance and Risk: Given a model or formula, Monte Carlo can enumerate potential outcomes and their probabilities. One application is the simulation of investment trajectories, which determines risk associated with different stocks.

## Resources

[Britannica (Law of Large Numbers)](https://www.britannica.com/science/law-of-large-numbers)

[IBM Cloud Education (comprehensive overview of Monte Carlo)](https://www.ibm.com/cloud/learn/monte-carlo-simulation)

[Investopedia (Monte Carlo applications in risk and finance)](https://www.investopedia.com/terms/m/montecarlosimulation.asp)

[Towards Data Science (introduction to MCMC)](https://towardsdatascience.com/monte-carlo-markov-chain-mcmc-explained-94e3a6c8de11)

Kroese, D. P., Brereton, T., Taimre, T., and Botev, Z. I. (2014). "Why the Monte Carlo method is so important today," _WIREs Computational Statistics (Vol 6)_ [online], 6, 386-392, DOI: [10.1002/wics.1314](https://doi.org/10.1002/wics.1314). Available at https://wires.onlinelibrary.wiley.com/doi/10.1002/wics.1314
> Provides a definition, uses, applications outside of pure statistics, and the potential future of the Monte Carlo simulation.

van Ravenzwajj, D., Cassey, P., Brown, S. D. (2018). "A simple introduction to Markov Chain Monte &ndash; Carlo sampling," _Psychonomic Bulliten and Review (Vol 25)_ [online], 143-154, DOI: [10.3758/s13423-016-1015-8](https://doi.org/10.3758/s13423-016-1015-8). Available at https://link.springer.com/article/10.3758/s13423-016-1015-8.
> Provides a definition of MCMC sampling and examples of Gibbs sampling and Metropolis random walk, as well as a technique that combines those two methods.
