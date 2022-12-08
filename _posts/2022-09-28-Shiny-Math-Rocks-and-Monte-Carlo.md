---
layout: post
title:  "Shiny Math Rocks and Monte Carlo"
date:   2022-09-28
author: Ellen Wight
description: A Bayesian difference between means analysis using Monte Carlo simulation and D&D data from Critical Role
image: /assets/images/DnD.jpg
---

_note: this blog assumes a fundamental understanding of Bayesian thinking and/or Bayesian analysis for inference on means of counts (Poisson-Gamma)_

## Introduction to Monte Carlo
Monte Carlo is a simulation technique that uses repeated random trials to model the potential outcomes of an uncertain event and the probability of those outcomes, such as the expected sum from rolling two standard dice in a casino (Monte Carlo is in fact named after the well-known casino in Monaco). Given a known distribution or formula for a random variable X, it produces a large simulated dataset by randomly selecting potential values for X. The applications for this technique are near infinite, from risk analysis in finance to studying physical processes.

How large of a sample is needed? Generally about 10,000 simulated values are used to fulfill the Law of Large Numbers. As a quick illustration, consider the probability of rolling one standard die represented as a discrete uniform distribution. In R, we can approximate the known probability distribution using the `purrr:rdunif(n, 1, 6)` function as our Monte Carlo simulation with sample size n. Below are three graphs with increasing sample sizes to show the computational approach to the mathematically true probabilities.

<img src="https://github.com/emwight/notes-from-a-statistician/raw/main/assets/images/rolls.png" height="400" align="middle"/>

This isn't interesting for any analysis, of course; it simply visualizes the intuitive nature of Monte Carlo. If we draw many values from a known distribution with set parameters, it follows that a high enough sample size would yield an approximate shape of that distribution. By combining distribution simulations, however, Monte Carlo becomes useful for approximating what can't be calculated mathematically. What if we wanted to simulate not only the average sum of many dice rolled, but also if certain players at a dice-rolling game, such as Dungeons & Dragons, were inexplicably better at rolling dice than others? I won't get into testing the validity of the legendary [Wil Wheaton Curse](http://folklore.usc.edu/dungeons-and-dragons-superstition-wil-wheaton-dice-curse/) yet, but Monte Carlo can be used to settle a long-standing feud between two other players: Liam O'Brien and Sam Riegel.


## Data Description: D&D Feud 

<img src="https://github.com/emwight/notes-from-a-statistician/raw/main/assets/images/vote.jpeg" width="250" align="right"/>

