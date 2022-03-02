---
title: 'Weekly Exercises #3'
author: "Brian Anderson"
output: 
  html_document:
    keep_md: TRUE
    toc: TRUE
    toc_float: TRUE
    df_print: paged
    code_download: true
---





```r
library(tidyverse)     # for graphing and data cleaning
library(gardenR)       # for Lisa's garden data
library(lubridate)     # for date manipulation
library(ggthemes)      # for even more plotting themes
library(geofacet)      # for special faceting with US map layout
theme_set(theme_minimal())       # My favorite ggplot() theme :)
```


```r
# Lisa's garden data
data("garden_harvest")

# Seeds/plants (and other garden supply) costs
data("garden_spending")

# Planting dates and locations
data("garden_planting")

# Tidy Tuesday dog breed data
breed_traits <- readr::read_csv('https://raw.githubusercontent.com/rfordatascience/tidytuesday/master/data/2022/2022-02-01/breed_traits.csv')
trait_description <- readr::read_csv('https://raw.githubusercontent.com/rfordatascience/tidytuesday/master/data/2022/2022-02-01/trait_description.csv')
breed_rank_all <- readr::read_csv('https://raw.githubusercontent.com/rfordatascience/tidytuesday/master/data/2022/2022-02-01/breed_rank.csv')

# Tidy Tuesday data for challenge problem
kids <- readr::read_csv('https://raw.githubusercontent.com/rfordatascience/tidytuesday/master/data/2020/2020-09-15/kids.csv')
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
  mutate(day_of_week = wday(date, label = TRUE)) %>% 
  group_by(vegetable, day_of_week) %>% 
  summarize(total_weight_lbs = sum(weight*0.00220462)) %>% 
  pivot_wider(id_cols = vegetable:total_weight_lbs,
              names_from = day_of_week,
              values_from = total_weight_lbs)
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["vegetable"],"name":[1],"type":["chr"],"align":["left"]},{"label":["Sat"],"name":[2],"type":["dbl"],"align":["right"]},{"label":["Mon"],"name":[3],"type":["dbl"],"align":["right"]},{"label":["Tue"],"name":[4],"type":["dbl"],"align":["right"]},{"label":["Thu"],"name":[5],"type":["dbl"],"align":["right"]},{"label":["Fri"],"name":[6],"type":["dbl"],"align":["right"]},{"label":["Sun"],"name":[7],"type":["dbl"],"align":["right"]},{"label":["Wed"],"name":[8],"type":["dbl"],"align":["right"]}],"data":[{"1":"apple","2":"0.34392072","3":"NA","4":"NA","5":"NA","6":"NA","7":"NA","8":"NA"},{"1":"asparagus","2":"0.04409240","3":"NA","4":"NA","5":"NA","6":"NA","7":"NA","8":"NA"},{"1":"basil","2":"0.41005932","3":"0.0661386","4":"0.11023100","5":"0.02645544","6":"0.46737944","7":"NA","8":"NA"},{"1":"beans","2":"4.70906832","3":"6.5080382","4":"4.38719380","5":"3.39291018","6":"1.52559704","7":"1.91361016","8":"4.08295624"},{"1":"beets","2":"0.37919464","3":"0.6724091","4":"0.15873264","5":"11.89172028","6":"0.02425082","7":"0.32187452","8":"0.18298346"},{"1":"broccoli","2":"NA","3":"0.8201186","4":"NA","5":"NA","6":"0.16534650","7":"1.25883802","8":"0.70768302"},{"1":"carrots","2":"2.33028334","3":"0.8708249","4":"0.35273920","5":"2.67420406","6":"2.13848140","7":"2.93655384","8":"5.56225626"},{"1":"chives","2":"NA","3":"NA","4":"NA","5":"NA","6":"NA","7":"NA","8":"0.01763696"},{"1":"cilantro","2":"0.03747854","3":"NA","4":"0.00440924","5":"NA","6":"0.07275246","7":"NA","8":"NA"},{"1":"corn","2":"1.31615814","3":"0.7583893","4":"0.72752460","5":"NA","6":"3.44802568","7":"1.45725382","8":"5.30211110"},{"1":"cucumbers","2":"9.64080326","3":"4.7752069","4":"10.04645334","5":"3.30693000","6":"7.42956940","7":"3.10410496","8":"5.30652034"},{"1":"edamame","2":"4.68922674","3":"NA","4":"1.40213832","5":"NA","6":"NA","7":"NA","8":"NA"},{"1":"hot peppers","2":"NA","3":"1.2588380","4":"0.14109568","5":"NA","6":"NA","7":"NA","8":"0.06834322"},{"1":"jalapeño","2":"1.50796008","3":"5.5534378","4":"0.54895038","5":"0.22487124","6":"1.29411194","7":"0.26234978","8":"0.48060716"},{"1":"kale","2":"1.49032312","3":"2.0679336","4":"0.28219136","5":"0.27998674","6":"0.38139926","7":"0.82673250","8":"0.61729360"},{"1":"kohlrabi","2":"NA","3":"NA","4":"NA","5":"0.42108242","6":"NA","7":"NA","8":"NA"},{"1":"lettuce","2":"1.31615814","3":"2.4581513","4":"0.91712192","5":"2.45153744","6":"1.80117454","7":"1.46607230","8":"1.18608556"},{"1":"onions","2":"1.91361016","3":"0.5092672","4":"0.70768302","5":"0.60186126","6":"0.07275246","7":"0.26014516","8":"NA"},{"1":"peas","2":"2.85277828","3":"4.6341112","4":"2.06793356","5":"3.39731942","6":"0.93696350","7":"2.05691046","8":"1.08026380"},{"1":"peppers","2":"1.38229674","3":"2.5264945","4":"1.44402610","5":"0.70988764","6":"0.33510224","7":"0.50265336","8":"2.44271896"},{"1":"potatoes","2":"2.80207202","3":"0.9700328","4":"NA","5":"11.85203712","6":"3.74124014","7":"NA","8":"4.57017726"},{"1":"pumpkins","2":"92.68883866","3":"30.1195184","4":"31.85675900","5":"NA","6":"NA","7":"NA","8":"NA"},{"1":"radish","2":"0.23148510","3":"0.1962112","4":"0.09479866","5":"0.14770954","6":"0.19400656","7":"0.08157094","8":"NA"},{"1":"raspberries","2":"0.53351804","3":"0.1300726","4":"0.33510224","5":"0.28880522","6":"0.57099658","7":"NA","8":"NA"},{"1":"rutabaga","2":"6.89825598","3":"NA","4":"NA","5":"NA","6":"3.57809826","7":"19.26396956","8":"NA"},{"1":"spinach","2":"0.26014516","3":"0.1477095","4":"0.49603950","5":"0.23368972","6":"0.19621118","7":"0.48722102","8":"0.21384814"},{"1":"squash","2":"56.22221924","3":"24.3345956","4":"18.46810174","5":"NA","6":"NA","7":"NA","8":"NA"},{"1":"strawberries","2":"0.16975574","3":"0.4784025","4":"NA","5":"0.08818480","6":"0.48722102","7":"0.08157094","8":"NA"},{"1":"Swiss chard","2":"0.73413846","3":"1.0736499","4":"0.07054784","5":"2.23107544","6":"0.61729360","7":"1.24781492","8":"0.90830344"},{"1":"tomatoes","2":"35.12621046","3":"11.4926841","4":"48.75076206","5":"34.51773534","6":"85.07628580","7":"75.60964752","8":"58.26590198"},{"1":"zucchini","2":"3.41495638","3":"12.1959578","4":"16.46851140","5":"34.63017096","6":"18.72163304","7":"12.23564100","8":"2.04147812"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>

  2. Summarize the `garden_harvest` data to find the total harvest in pound for each vegetable variety and then try adding the plot from the `garden_planting` table. This will not turn out perfectly. What is the problem? How might you fix it?


```r
garden_harvest %>% 
  group_by(vegetable, variety) %>% 
  summarize(total_weight_lbs = sum(weight*0.00220462)) %>% 
  left_join(garden_planting,
            by = "variety")
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["vegetable.x"],"name":[1],"type":["chr"],"align":["left"]},{"label":["variety"],"name":[2],"type":["chr"],"align":["left"]},{"label":["total_weight_lbs"],"name":[3],"type":["dbl"],"align":["right"]},{"label":["plot"],"name":[4],"type":["chr"],"align":["left"]},{"label":["vegetable.y"],"name":[5],"type":["chr"],"align":["left"]},{"label":["number_seeds_planted"],"name":[6],"type":["dbl"],"align":["right"]},{"label":["date"],"name":[7],"type":["date"],"align":["right"]},{"label":["number_seeds_exact"],"name":[8],"type":["lgl"],"align":["right"]},{"label":["notes"],"name":[9],"type":["chr"],"align":["left"]}],"data":[{"1":"apple","2":"unknown","3":"0.34392072","4":"NA","5":"NA","6":"NA","7":"<NA>","8":"NA","9":"NA"},{"1":"asparagus","2":"asparagus","3":"0.04409240","4":"NA","5":"NA","6":"NA","7":"<NA>","8":"NA","9":"NA"},{"1":"basil","2":"Isle of Naxos","3":"1.08026380","4":"potB","5":"basil","6":"40","7":"2020-05-16","8":"FALSE","9":"NA"},{"1":"beans","2":"Bush Bush Slender","3":"22.12997556","4":"M","5":"beans","6":"30","7":"2020-05-16","8":"FALSE","9":"NA"},{"1":"beans","2":"Bush Bush Slender","3":"22.12997556","4":"D","5":"beans","6":"10","7":"2020-05-21","8":"TRUE","9":"NA"},{"1":"beans","2":"Chinese Red Noodle","3":"0.78484472","4":"K","5":"beans","6":"5","7":"2020-05-25","8":"TRUE","9":"NA"},{"1":"beans","2":"Chinese Red Noodle","3":"0.78484472","4":"L","5":"beans","6":"5","7":"2020-05-25","8":"TRUE","9":"NA"},{"1":"beans","2":"Classic Slenderette","3":"3.60455370","4":"E","5":"beans","6":"29","7":"2020-06-20","8":"TRUE","9":"NA"},{"1":"beets","2":"Gourmet Golden","3":"7.02171470","4":"H","5":"beets","6":"40","7":"2020-05-02","8":"FALSE","9":"NA"},{"1":"beets","2":"leaves","3":"0.22266662","4":"NA","5":"NA","6":"NA","7":"<NA>","8":"NA","9":"NA"},{"1":"beets","2":"Sweet Merlin","3":"6.38678414","4":"H","5":"beets","6":"40","7":"2020-05-02","8":"FALSE","9":"NA"},{"1":"broccoli","2":"Main Crop Bravado","3":"2.13186754","4":"D","5":"broccoli","6":"7","7":"2020-05-22","8":"TRUE","9":"NA"},{"1":"broccoli","2":"Main Crop Bravado","3":"2.13186754","4":"I","5":"broccoli","6":"7","7":"2020-05-22","8":"TRUE","9":"NA"},{"1":"broccoli","2":"Yod Fah","3":"0.82011864","4":"P","5":"broccoli","6":"25","7":"2020-05-16","8":"FALSE","9":"NA"},{"1":"carrots","2":"Bolero","3":"8.29157582","4":"H","5":"carrots","6":"50","7":"2020-05-02","8":"FALSE","9":"NA"},{"1":"carrots","2":"Bolero","3":"8.29157582","4":"L","5":"carrots","6":"50","7":"2020-05-25","8":"FALSE","9":"NA"},{"1":"carrots","2":"Dragon","3":"4.10500244","4":"H","5":"carrots","6":"40","7":"2020-05-02","8":"FALSE","9":"NA"},{"1":"carrots","2":"Dragon","3":"4.10500244","4":"L","5":"carrots","6":"50","7":"2020-05-25","8":"FALSE","9":"NA"},{"1":"carrots","2":"greens","3":"0.37258078","4":"NA","5":"NA","6":"NA","7":"<NA>","8":"NA","9":"NA"},{"1":"carrots","2":"King Midas","3":"4.09618396","4":"H","5":"carrots","6":"50","7":"2020-05-02","8":"FALSE","9":"NA"},{"1":"carrots","2":"King Midas","3":"4.09618396","4":"L","5":"carrots","6":"50","7":"2020-05-25","8":"FALSE","9":"NA"},{"1":"chives","2":"perrenial","3":"0.01763696","4":"NA","5":"NA","6":"NA","7":"<NA>","8":"NA","9":"NA"},{"1":"cilantro","2":"cilantro","3":"0.11464024","4":"potD","5":"cilantro","6":"15","7":"2020-05-16","8":"FALSE","9":"NA"},{"1":"cilantro","2":"cilantro","3":"0.11464024","4":"E","5":"cilantro","6":"20","7":"2020-06-20","8":"FALSE","9":"NA"},{"1":"corn","2":"Dorinny Sweet","3":"11.40670388","4":"A","5":"corn","6":"20","7":"2020-05-25","8":"FALSE","9":"NA"},{"1":"corn","2":"Golden Bantam","3":"1.60275874","4":"B","5":"corn","6":"20","7":"2020-05-25","8":"FALSE","9":"NA"},{"1":"cucumbers","2":"pickling","3":"43.60958822","4":"L","5":"cucumbers","6":"20","7":"2020-05-25","8":"FALSE","9":"NA"},{"1":"edamame","2":"edamame","3":"6.09136506","4":"O","5":"edamame","6":"25","7":"2020-05-16","8":"FALSE","9":"NA"},{"1":"hot peppers","2":"thai","3":"0.14770954","4":"potB","5":"hot peppers","6":"1","7":"2020-05-21","8":"TRUE","9":"NA"},{"1":"hot peppers","2":"variety","3":"1.32056738","4":"potA","5":"peppers","6":"3","7":"2020-05-21","8":"TRUE","9":"NA"},{"1":"hot peppers","2":"variety","3":"1.32056738","4":"potA","5":"peppers","6":"3","7":"2020-05-21","8":"TRUE","9":"NA"},{"1":"hot peppers","2":"variety","3":"1.32056738","4":"potC","5":"hot peppers","6":"6","7":"2020-05-21","8":"TRUE","9":"NA"},{"1":"hot peppers","2":"variety","3":"1.32056738","4":"potD","5":"peppers","6":"1","7":"2020-05-21","8":"TRUE","9":"NA"},{"1":"jalapeño","2":"giant","3":"9.87228836","4":"L","5":"jalapeño","6":"4","7":"2020-05-21","8":"TRUE","9":"NA"},{"1":"kale","2":"Heirloom Lacinto","3":"5.94586014","4":"P","5":"kale","6":"30","7":"2020-05-02","8":"FALSE","9":"NA"},{"1":"kale","2":"Heirloom Lacinto","3":"5.94586014","4":"front","5":"kale","6":"30","7":"2020-06-20","8":"FALSE","9":"NA"},{"1":"kohlrabi","2":"Crispy Colors Duo","3":"0.42108242","4":"front","5":"kohlrabi","6":"10","7":"2020-05-20","8":"FALSE","9":"NA"},{"1":"lettuce","2":"Farmer's Market Blend","3":"3.80296950","4":"C","5":"lettuce","6":"60","7":"2020-05-02","8":"FALSE","9":"NA"},{"1":"lettuce","2":"Farmer's Market Blend","3":"3.80296950","4":"L","5":"lettuce","6":"60","7":"2020-05-16","8":"FALSE","9":"NA"},{"1":"lettuce","2":"Lettuce Mixture","3":"4.74875148","4":"G","5":"lettuce","6":"200","7":"2020-06-20","8":"FALSE","9":"NA"},{"1":"lettuce","2":"mustard greens","3":"0.05070626","4":"NA","5":"NA","6":"NA","7":"<NA>","8":"NA","9":"NA"},{"1":"lettuce","2":"reseed","3":"0.09920790","4":"NA","5":"NA","6":"NA","7":"<NA>","8":"NA","9":"NA"},{"1":"lettuce","2":"Tatsoi","3":"2.89466606","4":"P","5":"lettuce","6":"25","7":"2020-05-02","8":"FALSE","9":"NA"},{"1":"onions","2":"Delicious Duo","3":"0.75398004","4":"P","5":"onions","6":"25","7":"2020-04-26","8":"FALSE","9":"NA"},{"1":"onions","2":"Long Keeping Rainbow","3":"3.31133924","4":"H","5":"onions","6":"40","7":"2020-04-26","8":"FALSE","9":"NA"},{"1":"peas","2":"Magnolia Blossom","3":"7.45822946","4":"B","5":"peas","6":"24","7":"2020-04-19","8":"TRUE","9":"NA"},{"1":"peas","2":"Super Sugar Snap","3":"9.56805080","4":"A","5":"peas","6":"22","7":"2020-04-19","8":"TRUE","9":"NA"},{"1":"peppers","2":"green","3":"5.69232884","4":"K","5":"peppers","6":"12","7":"2020-05-21","8":"TRUE","9":"NA"},{"1":"peppers","2":"green","3":"5.69232884","4":"O","5":"peppers","6":"5","7":"2020-05-21","8":"TRUE","9":"NA"},{"1":"peppers","2":"variety","3":"3.65085072","4":"potA","5":"peppers","6":"3","7":"2020-05-21","8":"TRUE","9":"NA"},{"1":"peppers","2":"variety","3":"3.65085072","4":"potA","5":"peppers","6":"3","7":"2020-05-21","8":"TRUE","9":"NA"},{"1":"peppers","2":"variety","3":"3.65085072","4":"potC","5":"hot peppers","6":"6","7":"2020-05-21","8":"TRUE","9":"NA"},{"1":"peppers","2":"variety","3":"3.65085072","4":"potD","5":"peppers","6":"1","7":"2020-05-21","8":"TRUE","9":"NA"},{"1":"potatoes","2":"purple","3":"3.00930630","4":"D","5":"potatoes","6":"5","7":"2020-05-02","8":"FALSE","9":"NA"},{"1":"potatoes","2":"red","3":"4.43349082","4":"I","5":"potatoes","6":"3","7":"2020-05-22","8":"FALSE","9":"NA"},{"1":"potatoes","2":"Russet","3":"9.09185288","4":"D","5":"potatoes","6":"8","7":"2020-05-02","8":"FALSE","9":"NA"},{"1":"potatoes","2":"yellow","3":"7.40090934","4":"I","5":"potatoes","6":"10","7":"2020-05-02","8":"FALSE","9":"NA"},{"1":"potatoes","2":"yellow","3":"7.40090934","4":"I","5":"potatoes","6":"8","7":"2020-05-22","8":"TRUE","9":"NA"},{"1":"pumpkins","2":"Cinderella's Carraige","3":"32.87308882","4":"B","5":"pumpkins","6":"3","7":"2020-05-20","8":"TRUE","9":"NA"},{"1":"pumpkins","2":"New England Sugar","3":"44.85960776","4":"K","5":"pumpkins","6":"4","7":"2020-05-25","8":"TRUE","9":"NA"},{"1":"pumpkins","2":"saved","3":"76.93241952","4":"B","5":"pumpkins","6":"8","7":"2020-05-20","8":"TRUE","9":"NA"},{"1":"radish","2":"Garden Party Mix","3":"0.94578198","4":"C","5":"radish","6":"20","7":"2020-05-02","8":"FALSE","9":"NA"},{"1":"radish","2":"Garden Party Mix","3":"0.94578198","4":"G","5":"radish","6":"30","7":"2020-05-02","8":"FALSE","9":"NA"},{"1":"radish","2":"Garden Party Mix","3":"0.94578198","4":"H","5":"radish","6":"15","7":"2020-05-02","8":"FALSE","9":"NA"},{"1":"raspberries","2":"perrenial","3":"1.85849466","4":"NA","5":"NA","6":"NA","7":"<NA>","8":"NA","9":"NA"},{"1":"rutabaga","2":"Improved Helenor","3":"29.74032380","4":"E","5":"rudabaga","6":"30","7":"2020-05-25","8":"FALSE","9":"NA"},{"1":"spinach","2":"Catalina","3":"2.03486426","4":"H","5":"spinach","6":"50","7":"2020-05-16","8":"FALSE","9":"NA"},{"1":"spinach","2":"Catalina","3":"2.03486426","4":"E","5":"spinach","6":"100","7":"2020-06-20","8":"FALSE","9":"NA"},{"1":"squash","2":"Blue (saved)","3":"41.52401770","4":"A","5":"squash","6":"4","7":"2020-05-20","8":"TRUE","9":"NA"},{"1":"squash","2":"Blue (saved)","3":"41.52401770","4":"B","5":"squash","6":"8","7":"2020-05-20","8":"TRUE","9":"NA"},{"1":"squash","2":"delicata","3":"10.49840044","4":"K","5":"squash","6":"8","7":"2020-05-25","8":"TRUE","9":"NA"},{"1":"squash","2":"Red Kuri","3":"22.73183682","4":"A","5":"squash","6":"4","7":"2020-05-20","8":"TRUE","9":"NA"},{"1":"squash","2":"Red Kuri","3":"22.73183682","4":"B","5":"squash","6":"4","7":"2020-05-20","8":"TRUE","9":"NA"},{"1":"squash","2":"Red Kuri","3":"22.73183682","4":"side","5":"squash","6":"1","7":"2020-05-20","8":"TRUE","9":"NA"},{"1":"squash","2":"Waltham Butternut","3":"24.27066158","4":"A","5":"squash","6":"4","7":"2020-05-20","8":"TRUE","9":"NA"},{"1":"squash","2":"Waltham Butternut","3":"24.27066158","4":"K","5":"squash","6":"6","7":"2020-05-25","8":"TRUE","9":"NA"},{"1":"strawberries","2":"perrenial","3":"1.30513504","4":"NA","5":"NA","6":"NA","7":"<NA>","8":"NA","9":"NA"},{"1":"Swiss chard","2":"Neon Glow","3":"6.88282364","4":"M","5":"Swiss chard","6":"25","7":"2020-05-02","8":"FALSE","9":"NA"},{"1":"tomatoes","2":"Amish Paste","3":"65.67342518","4":"J","5":"tomatoes","6":"1","7":"2020-05-20","8":"TRUE","9":"NA"},{"1":"tomatoes","2":"Amish Paste","3":"65.67342518","4":"N","5":"tomatoes","6":"2","7":"2020-05-20","8":"TRUE","9":"NA"},{"1":"tomatoes","2":"Better Boy","3":"34.00846812","4":"J","5":"tomatoes","6":"1","7":"2020-05-20","8":"TRUE","9":"NA"},{"1":"tomatoes","2":"Better Boy","3":"34.00846812","4":"N","5":"tomatoes","6":"1","7":"2020-05-20","8":"TRUE","9":"NA"},{"1":"tomatoes","2":"Big Beef","3":"24.99377694","4":"N","5":"tomatoes","6":"1","7":"2020-05-20","8":"TRUE","9":"NA"},{"1":"tomatoes","2":"Black Krim","3":"15.80712540","4":"N","5":"tomatoes","6":"1","7":"2020-05-20","8":"TRUE","9":"NA"},{"1":"tomatoes","2":"Bonny Best","3":"24.92322910","4":"J","5":"tomatoes","6":"1","7":"2020-05-20","8":"TRUE","9":"NA"},{"1":"tomatoes","2":"Brandywine","3":"15.64618814","4":"J","5":"tomatoes","6":"1","7":"2020-05-20","8":"TRUE","9":"NA"},{"1":"tomatoes","2":"Cherokee Purple","3":"15.71232674","4":"J","5":"tomatoes","6":"1","7":"2020-05-20","8":"TRUE","9":"NA"},{"1":"tomatoes","2":"grape","3":"32.39468628","4":"O","5":"tomatoes","6":"1","7":"2020-05-20","8":"TRUE","9":"NA"},{"1":"tomatoes","2":"Jet Star","3":"15.02448530","4":"N","5":"tomatoes","6":"1","7":"2020-05-20","8":"TRUE","9":"NA"},{"1":"tomatoes","2":"Mortgage Lifter","3":"26.32536742","4":"J","5":"tomatoes","6":"1","7":"2020-05-20","8":"TRUE","9":"died"},{"1":"tomatoes","2":"Mortgage Lifter","3":"26.32536742","4":"N","5":"tomatoes","6":"1","7":"2020-05-20","8":"TRUE","9":"NA"},{"1":"tomatoes","2":"Old German","3":"26.71778978","4":"J","5":"tomatoes","6":"1","7":"2020-05-20","8":"TRUE","9":"NA"},{"1":"tomatoes","2":"volunteers","3":"51.61235882","4":"N","5":"tomatoes","6":"1","7":"2020-06-03","8":"TRUE","9":"NA"},{"1":"tomatoes","2":"volunteers","3":"51.61235882","4":"J","5":"tomatoes","6":"1","7":"2020-06-03","8":"TRUE","9":"NA"},{"1":"tomatoes","2":"volunteers","3":"51.61235882","4":"front","5":"tomatoes","6":"5","7":"2020-06-03","8":"TRUE","9":"NA"},{"1":"tomatoes","2":"volunteers","3":"51.61235882","4":"O","5":"tomatoes","6":"2","7":"2020-06-03","8":"TRUE","9":"NA"},{"1":"zucchini","2":"Romanesco","3":"99.70834874","4":"D","5":"zucchini","6":"3","7":"2020-05-21","8":"TRUE","9":"NA"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>

*Some vegetable varieties seem to have the same total harvest weight despite being in different plots. There are more observations than there should be because the plot from which it was collected was not included in the initial garden harvest data. As a result, when a variety is planted in two plots and then combined, the total weights are reported twice. To fix this, you would want to first summarize the garden_planting data so that each variety only has one observation. In addition, we could possibly fix this by using a different type of join function.

  3. I would like to understand how much money I "saved" by gardening, for each vegetable type. Describe how I could use the `garden_harvest` and `garden_spending` datasets, along with data from somewhere like [this](https://products.wholefoodsmarket.com/search?sort=relevance&store=10542) to answer this question. You can answer this in words, referencing various join functions. You don't need R code but could provide some if it's helpful.
  
*We could use the 'garden_harvest' and 'garden_spending' data sets to understand how much money that was saved for each vegetable by assuming that the price in 'garden_spending' is price per pound and and by joining the two charts by vegetable and variety. In addition, we can make a new variable that multiplies the price by the total weight to see how much money that saved.

  4. Subset the data to tomatoes. Reorder the tomato varieties from smallest to largest first harvest date. Create a barplot of total harvest in pounds for each variety, in the new order.CHALLENGE: add the date near the end of the bar. (This is probably not a super useful graph because it's difficult to read. This is more an exercise in using some of the functions you just learned.)


```r
garden_harvest %>% 
  filter(vegetable == "tomatoes") %>% 
  mutate(variety2 = fct_reorder(variety, date, min, .desc = TRUE)) %>%
  group_by(variety2, date) %>% 
  summarize(total_weight_lbs = sum(weight*0.00220462)) %>%
  arrange(total_weight_lbs, date) %>% 
  ggplot(aes(x = total_weight_lbs,
             y = variety2)) +
  geom_col() +
  labs(x = "Weight (lbs)",
       y = "",
       title = "Total Harvest for Each Tomato Variety From Earliest to Latest First Harvest Date")
```

![](03_exercises_files/figure-html/unnamed-chunk-3-1.png)<!-- -->

  5. In the `garden_harvest` data, create two new variables: one that makes the varieties lowercase and another that finds the length of the variety name. Arrange the data by vegetable and length of variety name (smallest to largest), with one row for each vegetable variety. HINT: use `str_to_lower()`, `str_length()`, and `distinct()`.
  

```r
garden_harvest %>% 
  mutate(variety_lower = str_to_lower(variety),
         length_variety_name = str_length(variety)) %>%
  distinct(variety, .keep_all = TRUE) %>%
  arrange(vegetable, length_variety_name)
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["vegetable"],"name":[1],"type":["chr"],"align":["left"]},{"label":["variety"],"name":[2],"type":["chr"],"align":["left"]},{"label":["date"],"name":[3],"type":["date"],"align":["right"]},{"label":["weight"],"name":[4],"type":["dbl"],"align":["right"]},{"label":["units"],"name":[5],"type":["chr"],"align":["left"]},{"label":["variety_lower"],"name":[6],"type":["chr"],"align":["left"]},{"label":["length_variety_name"],"name":[7],"type":["int"],"align":["right"]}],"data":[{"1":"apple","2":"unknown","3":"2020-09-26","4":"156","5":"grams","6":"unknown","7":"7"},{"1":"asparagus","2":"asparagus","3":"2020-06-20","4":"20","5":"grams","6":"asparagus","7":"9"},{"1":"basil","2":"Isle of Naxos","3":"2020-06-23","4":"5","5":"grams","6":"isle of naxos","7":"13"},{"1":"beans","2":"Bush Bush Slender","3":"2020-07-06","4":"235","5":"grams","6":"bush bush slender","7":"17"},{"1":"beans","2":"Chinese Red Noodle","3":"2020-08-08","4":"108","5":"grams","6":"chinese red noodle","7":"18"},{"1":"beans","2":"Classic Slenderette","3":"2020-08-05","4":"41","5":"grams","6":"classic slenderette","7":"19"},{"1":"beets","2":"leaves","3":"2020-06-11","4":"8","5":"grams","6":"leaves","7":"6"},{"1":"beets","2":"Sweet Merlin","3":"2020-07-07","4":"10","5":"grams","6":"sweet merlin","7":"12"},{"1":"beets","2":"Gourmet Golden","3":"2020-07-07","4":"62","5":"grams","6":"gourmet golden","7":"14"},{"1":"broccoli","2":"Yod Fah","3":"2020-07-27","4":"372","5":"grams","6":"yod fah","7":"7"},{"1":"broccoli","2":"Main Crop Bravado","3":"2020-09-09","4":"102","5":"grams","6":"main crop bravado","7":"17"},{"1":"carrots","2":"Dragon","3":"2020-07-24","4":"80","5":"grams","6":"dragon","7":"6"},{"1":"carrots","2":"Bolero","3":"2020-07-30","4":"116","5":"grams","6":"bolero","7":"6"},{"1":"carrots","2":"greens","3":"2020-08-29","4":"169","5":"grams","6":"greens","7":"6"},{"1":"carrots","2":"King Midas","3":"2020-07-23","4":"56","5":"grams","6":"king midas","7":"10"},{"1":"chives","2":"perrenial","3":"2020-06-17","4":"8","5":"grams","6":"perrenial","7":"9"},{"1":"cilantro","2":"cilantro","3":"2020-06-23","4":"2","5":"grams","6":"cilantro","7":"8"},{"1":"corn","2":"Dorinny Sweet","3":"2020-08-11","4":"330","5":"grams","6":"dorinny sweet","7":"13"},{"1":"corn","2":"Golden Bantam","3":"2020-08-15","4":"383","5":"grams","6":"golden bantam","7":"13"},{"1":"cucumbers","2":"pickling","3":"2020-07-08","4":"181","5":"grams","6":"pickling","7":"8"},{"1":"edamame","2":"edamame","3":"2020-08-11","4":"109","5":"grams","6":"edamame","7":"7"},{"1":"hot peppers","2":"thai","3":"2020-07-20","4":"12","5":"grams","6":"thai","7":"4"},{"1":"hot peppers","2":"variety","3":"2020-07-20","4":"559","5":"grams","6":"variety","7":"7"},{"1":"jalapeño","2":"giant","3":"2020-07-17","4":"20","5":"grams","6":"giant","7":"5"},{"1":"kale","2":"Heirloom Lacinto","3":"2020-06-13","4":"10","5":"grams","6":"heirloom lacinto","7":"16"},{"1":"kohlrabi","2":"Crispy Colors Duo","3":"2020-09-17","4":"191","5":"grams","6":"crispy colors duo","7":"17"},{"1":"lettuce","2":"reseed","3":"2020-06-06","4":"20","5":"grams","6":"reseed","7":"6"},{"1":"lettuce","2":"Tatsoi","3":"2020-06-20","4":"18","5":"grams","6":"tatsoi","7":"6"},{"1":"lettuce","2":"mustard greens","3":"2020-06-29","4":"23","5":"grams","6":"mustard greens","7":"14"},{"1":"lettuce","2":"Lettuce Mixture","3":"2020-07-22","4":"23","5":"grams","6":"lettuce mixture","7":"15"},{"1":"lettuce","2":"Farmer's Market Blend","3":"2020-06-11","4":"12","5":"grams","6":"farmer's market blend","7":"21"},{"1":"onions","2":"Delicious Duo","3":"2020-07-16","4":"50","5":"grams","6":"delicious duo","7":"13"},{"1":"onions","2":"Long Keeping Rainbow","3":"2020-07-20","4":"102","5":"grams","6":"long keeping rainbow","7":"20"},{"1":"peas","2":"Magnolia Blossom","3":"2020-06-17","4":"8","5":"grams","6":"magnolia blossom","7":"16"},{"1":"peas","2":"Super Sugar Snap","3":"2020-06-17","4":"121","5":"grams","6":"super sugar snap","7":"16"},{"1":"peppers","2":"green","3":"2020-08-04","4":"81","5":"grams","6":"green","7":"5"},{"1":"potatoes","2":"red","3":"2020-10-15","4":"1718","5":"grams","6":"red","7":"3"},{"1":"potatoes","2":"purple","3":"2020-08-06","4":"317","5":"grams","6":"purple","7":"6"},{"1":"potatoes","2":"yellow","3":"2020-08-06","4":"439","5":"grams","6":"yellow","7":"6"},{"1":"potatoes","2":"Russet","3":"2020-09-16","4":"629","5":"grams","6":"russet","7":"6"},{"1":"pumpkins","2":"saved","3":"2020-09-01","4":"4758","5":"grams","6":"saved","7":"5"},{"1":"pumpkins","2":"New England Sugar","3":"2020-09-19","4":"1109","5":"grams","6":"new england sugar","7":"17"},{"1":"pumpkins","2":"Cinderella's Carraige","3":"2020-09-01","4":"7350","5":"grams","6":"cinderella's carraige","7":"21"},{"1":"radish","2":"Garden Party Mix","3":"2020-06-06","4":"36","5":"grams","6":"garden party mix","7":"16"},{"1":"rutabaga","2":"Improved Helenor","3":"2020-10-16","4":"883","5":"grams","6":"improved helenor","7":"16"},{"1":"spinach","2":"Catalina","3":"2020-06-11","4":"9","5":"grams","6":"catalina","7":"8"},{"1":"squash","2":"delicata","3":"2020-09-19","4":"307","5":"grams","6":"delicata","7":"8"},{"1":"squash","2":"Red Kuri","3":"2020-09-19","4":"1178","5":"grams","6":"red kuri","7":"8"},{"1":"squash","2":"Blue (saved)","3":"2020-09-01","4":"3227","5":"grams","6":"blue (saved)","7":"12"},{"1":"squash","2":"Waltham Butternut","3":"2020-09-19","4":"1834","5":"grams","6":"waltham butternut","7":"17"},{"1":"Swiss chard","2":"Neon Glow","3":"2020-06-21","4":"19","5":"grams","6":"neon glow","7":"9"},{"1":"tomatoes","2":"grape","3":"2020-07-11","4":"24","5":"grams","6":"grape","7":"5"},{"1":"tomatoes","2":"Big Beef","3":"2020-07-21","4":"137","5":"grams","6":"big beef","7":"8"},{"1":"tomatoes","2":"Jet Star","3":"2020-07-28","4":"315","5":"grams","6":"jet star","7":"8"},{"1":"tomatoes","2":"Bonny Best","3":"2020-07-21","4":"339","5":"grams","6":"bonny best","7":"10"},{"1":"tomatoes","2":"Better Boy","3":"2020-07-24","4":"220","5":"grams","6":"better boy","7":"10"},{"1":"tomatoes","2":"Old German","3":"2020-07-28","4":"611","5":"grams","6":"old german","7":"10"},{"1":"tomatoes","2":"Brandywine","3":"2020-08-01","4":"320","5":"grams","6":"brandywine","7":"10"},{"1":"tomatoes","2":"Black Krim","3":"2020-08-01","4":"436","5":"grams","6":"black krim","7":"10"},{"1":"tomatoes","2":"volunteers","3":"2020-08-04","4":"73","5":"grams","6":"volunteers","7":"10"},{"1":"tomatoes","2":"Amish Paste","3":"2020-07-25","4":"463","5":"grams","6":"amish paste","7":"11"},{"1":"tomatoes","2":"Cherokee Purple","3":"2020-07-24","4":"247","5":"grams","6":"cherokee purple","7":"15"},{"1":"tomatoes","2":"Mortgage Lifter","3":"2020-07-27","4":"801","5":"grams","6":"mortgage lifter","7":"15"},{"1":"zucchini","2":"Romanesco","3":"2020-07-06","4":"175","5":"grams","6":"romanesco","7":"9"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>

  6. In the `garden_harvest` data, find all distinct vegetable varieties that have "er" or "ar" in their name. HINT: `str_detect()` with an "or" statement (use the | for "or") and `distinct()`.


```r
garden_harvest %>%
  distinct(variety, .keep_all = TRUE) %>%
  filter(str_detect(variety, "er")|str_detect(variety, "ar"))
``

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
```

```
## Error: attempt to use zero-length variable name
```

```r
data_site <- 
  "https://www.macalester.edu/~dshuman1/data/112/2014-Q4-Trips-History-Data.rds" 
Trips <- readRDS(gzcon(url(data_site)))
Stations<-read_csv("http://www.macalester.edu/~dshuman1/data/112/DC-Stations.csv")
```

**NOTE:** The `Trips` data table is a random subset of 10,000 trips from the full quarterly data. Start with this small data table to develop your analysis commands. **When you have this working well, you should access the full data set of more than 600,000 events by removing `-Small` from the name of the `data_site`.**

### Temporal patterns

It's natural to expect that bikes are rented more at some times of day, some days of the week, some months of the year than others. The variable `sdate` gives the time (including the date) that the rental started. Make the following plots and interpret them:

  7. A density plot, which is a smoothed out histogram, of the events versus `sdate`. Use `geom_density()`.
  

```r
Trips %>% 
  ggplot(aes(sdate)) +
  geom_density() + 
  labs(x = "",
       y = "Frequency",
       title = "Density Plot of Bike Rental Trends Over Time")
```

![](03_exercises_files/figure-html/unnamed-chunk-7-1.png)<!-- -->

*This density plot illustrates the amount of times individuals rented the bikes at a date and time from October to the start of January. The most amount of bike rentals occurred from the month of October to the middle of November.
  
  8. A density plot of the events versus time of day.  You can use `mutate()` with `lubridate`'s  `hour()` and `minute()` functions to extract the hour of the day and minute within the hour from `sdate`. Hint: A minute is 1/60 of an hour, so create a variable where 3:30 is 3.5 and 3:45 is 3.75.
  

```r
Trips %>% 
  mutate(time = hour(sdate) + (minute(sdate)/60)) %>% 
  ggplot(aes(time)) +
  geom_density() +
  labs(x = "Time",
       y = "Frequency",
       title = "Density Plot of Bike Rental Trends Throughout the Day")
```

![](03_exercises_files/figure-html/unnamed-chunk-8-1.png)<!-- -->
  
*In this graph, we can see the times of day in which most individuals tend to rent the bikes. The most popular time of day to rent a bike are between 8-9 AM and 4-5 PM.
  
  9. A bar graph of the events versus day of the week. Put day on the y-axis.
  

```r
Trips %>% 
  mutate(day_of_week = wday(sdate)) %>% 
  ggplot(aes(y = day_of_week)) +
  geom_bar() +
  scale_y_discrete(
    limits = c("Sunday","Monday","Tuesday","Wednesday","Thursday","Friday","Saturday"),
    breaks = c("Sunday","Monday","Tuesday","Wednesday","Thursday","Friday","Saturday"),
    labels = c("Sunday","Monday","Tuesday","Wednesday","Thursday","Friday","Saturday")) +
  labs(x = "",
       y = "",
       title = "Amount of Rentals For Each Day of the Week")
```

![](03_exercises_files/figure-html/unnamed-chunk-9-1.png)<!-- -->

*From the graph, we see the total amount of rentals placed on each day of the week. Friday and Monday seem to be most popular day of the week to rent a bike.

  10. Facet your graph from exercise 8. by day of the week. Is there a pattern?
  

```r
Trips %>% 
  mutate(time = hour(sdate) + (minute(sdate)/60),
         day_of_week = wday(sdate, label = TRUE)) %>% 
  ggplot(aes(time)) + 
  geom_density() +
  facet_wrap(~day_of_week) +
  labs(x = "Time",
       y = "Frequency",
       title = "Bike Rental Trends Throughout the Day for Each Day of the Week")
```

![](03_exercises_files/figure-html/unnamed-chunk-10-1.png)<!-- -->

*After faceting, there seems to be pattern that is recurrent for each weekday and each weekend. The weekdays seem to have the same trend with two peaks of popularity at 8-9AM and 4-5PM with a lull in between, while the weekends have one major peak from 7AM-8PM. This appears to depict a typical workday, with people renting bikes either before or after work (additionally this might be people who commute by bike).

The variable `client` describes whether the renter is a regular user (level `Registered`) or has not joined the bike-rental organization (`Causal`). The next set of exercises investigate whether these two different categories of users show different rental behavior and how `client` interacts with the patterns you found in the previous exercises. 

  11. Change the graph from exercise 10 to set the `fill` aesthetic for `geom_density()` to the `client` variable. You should also set `alpha = .5` for transparency and `color=NA` to suppress the outline of the density function.
  

```r
Trips %>% 
  mutate(time = hour(sdate) + (minute(sdate)/60),
         day_of_week = wday(sdate, label = TRUE)) %>% 
  ggplot(aes(time, 
             fill = client, 
             alpha = .5)) + 
  geom_density(color = NA) +
  facet_wrap(~day_of_week) +
  labs(x = "Time",
       y = "Frequency",
       title = "Bike Rental Trends Throughout the Day for Each Day of the Week with Client Type")
```

![](03_exercises_files/figure-html/unnamed-chunk-11-1.png)<!-- -->

  12. Change the previous graph by adding the argument `position = position_stack()` to `geom_density()`. In your opinion, is this better or worse in terms of telling a story? What are the advantages/disadvantages of each?
  

```r
Trips %>% 
  mutate(time = hour(sdate) + (minute(sdate)/60),
         day_of_week = wday(sdate, label = TRUE)) %>% 
  ggplot(aes(time, 
             fill = client, 
             alpha = .5)) + 
  geom_density(color = NA, 
               position = position_stack()) +
  facet_wrap(~day_of_week) +
    labs(x = "Time",
       y = "Frequency",
       title = "Bike Rental Trends Throughout the Day for Each Day of the Week with Client Type")
```

![](03_exercises_files/figure-html/unnamed-chunk-12-1.png)<!-- -->
  
*In terms of telling a story, this graph is worse as it adds more to the graph making it hard to view the real relationship between clients as casual clients differ in times of registered clients. The graph from number 11 allows you to compare the trends individually, without the registered riders' values influencing the casual riders' values. The overlapping graph, on the other hand, makes determining the proportion of total riders that each group represents more difficult. The stacked graph would be ideal if you wanted to know which group had more riders out at a specific time of day. When comparing each group to the overall, stacked graphs are preferable, but overlapped graphs are preferable when comparing each group to each other.
  
  13. In this graph, go back to using the regular density plot (without `position = position_stack()`). Add a new variable to the dataset called `weekend` which will be "weekend" if the day is Saturday or Sunday and  "weekday" otherwise (HINT: use the `ifelse()` function and the `wday()` function from `lubridate`). Then, update the graph from the previous problem by faceting on the new `weekend` variable. 
  

```r
Trips %>% 
  mutate(time = hour(sdate) + (minute(sdate)/60),
         day_of_week = wday(sdate),
         isweekend = ifelse(day_of_week == 1|day_of_week ==7, "Weekend", "Weekday")) %>% 
  ggplot(aes(time, 
             fill = client, 
             alpha = .5)) + 
  geom_density(color = NA) +
  facet_wrap(~isweekend) +
  labs(x = "Time", 
       y = "Density",
       title = "Density Plots of Bike Trips Over the Time of Day, 
       for Weekdays and Weekends")
```

![](03_exercises_files/figure-html/unnamed-chunk-13-1.png)<!-- -->
  
  14. Change the graph from the previous problem to facet on `client` and fill with `weekday`. What information does this graph tell you that the previous didn't? Is one graph better than the other?
  

```r
Trips %>% 
  mutate(time = hour(sdate) + (minute(sdate)/60),
         day_of_week = wday(sdate),
         isweekend = ifelse(day_of_week == 1|day_of_week ==7, "Weekend", "Weekday")) %>% 
  ggplot(aes(time, fill = isweekend, alpha = .5)) + 
  geom_density(color = NA) +
  facet_wrap(~client)+
  labs(x = "Time", 
       y = "Density",
       title = "Density Plots of Bike Trips Over the Time of Day, 
       for Weekdays and Weekends")
```

![](03_exercises_files/figure-html/unnamed-chunk-14-1.png)<!-- -->

*I think the first graph from 13 is better both in terms of being aesthetically pleasing and in interpretation. This graph depicts how each type of client rides differently depending on whether it is a weekend or weekday. The other graph illustrated how the types of clients differed from each other depending on whether it was a weekend or a weekday. What you're aiming to achieve determines whether one graph is better than the other. If you're looking to compare within client types, the graph of 14 is preferable. If you're attempting to compare between client types, the graph of 13 is preferable.
  
### Spatial patterns

  15. Use the latitude and longitude variables in `Stations` to make a visualization of the total number of departures from each station in the `Trips` data. Use either color or size to show the variation in number of departures. We will improve this plot next week when we learn about maps!
  

```r
Stations %>% 
  mutate(sstation = name) %>% 
  left_join(Trips, 
            by = "sstation") %>% 
  group_by(name, lat, long) %>% 
  count() %>% 
  ggplot(aes(x = lat, 
             y = long, 
             color = n)) +
  geom_point() +
  labs(x = "Latitude",
       y = "Longitude",
       title = "Total Number of Departures from Each Station",
       col = "Departures")
```

![](03_exercises_files/figure-html/unnamed-chunk-15-1.png)<!-- -->
  
  16. Only 14.4% of the trips in our data are carried out by casual users. Create a plot that shows which area(s) have stations with a much higher percentage of departures by casual users. What patterns do you notice? (Again, we'll improve this next week when we learn about maps).
  

```r
Stations %>% 
  mutate(sstation = name) %>% 
  left_join(Trips, 
            by = "sstation") %>% 
  group_by(name, lat, long) %>% 
  summarize(percent_casual = sum(client == "Casual")/n()) %>% 
  ggplot(aes(x = lat, 
             y = long, 
             color = percent_casual)) +
  geom_point() +
  labs(x = "Latitude",
       y = "Longitude",
       title = "Proportion of Departures from Each Station that are Casual Riders",
       col = "Percentage of Casual")
```

![](03_exercises_files/figure-html/unnamed-chunk-16-1.png)<!-- -->

*The center of graph shows a pattern of the major bulk of the data points, where it displays more of registered client stations. Registered clients tend to use the bikes in more closely dense areas compared to casual riders, which make up a few stations. 
  
**DID YOU REMEMBER TO GO BACK AND CHANGE THIS SET OF EXERCISES TO THE LARGER DATASET? IF NOT, DO THAT NOW.**

## Dogs!

In this section, we'll use the data from 2022-02-01 Tidy Tuesday. If you didn't use that data or need a little refresher on it, see the [website](https://github.com/rfordatascience/tidytuesday/blob/master/data/2022/2022-02-01/readme.md).


  17. The final product of this exercise will be a graph that has breed on the y-axis and the sum of the numeric ratings in the `breed_traits` dataset on the x-axis, with a dot for each rating. First, create a new dataset called `breed_traits_total` that has two variables -- `Breed` and `total_rating`. The `total_rating` variable is the sum of the numeric ratings in the `breed_traits` dataset (we'll use this dataset again in the next problem). Then, create the graph just described. Omit Breeds with a `total_rating` of 0 and order the Breeds from highest to lowest ranked. You may want to adjust the `fig.height` and `fig.width` arguments inside the code chunk options (eg. `{r, fig.height=8, fig.width=4}`) so you can see things more clearly - check this after you knit the file to assure it looks like what you expected.


```r
breed_traits_total <- breed_traits %>% 
  pivot_longer(cols = -c(Breed,`Coat Type`, `Coat Length`),
               values_to = "ratings") %>% 
  group_by(Breed) %>% 
  summarize(total_ratings = sum(ratings)) %>% 
  arrange(desc(total_ratings))

 
breed_traits_total %>%
  filter(total_ratings != 0) %>% 
  ggplot(aes(x = total_ratings, 
             y = fct_reorder(Breed, total_ratings))) +
  geom_point() +
  labs(x = "Rating", 
       y = "Breed", 
       title = "Total Ranking of Dog Breeds")
```

![](03_exercises_files/figure-html/unnamed-chunk-17-1.png)<!-- -->

  18. The final product of this exercise will be a graph with the top-20 dogs in total ratings (from previous problem) on the y-axis, year on the x-axis, and points colored by each breed's ranking for that year (from the `breed_rank_all` dataset). The points within each breed will be connected by a line, and the breeds should be arranged from the highest median rank to lowest median rank ("highest" is actually the smallest numer, eg. 1 = best). After you're finished, think of AT LEAST one thing you could you do to make this graph better. HINTS: 1. Start with the `breed_rank_all` dataset and pivot it so year is a variable. 2. Use the `separate()` function to get year alone, and there's an extra argument in that function that can make it numeric. 3. For both datasets used, you'll need to `str_squish()` Breed before joining. 
  

```r
total20 <- breed_traits_total %>%
  filter(total_ratings >= 55) %>%
  mutate(breed = str_squish(Breed))


rank_breeds <-breed_rank_all %>%
  pivot_longer(cols = -c(Breed, links, Image),
               names_to = "years",
               values_to = "ranks") %>%
  select(Breed, years, ranks) %>%
  separate(years, into = c("years"), convert = TRUE) %>%
  mutate(breed = str_squish(Breed))


total20 %>%  
  left_join(rank_breeds,
            by = "breed") %>%
  select(c("breed","total_ratings","years", "ranks")) %>%
  ggplot(aes(x = years,
             y = fct_reorder(breed,desc(ranks)), 
             color = ranks, 
             size = total_ratings))+
  geom_point() +
  labs(x = "Year",
       y = "Breed")
```

![](03_exercises_files/figure-html/unnamed-chunk-18-1.png)<!-- -->

*One of the things that I could do to make this better is by changing the colour so that it will be easier to tell where the breeds fall when it comes to the rankings.

  19. Create your own! Requirements: use a `join` or `pivot` function (or both, if you'd like), a `str_XXX()` function, and a `fct_XXX()` function to create a graph using any of the dog datasets. One suggestion is to try to improve the graph you created for the Tidy Tuesday assignment. If you want an extra challenge, find a way to use the dog images in the `breed_rank_all` file - check out the `ggimage` library and [this resource](https://wilkelab.org/ggtext/) for putting images as labels.
  

```r
total20 %>%  
  left_join(rank_breeds,
            by = "breed") %>%
  select(c("breed",
           "total_ratings",
           "years", 
           "ranks")) %>%
  ggplot(aes(x = years,
             y = ranks, 
             color = breed))+
  geom_point() +
  geom_line() +
  labs(x = "Year",
       y = "Ranks",
       title = "Yearly Ranking of Top 20 Dog Breeds")
```

![](03_exercises_files/figure-html/unnamed-chunk-19-1.png)<!-- -->
  
## GitHub link

  20. Below, provide a link to your GitHub page with this set of Weekly Exercises. Specifically, if the name of the file is 03_exercises.Rmd, provide a link to the 03_exercises.md file, which is the one that will be most readable on GitHub.
  
https://github.com/Btpanderson/Weekly-Exercise-3/blob/master/03_exercises.md

## Challenge problem! 

This problem uses the data from the Tidy Tuesday competition this week, `kids`. If you need to refresh your memory on the data, read about it [here](https://github.com/rfordatascience/tidytuesday/blob/master/data/2020/2020-09-15/readme.md). 

  21. In this exercise, you are going to try to replicate the graph below, created by Georgios Karamanis. I'm sure you can find the exact code on GitHub somewhere, but **DON'T DO THAT!** You will only be graded for putting an effort into this problem. So, give it a try and see how far you can get without doing too much googling. HINT: use `facet_geo()`. The graphic won't load below since it came from a location on my computer. So, you'll have to reference the original html on the moodle page to see it.
  


**DID YOU REMEMBER TO UNCOMMENT THE OPTIONS AT THE TOP?**
