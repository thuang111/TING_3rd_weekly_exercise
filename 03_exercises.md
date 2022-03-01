---
title: 'Weekly Exercises #3'
author: "Ting Huang"
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
  mutate(day_of_week = wday(date)) %>% 
  mutate(weight_lbs = weight * 0.002204623) %>% 
  group_by(vegetable, day_of_week) %>% 
  summarize(sum(weight_lbs))
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["vegetable"],"name":[1],"type":["chr"],"align":["left"]},{"label":["day_of_week"],"name":[2],"type":["dbl"],"align":["right"]},{"label":["sum(weight_lbs)"],"name":[3],"type":["dbl"],"align":["right"]}],"data":[{"1":"apple","2":"7","3":"0.343921188"},{"1":"asparagus","2":"7","3":"0.044092460"},{"1":"basil","2":"2","3":"0.066138690"},{"1":"basil","2":"3","3":"0.110231150"},{"1":"basil","2":"5","3":"0.026455476"},{"1":"basil","2":"6","3":"0.467380076"},{"1":"basil","2":"7","3":"0.410059878"},{"1":"beans","2":"1","3":"1.913612764"},{"1":"beans","2":"2","3":"6.508047096"},{"1":"beans","2":"3","3":"4.387199770"},{"1":"beans","2":"4","3":"4.082961796"},{"1":"beans","2":"5","3":"3.392914797"},{"1":"beans","2":"6","3":"1.525599116"},{"1":"beans","2":"7","3":"4.709074728"},{"1":"beets","2":"1","3":"0.321874958"},{"1":"beets","2":"2","3":"0.672410015"},{"1":"beets","2":"3","3":"0.158732856"},{"1":"beets","2":"4","3":"0.182983709"},{"1":"beets","2":"5","3":"11.891736462"},{"1":"beets","2":"6","3":"0.024250853"},{"1":"beets","2":"7","3":"0.379195156"},{"1":"broccoli","2":"1","3":"1.258839733"},{"1":"broccoli","2":"2","3":"0.820119756"},{"1":"broccoli","2":"4","3":"0.707683983"},{"1":"broccoli","2":"6","3":"0.165346725"},{"1":"carrots","2":"1","3":"2.936557836"},{"1":"carrots","2":"2","3":"0.870826085"},{"1":"carrots","2":"3","3":"0.352739680"},{"1":"carrots","2":"4","3":"5.562263829"},{"1":"carrots","2":"5","3":"2.674207699"},{"1":"carrots","2":"6","3":"2.138484310"},{"1":"carrots","2":"7","3":"2.330286511"},{"1":"chives","2":"4","3":"0.017636984"},{"1":"cilantro","2":"3","3":"0.004409246"},{"1":"cilantro","2":"6","3":"0.072752559"},{"1":"cilantro","2":"7","3":"0.037478591"},{"1":"corn","2":"1","3":"1.457255803"},{"1":"corn","2":"2","3":"0.758390312"},{"1":"corn","2":"3","3":"0.727525590"},{"1":"corn","2":"4","3":"5.302118315"},{"1":"corn","2":"6","3":"3.448030372"},{"1":"corn","2":"7","3":"1.316159931"},{"1":"cucumbers","2":"1","3":"3.104109184"},{"1":"cucumbers","2":"2","3":"4.775213418"},{"1":"cucumbers","2":"3","3":"10.046467011"},{"1":"cucumbers","2":"4","3":"5.306527561"},{"1":"cucumbers","2":"5","3":"3.306934500"},{"1":"cucumbers","2":"6","3":"7.429579510"},{"1":"cucumbers","2":"7","3":"9.640816379"},{"1":"edamame","2":"3","3":"1.402140228"},{"1":"edamame","2":"7","3":"4.689233121"},{"1":"hot peppers","2":"2","3":"1.258839733"},{"1":"hot peppers","2":"3","3":"0.141095872"},{"1":"hot peppers","2":"4","3":"0.068343313"},{"1":"jalapeño","2":"1","3":"0.262350137"},{"1":"jalapeño","2":"2","3":"5.553445337"},{"1":"jalapeño","2":"3","3":"0.548951127"},{"1":"jalapeño","2":"4","3":"0.480607814"},{"1":"jalapeño","2":"5","3":"0.224871546"},{"1":"jalapeño","2":"6","3":"1.294113701"},{"1":"jalapeño","2":"7","3":"1.507962132"},{"1":"kale","2":"1","3":"0.826733625"},{"1":"kale","2":"2","3":"2.067936374"},{"1":"kale","2":"3","3":"0.282191744"},{"1":"kale","2":"4","3":"0.617294440"},{"1":"kale","2":"5","3":"0.279987121"},{"1":"kale","2":"6","3":"0.381399779"},{"1":"kale","2":"7","3":"1.490325148"},{"1":"kohlrabi","2":"5","3":"0.421082993"},{"1":"lettuce","2":"1","3":"1.466074295"},{"1":"lettuce","2":"2","3":"2.458154645"},{"1":"lettuce","2":"3","3":"0.917123168"},{"1":"lettuce","2":"4","3":"1.186087174"},{"1":"lettuce","2":"5","3":"2.451540776"},{"1":"lettuce","2":"6","3":"1.801176991"},{"1":"lettuce","2":"7","3":"1.316159931"},{"1":"onions","2":"1","3":"0.260145514"},{"1":"onions","2":"2","3":"0.509267913"},{"1":"onions","2":"3","3":"0.707683983"},{"1":"onions","2":"5","3":"0.601862079"},{"1":"onions","2":"6","3":"0.072752559"},{"1":"onions","2":"7","3":"1.913612764"},{"1":"peas","2":"1","3":"2.056913259"},{"1":"peas","2":"2","3":"4.634117546"},{"1":"peas","2":"3","3":"2.067936374"},{"1":"peas","2":"4","3":"1.080265270"},{"1":"peas","2":"5","3":"3.397324043"},{"1":"peas","2":"6","3":"0.936964775"},{"1":"peas","2":"7","3":"2.852782162"},{"1":"peppers","2":"1","3":"0.502654044"},{"1":"peppers","2":"2","3":"2.526497958"},{"1":"peppers","2":"3","3":"1.444028065"},{"1":"peppers","2":"4","3":"2.442722284"},{"1":"peppers","2":"5","3":"0.709888606"},{"1":"peppers","2":"6","3":"0.335102696"},{"1":"peppers","2":"7","3":"1.382298621"},{"1":"potatoes","2":"2","3":"0.970034120"},{"1":"potatoes","2":"4","3":"4.570183479"},{"1":"potatoes","2":"5","3":"11.852053248"},{"1":"potatoes","2":"6","3":"3.741245231"},{"1":"potatoes","2":"7","3":"2.802075833"},{"1":"pumpkins","2":"2","3":"30.119559426"},{"1":"pumpkins","2":"3","3":"31.856802350"},{"1":"pumpkins","2":"7","3":"92.688964789"},{"1":"radish","2":"1","3":"0.081571051"},{"1":"radish","2":"2","3":"0.196211447"},{"1":"radish","2":"3","3":"0.094798789"},{"1":"radish","2":"5","3":"0.147709741"},{"1":"radish","2":"6","3":"0.194006824"},{"1":"radish","2":"7","3":"0.231485415"},{"1":"raspberries","2":"2","3":"0.130072757"},{"1":"raspberries","2":"3","3":"0.335102696"},{"1":"raspberries","2":"5","3":"0.288805613"},{"1":"raspberries","2":"6","3":"0.570997357"},{"1":"raspberries","2":"7","3":"0.533518766"},{"1":"rutabaga","2":"1","3":"19.263995774"},{"1":"rutabaga","2":"6","3":"3.578103129"},{"1":"rutabaga","2":"7","3":"6.898265367"},{"1":"spinach","2":"1","3":"0.487221683"},{"1":"spinach","2":"2","3":"0.147709741"},{"1":"spinach","2":"3","3":"0.496040175"},{"1":"spinach","2":"4","3":"0.213848431"},{"1":"spinach","2":"5","3":"0.233690038"},{"1":"spinach","2":"6","3":"0.196211447"},{"1":"spinach","2":"7","3":"0.260145514"},{"1":"squash","2":"2","3":"24.334628674"},{"1":"squash","2":"3","3":"18.468126871"},{"1":"squash","2":"7","3":"56.222295746"},{"1":"strawberries","2":"1","3":"0.081571051"},{"1":"strawberries","2":"2","3":"0.478403191"},{"1":"strawberries","2":"5","3":"0.088184920"},{"1":"strawberries","2":"6","3":"0.487221683"},{"1":"strawberries","2":"7","3":"0.169755971"},{"1":"Swiss chard","2":"1","3":"1.247816618"},{"1":"Swiss chard","2":"2","3":"1.073651401"},{"1":"Swiss chard","2":"3","3":"0.070547936"},{"1":"Swiss chard","2":"4","3":"0.908304676"},{"1":"Swiss chard","2":"5","3":"2.231078476"},{"1":"Swiss chard","2":"6","3":"0.617294440"},{"1":"Swiss chard","2":"7","3":"0.734139459"},{"1":"tomatoes","2":"1","3":"75.609750408"},{"1":"tomatoes","2":"2","3":"11.492699699"},{"1":"tomatoes","2":"3","3":"48.750828399"},{"1":"tomatoes","2":"4","3":"58.265981267"},{"1":"tomatoes","2":"5","3":"34.517782311"},{"1":"tomatoes","2":"6","3":"85.076401570"},{"1":"tomatoes","2":"7","3":"35.126258259"},{"1":"zucchini","2":"1","3":"12.235657650"},{"1":"zucchini","2":"2","3":"12.195974436"},{"1":"zucchini","2":"3","3":"16.468533810"},{"1":"zucchini","2":"4","3":"2.041480898"},{"1":"zucchini","2":"5","3":"34.630218084"},{"1":"zucchini","2":"6","3":"18.721658516"},{"1":"zucchini","2":"7","3":"3.414961027"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>

  2. Summarize the `garden_harvest` data to find the total harvest in pound for each vegetable variety and then try adding the plot from the `garden_planting` table. This will not turn out perfectly. What is the problem? How might you fix it?


```r
garden_harvest %>% 
  mutate(weight_lbs = weight * 0.002204623) %>% 
  group_by(variety) %>% 
  summarize(sum(weight_lbs)) %>% 
  left_join(garden_planting,
            by = c("variety")) %>% 
  select(-vegetable,
         -number_seeds_planted,
         -date,
         -number_seeds_exact,
         -notes)
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["variety"],"name":[1],"type":["chr"],"align":["left"]},{"label":["sum(weight_lbs)"],"name":[2],"type":["dbl"],"align":["right"]},{"label":["plot"],"name":[3],"type":["chr"],"align":["left"]}],"data":[{"1":"Amish Paste","2":"65.67351455","3":"J"},{"1":"Amish Paste","2":"65.67351455","3":"N"},{"1":"asparagus","2":"0.04409246","3":"NA"},{"1":"Better Boy","2":"34.00851440","3":"J"},{"1":"Better Boy","2":"34.00851440","3":"N"},{"1":"Big Beef","2":"24.99381095","3":"N"},{"1":"Black Krim","2":"15.80714691","3":"N"},{"1":"Blue (saved)","2":"41.52407421","3":"A"},{"1":"Blue (saved)","2":"41.52407421","3":"B"},{"1":"Bolero","2":"8.29158710","3":"H"},{"1":"Bolero","2":"8.29158710","3":"L"},{"1":"Bonny Best","2":"24.92326302","3":"J"},{"1":"Brandywine","2":"15.64620943","3":"J"},{"1":"Bush Bush Slender","2":"22.13000567","3":"M"},{"1":"Bush Bush Slender","2":"22.13000567","3":"D"},{"1":"Catalina","2":"2.03486703","3":"H"},{"1":"Catalina","2":"2.03486703","3":"E"},{"1":"Cherokee Purple","2":"15.71234812","3":"J"},{"1":"Chinese Red Noodle","2":"0.78484579","3":"K"},{"1":"Chinese Red Noodle","2":"0.78484579","3":"L"},{"1":"cilantro","2":"0.11464040","3":"potD"},{"1":"cilantro","2":"0.11464040","3":"E"},{"1":"Cinderella's Carraige","2":"32.87313355","3":"B"},{"1":"Classic Slenderette","2":"3.60455861","3":"E"},{"1":"Crispy Colors Duo","2":"0.42108299","3":"front"},{"1":"delicata","2":"10.49841473","3":"K"},{"1":"Delicious Duo","2":"0.75398107","3":"P"},{"1":"Dorinny Sweet","2":"11.40671940","3":"A"},{"1":"Dragon","2":"4.10500803","3":"H"},{"1":"Dragon","2":"4.10500803","3":"L"},{"1":"edamame","2":"6.09137335","3":"O"},{"1":"Farmer's Market Blend","2":"3.80297468","3":"C"},{"1":"Farmer's Market Blend","2":"3.80297468","3":"L"},{"1":"Garden Party Mix","2":"0.94578327","3":"C"},{"1":"Garden Party Mix","2":"0.94578327","3":"G"},{"1":"Garden Party Mix","2":"0.94578327","3":"H"},{"1":"giant","2":"9.87230179","3":"L"},{"1":"Golden Bantam","2":"1.60276092","3":"B"},{"1":"Gourmet Golden","2":"7.02172426","3":"H"},{"1":"grape","2":"32.39473036","3":"O"},{"1":"green","2":"5.69233659","3":"K"},{"1":"green","2":"5.69233659","3":"O"},{"1":"greens","2":"0.37258129","3":"NA"},{"1":"Heirloom Lacinto","2":"5.94586823","3":"P"},{"1":"Heirloom Lacinto","2":"5.94586823","3":"front"},{"1":"Improved Helenor","2":"29.74036427","3":"E"},{"1":"Isle of Naxos","2":"1.08026527","3":"potB"},{"1":"Jet Star","2":"15.02450575","3":"N"},{"1":"King Midas","2":"4.09618953","3":"H"},{"1":"King Midas","2":"4.09618953","3":"L"},{"1":"leaves","2":"0.22266692","3":"NA"},{"1":"Lettuce Mixture","2":"4.74875794","3":"G"},{"1":"Long Keeping Rainbow","2":"3.31134375","3":"H"},{"1":"Magnolia Blossom","2":"7.45823961","3":"B"},{"1":"Main Crop Bravado","2":"2.13187044","3":"D"},{"1":"Main Crop Bravado","2":"2.13187044","3":"I"},{"1":"Mortgage Lifter","2":"26.32540324","3":"J"},{"1":"Mortgage Lifter","2":"26.32540324","3":"N"},{"1":"mustard greens","2":"0.05070633","3":"NA"},{"1":"Neon Glow","2":"6.88283301","3":"M"},{"1":"New England Sugar","2":"44.85966880","3":"K"},{"1":"Old German","2":"26.71782614","3":"J"},{"1":"perrenial","2":"3.18127099","3":"NA"},{"1":"pickling","2":"43.60964756","3":"L"},{"1":"purple","2":"3.00931039","3":"D"},{"1":"red","2":"4.43349685","3":"I"},{"1":"Red Kuri","2":"22.73186775","3":"A"},{"1":"Red Kuri","2":"22.73186775","3":"B"},{"1":"Red Kuri","2":"22.73186775","3":"side"},{"1":"reseed","2":"0.09920803","3":"NA"},{"1":"Romanesco","2":"99.70848442","3":"D"},{"1":"Russet","2":"9.09186525","3":"D"},{"1":"saved","2":"76.93252421","3":"B"},{"1":"Super Sugar Snap","2":"9.56806382","3":"A"},{"1":"Sweet Merlin","2":"6.38679283","3":"H"},{"1":"Tatsoi","2":"2.89467000","3":"P"},{"1":"thai","2":"0.14770974","3":"potB"},{"1":"unknown","2":"0.34392119","3":"NA"},{"1":"variety","2":"4.97142487","3":"potA"},{"1":"variety","2":"4.97142487","3":"potA"},{"1":"variety","2":"4.97142487","3":"potC"},{"1":"variety","2":"4.97142487","3":"potD"},{"1":"volunteers","2":"51.61242905","3":"N"},{"1":"volunteers","2":"51.61242905","3":"J"},{"1":"volunteers","2":"51.61242905","3":"front"},{"1":"volunteers","2":"51.61242905","3":"O"},{"1":"Waltham Butternut","2":"24.27069461","3":"A"},{"1":"Waltham Butternut","2":"24.27069461","3":"K"},{"1":"yellow","2":"7.40091941","3":"I"},{"1":"yellow","2":"7.40091941","3":"I"},{"1":"Yod Fah","2":"0.82011976","3":"P"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>
One issue is that varieties that have been planted in more than one plot record the same weight in pounds for both plots which is potentially misleading. It might be possible to use the number of seeds planted in each plot for each variety to get an approximate ratio for how much was harvested from each plot. However, this likely isn't the most accurate solution.


  3. I would like to understand how much money I "saved" by gardening, for each vegetable type. Describe how I could use the `garden_harvest` and `garden_spending` datasets, along with data from somewhere like [this](https://products.wholefoodsmarket.com/search?sort=relevance&store=10542) to answer this question. You can answer this in words, referencing various join functions. You don't need R code but could provide some if it's helpful.
  
 The first step would be to full join `garden_harvest` and `garden_spending` since not every vegetable appears in the `garden_spending` dataset. Next, I would semi join this larger dataset with the grocery store prices dataset so that only vegetables that are being grown appear. It would probably be useful to remove any columns that are not relevant to the current problem like 'brand' for ease of use. Then you could use mutate to create variables that represent the cost for each vegetable per gram at the store and each vegetable grown. The latter variable would have to take into account the number of seeds planted as well as the total harvested weight for each vegetable. 

  4. Subset the data to tomatoes. Reorder the tomato varieties from smallest to largest first harvest date. Create a barplot of total harvest in pounds for each variety, in the new order.CHALLENGE: add the date near the end of the bar. (This is probably not a super useful graph because it's difficult to read. This is more an exercise in using some of the functions you just learned.)


```r
garden_harvest %>% 
  filter(vegetable == "tomatoes") %>% 
  mutate(weight_lbs = weight * 0.002204623) %>% 
  group_by(variety) %>% 
  mutate(total_harvest_lbs = sum(weight_lbs)) %>% 
  filter(date == min(date)) %>%
  ggplot(aes(x = total_harvest_lbs, y = reorder(variety, rev(+date)))) + #order variety by largest to smallest date in reverse
  geom_bar(stat = "identity")
```

![](03_exercises_files/figure-html/unnamed-chunk-3-1.png)<!-- -->

  5. In the `garden_harvest` data, create two new variables: one that makes the varieties lowercase and another that finds the length of the variety name. Arrange the data by vegetable and length of variety name (smallest to largest), with one row for each vegetable variety. HINT: use `str_to_lower()`, `str_length()`, and `distinct()`.
  

```r
garden_harvest %>% 
  mutate(variety_lc = str_to_lower(variety)) %>% 
  mutate(variety_length = str_length(variety)) %>%
  arrange(vegetable,
          variety_length) %>% 
  distinct(variety,
           .keep_all = TRUE)
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["vegetable"],"name":[1],"type":["chr"],"align":["left"]},{"label":["variety"],"name":[2],"type":["chr"],"align":["left"]},{"label":["date"],"name":[3],"type":["date"],"align":["right"]},{"label":["weight"],"name":[4],"type":["dbl"],"align":["right"]},{"label":["units"],"name":[5],"type":["chr"],"align":["left"]},{"label":["variety_lc"],"name":[6],"type":["chr"],"align":["left"]},{"label":["variety_length"],"name":[7],"type":["int"],"align":["right"]}],"data":[{"1":"apple","2":"unknown","3":"2020-09-26","4":"156","5":"grams","6":"unknown","7":"7"},{"1":"asparagus","2":"asparagus","3":"2020-06-20","4":"20","5":"grams","6":"asparagus","7":"9"},{"1":"basil","2":"Isle of Naxos","3":"2020-06-23","4":"5","5":"grams","6":"isle of naxos","7":"13"},{"1":"beans","2":"Bush Bush Slender","3":"2020-07-06","4":"235","5":"grams","6":"bush bush slender","7":"17"},{"1":"beans","2":"Chinese Red Noodle","3":"2020-08-08","4":"108","5":"grams","6":"chinese red noodle","7":"18"},{"1":"beans","2":"Classic Slenderette","3":"2020-08-05","4":"41","5":"grams","6":"classic slenderette","7":"19"},{"1":"beets","2":"leaves","3":"2020-06-11","4":"8","5":"grams","6":"leaves","7":"6"},{"1":"beets","2":"Sweet Merlin","3":"2020-07-07","4":"10","5":"grams","6":"sweet merlin","7":"12"},{"1":"beets","2":"Gourmet Golden","3":"2020-07-07","4":"62","5":"grams","6":"gourmet golden","7":"14"},{"1":"broccoli","2":"Yod Fah","3":"2020-07-27","4":"372","5":"grams","6":"yod fah","7":"7"},{"1":"broccoli","2":"Main Crop Bravado","3":"2020-09-09","4":"102","5":"grams","6":"main crop bravado","7":"17"},{"1":"carrots","2":"Dragon","3":"2020-07-24","4":"80","5":"grams","6":"dragon","7":"6"},{"1":"carrots","2":"Bolero","3":"2020-07-30","4":"116","5":"grams","6":"bolero","7":"6"},{"1":"carrots","2":"greens","3":"2020-08-29","4":"169","5":"grams","6":"greens","7":"6"},{"1":"carrots","2":"King Midas","3":"2020-07-23","4":"56","5":"grams","6":"king midas","7":"10"},{"1":"chives","2":"perrenial","3":"2020-06-17","4":"8","5":"grams","6":"perrenial","7":"9"},{"1":"cilantro","2":"cilantro","3":"2020-06-23","4":"2","5":"grams","6":"cilantro","7":"8"},{"1":"corn","2":"Dorinny Sweet","3":"2020-08-11","4":"330","5":"grams","6":"dorinny sweet","7":"13"},{"1":"corn","2":"Golden Bantam","3":"2020-08-15","4":"383","5":"grams","6":"golden bantam","7":"13"},{"1":"cucumbers","2":"pickling","3":"2020-07-08","4":"181","5":"grams","6":"pickling","7":"8"},{"1":"edamame","2":"edamame","3":"2020-08-11","4":"109","5":"grams","6":"edamame","7":"7"},{"1":"hot peppers","2":"thai","3":"2020-07-20","4":"12","5":"grams","6":"thai","7":"4"},{"1":"hot peppers","2":"variety","3":"2020-07-20","4":"559","5":"grams","6":"variety","7":"7"},{"1":"jalapeño","2":"giant","3":"2020-07-17","4":"20","5":"grams","6":"giant","7":"5"},{"1":"kale","2":"Heirloom Lacinto","3":"2020-06-13","4":"10","5":"grams","6":"heirloom lacinto","7":"16"},{"1":"kohlrabi","2":"Crispy Colors Duo","3":"2020-09-17","4":"191","5":"grams","6":"crispy colors duo","7":"17"},{"1":"lettuce","2":"reseed","3":"2020-06-06","4":"20","5":"grams","6":"reseed","7":"6"},{"1":"lettuce","2":"Tatsoi","3":"2020-06-20","4":"18","5":"grams","6":"tatsoi","7":"6"},{"1":"lettuce","2":"mustard greens","3":"2020-06-29","4":"23","5":"grams","6":"mustard greens","7":"14"},{"1":"lettuce","2":"Lettuce Mixture","3":"2020-07-22","4":"23","5":"grams","6":"lettuce mixture","7":"15"},{"1":"lettuce","2":"Farmer's Market Blend","3":"2020-06-11","4":"12","5":"grams","6":"farmer's market blend","7":"21"},{"1":"onions","2":"Delicious Duo","3":"2020-07-16","4":"50","5":"grams","6":"delicious duo","7":"13"},{"1":"onions","2":"Long Keeping Rainbow","3":"2020-07-20","4":"102","5":"grams","6":"long keeping rainbow","7":"20"},{"1":"peas","2":"Magnolia Blossom","3":"2020-06-17","4":"8","5":"grams","6":"magnolia blossom","7":"16"},{"1":"peas","2":"Super Sugar Snap","3":"2020-06-17","4":"121","5":"grams","6":"super sugar snap","7":"16"},{"1":"peppers","2":"green","3":"2020-08-04","4":"81","5":"grams","6":"green","7":"5"},{"1":"potatoes","2":"red","3":"2020-10-15","4":"1718","5":"grams","6":"red","7":"3"},{"1":"potatoes","2":"purple","3":"2020-08-06","4":"317","5":"grams","6":"purple","7":"6"},{"1":"potatoes","2":"yellow","3":"2020-08-06","4":"439","5":"grams","6":"yellow","7":"6"},{"1":"potatoes","2":"Russet","3":"2020-09-16","4":"629","5":"grams","6":"russet","7":"6"},{"1":"pumpkins","2":"saved","3":"2020-09-01","4":"4758","5":"grams","6":"saved","7":"5"},{"1":"pumpkins","2":"New England Sugar","3":"2020-09-19","4":"1109","5":"grams","6":"new england sugar","7":"17"},{"1":"pumpkins","2":"Cinderella's Carraige","3":"2020-09-01","4":"7350","5":"grams","6":"cinderella's carraige","7":"21"},{"1":"radish","2":"Garden Party Mix","3":"2020-06-06","4":"36","5":"grams","6":"garden party mix","7":"16"},{"1":"rutabaga","2":"Improved Helenor","3":"2020-10-16","4":"883","5":"grams","6":"improved helenor","7":"16"},{"1":"spinach","2":"Catalina","3":"2020-06-11","4":"9","5":"grams","6":"catalina","7":"8"},{"1":"squash","2":"delicata","3":"2020-09-19","4":"307","5":"grams","6":"delicata","7":"8"},{"1":"squash","2":"Red Kuri","3":"2020-09-19","4":"1178","5":"grams","6":"red kuri","7":"8"},{"1":"squash","2":"Blue (saved)","3":"2020-09-01","4":"3227","5":"grams","6":"blue (saved)","7":"12"},{"1":"squash","2":"Waltham Butternut","3":"2020-09-19","4":"1834","5":"grams","6":"waltham butternut","7":"17"},{"1":"Swiss chard","2":"Neon Glow","3":"2020-06-21","4":"19","5":"grams","6":"neon glow","7":"9"},{"1":"tomatoes","2":"grape","3":"2020-07-11","4":"24","5":"grams","6":"grape","7":"5"},{"1":"tomatoes","2":"Big Beef","3":"2020-07-21","4":"137","5":"grams","6":"big beef","7":"8"},{"1":"tomatoes","2":"Jet Star","3":"2020-07-28","4":"315","5":"grams","6":"jet star","7":"8"},{"1":"tomatoes","2":"Bonny Best","3":"2020-07-21","4":"339","5":"grams","6":"bonny best","7":"10"},{"1":"tomatoes","2":"Better Boy","3":"2020-07-24","4":"220","5":"grams","6":"better boy","7":"10"},{"1":"tomatoes","2":"Old German","3":"2020-07-28","4":"611","5":"grams","6":"old german","7":"10"},{"1":"tomatoes","2":"Brandywine","3":"2020-08-01","4":"320","5":"grams","6":"brandywine","7":"10"},{"1":"tomatoes","2":"Black Krim","3":"2020-08-01","4":"436","5":"grams","6":"black krim","7":"10"},{"1":"tomatoes","2":"volunteers","3":"2020-08-04","4":"73","5":"grams","6":"volunteers","7":"10"},{"1":"tomatoes","2":"Amish Paste","3":"2020-07-25","4":"463","5":"grams","6":"amish paste","7":"11"},{"1":"tomatoes","2":"Cherokee Purple","3":"2020-07-24","4":"247","5":"grams","6":"cherokee purple","7":"15"},{"1":"tomatoes","2":"Mortgage Lifter","3":"2020-07-27","4":"801","5":"grams","6":"mortgage lifter","7":"15"},{"1":"zucchini","2":"Romanesco","3":"2020-07-06","4":"175","5":"grams","6":"romanesco","7":"9"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>

  6. In the `garden_harvest` data, find all distinct vegetable varieties that have "er" or "ar" in their name. HINT: `str_detect()` with an "or" statement (use the | for "or") and `distinct()`.


```r
garden_harvest %>% 
  mutate(has_er_ar = str_detect(variety,
                                "er|ar")) %>% 
  distinct(variety)
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["variety"],"name":[1],"type":["chr"],"align":["left"]}],"data":[{"1":"reseed"},{"1":"Garden Party Mix"},{"1":"Farmer's Market Blend"},{"1":"Catalina"},{"1":"leaves"},{"1":"Heirloom Lacinto"},{"1":"Magnolia Blossom"},{"1":"Super Sugar Snap"},{"1":"perrenial"},{"1":"Tatsoi"},{"1":"asparagus"},{"1":"Neon Glow"},{"1":"cilantro"},{"1":"Isle of Naxos"},{"1":"mustard greens"},{"1":"Romanesco"},{"1":"Bush Bush Slender"},{"1":"Gourmet Golden"},{"1":"Sweet Merlin"},{"1":"pickling"},{"1":"grape"},{"1":"Delicious Duo"},{"1":"giant"},{"1":"thai"},{"1":"variety"},{"1":"Long Keeping Rainbow"},{"1":"Big Beef"},{"1":"Bonny Best"},{"1":"Lettuce Mixture"},{"1":"King Midas"},{"1":"Cherokee Purple"},{"1":"Better Boy"},{"1":"Dragon"},{"1":"Amish Paste"},{"1":"Mortgage Lifter"},{"1":"Yod Fah"},{"1":"Old German"},{"1":"Jet Star"},{"1":"Bolero"},{"1":"Brandywine"},{"1":"Black Krim"},{"1":"volunteers"},{"1":"green"},{"1":"Classic Slenderette"},{"1":"purple"},{"1":"yellow"},{"1":"Chinese Red Noodle"},{"1":"edamame"},{"1":"Dorinny Sweet"},{"1":"Golden Bantam"},{"1":"greens"},{"1":"saved"},{"1":"Blue (saved)"},{"1":"Cinderella's Carraige"},{"1":"Main Crop Bravado"},{"1":"Russet"},{"1":"Crispy Colors Duo"},{"1":"delicata"},{"1":"Waltham Butternut"},{"1":"Red Kuri"},{"1":"New England Sugar"},{"1":"unknown"},{"1":"red"},{"1":"Improved Helenor"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
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

**NOTE:** The `Trips` data table is a random subset of 10,000 trips from the full quarterly data. Start with this small data table to develop your analysis commands. **When you have this working well, you should access the full data set of more than 600,000 events by removing `-Small` from the name of the `data_site`.**

### Temporal patterns

It's natural to expect that bikes are rented more at some times of day, some days of the week, some months of the year than others. The variable `sdate` gives the time (including the date) that the rental started. Make the following plots and interpret them:

  7. A density plot, which is a smoothed out histogram, of the events versus `sdate`. Use `geom_density()`.
  

```r
Trips %>% 
  ggplot(aes(x = sdate)) +
  geom_density()
```

![](03_exercises_files/figure-html/unnamed-chunk-7-1.png)<!-- -->
  There are more rentals during October and November than there are during December. During the time when one would expect temperatures to drop, people seem to rent bikes less.
  
  
  8. A density plot of the events versus time of day.  You can use `mutate()` with `lubridate`'s  `hour()` and `minute()` functions to extract the hour of the day and minute within the hour from `sdate`. Hint: A minute is 1/60 of an hour, so create a variable where 3:30 is 3.5 and 3:45 is 3.75.
  

```r
Trips %>% 
  mutate(shour = hour(sdate)) %>% 
  mutate(sminute = minute(sdate)/60) %>% 
  mutate(time_of_day = shour + sminute) %>% 
  ggplot(aes(x = time_of_day)) +
  geom_density()
```

![](03_exercises_files/figure-html/unnamed-chunk-8-1.png)<!-- -->
  There are two peaks early in the morning and late in the afternoon. People rent the least amount of bikes during the middle of the night.
  
  
  9. A bar graph of the events versus day of the week. Put day on the y-axis.
  

```r
Trips %>% 
  mutate(day_of_week = wday(sdate)) %>% 
  ggplot(aes(y = day_of_week)) +
  geom_bar()
```

![](03_exercises_files/figure-html/unnamed-chunk-9-1.png)<!-- -->
  There seems to be a fairly even spread across the week for rentals though there are the least amount of rentals during the weekends.
  
  
  10. Facet your graph from exercise 8. by day of the week. Is there a pattern?
  

```r
Trips %>% 
  mutate(day_of_week = wday(sdate)) %>%
  mutate(shour = hour(sdate)) %>% 
  mutate(sminute = minute(sdate)/60) %>% 
  mutate(time_of_day = shour + sminute) %>% 
  ggplot(aes(x = time_of_day)) +
  geom_density() +
  facet_wrap(vars(day_of_week))
```

![](03_exercises_files/figure-html/unnamed-chunk-10-1.png)<!-- -->
  During the weekends, there is only one peak during the afternoon while during the week there are two peaks during the morning and afternoon.
  
  
The variable `client` describes whether the renter is a regular user (level `Registered`) or has not joined the bike-rental organization (`Causal`). The next set of exercises investigate whether these two different categories of users show different rental behavior and how `client` interacts with the patterns you found in the previous exercises. 

  11. Change the graph from exercise 10 to set the `fill` aesthetic for `geom_density()` to the `client` variable. You should also set `alpha = .5` for transparency and `color=NA` to suppress the outline of the density function.
  

```r
Trips %>% 
  mutate(day_of_week = wday(sdate)) %>%
  mutate(shour = hour(sdate)) %>% 
  mutate(sminute = minute(sdate)/60) %>% 
  mutate(time_of_day = shour + sminute) %>% 
  ggplot(aes(x = time_of_day,
             fill = client, 
             alpha = .5)) + #error when `color=NA`
  geom_density() +
  facet_wrap(vars(day_of_week))
```

![](03_exercises_files/figure-html/unnamed-chunk-11-1.png)<!-- -->
Registered riders tend to rent bikes more frequently in the morning and afternoon while casual riders tend to rent bikes only in the afternoon. During the weekends both registered and casual riders seem to rent bikes mainly in the afternoon.


  12. Change the previous graph by adding the argument `position = position_stack()` to `geom_density()`. In your opinion, is this better or worse in terms of telling a story? What are the advantages/disadvantages of each?
  

```r
Trips %>% 
  mutate(day_of_week = wday(sdate)) %>%
  mutate(shour = hour(sdate)) %>% 
  mutate(sminute = minute(sdate)/60) %>% 
  mutate(time_of_day = shour + sminute) %>% 
  ggplot(aes(x = time_of_day,
             fill = client, 
             alpha = .5)) + 
  geom_density(position = position_stack()) +
  facet_wrap(vars(day_of_week))
```

![](03_exercises_files/figure-html/unnamed-chunk-12-1.png)<!-- -->
  I think this graph is worse in terms of telling a story because it is more difficult to understand what the graph is telling us. The advantage of this graph is that we can see the individual patterns of rentals by both types of riders better. However, the disadvantage of this graph is that it could be easily interpreted to say that there are significantly more casual riders than registered riders renting everyday at most times of day and that does not seem to be true.
  
  
  13. In this graph, go back to using the regular density plot (without `position = position_stack()`). Add a new variable to the dataset called `weekend` which will be "weekend" if the day is Saturday or Sunday and  "weekday" otherwise (HINT: use the `ifelse()` function and the `wday()` function from `lubridate`). Then, update the graph from the previous problem by faceting on the new `weekend` variable. 
  

```r
Trips %>% 
  mutate(day_of_week = wday(sdate)) %>%
  mutate(weekend = ifelse(day_of_week == 1|day_of_week == 7, #if day_of_week is 1 or 7 then weekend is True
                          "weekend",
                          "weekday")) %>% 
  mutate(shour = hour(sdate)) %>% 
  mutate(sminute = minute(sdate)/60) %>% 
  mutate(time_of_day = shour + sminute) %>% 
  ggplot(aes(x = time_of_day,
             fill = client, 
             alpha = .5)) + 
  geom_density() +
  facet_wrap(vars(weekend))
```

![](03_exercises_files/figure-html/unnamed-chunk-13-1.png)<!-- -->
  We can see that more casual riders tend to rent bikes during the weekend than registered riders. Casual riders also tend to follow a similar rental pattern no matter the day while registered riders rent more bikes during the morning and and late afternoon during the week but tend to rent more frequently in the afternoon only during the weekend. 
  
  
  14. Change the graph from the previous problem to facet on `client` and fill with `weekday`. What information does this graph tell you that the previous didn't? Is one graph better than the other?
  

```r
Trips %>% 
  mutate(day_of_week = wday(sdate)) %>%
  mutate(weekend = ifelse(day_of_week == 1|day_of_week == 7,
                          "weekend",
                          "weekday")) %>% 
  mutate(shour = hour(sdate)) %>% 
  mutate(sminute = minute(sdate)/60) %>% 
  mutate(time_of_day = shour + sminute) %>% 
  ggplot(aes(x = time_of_day,
             fill = weekend, 
             alpha = .5)) + 
  geom_density() +
  facet_wrap(vars(client))
```

![](03_exercises_files/figure-html/unnamed-chunk-14-1.png)<!-- -->
  This graph gives us similar information as the last one. We can tell definitively that registered riders have higher peaks of rentals during the week than during the weekend while the opposite is true for casual riders. I don't really see one graph as better than the other in this case since the information is very similar and the information that is not explicitly given by either graph can still be gathered just not as easily.
  
  
### Spatial patterns

  15. Use the latitude and longitude variables in `Stations` to make a visualization of the total number of departures from each station in the `Trips` data. Use either color or size to show the variation in number of departures. We will improve this plot next week when we learn about maps!
  

```r
departures <- as.data.frame(table(Trips$sstation)) #obtain frequency of departures from each station

Stations %>% 
  left_join(departures,
            by = c("name" = "Var1")) %>% 
  ggplot(aes(x = lat, y = long, color = Freq)) +
  geom_point()
```

![](03_exercises_files/figure-html/unnamed-chunk-15-1.png)<!-- -->
  
  16. Only 14.4% of the trips in our data are carried out by casual users. Create a plot that shows which area(s) have stations with a much higher percentage of departures by casual users. What patterns do you notice? (Again, we'll improve this next week when we learn about maps).
  

```r
rider_cat_departures <- as.data.frame(table(x = Trips$client,
                                            name = Trips$sstation)) #obtain frequency of departures of each station by each type of rider

rider_cat_departures <- rider_cat_departures %>% 
  pivot_wider(names_from = x, 
              values_from = Freq) %>% #shift type of rider into variables of frequency
  mutate(perc_of_cas_riders = Casual/(Casual+Registered)) #percentage of casual riders departing

Stations %>% 
  left_join(rider_cat_departures,
            by = c("name")) %>% 
  ggplot(aes(x = lat, 
             y = long, 
             size = perc_of_cas_riders,
             color = perc_of_cas_riders, #color and size makes it slightly easier to see rather than having either one individually
             alpha = .5)) +
  geom_point()
```

![](03_exercises_files/figure-html/unnamed-chunk-16-1.png)<!-- -->
 It is difficult to determine a definitive pattern but there definitely seems to be a higher percentage of casual riders departing from around the lower left of the central city.
 
  
**DID YOU REMEMBER TO GO BACK AND CHANGE THIS SET OF EXERCISES TO THE LARGER DATASET? IF NOT, DO THAT NOW.**

## Dogs!

In this section, we'll use the data from 2022-02-01 Tidy Tuesday. If you didn't use that data or need a little refresher on it, see the [website](https://github.com/rfordatascience/tidytuesday/blob/master/data/2022/2022-02-01/readme.md).

  17. The final product of this exercise will be a graph that has breed on the y-axis and the sum of the numeric ratings in the `breed_traits` dataset on the x-axis, with a dot for each rating. First, create a new dataset called `breed_traits_total` that has two variables -- `Breed` and `total_rating`. The `total_rating` variable is the sum of the numeric ratings in the `breed_traits` dataset (we'll use this dataset again in the next problem). Then, create the graph just described. Omit Breeds with a `total_rating` of 0 and order the Breeds from highest to lowest ranked. You may want to adjust the `fig.height` and `fig.width` arguments inside the code chunk options (eg. `{r, fig.height=8, fig.width=4}`) so you can see things more clearly - check this after you knit the file to assure it looks like what you expected.


```r
total_rating <- breed_traits %>% 
  select(-c(`Coat Type`,`Coat Length`,`Breed`)) %>% 
  rowSums()

breed_traits_total <- data.frame(Breed = breed_traits$Breed,total_rating)

breed_traits_total %>% 
  filter(total_rating > 0) %>% 
  ggplot(aes(x = total_rating, y = Breed)) +
  geom_point()
```

![](03_exercises_files/figure-html/unnamed-chunk-17-1.png)<!-- -->

  18. The final product of this exercise will be a graph with the top-20 dogs in total ratings (from previous problem) on the y-axis, year on the x-axis, and points colored by each breed's ranking for that year (from the `breed_rank_all` dataset). The points within each breed will be connected by a line, and the breeds should be arranged from the highest median rank to lowest median rank ("highest" is actually the smallest numer, eg. 1 = best). After you're finished, think of AT LEAST one thing you could you do to make this graph better. HINTS: 1. Start with the `breed_rank_all` dataset and pivot it so year is a variable. 2. Use the `separate()` function to get year alone, and there's an extra argument in that function that can make it numeric. 3. For both datasets used, you'll need to `str_squish()` Breed before joining. 
  

```r
breed_traits_top_20 <- breed_traits_total %>% 
  arrange(desc(total_rating)) %>% 
  head(n = 20) %>% 
  mutate(Breed = str_squish(Breed))

breed_rank_all %>% 
  pivot_longer(cols = starts_with("20"),
               names_to = "year",
               values_to = "rank") %>% 
  separate(year, into = c("year",NA)) %>% 
  mutate(Breed = str_squish(Breed)) %>% 
  left_join(breed_traits_top_20,
            by = "Breed") %>% 
  filter(!is.na(total_rating)) %>% 
  ggplot(aes(x = year, 
             y = reorder(Breed,-rank,median),
             color = rank)) +
  geom_point() #not sure what you mean by points within each breed being connected by a line
```

![](03_exercises_files/figure-html/unnamed-chunk-18-1.png)<!-- -->
  It is difficult to see the change in rank over the years. I would like to use a better color palette or just have another method that makes it easier to see the change in rank. I would also like to move Miniature American Shepherds to the bottom because it basically only has NA values.
  
  
  19. Create your own! Requirements: use a `join` or `pivot` function (or both, if you'd like), a `str_XXX()` function, and a `fct_XXX()` function to create a graph using any of the dog datasets. One suggestion is to try to improve the graph you created for the Tidy Tuesday assignment. If you want an extra challenge, find a way to use the dog images in the `breed_rank_all` file - check out the `ggimage` library and [this resource](https://wilkelab.org/ggtext/) for putting images as labels.
  

```r
breed_rank_all %>% 
  pivot_longer(cols = starts_with("20"),
               names_to = "year",
               values_to = "rank") %>% 
  separate(year, into = c("year",NA)) %>% 
  mutate(Breed = str_squish(Breed)) %>% 
  left_join(breed_traits_top_20,
            by = "Breed") %>% 
  filter(!is.na(total_rating)) %>% 
  ggplot(aes(x = year, 
             y = fct_reorder(Breed,total_rating),
             size = rank)) +
  geom_point()
```

![](03_exercises_files/figure-html/unnamed-chunk-19-1.png)<!-- -->
  
## GitHub link

  20. Below, provide a link to your GitHub page with this set of Weekly Exercises. Specifically, if the name of the file is 03_exercises.Rmd, provide a link to the 03_exercises.md file, which is the one that will be most readable on GitHub.

## Challenge problem! 

This problem uses the data from the Tidy Tuesday competition this week, `kids`. If you need to refresh your memory on the data, read about it [here](https://github.com/rfordatascience/tidytuesday/blob/master/data/2020/2020-09-15/readme.md). 

  21. In this exercise, you are going to try to replicate the graph below, created by Georgios Karamanis. I'm sure you can find the exact code on GitHub somewhere, but **DON'T DO THAT!** You will only be graded for putting an effort into this problem. So, give it a try and see how far you can get without doing too much googling. HINT: use `facet_geo()`. The graphic won't load below since it came from a location on my computer. So, you'll have to reference the original html on the moodle page to see it.

**DID YOU REMEMBER TO UNCOMMENT THE OPTIONS AT THE TOP?**
