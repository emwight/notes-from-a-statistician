---
layout: post
title:  "Last Seen: the FBI API for Art Crimes"
date:   2022-10-19
author: Ellen Wight
description: Stories of art crimes, while not the most-viewed subgenre of true crime, still capture national attention. This post shows how accessible the FBI art crime is and a possible way to clean the data for future analysis.
image: /assets/images/paints.jpg
---
_Complete code and data found in this [repo](https://github.com/emwight/artscraper)_

## Introduction
Imagine walking through a museum and entering a room full of frames like the ones pictured below. What do you see?

<img src="https://github.com/emwight/stat386-projects/raw/main/assets/images/missing.jpg" height="400" align="middle"/>

In 2013, French artist Sophie Callie posed [this question](https://aperture.org/editorial/what-do-you-see/) to visitors of the Isabella Stewart Gardner Museum. Both that project and her previous work _Last Seen_ (in which she instead interviewed museum employees) explored the cultural and artistic loss from a [theft in 1990](https://www.gardnermuseum.org/organization/theft), which involved thirteen works of art and still remains unsolved. This isn't an uncommon occurrence; the FBI has over 3,500 stolen art pieces publicly on file. Are there any significant commonalities or differences between the art pieces that are either taken or refuse to be found? For this project, I will get art theft data from the FBI, clean it for analysis, and perform an EDA on the resulting dataset in a future blog post. 

## Data Collection

<a title="Federal Bureau of Investigation, Public domain, via Wikimedia Commons" href="https://commons.wikimedia.org/wiki/File:Seal_of_the_Federal_Bureau_of_Investigation.svg"><img width="150" alt="Seal of the Federal Bureau of Investigation" src="https://upload.wikimedia.org/wikipedia/commons/thumb/d/da/Seal_of_the_Federal_Bureau_of_Investigation.svg/512px-Seal_of_the_Federal_Bureau_of_Investigation.svg.png" align="right"></a>

First, some ethical considerations about getting data off the web. Some sites forbid web scraping due to privacy or other concerns, and the last thing we want is to become thieves ourselves while obtaining art theft data. Some companies and organizations, however, provide an API, which is an ethical and user-friendly way to bridge the gap between website and extracted data. The FBI, for instance, has readily-available data on its [national stolen art file](https://www.fbi.gov/investigate/violent-crime/art-theft/national-stolen-art-file) through its [API](https://api.fbi.gov/docs#!/) and doesn't even require an API key.

### Requests
For the particularly coding-averse, the site with the API documentation includes "try it out" options to get direct JSON results and experiment with how the parameters work (for example, the **pageSize** parameter returns 50 results max, even if no error is thrown when a higher number is entered). In order to clean the data using the functionality of Python's `pandas`, however, it's more efficient to use the API in `requests` using the endpoint `https://api.fbi.gov/@artcrimes`. Through trial and error on the site, I found that the most important parameters for this API are **page**, which returns the specified page or section of the full JSON dictionary (1-72 pages), and **pageSize**, which returns the specified number of stolen art pieces from said page (1-50 results). By randomly selecting 16 pages and getting 25 results from each page, I easily "imported" my data into Python.

```
pages = [str(x) for x in rand.sample(range(1,73), 16)]
params = [{"pageSize" : "25", "page" : x} for x in pages_ex]
rs = [requests.get("https://api.fbi.gov/@artcrimes", param) for param in params]
```

I then extracted the JSON 'items' dictionary from each page and concatenated them into a pandas dataframe of stolen art items, keeping only columns of interest for analysis.
```
thievery = pd.concat([pd.DataFrame(r.json()['items']) for r in rs], ignore_index=True)
thievery = thievery.loc[:,['title', 'maker', 'crimeCategory', 'materials', 'period', 'measurements']]
```

<img src="https://github.com/emwight/stat386-projects/raw/main/assets/images/raw.png" height="400" align="middle"/>

Doesn't look very pretty, does it? At least for analysis purposes. Either because the site wasn't created with web scraping in mind (and most aren't), or the data entry team didn't set standardized practices for reporting measurements, or a myriad of other reasons. Before analysis, we will have to clean the data to get it into a more readable format for the computer instead of humans.

## Data Cleaning
Python, especially `pandas`, has countless functions to streamline data cleaning. The process is still tedious at best and endlessly time-consuming at worst, but going through each variable and cleaning on a case-by-case basis turns the frustration into a (hopefully) satisfying puzzle. Our dataset starts out with only string-type data, so these three functions will be our best friends:

`DataFrame['variable'].value_counts()`: This returns each level, and the corresponding frequency, of a categorical variable. For us, it's a way to check that every case has been accounted for.

`DataFrame['variable'].replace(string to find, string to replace, regex=True, inplace=True)`: This looks for a string within each value of the column and replaces it as specified. `regex = True` tells Python to use [regex](https://www.oreilly.com/content/an-introduction-to-regular-expressions/) rather than exact matching; inplace=True tells it to directly update the values in the dataset rather than return a copy of the data.

`np.where(condition, value if True, value if False)`: This is the equivalent of a simple if_else statement.

Most of the other functions used come from the pandas.Series.[str](https://pandas.pydata.org/docs/reference/api/pandas.Series.str.capitalize.html) methods, which are existing Python regex and other string functions applied to a Pandas column.

### The Good
Maker, Crime Category, and Materials were the simplest to standardize, requiring only small changes to correct differences in how data was entered. For example, a Crime Category would be coded either as "dolls,and,figurines" or "dolls-and-figurines," which give the same information to a human but look very different to a computer. This is relatively easy to fix, but demonstrates why standardizing data entry methods is crucial to saving time when the data is needed in the future.

### The Bad
 - period

### The Ugly
 - turning measurements into size (w/ units)

## Conclusion


<a title="Andre Carrotflower, CC BY-SA 4.0 &lt;https://creativecommons.org/licenses/by-sa/4.0&gt;, via Wikimedia Commons" href="https://commons.wikimedia.org/wiki/File:20180527_-_20_-_Boston,_MA_(Isabella_Stewart_Gardner_Museum).jpg"><img width="512" alt="20180527 - 20 - Boston, MA (Isabella Stewart Gardner Museum)" src="https://upload.wikimedia.org/wikipedia/commons/thumb/c/c5/20180527_-_20_-_Boston%2C_MA_%28Isabella_Stewart_Gardner_Museum%29.jpg/512px-20180527_-_20_-_Boston%2C_MA_%28Isabella_Stewart_Gardner_Museum%29.jpg"></a>

