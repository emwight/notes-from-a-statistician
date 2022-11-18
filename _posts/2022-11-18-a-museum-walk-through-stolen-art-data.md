---
layout: post
title:  "A Museum Walk through Stolen Art Data"
date:   2022-11-18
author: Ellen Wight
description: Building upon the last post about collecting FBI API data, we now move to exploring our data for patterns and interesting outliers.
image: /assets/images/painting.jpg
---

# Introduction
Art theft isn't an uncommon practice (as evidenced by the over 4,000 pieces listed on just the FBI's [national stolen art file](https://www.fbi.gov/investigate/violent-crime/art-theft/national-stolen-art-file)), but what kinds of art pieces are more likely to be stolen or remain hidden? Using the FBI's [API](https://api.fbi.gov/docs#!/), I randomly sampled 1,000 pieces on file and cleaned the data to determine if there would be any patterns or significant outliers. More information can be found in my original [data cleaning post](https://emwight.github.io/stat386-projects/2022/10/19/Last-Seen-the-FBI-API-for-Art-Crimes.html), with updates to my process listed in my [repo README](https://github.com/emwight/artscraper).

There are a few limitations of this exploratory data analysis: The FBI really needs to release more information if it wants the public to help look for these stolen pieces (maybe it doesn't, in which case good job). While there is readily available data to describe the characteristics of each piece, there was little to no mention made of the country of origin, monetary worth, or date of theft. If anyone has ideas on how to more easily find that data in an ethical way, please let me know in the comments!

# Measurements: How Big Is Too Big to Steal?
<img src="https://github.com/emwight/stat386-projects/raw/main/assets/images/with outliers.png" width="500" align="right"/>

One limitation of an art theft is how big an art piece is, so what's the general size of a stolen piece? Works were either coded with their length (such as swords), area (paintings), or volume (sculptures), so boxplots of the sizes for each were made. As can be clearly seen from the initial plot, there are a lot of outliers. I tested out dealing with this in two ways: removing them and filling them in. <br /><br /><br /><br /><br /><br />

## Outlier Exploration
When removing outliers, I set arbitrary cut-offs based on the initial plot to reduce the number of outliers without removing all of my data. When filling outliers, I replaced values above the 95th percentile with values randomly sampled from the interquartile range. This inflated the amount of data in the middle of the range but had more variation than simply replacing outliers with the median. <br />

<p float="left">
  
  <img src="https://github.com/emwight/stat386-projects/raw/main/assets/images/without outliers.png" width="400" />
  <img src="https://github.com/emwight/stat386-projects/raw/main/assets/images/filled outliers.png" width="400" /> 
  
</p>

Summary statistics from each method can be seen in the table below. The median didn't change much between the different methods, but the mean and standard deviation did. This may not be an issue since the data are skewed already. If we wanted to perform any means-based analysis, however, we'd want to be cautious about which method we went with to fill in values.

<img src="https://github.com/emwight/stat386-projects/raw/main/assets/images/summarize.png" width="500"/>


## Most Extreme Outliers
Finally, it's important to look at why the outliers might exist. Most stolen pieces are easily small enough for a thief to carry to a small truck, but how were the larger items likely stolen? Interestingly, the largest outliers for two-dimensional art pieces weren't bulky paintings - they were tapestries that a thief could roll up and carry with more ease. The largest one-dimensional outlier wasn't too long, all things considered, and the three-dimensional outliers look like cases of questionable data entry or choice of measurement (granted, I don't know much about rifles; they could come in sizes that large). When analyzing the data later, it might be fine to remove these high outlier values and replace the rest with random values from the IQR.

<img src="https://github.com/emwight/stat386-projects/raw/main/assets/images/outliers.png" align="middle" width="800"/>

<br />

# Theft Popularity by Artwork Characteristic
## Creator
I pulled up a list of the top artists (companies or countries weren't considered) and manually found the country of origin for each artist. Most are from the U.S., which isn't surprising considering the data comes from the FBI, but it was interesting that I only recognized two of the names on the list. It would be interesting to explore whether these artists are more rare/valuable, or if they simply produced more works that were stored in museums that had fewer security measures.

<img src="https://github.com/emwight/stat386-projects/raw/main/assets/images/creator.png" align="middle"/>

## Century
The most stolen paintings came from the 20th century, which suggests that the U.S. hosts more modern art pieces than other countries that are known for their older works (looking at you, France). Alternatively, older pieces are placed under stricter security measures and are less likely to be stolen.

<img src="https://github.com/emwight/stat386-projects/raw/main/assets/images/century.png" width = "600" align="middle"/>

## Materials
Finally, I looked at the most common materials used to make the most commonly stolen types of artwork, as shown below. Like creator, this could indicate greater value or simply greater frequency of said category or material.

<img src="https://github.com/emwight/stat386-projects/raw/main/assets/images/materials.png" width = "800" align="middle"/>

# Conclusion
Exploring this dataset showed which types of artwork (characterized by category, century created, or creator) have most frequently been stolen and remain missing. Most art thefts are relatively smaller pieces, which could indicate small-time thefts or larger robberies of many smaller items. In a future analysis of the data, it would probably be most interesting to look at what sets the outliers apart from the average pieces and offer suggestions as to which types of art should be kept under tighter security. It would be helpful to have monetary value, date stolen, and additional data of artworks that had been stolen and recovered, but that can be added to this preliminary analysis later. Check back in soon for a more interactive dashboard of my further findings!
