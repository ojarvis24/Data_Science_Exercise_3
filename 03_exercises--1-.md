---
title: 'Weekly Exercises #3'
author: "Olivia Jarvis"
output: 
  html_document:
    keep_md: TRUE
    toc: TRUE
    toc_float: TRUE
    df_print: paged
    code_download: true
---


```r
library(tidyverse)     
```

```
## ── Attaching packages ─────────────────────────────────────── tidyverse 1.3.1 ──
```

```
## ✓ ggplot2 3.3.5     ✓ purrr   0.3.4
## ✓ tibble  3.1.6     ✓ dplyr   1.0.7
## ✓ tidyr   1.1.4     ✓ stringr 1.4.0
## ✓ readr   2.1.1     ✓ forcats 0.5.1
```

```
## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
## x dplyr::filter() masks stats::filter()
## x dplyr::lag()    masks stats::lag()
```

```r
library(gardenR)       
library(lubridate)     
```

```
## 
## Attaching package: 'lubridate'
```

```
## The following objects are masked from 'package:base':
## 
##     date, intersect, setdiff, union
```

```r
library(ggthemes)      
library(geofacet)      
theme_set(theme_minimal())       
```


```r
data("garden_harvest")
data("garden_spending")
data("garden_planting")
breed_traits <- readr::read_csv('https://raw.githubusercontent.com/rfordatascience/tidytuesday/master/data/2022/2022-02-01/breed_traits.csv')
```

```
## Rows: 195 Columns: 17
```

```
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr  (3): Breed, Coat Type, Coat Length
## dbl (14): Affectionate With Family, Good With Young Children, Good With Othe...
```

```
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

```r
trait_description <- readr::read_csv('https://raw.githubusercontent.com/rfordatascience/tidytuesday/master/data/2022/2022-02-01/trait_description.csv')
```

```
## Rows: 16 Columns: 4
```

```
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr (4): Trait, Trait_1, Trait_5, Description
```

```
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

```r
breed_rank_all <- readr::read_csv('https://raw.githubusercontent.com/rfordatascience/tidytuesday/master/data/2022/2022-02-01/breed_rank.csv')
```

```
## Rows: 195 Columns: 11
```

```
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr (3): Breed, links, Image
## dbl (8): 2013 Rank, 2014 Rank, 2015 Rank, 2016 Rank, 2017 Rank, 2018 Rank, 2...
```

```
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

```r
kids <- readr::read_csv('https://raw.githubusercontent.com/rfordatascience/tidytuesday/master/data/2020/2020-09-15/kids.csv')
```

```
## Rows: 23460 Columns: 6
```

```
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr (2): state, variable
## dbl (4): year, raw, inf_adj, inf_adj_perchild
```

```
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

## Setting up on GitHub!