Back in 2019, two players from the popular D&D show _Critical Role_ embarked on a [presidential campaign](https://criticalrole.fandom.com/wiki/D%26D_Beyond_Presidential_Campaign) for the company D&D Beyond. The two eventually reconciled and became copresidents, or maybe Sam just ran out of comedic ad read material from that bit. If in the future another election should approach, I suggest a battle royale rematch (in-game of course). Predicting the winner then begs the question: who deals the most damage on average? Both Sam and Liam have played rogues and characters in a full or partial-casting class (bard/artificer and wizard). As such, the deciding factor, besides chance, is how much each player builds their character to deal direct damage to enemies.

To model the probability of one player rolling better than another on average, I will use a Bayesian analysis of the Poisson-Gamma conjugate prior. Often, sums or averages of dice rolls suggest a Normal distribution. Considering the varying number of and type of dice rolled, however, I will treat the dice rolls as counts of damage dealt (measured in hit points) for the total damage roll from each attack. The Gamma distribution then models the average damage dealt, which generally will be on the lower end with some really high rolls as characters level up.

With a Gamma(1, 0.1) prior for both Liam and Sam, I updated the prior using the likelihood of observing the data that I obtained from [CritRoleStats](https://www.critrolestats.com). From that, I constructed a posterior distribution for each player:

<img src="https://github.com/emwight/notes-from-a-statistician/raw/main/assets/images/posterior.png" height="400"/>

Clearly Liam is the winner, but by what margin? After all this exposition, it's finally time to discover the power of Monte Carlo.

## Difference between Population Means

<img src="https://github.com/emwight/notes-from-a-statistician/raw/main/assets/images/bad plot.png" align="right" height="350"/>

Our analysis goal is to find a distribution representing the difference between average damage rolls for Liam and those for Sam, with the assumption that those two players are independent in how they roll. However, this isn't as simple as subtracting the parameters of the two posterior distributions. For one, let's say we wanted to find (Sam - Liam) as our difference. We'd end up with a Gamma(-4370,-149), which doesn't exist because the parameters for this distribution must be greater than 0. If we did (Liam - Sam), we'd at least get a distribution, but it doesn't reflect what we're trying to model.  We know we want a distribution that's centered around 0-5 HP, considering the visual difference between the plots of the posteriors. However, the graph to the right visualizes that merely subtracting parameters doesn't yield those results. This can also be proven mathematically through the Moment Generating Function, but the point is we need to approximate this distribution through a Monte Carlo simulation.

**General Algorithm**
1. Randomly sample a value from the posterior distribution of the first population, representing a randomly selected average for the variable of interest
2. Randomly sample a value from the posterior distribution of the second population, representing the same but for the other independent population
3. Calculate the difference between the two (value 1 - value 2 or the other way around; focus on what's easier for interpretation)
4. Repeat many times: As the sampled values from the two populations approach their true and known distributions, their difference approaches its true distribution

Without computers, this would be a long, arduous process. Luckily, R can both randomly sample from distributions with known parameters and easily perform element-wise operations on vectors. 

## The Victor of O'Brien v. Riegel
Applied to our problem, we translate the algorithm into R code as follows:

```{r}
# Step 0 (optional). Set the current time as a seed so the sampling is more random
set.seed(as.numeric(Sys.Date()))

# Step 4: Each rgamma(n, gamma, phi) will sample n=10000 values

# Step 1: Randomly sample values for average damage dealt from Liam's posterior distribution 
## (gamma and phi values seen in the earlier graph of posteriors)
liam_sim <- rgamma(10000, 12711, 571.1)

# Step 2: Randomly sample these averages, but from Sam's posterior distribution
sam_sim <- rgamma(10000, 8341, 422.1)

# Step 3: For each element in the vector of simulated values from Liam, subtract the corresponding simulated value from Sam
d_sim <- liam_sim - sam_sim
```

This final `d_sim` object stores the simulated values of our approximated distribution, and with it, the definitive victor.

![Difference between Means](https://github.com/emwight/notes-from-a-statistician/raw/main/assets/images/mc_diff.png)

As the graph above shows, Sam is nowhere close to building characters that deal the same amount of direct damage as Liam. In fact, there is a 95% probability that Liam, using our posterior distributions, rolls higher between about 1.9 and 3.9 hit points on average. These values, and the shape of the distribution, will change from iteration to iteration of the simulation, but by a marginal amount considering the high sample size of 10,000.

## Implications

On a slightly serious note, this isn't to say that Liam is better at character construction than Sam. It simply suggests that he focuses more on crafting his characters to deal direct combat in battle, while Sam focuses on other ways to assist in an encounter. Healing, buffing allies, debuffing enemies, and other tactics all contribute to the effectiveness of a character in taking down enemies while keeping the party as a whole alive; this analysis contributes just one small part of the picture.

On a slightly more serious note, this was a purposefully silly example that provided a lighthearted introduction to Monte Carlo capabilities. The exact algorithm we used to analyze D&D data could instead be used in a medical study comparing the average effects of different medications, comparing the extent of air pollution in two areas through average parts per million, or other more practical scenarios. Furthermore, applications of Monte Carlo extend far beyond comparing two means, exponentially increasing the inferential capabilities of Bayesian statistics.

## MC References

[IBM Cloud Education (provides a comprehensive overview)](https://www.ibm.com/cloud/learn/monte-carlo-simulation)

[Investopedia (applications in risk and finance)](https://www.investopedia.com/terms/m/montecarlosimulation.asp)

Kroese, D. P., Brereton, T., Taimre, T., and Botev, Z. I. (2014). "Why the Monte Carlo method is so important today," _WIREs Computational Statistics (Vol 6)_ [online], 6, 386-392, DOI: [10.1002/wics.1314](https://doi.org/10.1002/wics.1314). Available at https://wires.onlinelibrary.wiley.com/doi/10.1002/wics.1314

## Other Sources
All images came from free stock photos or graphs made with R by the author, except the image of the D&D Beyond presidential campaign (link [here](https://www.dndbeyond.com/critical-role-election))

As mentioned before, data for this analysis is thanks to [CritRoleStats](https://www.critrolestats.com), a fan-run website that keeps track of stats and lore from Critical Role
