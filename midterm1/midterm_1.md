---
title: "Midterm 1"
author: "Julie Trundle"
date: "2022-01-25"
output:
  html_document: 
    theme: spacelab
    keep_md: yes
---



## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your code should be organized, clean, and run free from errors. Remember, you must remove the `#` for any included code chunks to run. Be sure to add your name to the author header above. You may use any resources to answer these questions (including each other), but you may not post questions to Open Stacks or external help sites. There are 15 total questions, each is worth 2 points.  

Make sure to use the formatting conventions of RMarkdown to make your report neat and clean!  

This exam is due by 12:00p on Thursday, January 27.  

## Load the tidyverse
If you plan to use any other libraries to complete this assignment then you should load them here.

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
library(janitor)
```

```
## 
## Attaching package: 'janitor'
```

```
## The following objects are masked from 'package:stats':
## 
##     chisq.test, fisher.test
```

```r
library(skimr)
```

## Questions  
Wikipedia's definition of [data science](https://en.wikipedia.org/wiki/Data_science): "Data science is an interdisciplinary field that uses scientific methods, processes, algorithms and systems to extract knowledge and insights from noisy, structured and unstructured data, and apply knowledge and actionable insights from data across a broad range of application domains."  

1. (2 points) Consider the definition of data science above. Although we are only part-way through the quarter, what specific elements of data science do you feel we have practiced? Provide at least one specific example.  

So far in the course, we have learned to extract certain data from large data sets that are difficult to look at all together. We are able to pull out certain variables and values we want to look at, and organize them into a new table or data frame so it is easier to analyze. 

2. (2 points) What is the most helpful or interesting thing you have learned so far in BIS 15L? What is something that you think needs more work or practice?  

Being able to select only a few variables I want to look at, and grouping them based on the different data entries has been super helpful for getting totals of certain variables. 

In the midterm 1 folder there is a second folder called `data`. Inside the `data` folder, there is a .csv file called `ElephantsMF`. These data are from Phyllis Lee, Stirling University, and are related to Lee, P., et al. (2013), "Enduring consequences of early experiences: 40-year effects on survival and success among African elephants (Loxodonta africana)," Biology Letters, 9: 20130011. [kaggle](https://www.kaggle.com/mostafaelseidy/elephantsmf).  

3. (2 points) Please load these data as a new object called `elephants`. Use the function(s) of your choice to get an idea of the structure of the data. Be sure to show the class of each variable.


```r
elephants <- readr::read_csv(file = "data/ElephantsMF.csv")
```

```
## Rows: 288 Columns: 3
```

```
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr (1): Sex
## dbl (2): Age, Height
```

```
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```


```r
view(elephants)
```


```r
skim(elephants)
```


Table: Data summary

|                         |          |
|:------------------------|:---------|
|Name                     |elephants |
|Number of rows           |288       |
|Number of columns        |3         |
|_______________________  |          |
|Column type frequency:   |          |
|character                |1         |
|numeric                  |2         |
|________________________ |          |
|Group variables          |None      |


**Variable type: character**

|skim_variable | n_missing| complete_rate| min| max| empty| n_unique| whitespace|
|:-------------|---------:|-------------:|---:|---:|-----:|--------:|----------:|
|Sex           |         0|             1|   1|   1|     0|        2|          0|


**Variable type: numeric**

|skim_variable | n_missing| complete_rate|   mean|   sd|    p0|    p25|    p50|    p75|   p100|hist  |
|:-------------|---------:|-------------:|------:|----:|-----:|------:|------:|------:|------:|:-----|
|Age           |         0|             1|  10.97|  8.4|  0.01|   4.58|   9.46|  16.50|  32.17|▆▇▂▂▂ |
|Height        |         0|             1| 187.68| 50.6| 75.46| 160.75| 200.00| 221.09| 304.06|▃▃▇▇▁ |


```r
glimpse(elephants)
```

```
## Rows: 288
## Columns: 3
## $ Age    <dbl> 1.40, 17.50, 12.75, 11.17, 12.67, 12.67, 12.25, 12.17, 28.17, 1…
## $ Height <dbl> 120.00, 227.00, 235.00, 210.00, 220.00, 189.00, 225.00, 204.00,…
## $ Sex    <chr> "M", "M", "M", "M", "M", "M", "M", "M", "M", "M", "M", "M", "M"…
```

4. (2 points) Change the names of the variables to lower case and change the class of the variable `sex` to a factor.


```r
elephants <- clean_names(elephants)
names(elephants)
```

```
## [1] "age"    "height" "sex"
```


```r
elephants$sex <- as.factor(elephants$sex)
class(elephants$sex)
```

```
## [1] "factor"
```

5. (2 points) How many male and female elephants are represented in the data?


```r
tabyl(elephants, sex)
```

```
##  sex   n   percent
##    F 150 0.5208333
##    M 138 0.4791667
```

There are 150 females and 138 males represented in the data. 

6. (2 points) What is the average age all elephants in the data?


```r
mean(elephants$age)
```

```
## [1] 10.97132
```

Interesting because I just went up to check if this was in one of the summaries I did earlier, and in the skimr function, it tells me the mean age is 11. So I know this answer is correct and is just more accurate than the mean that is given in skimr. 

7. (2 points) How does the average age and height of elephants compare by sex?


```r
elephants %>%
  group_by(sex) %>%
  summarise(mean_age=mean(age),
            mean_height=mean(height))
