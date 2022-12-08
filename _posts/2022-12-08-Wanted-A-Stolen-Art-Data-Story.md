---
layout: post
title:  "Wanted: A Stolen Art Data Story"
date:   2022-12-08
author: Ellen Wight
description: After collecting and exploring the FBI API data, this post presents a graphic of primary findings
image: /assets/images/museum.png
---
_Complete code and data found in this [repo](https://github.com/emwight/artscraper)_

# Project Summary
Over 30 years ago, 13 valuable art pieces were stolen from the [Isabella Gardner Stewart Museum](https://www.gardnermuseum.org/organization/theft). Despite an FBI investigation and creative press coverage from [Sophie Calle's art projects](https://aperture.org/editorial/what-do-you-see/) to a [_Buzzfeed Unsolved_ episode](https://youtu.be/mkxQXxKSWKQ), these works have never been recovered. Unfortunately, this doesn't seem to be a rare occurrence, as there are over 4,000 entries in the FBI [National Stole Art File](https://www.fbi.gov/investigate/violent-crime/art-theft/national-stolen-art-file) alone. Since some of that FBI data is readily and publicly available through the bureau's [API](https://api.fbi.gov/docs#!/Art32Crimes/get_artcrimes), this project analyzes a random sample of those stolen works to see if there are any significant similarities or differences between what pieces are either stolen in the first place or refuse to be found after decades of searching.

Now, the FBI doesn't provide every detail about these stolen works. Perhaps they don't want thieves to use this data to strategically plan easy thefts, or they don't want to turn the public into a massive art thief detective agency. Valuable information such as the monetary value of the piece, when and from where it was stolen, and (most unfortunately) the reward amount offered are all missing from the dataset. However, interesting facts about the physical characteristics of each piece are included, which can give us a sense of what kinds of art are targeted most often. After extensive data cleaning (see [this post](https://emwight.github.io/notes-from-a-statistician/2022/10/19/Last-Seen-the-FBI-API-for-Art-Crimes.html) and the ReadMe of [this repo](https://github.com/emwight/artscraper) for information on data collection) and some [exploration](https://emwight.github.io/notes-from-a-statistician/2022/11/18/a-museum-walk-through-stolen-art-data.html), a few patterns emerged.

# Data Story
The graphic below summarizes and expands on the most interesting findings from the earlier exploratory data analysis. The three most common times of art pieces stolen were paintings, sculptures, and print (photographs), so the data story focuses most on those categories. The artist category varied a lot, so while a section on the most common artists was included, artist was not considered when determining the most typical piece stolen. If worldwide art theft data could be obtained, country of origin would also be included on that list. Finally, the EDA heavily explored the distribution of the size of stolen works, including possible ways to deal with outliers. However, for a brief summary such as this, it seemed more fitting to focus on the largest pieces and consider how such large pieces were stolen. The largest two-dimensional piece, for example, was a tapestry, meaning that a thief could roll it up and carry it out more easily than they would be able to do with a painting.

<img src="https://github.com/emwight/notes-from-a-statistician/raw/main/assets/images/Stat 386 data story.png"/>

I hope this data story has been illuminating; please comment any ideas for further questions or variables to consider in future analysis or feel free to use my code or data to find your own conclusions!