Before starting your assignment, you need to get yourself set up on GitHub and make sure GitHub is connected to R Studio. To do that, you should read the instruction (through the "Cloning a repo" section) and watch the video [here](https://github.com/llendway/github_for_collaboration/blob/master/github_for_collaboration.md). Then, do the following (if you get stuck on a step, don't worry, I will help! You can always get started on the homework and we can figure out the GitHub piece later):

* Create a repository on GitHub, giving it a nice name so you know it is for the 3rd weekly exercise assignment (follow the instructions in the document/video).  
* Copy the repo name so you can clone it to your computer. In R Studio, go to file --> New project --> Version control --> Git and follow the instructions from the document/video.  
* Download the code from this document and save it in the repository folder/project on your computer.  
* In R Studio, you should then see the .Rmd file in the upper right corner in the Git tab (along with the .Rproj file and probably .gitignore).  
* Check all the boxes of the files in the Git tab and choose commit.  
* In the commit window, write a commit message, something like "Initial upload" would be appropriate, and commit the files.  
* Either click the green up arrow in the commit window or close the commit window and click the green up arrow in the Git tab to push your changes to GitHub.  
* Refresh your GitHub page (online) and make sure the new documents have been pushed out.  
* Back in R Studio, knit the .Rmd file. When you do that, you should have two (as long as you didn't make any changes to the .Rmd file, in which case you might have three) files show up in the Git tab - an .html file and an .md file. The .md file is something we haven't seen before and is here because I included `keep_md: TRUE` in the YAML heading. The .md file is a markdown (NOT R Markdown) file that is an interim step to creating the html file. They are displayed fairly nicely in GitHub, so we want to keep it and look at it there. Click the boxes next to these two files, commit changes (remember to include a commit message), and push them (green up arrow).  
* As you work through your homework, save and commit often, push changes occasionally (maybe after you feel finished with an exercise?), and go check to see what the .md file looks like on GitHub.  
* If you have issues, let me know! This is new to many of you and may not be intuitive at first. But, I promise, you'll get the hang of it! 



## Instructions

* Put your name at the top of the document. 

* **For ALL graphs, you should include appropriate labels.** 

* Feel free to change the default theme, which I currently have set to `theme_minimal()`. 

* Use good coding practice. Read the short sections on good code with [pipes](https://style.tidyverse.org/pipes.html) and [ggplot2](https://style.tidyverse.org/ggplot2.html). **This is part of your grade!**

* When you are finished with ALL the exercises, uncomment the options at the top so your document looks nicer. Don't do it before then, or else you might miss some important warnings and messages.


## Warm-up exercises with garden data

These exercises will reiterate what you learned in the "Expanding the data wrangling toolkit" tutorial. If you haven't gone through the tutorial yet, you should do that first.

  1. Summarize the `garden_harvest` data to find the total harvest weight in pounds for each vegetable and day of week (HINT: use the `wday()` function from `lubridate`). Display the results so that the vegetables are rows but the days of the week are columns.


```r
garden_harvest %>% 
  mutate(weight_lbs = (weight*(0.00220462))) %>% 
  mutate(weekday = wday(date, label = TRUE)) %>% 
  group_by(vegetable,weekday) %>% 
  summarize(totalweight_weekday = sum(weight_lbs)) %>% 
  arrange(desc(weekday)) %>% 
  pivot_wider(names_from = weekday, 
              values_from = totalweight_weekday)
```

```
## `summarise()` has grouped output by 'vegetable'. You can override using the `.groups` argument.
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["vegetable"],"name":[1],"type":["chr"],"align":["left"]},{"label":["Sat"],"name":[2],"type":["dbl"],"align":["right"]},{"label":["Fri"],"name":[3],"type":["dbl"],"align":["right"]},{"label":["Thu"],"name":[4],"type":["dbl"],"align":["right"]},{"label":["Wed"],"name":[5],"type":["dbl"],"align":["right"]},{"label":["Tue"],"name":[6],"type":["dbl"],"align":["right"]},{"label":["Mon"],"name":[7],"type":["dbl"],"align":["right"]},{"label":["Sun"],"name":[8],"type":["dbl"],"align":["right"]}],"data":[{"1":"apple","2":"0.34392072","3":"NA","4":"NA","5":"NA","6":"NA","7":"NA","8":"NA"},{"1":"asparagus","2":"0.04409240","3":"NA","4":"NA","5":"NA","6":"NA","7":"NA","8":"NA"},{"1":"basil","2":"0.41005932","3":"0.46737944","4":"0.02645544","5":"NA","6":"0.11023100","7":"0.0661386","8":"NA"},{"1":"beans","2":"4.70906832","3":"1.52559704","4":"3.39291018","5":"4.08295624","6":"4.38719380","7":"6.5080382","8":"1.91361016"},{"1":"beets","2":"0.37919464","3":"0.02425082","4":"11.89172028","5":"0.18298346","6":"0.15873264","7":"0.6724091","8":"0.32187452"},{"1":"carrots","2":"2.33028334","3":"2.13848140","4":"2.67420406","5":"5.56225626","6":"0.35273920","7":"0.8708249","8":"2.93655384"},{"1":"cilantro","2":"0.03747854","3":"0.07275246","4":"NA","5":"NA","6":"0.00440924","7":"NA","8":"NA"},{"1":"corn","2":"1.31615814","3":"3.44802568","4":"NA","5":"5.30211110","6":"0.72752460","7":"0.7583893","8":"1.45725382"},{"1":"cucumbers","2":"9.64080326","3":"7.42956940","4":"3.30693000","5":"5.30652034","6":"10.04645334","7":"4.7752069","8":"3.10410496"},{"1":"edamame","2":"4.68922674","3":"NA","4":"NA","5":"NA","6":"1.40213832","7":"NA","8":"NA"},{"1":"jalapeño","2":"1.50796008","3":"1.29411194","4":"0.22487124","5":"0.48060716","6":"0.54895038","7":"5.5534378","8":"0.26234978"},{"1":"kale","2":"1.49032312","3":"0.38139926","4":"0.27998674","5":"0.61729360","6":"0.28219136","7":"2.0679336","8":"0.82673250"},{"1":"lettuce","2":"1.31615814","3":"1.80117454","4":"2.45153744","5":"1.18608556","6":"0.91712192","7":"2.4581513","8":"1.46607230"},{"1":"onions","2":"1.91361016","3":"0.07275246","4":"0.60186126","5":"NA","6":"0.70768302","7":"0.5092672","8":"0.26014516"},{"1":"peas","2":"2.85277828","3":"0.93696350","4":"3.39731942","5":"1.08026380","6":"2.06793356","7":"4.6341112","8":"2.05691046"},{"1":"peppers","2":"1.38229674","3":"0.33510224","4":"0.70988764","5":"2.44271896","6":"1.44402610","7":"2.5264945","8":"0.50265336"},{"1":"potatoes","2":"2.80207202","3":"3.74124014","4":"11.85203712","5":"4.57017726","6":"NA","7":"0.9700328","8":"NA"},{"1":"pumpkins","2":"92.68883866","3":"NA","4":"NA","5":"NA","6":"31.85675900","7":"30.1195184","8":"NA"},{"1":"radish","2":"0.23148510","3":"0.19400656","4":"0.14770954","5":"NA","6":"0.09479866","7":"0.1962112","8":"0.08157094"},{"1":"raspberries","2":"0.53351804","3":"0.57099658","4":"0.28880522","5":"NA","6":"0.33510224","7":"0.1300726","8":"NA"},{"1":"rutabaga","2":"6.89825598","3":"3.57809826","4":"NA","5":"NA","6":"NA","7":"NA","8":"19.26396956"},{"1":"spinach","2":"0.26014516","3":"0.19621118","4":"0.23368972","5":"0.21384814","6":"0.49603950","7":"0.1477095","8":"0.48722102"},{"1":"squash","2":"56.22221924","3":"NA","4":"NA","5":"NA","6":"18.46810174","7":"24.3345956","8":"NA"},{"1":"strawberries","2":"0.16975574","3":"0.48722102","4":"0.08818480","5":"NA","6":"NA","7":"0.4784025","8":"0.08157094"},{"1":"Swiss chard","2":"0.73413846","3":"0.61729360","4":"2.23107544","5":"0.90830344","6":"0.07054784","7":"1.0736499","8":"1.24781492"},{"1":"tomatoes","2":"35.12621046","3":"85.07628580","4":"34.51773534","5":"58.26590198","6":"48.75076206","7":"11.4926841","8":"75.60964752"},{"1":"zucchini","2":"3.41495638","3":"18.72163304","4":"34.63017096","5":"2.04147812","6":"16.46851140","7":"12.1959578","8":"12.23564100"},{"1":"broccoli","2":"NA","3":"0.16534650","4":"NA","5":"0.70768302","6":"NA","7":"0.8201186","8":"1.25883802"},{"1":"kohlrabi","2":"NA","3":"NA","4":"0.42108242","5":"NA","6":"NA","7":"NA","8":"NA"},{"1":"chives","2":"NA","3":"NA","4":"NA","5":"0.01763696","6":"NA","7":"NA","8":"NA"},{"1":"hot peppers","2":"NA","3":"NA","4":"NA","5":"0.06834322","6":"0.14109568","7":"1.2588380","8":"NA"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>

  2. Summarize the `garden_harvest` data to find the total harvest in pound for each vegetable variety and then try adding the plot from the `garden_planting` table. This will not turn out perfectly. What is the problem? How might you fix it?


```r
garden_harvest %>% 
  mutate(weight_lbs = (weight*(0.00220462))) %>%
  group_by(vegetable) %>% 
  summarize(totalweight_lbs = sum(weight_lbs)) %>% 
  left_join(garden_planting, select("plot"), 
            by = c("vegetable"))
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["vegetable"],"name":[1],"type":["chr"],"align":["left"]},{"label":["totalweight_lbs"],"name":[2],"type":["dbl"],"align":["right"]},{"label":["plot"],"name":[3],"type":["chr"],"align":["left"]},{"label":["variety"],"name":[4],"type":["chr"],"align":["left"]},{"label":["number_seeds_planted"],"name":[5],"type":["dbl"],"align":["right"]},{"label":["date"],"name":[6],"type":["date"],"align":["right"]},{"label":["number_seeds_exact"],"name":[7],"type":["lgl"],"align":["right"]},{"label":["notes"],"name":[8],"type":["chr"],"align":["left"]}],"data":[{"1":"apple","2":"0.34392072","3":"NA","4":"NA","5":"NA","6":"<NA>","7":"NA","8":"NA"},{"1":"asparagus","2":"0.04409240","3":"NA","4":"NA","5":"NA","6":"<NA>","7":"NA","8":"NA"},{"1":"basil","2":"1.08026380","3":"potB","4":"Isle of Naxos","5":"40","6":"2020-05-16","7":"FALSE","8":"NA"},{"1":"beans","2":"26.51937398","3":"M","4":"Bush Bush Slender","5":"30","6":"2020-05-16","7":"FALSE","8":"NA"},{"1":"beans","2":"26.51937398","3":"D","4":"Bush Bush Slender","5":"10","6":"2020-05-21","7":"TRUE","8":"NA"},{"1":"beans","2":"26.51937398","3":"K","4":"Chinese Red Noodle","5":"5","6":"2020-05-25","7":"TRUE","8":"NA"},{"1":"beans","2":"26.51937398","3":"L","4":"Chinese Red Noodle","5":"5","6":"2020-05-25","7":"TRUE","8":"NA"},{"1":"beans","2":"26.51937398","3":"E","4":"Classic Slenderette","5":"29","6":"2020-06-20","7":"TRUE","8":"NA"},{"1":"beets","2":"13.63116546","3":"H","4":"Gourmet Golden","5":"40","6":"2020-05-02","7":"FALSE","8":"NA"},{"1":"beets","2":"13.63116546","3":"H","4":"Sweet Merlin","5":"40","6":"2020-05-02","7":"FALSE","8":"NA"},{"1":"broccoli","2":"2.95198618","3":"P","4":"Yod Fah","5":"25","6":"2020-05-16","7":"FALSE","8":"NA"},{"1":"broccoli","2":"2.95198618","3":"D","4":"Main Crop Bravado","5":"7","6":"2020-05-22","7":"TRUE","8":"NA"},{"1":"broccoli","2":"2.95198618","3":"I","4":"Main Crop Bravado","5":"7","6":"2020-05-22","7":"TRUE","8":"NA"},{"1":"carrots","2":"16.86534300","3":"H","4":"Bolero","5":"50","6":"2020-05-02","7":"FALSE","8":"NA"},{"1":"carrots","2":"16.86534300","3":"H","4":"King Midas","5":"50","6":"2020-05-02","7":"FALSE","8":"NA"},{"1":"carrots","2":"16.86534300","3":"H","4":"Dragon","5":"40","6":"2020-05-02","7":"FALSE","8":"NA"},{"1":"carrots","2":"16.86534300","3":"L","4":"Bolero","5":"50","6":"2020-05-25","7":"FALSE","8":"NA"},{"1":"carrots","2":"16.86534300","3":"L","4":"King Midas","5":"50","6":"2020-05-25","7":"FALSE","8":"NA"},{"1":"carrots","2":"16.86534300","3":"L","4":"Dragon","5":"50","6":"2020-05-25","7":"FALSE","8":"NA"},{"1":"chives","2":"0.01763696","3":"NA","4":"NA","5":"NA","6":"<NA>","7":"NA","8":"NA"},{"1":"cilantro","2":"0.11464024","3":"potD","4":"cilantro","5":"15","6":"2020-05-16","7":"FALSE","8":"NA"},{"1":"cilantro","2":"0.11464024","3":"E","4":"cilantro","5":"20","6":"2020-06-20","7":"FALSE","8":"NA"},{"1":"corn","2":"13.00946262","3":"A","4":"Dorinny Sweet","5":"20","6":"2020-05-25","7":"FALSE","8":"NA"},{"1":"corn","2":"13.00946262","3":"B","4":"Golden Bantam","5":"20","6":"2020-05-25","7":"FALSE","8":"NA"},{"1":"cucumbers","2":"43.60958822","3":"L","4":"pickling","5":"20","6":"2020-05-25","7":"FALSE","8":"NA"},{"1":"edamame","2":"6.09136506","3":"O","4":"edamame","5":"25","6":"2020-05-16","7":"FALSE","8":"NA"},{"1":"hot peppers","2":"1.46827692","3":"potB","4":"thai","5":"1","6":"2020-05-21","7":"TRUE","8":"NA"},{"1":"hot peppers","2":"1.46827692","3":"potC","4":"variety","5":"6","6":"2020-05-21","7":"TRUE","8":"NA"},{"1":"jalapeño","2":"9.87228836","3":"L","4":"giant","5":"4","6":"2020-05-21","7":"TRUE","8":"NA"},{"1":"kale","2":"5.94586014","3":"P","4":"Heirloom Lacinto","5":"30","6":"2020-05-02","7":"FALSE","8":"NA"},{"1":"kale","2":"5.94586014","3":"front","4":"Heirloom Lacinto","5":"30","6":"2020-06-20","7":"FALSE","8":"NA"},{"1":"kohlrabi","2":"0.42108242","3":"front","4":"Crispy Colors Duo","5":"10","6":"2020-05-20","7":"FALSE","8":"NA"},{"1":"lettuce","2":"11.59630120","3":"C","4":"Farmer's Market Blend","5":"60","6":"2020-05-02","7":"FALSE","8":"NA"},{"1":"lettuce","2":"11.59630120","3":"P","4":"Tatsoi","5":"25","6":"2020-05-02","7":"FALSE","8":"NA"},{"1":"lettuce","2":"11.59630120","3":"L","4":"Farmer's Market Blend","5":"60","6":"2020-05-16","7":"FALSE","8":"NA"},{"1":"lettuce","2":"11.59630120","3":"G","4":"Lettuce Mixture","5":"200","6":"2020-06-20","7":"FALSE","8":"NA"},{"1":"onions","2":"4.06531928","3":"H","4":"Long Keeping Rainbow","5":"40","6":"2020-04-26","7":"FALSE","8":"NA"},{"1":"onions","2":"4.06531928","3":"P","4":"Delicious Duo","5":"25","6":"2020-04-26","7":"FALSE","8":"NA"},{"1":"peas","2":"17.02628026","3":"A","4":"Super Sugar Snap","5":"22","6":"2020-04-19","7":"TRUE","8":"NA"},{"1":"peas","2":"17.02628026","3":"B","4":"Magnolia Blossom","5":"24","6":"2020-04-19","7":"TRUE","8":"NA"},{"1":"peppers","2":"9.34317956","3":"K","4":"green","5":"12","6":"2020-05-21","7":"TRUE","8":"NA"},{"1":"peppers","2":"9.34317956","3":"O","4":"green","5":"5","6":"2020-05-21","7":"TRUE","8":"NA"},{"1":"peppers","2":"9.34317956","3":"potA","4":"variety","5":"3","6":"2020-05-21","7":"TRUE","8":"NA"},{"1":"peppers","2":"9.34317956","3":"potA","4":"variety","5":"3","6":"2020-05-21","7":"TRUE","8":"NA"},{"1":"peppers","2":"9.34317956","3":"potD","4":"variety","5":"1","6":"2020-05-21","7":"TRUE","8":"NA"},{"1":"potatoes","2":"23.93555934","3":"D","4":"purple","5":"5","6":"2020-05-02","7":"FALSE","8":"NA"},{"1":"potatoes","2":"23.93555934","3":"D","4":"Russet","5":"8","6":"2020-05-02","7":"FALSE","8":"NA"},{"1":"potatoes","2":"23.93555934","3":"I","4":"yellow","5":"10","6":"2020-05-02","7":"FALSE","8":"NA"},{"1":"potatoes","2":"23.93555934","3":"I","4":"yellow","5":"8","6":"2020-05-22","7":"TRUE","8":"NA"},{"1":"potatoes","2":"23.93555934","3":"I","4":"red","5":"3","6":"2020-05-22","7":"FALSE","8":"NA"},{"1":"pumpkins","2":"154.66511610","3":"A","4":"Cinderalla's Carraige","5":"3","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"pumpkins","2":"154.66511610","3":"B","4":"Cinderella's Carraige","5":"3","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"pumpkins","2":"154.66511610","3":"B","4":"saved","5":"8","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"pumpkins","2":"154.66511610","3":"side","4":"Big Max","5":"6","6":"2020-05-24","7":"TRUE","8":"NA"},{"1":"pumpkins","2":"154.66511610","3":"side","4":"Cinderalla's Carraige","5":"6","6":"2020-05-24","7":"TRUE","8":"NA"},{"1":"pumpkins","2":"154.66511610","3":"K","4":"New England Sugar","5":"4","6":"2020-05-25","7":"TRUE","8":"NA"},{"1":"radish","2":"0.94578198","3":"C","4":"Garden Party Mix","5":"20","6":"2020-05-02","7":"FALSE","8":"NA"},{"1":"radish","2":"0.94578198","3":"G","4":"Garden Party Mix","5":"30","6":"2020-05-02","7":"FALSE","8":"NA"},{"1":"radish","2":"0.94578198","3":"H","4":"Garden Party Mix","5":"15","6":"2020-05-02","7":"FALSE","8":"NA"},{"1":"raspberries","2":"1.85849466","3":"NA","4":"NA","5":"NA","6":"<NA>","7":"NA","8":"NA"},{"1":"rutabaga","2":"29.74032380","3":"NA","4":"NA","5":"NA","6":"<NA>","7":"NA","8":"NA"},{"1":"spinach","2":"2.03486426","3":"H","4":"Catalina","5":"50","6":"2020-05-16","7":"FALSE","8":"NA"},{"1":"spinach","2":"2.03486426","3":"E","4":"Catalina","5":"100","6":"2020-06-20","7":"FALSE","8":"NA"},{"1":"squash","2":"99.02491654","3":"A","4":"Butternut (saved)","5":"8","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"squash","2":"99.02491654","3":"A","4":"Red Kuri","5":"4","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"squash","2":"99.02491654","3":"A","4":"Blue (saved)","5":"4","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"squash","2":"99.02491654","3":"A","4":"Waltham Butternut","5":"4","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"squash","2":"99.02491654","3":"B","4":"Red Kuri","5":"4","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"squash","2":"99.02491654","3":"B","4":"Blue (saved)","5":"8","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"squash","2":"99.02491654","3":"K","4":"Waltham Butternut","5":"6","6":"2020-05-25","7":"TRUE","8":"NA"},{"1":"squash","2":"99.02491654","3":"K","4":"delicata","5":"8","6":"2020-05-25","7":"TRUE","8":"NA"},{"1":"squash","2":"99.02491654","3":"side","4":"Red Kuri","5":"1","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"strawberries","2":"1.30513504","3":"F","4":"perennial","5":"NA","6":"<NA>","7":"NA","8":"NA"},{"1":"Swiss chard","2":"6.88282364","3":"M","4":"Neon Glow","5":"25","6":"2020-05-02","7":"FALSE","8":"NA"},{"1":"tomatoes","2":"348.83922722","3":"J","4":"Mortgage Lifter","5":"1","6":"2020-05-20","7":"TRUE","8":"died"},{"1":"tomatoes","2":"348.83922722","3":"J","4":"Old German","5":"1","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"tomatoes","2":"348.83922722","3":"J","4":"Better Boy","5":"1","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"tomatoes","2":"348.83922722","3":"J","4":"Amish Paste","5":"1","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"tomatoes","2":"348.83922722","3":"J","4":"Cherokee Purple","5":"1","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"tomatoes","2":"348.83922722","3":"J","4":"Brandywine","5":"1","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"tomatoes","2":"348.83922722","3":"J","4":"Bonny Best","5":"1","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"tomatoes","2":"348.83922722","3":"N","4":"Amish Paste","5":"2","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"tomatoes","2":"348.83922722","3":"N","4":"Big Beef","5":"1","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"tomatoes","2":"348.83922722","3":"N","4":"Mortgage Lifter","5":"1","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"tomatoes","2":"348.83922722","3":"N","4":"Black Krim","5":"1","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"tomatoes","2":"348.83922722","3":"N","4":"Jet Star","5":"1","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"tomatoes","2":"348.83922722","3":"N","4":"Better Boy","5":"1","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"tomatoes","2":"348.83922722","3":"O","4":"grape","5":"1","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"tomatoes","2":"348.83922722","3":"N","4":"volunteers","5":"1","6":"2020-06-03","7":"TRUE","8":"NA"},{"1":"tomatoes","2":"348.83922722","3":"J","4":"volunteers","5":"1","6":"2020-06-03","7":"TRUE","8":"NA"},{"1":"tomatoes","2":"348.83922722","3":"front","4":"volunteers","5":"5","6":"2020-06-03","7":"TRUE","8":"NA"},{"1":"tomatoes","2":"348.83922722","3":"O","4":"volunteers","5":"2","6":"2020-06-03","7":"TRUE","8":"NA"},{"1":"zucchini","2":"99.70834874","3":"D","4":"Romanesco","5":"3","6":"2020-05-21","7":"TRUE","8":"NA"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>

When I joined the datasets, all of the columns joined, not just the plot column. A way that I could fix this is to then filter out the additional columns. 

  3. I would like to understand how much money I "saved" by gardening, for each vegetable type. Describe how I could use the `garden_harvest` and `garden_spending` datasets, along with data from somewhere like [this](https://products.wholefoodsmarket.com/search?sort=relevance&store=10542) to answer this question. You can answer this in words, referencing various join functions. You don't need R code but could provide some if it's helpful.

You could do a left join with the garden_spending data on the left and the garden_harvest on the right. This would join the two together so you could see how much money you spent on the plants themselves, along with the other purchases that wouldn't show up if you did a left join with garden_harvest on the left because there wouldn't be a matching column for things like dirt. You could then use the data from the Whole Foods website to find the price per pound and add that as a column using left join with the combined garden_spending and garden_harvest on the left. You could then take the column with price and then use the column with total weight to find the total cost if you bought the same amount of produce from the store. Finally, you could then find the total saved by taking the sum of how much the produce would cost at a store and subtract the sum of money spent on the garden. 

  4. Subset the data to tomatoes. Reorder the tomato varieties from smallest to largest first harvest date. Create a barplot of total harvest in pounds for each variety, in the new order.CHALLENGE: add the date near the end of the bar. (This is probably not a super useful graph because it's difficult to read. This is more an exercise in using some of the functions you just learned.)


```r
garden_harvest %>% 
  filter(vegetable == "tomatoes") %>% 
  mutate(variety_date = fct_reorder(variety, date, min)) %>% 
  group_by(variety_date) %>% 
  mutate(group_variety = combine(variety)) %>% 
  mutate(total_weight_lbs = (sum(weight)*(0.00220462))) %>%
  ggplot(aes(y = total_weight_lbs, x = variety_date, fill = `variety`)) +
  geom_col() + 
  labs(x = "Variety" , y = "Total Weight (grams)", 
       title = "Which variety of tomato produced the most?") + 
  guides(fill="none") +
  theme(axis.text.x = (element_text(angle = 90)), panel.grid.major.x = element_blank())
```

```
## Warning: `combine()` was deprecated in dplyr 1.0.0.
## Please use `vctrs::vec_c()` instead.
## This warning is displayed once every 8 hours.
## Call `lifecycle::last_lifecycle_warnings()` to see where this warning was generated.
```

![](03_exercises--1-_files/figure-html/unnamed-chunk-3-1.png)<!-- -->

  5. In the `garden_harvest` data, create two new variables: one that makes the varieties lowercase and another that finds the length of the variety name. Arrange the data by vegetable and length of variety name (smallest to largest), with one row for each vegetable variety. HINT: use `str_to_lower()`, `str_length()`, and `distinct()`.
  

```r
garden_harvest %>% 
  mutate(variety_lower = str_to_lower(variety)) %>% 
  mutate(name_length = str_length(variety)) %>% 
  arrange(name_length, vegetable) %>% 
  distinct(variety_lower, vegetable, name_length)
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["vegetable"],"name":[1],"type":["chr"],"align":["left"]},{"label":["variety_lower"],"name":[2],"type":["chr"],"align":["left"]},{"label":["name_length"],"name":[3],"type":["int"],"align":["right"]}],"data":[{"1":"potatoes","2":"red","3":"3"},{"1":"hot peppers","2":"thai","3":"4"},{"1":"jalapeño","2":"giant","3":"5"},{"1":"peppers","2":"green","3":"5"},{"1":"pumpkins","2":"saved","3":"5"},{"1":"tomatoes","2":"grape","3":"5"},{"1":"beets","2":"leaves","3":"6"},{"1":"carrots","2":"dragon","3":"6"},{"1":"carrots","2":"bolero","3":"6"},{"1":"carrots","2":"greens","3":"6"},{"1":"lettuce","2":"reseed","3":"6"},{"1":"lettuce","2":"tatsoi","3":"6"},{"1":"potatoes","2":"purple","3":"6"},{"1":"potatoes","2":"yellow","3":"6"},{"1":"potatoes","2":"russet","3":"6"},{"1":"apple","2":"unknown","3":"7"},{"1":"broccoli","2":"yod fah","3":"7"},{"1":"edamame","2":"edamame","3":"7"},{"1":"hot peppers","2":"variety","3":"7"},{"1":"peppers","2":"variety","3":"7"},{"1":"cilantro","2":"cilantro","3":"8"},{"1":"cucumbers","2":"pickling","3":"8"},{"1":"spinach","2":"catalina","3":"8"},{"1":"squash","2":"delicata","3":"8"},{"1":"squash","2":"red kuri","3":"8"},{"1":"tomatoes","2":"big beef","3":"8"},{"1":"tomatoes","2":"jet star","3":"8"},{"1":"asparagus","2":"asparagus","3":"9"},{"1":"chives","2":"perrenial","3":"9"},{"1":"raspberries","2":"perrenial","3":"9"},{"1":"strawberries","2":"perrenial","3":"9"},{"1":"Swiss chard","2":"neon glow","3":"9"},{"1":"zucchini","2":"romanesco","3":"9"},{"1":"carrots","2":"king midas","3":"10"},{"1":"tomatoes","2":"bonny best","3":"10"},{"1":"tomatoes","2":"better boy","3":"10"},{"1":"tomatoes","2":"old german","3":"10"},{"1":"tomatoes","2":"brandywine","3":"10"},{"1":"tomatoes","2":"black krim","3":"10"},{"1":"tomatoes","2":"volunteers","3":"10"},{"1":"tomatoes","2":"amish paste","3":"11"},{"1":"beets","2":"sweet merlin","3":"12"},{"1":"squash","2":"blue (saved)","3":"12"},{"1":"basil","2":"isle of naxos","3":"13"},{"1":"corn","2":"dorinny sweet","3":"13"},{"1":"corn","2":"golden bantam","3":"13"},{"1":"onions","2":"delicious duo","3":"13"},{"1":"beets","2":"gourmet golden","3":"14"},{"1":"lettuce","2":"mustard greens","3":"14"},{"1":"lettuce","2":"lettuce mixture","3":"15"},{"1":"tomatoes","2":"cherokee purple","3":"15"},{"1":"tomatoes","2":"mortgage lifter","3":"15"},{"1":"kale","2":"heirloom lacinto","3":"16"},{"1":"peas","2":"magnolia blossom","3":"16"},{"1":"peas","2":"super sugar snap","3":"16"},{"1":"radish","2":"garden party mix","3":"16"},{"1":"rutabaga","2":"improved helenor","3":"16"},{"1":"beans","2":"bush bush slender","3":"17"},{"1":"broccoli","2":"main crop bravado","3":"17"},{"1":"kohlrabi","2":"crispy colors duo","3":"17"},{"1":"pumpkins","2":"new england sugar","3":"17"},{"1":"squash","2":"waltham butternut","3":"17"},{"1":"beans","2":"chinese red noodle","3":"18"},{"1":"beans","2":"classic slenderette","3":"19"},{"1":"onions","2":"long keeping rainbow","3":"20"},{"1":"lettuce","2":"farmer's market blend","3":"21"},{"1":"pumpkins","2":"cinderella's carraige","3":"21"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>

  6. In the `garden_harvest` data, find all distinct vegetable varieties that have "er" or "ar" in their name. HINT: `str_detect()` with an "or" statement (use the | for "or") and `distinct()`.


```r
garden_harvest %>% 
  filter(str_detect(variety, "er|ar")) %>% 
  distinct(vegetable, variety)
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["vegetable"],"name":[1],"type":["chr"],"align":["left"]},{"label":["variety"],"name":[2],"type":["chr"],"align":["left"]}],"data":[{"1":"radish","2":"Garden Party Mix"},{"1":"lettuce","2":"Farmer's Market Blend"},{"1":"peas","2":"Super Sugar Snap"},{"1":"chives","2":"perrenial"},{"1":"strawberries","2":"perrenial"},{"1":"asparagus","2":"asparagus"},{"1":"lettuce","2":"mustard greens"},{"1":"raspberries","2":"perrenial"},{"1":"beans","2":"Bush Bush Slender"},{"1":"beets","2":"Sweet Merlin"},{"1":"hot peppers","2":"variety"},{"1":"tomatoes","2":"Cherokee Purple"},{"1":"tomatoes","2":"Better Boy"},{"1":"peppers","2":"variety"},{"1":"tomatoes","2":"Mortgage Lifter"},{"1":"tomatoes","2":"Old German"},{"1":"tomatoes","2":"Jet Star"},{"1":"carrots","2":"Bolero"},{"1":"tomatoes","2":"volunteers"},{"1":"beans","2":"Classic Slenderette"},{"1":"pumpkins","2":"Cinderella's Carraige"},{"1":"squash","2":"Waltham Butternut"},{"1":"pumpkins","2":"New England Sugar"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>


## Bicycle-Use Patterns

In this activity, you'll examine some factors that may influence the use of bicycles in a bike-renting program.  The data come from Washington, DC and cover the last quarter of 2014.

<center>

![A typical Capital Bikeshare station. This one is at Florida and California, next to Pleasant Pops.](https://www.macalester.edu/~dshuman1/data/112/bike_station.jpg){width="30%"}


![One of the vans used to redistribute bicycles to different stations.](https://www.macalester.edu/~dshuman1/data/112/bike_van.jpg){width="30%"}

</center>

Two data tables are available:

- `Trips` contains records of individual rentals
- `Stations` gives the locations of the bike rental stations

Here is the code to read in the data. We do this a little differently than usual, which is why it is included here rather than at the top of this file. To avoid repeatedly re-reading the files, start the data import chunk with `{r cache = TRUE}` rather than the usual `{r}`.


```r
data_site <- 
  "https://www.macalester.edu/~dshuman1/data/112/2014-Q4-Trips-History-Data.rds" 
Trips <- readRDS(gzcon(url(data_site)))
Stations<-read_csv("http://www.macalester.edu/~dshuman1/data/112/DC-Stations.csv")
```

```
## Rows: 347 Columns: 5
```

```
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr (1): name
## dbl (4): lat, long, nbBikes, nbEmptyDocks
```

```
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

**NOTE:** The `Trips` data table is a random subset of 10,000 trips from the full quarterly data. Start with this small data table to develop your analysis commands. **When you have this working well, you should access the full data set of more than 600,000 events by removing `-Small` from the name of the `data_site`.**

### Temporal patterns

It's natural to expect that bikes are rented more at some times of day, some days of the week, some months of the year than others. The variable `sdate` gives the time (including the date) that the rental started. Make the following plots and interpret them:

  7. A density plot, which is a smoothed out histogram, of the events versus `sdate`. Use `geom_density()`.
  

```r
Trips %>% 
  ggplot() +
  geom_density(aes(x = sdate)) + 
  labs(x = "Date" , y = "Number of Rentals", 
       title = "What time of year are bikes rented the most?")
```

![](03_exercises--1-_files/figure-html/unnamed-chunk-7-1.png)<!-- -->

This plot shows that bike rentals increase in October and then decrease after November and into the winter months. 

  8. A density plot of the events versus time of day.  You can use `mutate()` with `lubridate`'s  `hour()` and `minute()` functions to extract the hour of the day and minute within the hour from `sdate`. Hint: A minute is 1/60 of an hour, so create a variable where 3:30 is 3.5 and 3:45 is 3.75.
  

```r
Trips %>% 
  mutate(hour = hour(sdate), 
         minute = minute(sdate), 
         time = (hour) + ((minute)/60)) %>% 
  ggplot + 
  geom_density(aes(x = time)) + 
  labs(x = "Time of Day (Military Time)" , y = "Number of Rentals", 
       title = "What time of day are bikes rented the most?") +
  scale_x_continuous(limits = c(0, 24))
```

![](03_exercises--1-_files/figure-html/unnamed-chunk-8-1.png)<!-- -->

This density plot shows that the most popular times to rent a bike are around 8:30 and 18:00 (6:00 pm). 

  9. A bar graph of the events versus day of the week. Put day on the y-axis.
  

```r
Trips %>% 
  mutate(day = wday(sdate, label = TRUE)) %>% 
  ggplot() +
  geom_bar(aes(y = day)) + 
  labs(x = "Number of Rentals" , y = "Day of the Week", 
       title = "What days of the week were bikes rented on the most?")
```

![](03_exercises--1-_files/figure-html/unnamed-chunk-9-1.png)<!-- -->
 This plot shows that the bike rentals were fairly spread out across the days of the week, with a slight increase during weekdays. 
 
  10. Facet your graph from exercise 8. by day of the week. Is there a pattern?
  

```r
Trips %>% 
  mutate(hour = hour(sdate), 
         minute = minute(sdate), 
         time = (hour) + ((minute)/60)) %>% 
  mutate(day = wday(sdate, label = TRUE)) %>%
  ggplot + 
  geom_density(aes(x = time)) + 
  labs(x = "Time of Day (Military Time)" , y = "Density", 
       title = "What time of day are bikes rented the most for each day of the week?") +
  facet_wrap(~ day, scales = "free")
```

![](03_exercises--1-_files/figure-html/unnamed-chunk-10-1.png)<!-- -->
There definitely is a pattern with weekday rentals following the original pattern of pears around 8:30 and 18:00 (6:00 pm), but the weekend just have peaks around 14:00 (2:00 pm). 

The variable `client` describes whether the renter is a regular user (level `Registered`) or has not joined the bike-rental organization (`Causal`). The next set of exercises investigate whether these two different categories of users show different rental behavior and how `client` interacts with the patterns you found in the previous exercises. 

  11. Change the graph from exercise 10 to set the `fill` aesthetic for `geom_density()` to the `client` variable. You should also set `alpha = .5` for transparency and `color=NA` to suppress the outline of the density function.
  

```r
Trips %>% 
  mutate(hour = hour(sdate), 
         minute = minute(sdate), 
         time = (hour) + ((minute)/60)) %>% 
  mutate(day = wday(sdate, label = TRUE)) %>%
  ggplot(aes(x = time))+ 
  geom_density(aes(fill = client), color = NA, alpha = 0.5) + 
  labs(x = "Time of Day (Military Time)" , y = "Density", 
       title = "What time of day are bikes rented the most by clients for each day of the week?") +
  facet_wrap(~ day, scales = "free")
```

![](03_exercises--1-_files/figure-html/unnamed-chunk-11-1.png)<!-- -->
This shows that on weekdays, casual users were more likely to rent bikes around 15:00 (3:00 pm) and registered users had two different peaks, one around 8:00 and another around 17:00 (5:00 pm). On the weekends, the usage was much more similar. 

  12. Change the previous graph by adding the argument `position = position_stack()` to `geom_density()`. In your opinion, is this better or worse in terms of telling a story? What are the advantages/disadvantages of each?
  

```r
Trips %>% 
  mutate(hour = hour(sdate), 
         minute = minute(sdate), 
         time = (hour) + ((minute)/60)) %>% 
  mutate(day = wday(sdate, label = TRUE)) %>%
  ggplot(aes(x = time))+ 
  geom_density(aes(fill = client), color = NA, alpha = 0.5, position = position_stack()) + 
  labs(x = "Time of Day (Military Time)" , y = "Density", 
       title = "What time of day are bikes rented the most by clients for each day of the week?") +
  facet_wrap(~ day, scales = "free")
```

![](03_exercises--1-_files/figure-html/unnamed-chunk-12-1.png)<!-- -->

I think this graph tells another part of the story because it seems that they have changed it so that instead of it being a simple density function, the relation between the relative numbers of registered versus casual bike users are shown more clearly. I think this graph is better at telling the story because it has the advantage of both being able to compare the patterns of usage, along with the number of bikers in each category. A disadvantage is that now the registered owns are much lower, which may make comparing casual and registered users a bit more difficult. 
  
13. In this graph, go back to using the regular density plot (without `position = position_stack()`). Add a new variable to the dataset called `weekend` which will be "weekend" if the day is Saturday or Sunday and  "weekday" otherwise (HINT: use the `ifelse()` function and the `wday()` function from `lubridate`). Then, update the graph from the previous problem by faceting on the new `weekend` variable. 
  

```r
Trips %>% 
  mutate(hour = hour(sdate), 
         minute = minute(sdate), 
         time = (hour) + ((minute)/60)) %>% 
  mutate(day = wday(sdate, label = TRUE)) %>%
  mutate(weekend =  ifelse(day %in% c("Sat", "Sun"), "weekend", "weekday")) %>%
  ggplot(aes(x = time))+ 
  geom_density(aes(fill = client), color = NA, alpha = 0.5) + 
  labs(x = "Time of Day (Military Time)" , y = " Density", 
       title = "What time are bikes rented by differnt clients on the weekdays vs the weekend?") +
  facet_wrap(~ weekend, scales = "free")
```

![](03_exercises--1-_files/figure-html/unnamed-chunk-13-1.png)<!-- -->
  
  14. Change the graph from the previous problem to facet on `client` and fill with `weekday`. What information does this graph tell you that the previous didn't? Is one graph better than the other?
  

```r
Trips %>% 
  mutate(hour = hour(sdate), 
         minute = minute(sdate), 
         time = (hour) + ((minute)/60)) %>% 
  mutate(day = wday(sdate, label = TRUE)) %>%
  mutate(weekend =  ifelse(day %in% c("Sat", "Sun"), "weekend", "weekday")) %>%
  ggplot(aes(x = time))+ 
  geom_density(aes(fill = weekend), color = NA, alpha = 0.5) + 
  labs(x = "Time of Day (Military Time)" , y = "Number of Rentals", 
       title = "What time are bikes rented by differnt clients on the weekdays vs the weekend?") +
  facet_wrap(~ client, scales = "free")
```

![](03_exercises--1-_files/figure-html/unnamed-chunk-14-1.png)<!-- -->
  
### Spatial patterns

  15. Use the latitude and longitude variables in `Stations` to make a visualization of the total number of departures from each station in the `Trips` data. Use either color or size to show the variation in number of departures. We will improve this plot next week when we learn about maps!
  

```r
Trips %>% 
  mutate(name = estation) %>% 
  left_join(Stations, 
            by = "name") %>% 
  group_by(lat, long) %>% 
  summarize(total_departure = n()) %>% 
  ggplot(aes(x = long, y = lat, color = total_departure)) +
  geom_point() + 
  guides(fill="none") + 
  labs(x = "Longuitude" , y = "Latitude", 
       title = "Where are bikes rented the most?", color = "Number of Departures") 
```

```
## `summarise()` has grouped output by 'lat'. You can override using the `.groups` argument.
```

```
## Warning: Removed 1 rows containing missing values (geom_point).
```

![](03_exercises--1-_files/figure-html/unnamed-chunk-15-1.png)<!-- -->
  
  16. Only 14.4% of the trips in our data are carried out by casual users. Create a plot that shows which area(s) have stations with a much higher percentage of departures by casual users. What patterns do you notice? (Again, we'll improve this next week when we learn about maps).
  

```r
Trips %>% 
  mutate(name = estation) %>% 
  left_join(Stations, 
            by = "name") %>% 
  group_by(lat, long) %>% 
  mutate(casual = (sum(client == "Casual"))) %>% 
  mutate(registered = (sum(client == "Registered"))) %>% 
  mutate(percentage_casual = ((casual/(casual + registered)*100))) %>% 
  ggplot(aes(x = long, y = lat, color = percentage_casual)) +
  geom_point() + 
  guides(fill="none") +
  scale_color_viridis_c() +
  labs(x = "Longuitude" , y = "Latitude", 
       title = "Where do casual users go to rent bikes?", color = "Percentage of Casual Users")
```

```
## Warning: Removed 10125 rows containing missing values (geom_point).
```

![](03_exercises--1-_files/figure-html/unnamed-chunk-16-1.png)<!-- -->
  
**DID YOU REMEMBER TO GO BACK AND CHANGE THIS SET OF EXERCISES TO THE LARGER DATASET? IF NOT, DO THAT NOW.**

## Dogs!

In this section, we'll use the data from 2022-02-01 Tidy Tuesday. If you didn't use that data or need a little refresher on it, see the [website](https://github.com/rfordatascience/tidytuesday/blob/master/data/2022/2022-02-01/readme.md).

  17. The final product of this exercise will be a graph that has breed on the y-axis and the sum of the numeric ratings in the `breed_traits` dataset on the x-axis, with a dot for each rating. First, create a new dataset called `breed_traits_total` that has two variables -- `Breed` and `total_rating`. The `total_rating` variable is the sum of the numeric ratings in the `breed_traits` dataset (we'll use this dataset again in the next problem). Then, create the graph just described. Omit Breeds with a `total_rating` of 0 and order the Breeds from highest to lowest ranked. You may want to adjust the `fig.height` and `fig.width` arguments inside the code chunk options (eg. `{r, fig.height=8, fig.width=4}`) so you can see things more clearly - check this after you knit the file to assure it looks like what you expected.


```r
breed_traits_total <- breed_traits %>% 
  select(- 'Coat Type', -'Coat Length') %>%
  pivot_longer(!Breed,
               names_to= "traits",
               values_to =  "rating") %>% 
  group_by(Breed) %>% 
  mutate(total_rating = sum(rating)) %>% 
  select(- 'traits', -'rating') %>% 
  distinct() %>% 
  filter(total_rating != 0) %>% 
  arrange(desc(total_rating)) 

breed_traits_total %>% 
  ggplot(aes(y = fct_reorder(Breed, total_rating), x = total_rating, fill = `Breed`)) +
  geom_point() + 
  labs(x = "Total Rating" , y = "Breed", 
       title = "Breeds of Dogs and their Total Rating") + 
  guides(fill="none") 
```

![](03_exercises--1-_files/figure-html/unnamed-chunk-17-1.png)<!-- -->

  18. The final product of this exercise will be a graph with the top-20 dogs in total ratings (from previous problem) on the y-axis, year on the x-axis, and points colored by each breed's ranking for that year (from the `breed_rank_all` dataset). The points within each breed will be connected by a line, and the breeds should be arranged from the highest median rank to lowest median rank ("highest" is actually the smallest numer, eg. 1 = best). After you're finished, think of AT LEAST one thing you could you do to make this graph better. HINTS: 1. Start with the `breed_rank_all` dataset and pivot it so year is a variable. 2. Use the `separate()` function to get year alone, and there's an extra argument in that function that can make it numeric. 3. For both datasets used, you'll need to `str_squish()` Breed before joining. 
  

```r
breed_rank_all %>% 
  pivot_longer(cols = `2013 Rank` : `2020 Rank`,
               names_to = "year",
               values_to = "rank") %>% 
  separate(year, into = c("year"), extra = "drop", convert = TRUE) %>% 
  mutate(new_breed = str_squish(Breed)) %>% 
  inner_join(breed_traits_total %>% 
               slice_max(n = 20, order_by = total_rating) %>% 
               mutate(new_breed = str_squish(Breed)),
             by = "new_breed") %>% 
  
  ggplot(aes(x = year, y = fct_rev(fct_reorder(new_breed, rank, median)))) +
  geom_line() +
  geom_point(aes(color = rank)) +
  labs(x = "Year" , y = "Breed", 
       title = "Ranking of Different Dog Breeds from 2013 to 2020")
```

![](03_exercises--1-_files/figure-html/unnamed-chunk-18-1.png)<!-- -->

Something that would make this plot better would be if the colors were more drastically different, which would make the visualization more clear. 

* I know this isn't filtered to the top 20, I tried working on it with another person and we did the exact same things but for some reason it just won't work for me.*

  19. Create your own! Requirements: use a `join` or `pivot` function (or both, if you'd like), a `str_XXX()` function, and a `fct_XXX()` function to create a graph using any of the dog datasets. One suggestion is to try to improve the graph you created for the Tidy Tuesday assignment. If you want an extra challenge, find a way to use the dog images in the `breed_rank_all` file - check out the `ggimage` library and [this resource](https://wilkelab.org/ggtext/) for putting images as labels.
  

```r
fur <- breed_traits %>% 
  select(Breed, "Coat Length") %>% 
  mutate(new_breed = str_squish(Breed)) %>% 
  select(new_breed, "Coat Length")

breed_rank_2020 <- breed_rank_all %>% 
  select(Breed, "2020 Rank") %>% 
  mutate(new_breed = str_squish(Breed)) %>% 
  select(new_breed, "2020 Rank") %>% 
  arrange ("2020 Rank") %>% 
  head(10)

fur %>% 
  right_join(breed_rank_2020, 
            by = "new_breed") %>% 
  ggplot(aes(y = fct_reorder(new_breed, `2020 Rank`), x = `2020 Rank`, color = `Coat Length`)) +
  geom_point() + 
  labs(x = "Rank" , y = "Breed", 
       title = "What coat length do the top 10 most popular dogs in 2020 have?") 
```

![](03_exercises--1-_files/figure-html/unnamed-chunk-19-1.png)<!-- -->
  
## GitHub link

  20. Below, provide a link to your GitHub page with this set of Weekly Exercises. Specifically, if the name of the file is 03_exercises.Rmd, provide a link to the 03_exercises.md file, which is the one that will be most readable on GitHub.

## Challenge problem! 

This problem uses the data from the Tidy Tuesday competition this week, `kids`. If you need to refresh your memory on the data, read about it [here](https://github.com/rfordatascience/tidytuesday/blob/master/data/2020/2020-09-15/readme.md). 

  21. In this exercise, you are going to try to replicate the graph below, created by Georgios Karamanis. I'm sure you can find the exact code on GitHub somewhere, but **DON'T DO THAT!** You will only be graded for putting an effort into this problem. So, give it a try and see how far you can get without doing too much googling. HINT: use `facet_geo()`. The graphic won't load below since it came from a location on my computer. So, you'll have to reference the original html on the moodle page to see it.
  

**DID YOU REMEMBER TO UNCOMMENT THE OPTIONS AT THE TOP?**