```

```
## # A tibble: 2 × 3
##   sex   mean_age mean_height
##   <fct>    <dbl>       <dbl>
## 1 F        12.8         190.
## 2 M         8.95        185.
```
The female elephants both have a higher mean age and weight in this particular data set. 

8. (2 points) How does the average height of elephants compare by sex for individuals over 20 years old. Include the min and max height as well as the number of individuals in the sample as part of your analysis.  


```r
elephants %>%
  group_by(sex) %>%
  filter(age>20) %>%
  summarise(mean_height=mean(height),
            min_height=min(height),
            max_height=max(height),
            number_individuals=n())
```

```
## # A tibble: 2 × 5
##   sex   mean_height min_height max_height number_individuals
##   <fct>       <dbl>      <dbl>      <dbl>              <int>
## 1 F            232.       193.       278.                 37
## 2 M            270.       229.       304.                 13
```
For elephants over 20 years old, the mean height is higher in males than in females. However, there are less males over 20 years old than there are females. 

For the next series of questions, we will use data from a study on vertebrate community composition and impacts from defaunation in [Gabon, Africa](https://en.wikipedia.org/wiki/Gabon). One thing to notice is that the data include 24 separate transects. Each transect represents a path through different forest management areas.  

Reference: Koerner SE, Poulsen JR, Blanchard EJ, Okouyi J, Clark CJ. Vertebrate community composition and diversity declines along a defaunation gradient radiating from rural villages in Gabon. _Journal of Applied Ecology_. 2016. This paper, along with a description of the variables is included inside the midterm 1 folder.  

9. (2 points) Load `IvindoData_DryadVersion.csv` and use the function(s) of your choice to get an idea of the overall structure. Change the variables `HuntCat` and `LandUse` to factors.


```r
vertebrates <- readr::read_csv(file = "data/IvindoData_DryadVersion.csv")
```

```
## Rows: 24 Columns: 26
```

```
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr  (2): HuntCat, LandUse
## dbl (24): TransectID, Distance, NumHouseholds, Veg_Rich, Veg_Stems, Veg_lian...
```

```
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```


```r
view(vertebrates)
```


```r
vertebrates <- clean_names(vertebrates)
vertebrates$hunt_cat <- as.factor(vertebrates$hunt_cat)
vertebrates$land_use <- as.factor(vertebrates$land_use)
names(vertebrates)
```

```
##  [1] "transect_id"              "distance"                
##  [3] "hunt_cat"                 "num_households"          
##  [5] "land_use"                 "veg_rich"                
##  [7] "veg_stems"                "veg_liana"               
##  [9] "veg_dbh"                  "veg_canopy"              
## [11] "veg_understory"           "ra_apes"                 
## [13] "ra_birds"                 "ra_elephant"             
## [15] "ra_monkeys"               "ra_rodent"               
## [17] "ra_ungulate"              "rich_all_species"        
## [19] "evenness_all_species"     "diversity_all_species"   
## [21] "rich_bird_species"        "evenness_bird_species"   
## [23] "diversity_bird_species"   "rich_mammal_species"     
## [25] "evenness_mammal_species"  "diversity_mammal_species"
```


```r
class(vertebrates$hunt_cat)
```

```
## [1] "factor"
```

```r
class(vertebrates$land_use)
```

```
## [1] "factor"
```

10. (4 points) For the transects with high and moderate hunting intensity, how does the average diversity of birds and mammals compare?


```r
vertebrates %>%
  group_by(hunt_cat) %>%
  filter(hunt_cat=="High"|hunt_cat=="Moderate") %>% 
  summarise(mean_diversity_birds=mean(diversity_bird_species),
            mean_diversity_mammals=mean(diversity_mammal_species))
```

```
## # A tibble: 2 × 3
##   hunt_cat mean_diversity_birds mean_diversity_mammals
##   <fct>                   <dbl>                  <dbl>
## 1 High                     1.66                   1.74
## 2 Moderate                 1.62                   1.68
```
The average diversity of mammals is higher than that of birds in both high and moderate hunting intensity categories. 

11. (4 points) One of the conclusions in the study is that the relative abundance of animals drops off the closer you get to a village. Let's try to reconstruct this (without the statistics). How does the relative abundance (RA) of apes, birds, elephants, monkeys, rodents, and ungulates compare between sites that are less than 3km from a village to sites that are greater than 25km from a village? The variable `Distance` measures the distance of the transect from the nearest village. Hint: try using the `across` operator.  


```r
names(vertebrates)
```

```
##  [1] "transect_id"              "distance"                
##  [3] "hunt_cat"                 "num_households"          
##  [5] "land_use"                 "veg_rich"                
##  [7] "veg_stems"                "veg_liana"               
##  [9] "veg_dbh"                  "veg_canopy"              
## [11] "veg_understory"           "ra_apes"                 
## [13] "ra_birds"                 "ra_elephant"             
## [15] "ra_monkeys"               "ra_rodent"               
## [17] "ra_ungulate"              "rich_all_species"        
## [19] "evenness_all_species"     "diversity_all_species"   
## [21] "rich_bird_species"        "evenness_bird_species"   
## [23] "diversity_bird_species"   "rich_mammal_species"     
## [25] "evenness_mammal_species"  "diversity_mammal_species"
```


```r
vertebrates %>% 
  select(distance, ra_apes, ra_birds, ra_elephant, ra_monkeys, ra_rodent, ra_ungulate) %>%
  filter(distance<3|distance>25) %>% 
  arrange(desc(distance))
```

```
## # A tibble: 3 × 7
##   distance ra_apes ra_birds ra_elephant ra_monkeys ra_rodent ra_ungulate
##      <dbl>   <dbl>    <dbl>       <dbl>      <dbl>     <dbl>       <dbl>
## 1    26.8     4.91     31.6        0         54.1       1.29        8.12
## 2     2.92    0.24     68.2        0         25.6       4.05        1.88
## 3     2.7     0        85.0        0.29       9.09      3.74        1.86
```
The conclusion that abundance drops off the closer you get to a village holds true for some of the animals, such as apes, monkeys, and ungulates. However, birds, elephants, and rodents do not follow the same pattern and seem to generally increase in abundance as they get closer to villages. 

12. (4 points) Based on your interest, do one exploratory analysis on the `gabon` data of your choice. This analysis needs to include a minimum of two functions in `dplyr.`


```r
vertebrates %>% 
  group_by(land_use) %>% 
  summarise(diversity_bird_species_mean=mean(diversity_bird_species),
            diversity_mammal_species_mean=mean(diversity_mammal_species))
```

```
## # A tibble: 3 × 3
##   land_use diversity_bird_species_mean diversity_mammal_species_mean
##   <fct>                          <dbl>                         <dbl>
## 1 Logging                         1.56                          1.68
## 2 Neither                         1.81                          1.66
## 3 Park                            1.77                          1.75
```
Birds have the highest diversity in neither logging or park land use, but mammals have the highest diversity in park land use.
