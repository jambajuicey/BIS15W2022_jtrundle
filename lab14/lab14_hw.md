---
title: "Lab 14 HW"
output: 
  html_document: 
    keep_md: yes
author: "Julie Trundle"
date: '2022-02-28'
---


### Load the Libraries

```r
library(tidyverse)
```

```
## ── Attaching packages ─────────────────────────────────────── tidyverse 1.3.1 ──
```

```
## ✓ ggplot2 3.3.5     ✓ purrr   0.3.4
## ✓ tibble  3.1.6     ✓ dplyr   1.0.8
## ✓ tidyr   1.2.0     ✓ stringr 1.4.0
## ✓ readr   2.1.2     ✓ forcats 0.5.1
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
library(dplyr)
library("palmerpenguins")

#install.packages("ggVennDiagram")
library(ggVennDiagram)
library(RColorBrewer)

#install.packages("ggworldcloud")
library(ggwordcloud)


options(scipen=999) #cancels the use of scientific notation for the session
```


### Data

```r
superhero_info <- readr::read_csv("data/heroes_information.csv", na = c("", "-99", "-"))
```

```
## Rows: 734 Columns: 10
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr (8): name, Gender, Eye color, Race, Hair color, Publisher, Skin color, A...
## dbl (2): Height, Weight
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

```r
superhero_powers <- readr::read_csv("data/super_hero_powers.csv", na = c("", "-99", "-"))
```

```
## Rows: 667 Columns: 168
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr   (1): hero_names
## lgl (167): Agility, Accelerated Healing, Lantern Power Ring, Dimensional Awa...
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

```r
beachbugs_long <- readr::read_csv("data/beachbugs_long.csv")
```

```
## Rows: 66 Columns: 3
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr (1): site
## dbl (2): year, buglevels
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

### 1. 
Clean up the column names (no capitals, not spaces) of `superhero_info`, then use 2 functions to remind yourself of structure of the `superhero_info` data set.


```r
superhero_info <- clean_names(superhero_info)
```


```r
names(superhero_info)
```

```
##  [1] "name"       "gender"     "eye_color"  "race"       "hair_color"
##  [6] "height"     "publisher"  "skin_color" "alignment"  "weight"
```


```r
glimpse(superhero_info)
```

```
## Rows: 734
## Columns: 10
## $ name       <chr> "A-Bomb", "Abe Sapien", "Abin Sur", "Abomination", "Abraxas…
## $ gender     <chr> "Male", "Male", "Male", "Male", "Male", "Male", "Male", "Ma…
## $ eye_color  <chr> "yellow", "blue", "blue", "green", "blue", "blue", "blue", …
## $ race       <chr> "Human", "Icthyo Sapien", "Ungaran", "Human / Radiation", "…
## $ hair_color <chr> "No Hair", "No Hair", "No Hair", "No Hair", "Black", "No Ha…
## $ height     <dbl> 203, 191, 185, 203, NA, 193, NA, 185, 173, 178, 191, 188, 1…
## $ publisher  <chr> "Marvel Comics", "Dark Horse Comics", "DC Comics", "Marvel …
## $ skin_color <chr> NA, "blue", "red", NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, …
## $ alignment  <chr> "good", "good", "good", "bad", "bad", "bad", "good", "good"…
## $ weight     <dbl> 441, 65, 90, 441, NA, 122, NA, 88, 61, 81, 104, 108, 90, 90…
```


```r
summary(superhero_info)
```

```
##      name              gender           eye_color             race          
##  Length:734         Length:734         Length:734         Length:734        
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##                                                                             
##   hair_color            height       publisher          skin_color       
##  Length:734         Min.   : 15.2   Length:734         Length:734        
##  Class :character   1st Qu.:173.0   Class :character   Class :character  
##  Mode  :character   Median :183.0   Mode  :character   Mode  :character  
##                     Mean   :186.7                                        
##                     3rd Qu.:191.0                                        
##                     Max.   :975.0                                        
##                     NA's   :217                                          
##   alignment             weight     
##  Length:734         Min.   :  2.0  
##  Class :character   1st Qu.: 61.0  
##  Mode  :character   Median : 81.0  
##                     Mean   :112.3  
##                     3rd Qu.:108.0  
##                     Max.   :900.0  
##                     NA's   :239
```

### 2.
Are bad guys bigger? Make box-plots of weight by `alignment` (alignment on the x-axis).

```r
superhero_info %>% 
  filter(alignment!="NA") %>% 
  select(weight, alignment) %>% 
  ggplot(aes(x=alignment, y=weight))+
  geom_boxplot()
```

```
## Warning: Removed 235 rows containing non-finite values (stat_boxplot).
```

![](lab14_hw_files/figure-html/unnamed-chunk-7-1.png)<!-- -->
Overall, the distribution of weight does not seem to have a correlation to alignment.  

### 3. 
Now, make a violin plot of weight by `alignment` (alignment on the x-axis). Add some color!
  What information can you observe in the violin plot that was not visible in the boxplot?

```r
superhero_info %>% 
  filter(alignment!="NA") %>% 
  select(weight, alignment) %>% 
  ggplot(aes(x=alignment, y=weight, fill=alignment))+
  geom_violin()
```

```
## Warning: Removed 235 rows containing non-finite values (stat_ydensity).
```

![](lab14_hw_files/figure-html/unnamed-chunk-8-1.png)<!-- -->
This shows that there are smaller peaks in the data at some higher weights, but the majority of superheros are all around the same weight and alignment doesn't seem to be correlated to weight. 

### 4. 
Use `alpha = .5` in `geom_boxplot()` and `geom_violin()` to layer both plots on top of one another. What does this tell you about the distribution of weight in "`bad`" guys?

```r
superhero_info %>% 
  filter(alignment!="NA") %>% 
  select(weight, alignment) %>% 
  ggplot(aes(x=alignment, y=weight, fill=alignment))+
  geom_violin(alpha=0.5)+
  geom_boxplot(alpha=0.5)
```

```
## Warning: Removed 235 rows containing non-finite values (stat_ydensity).
```

```
## Warning: Removed 235 rows containing non-finite values (stat_boxplot).
```

![](lab14_hw_files/figure-html/unnamed-chunk-9-1.png)<!-- -->
Most bad guys have a weight around 100. 

### 5. 
Box plots are great for showing how the distribution of a numeric variable (e.g. weight) varies among a categorical variable (e.g. alignment).
  Make your own violin plot with the superhero data. 
  What is your numeric data? 
  What is your categorical variable?


```r
names(superhero_info)
```

```
##  [1] "name"       "gender"     "eye_color"  "race"       "hair_color"
##  [6] "height"     "publisher"  "skin_color" "alignment"  "weight"
```


```r
superhero_info %>% 
  filter(gender!="NA") %>% 
  ggplot(aes(x=gender, y=height, fill=gender))+
  geom_boxplot(alpha=0.5)+
  geom_violin(alpha=0.5)
```

```
## Warning: Removed 203 rows containing non-finite values (stat_boxplot).
```

```
## Warning: Removed 203 rows containing non-finite values (stat_ydensity).
```

![](lab14_hw_files/figure-html/unnamed-chunk-11-1.png)<!-- -->

### 6. 
Remind yourself what `beachbugs` looks like. Then generate a heatmap of buglevels by site and year. 
color it with `scale_fill_gradient(low="yellow", high="red")` or colors of your choice. you may find it looks best with one color!
(dont forget, `coord_flip()` is a quick way to improve the look of your plot if you dont like the default orientation)


```r
glimpse(beachbugs_long)
```

```
## Rows: 66
## Columns: 3
## $ year      <dbl> 2013, 2013, 2013, 2013, 2013, 2013, 2013, 2013, 2013, 2013, …
## $ site      <chr> "Bondi Beach", "Bronte Beach", "Clovelly Beach", "Coogee Bea…
## $ buglevels <dbl> 32.224138, 26.758621, 9.275862, 39.672414, 24.772727, 121.53…
```


```r
beachbugs_long
```

```
## # A tibble: 66 × 3
##     year site                    buglevels
##    <dbl> <chr>                       <dbl>
##  1  2013 Bondi Beach                 32.2 
##  2  2013 Bronte Beach                26.8 
##  3  2013 Clovelly Beach               9.28
##  4  2013 Coogee Beach                39.7 
##  5  2013 Gordons Bay (East)          24.8 
##  6  2013 Little Bay Beach           122.  
##  7  2013 Malabar Beach              101.  
##  8  2013 Maroubra Beach              47.1 
##  9  2013 South Maroubra Beach        39.3 
## 10  2013 South Maroubra Rockpool     96.4 
## # … with 56 more rows
```

```r
beachbugs_long %>% 
  ggplot(aes(x=site, y=year, fill=buglevels))+
  geom_tile()+
  scale_fill_gradient(low="yellow", high="red")+
  theme(axis.text.x = element_text(angle=60))
```

![](lab14_hw_files/figure-html/unnamed-chunk-14-1.png)<!-- -->

### 7.  
Use the provided code to normalize the beachbug data set. 
Then make a heatmap with the `beachbugs_normalized` data, and use the same color chois as above. Which heatmap do you think is more informative? why?


```r
#makes a new column of the highest buglevel observed at each site
beachbugs_w_max <- beachbugs_long %>% 
  group_by(site) %>% 
  mutate(max_buglevel = max(buglevels, na.rm=T)) %>% 
  arrange(site)
beachbugs_w_max
```

```
## # A tibble: 66 × 4
## # Groups:   site [11]
##     year site         buglevels max_buglevel
##    <dbl> <chr>            <dbl>        <dbl>
##  1  2013 Bondi Beach       32.2         32.2
##  2  2014 Bondi Beach       11.1         32.2
##  3  2015 Bondi Beach       14.3         32.2
##  4  2016 Bondi Beach       19.4         32.2
##  5  2017 Bondi Beach       13.2         32.2
##  6  2018 Bondi Beach       22.9         32.2
##  7  2013 Bronte Beach      26.8         61.3
##  8  2014 Bronte Beach      17.5         61.3
##  9  2015 Bronte Beach      23.6         61.3
## 10  2016 Bronte Beach      61.3         61.3
## # … with 56 more rows
```

```r
#makes a new table where the buglevels are normalized to the max_buglevel
beachbugs_normalized <- beachbugs_w_max %>% 
  group_by(site) %>% 
  mutate(norm_buglevel = buglevels/max_buglevel) %>% 
  arrange(site,year) %>%
  select(site, year, norm_buglevel)   # you dont have to select(), but I think its a clearer looking table
beachbugs_normalized
```

```
## # A tibble: 66 × 3
## # Groups:   site [11]
##    site          year norm_buglevel
##    <chr>        <dbl>         <dbl>
##  1 Bondi Beach   2013         1    
##  2 Bondi Beach   2014         0.344
##  3 Bondi Beach   2015         0.445
##  4 Bondi Beach   2016         0.601
##  5 Bondi Beach   2017         0.409
##  6 Bondi Beach   2018         0.710
##  7 Bronte Beach  2013         0.436
##  8 Bronte Beach  2014         0.285
##  9 Bronte Beach  2015         0.385
## 10 Bronte Beach  2016         1    
## # … with 56 more rows
```
 

```r
beachbugs_normalized %>% 
  ggplot(aes(x=site, y=year, fill=norm_buglevel))+
  geom_tile()+
  scale_fill_gradient(low="yellow", high="red")+
  theme(axis.text.x = element_text(angle=60))
```

![](lab14_hw_files/figure-html/unnamed-chunk-16-1.png)<!-- -->
This heat map gives us a wider range of colors, so the difference between buglevels is more obvious than in the first heat map. 

### 8.
Let's make a venn diagram of `superhero_info`, from 4 questions:
Is their alignment evil? 
Are their eyes red?
Are they male?
Are they bald?

Start by making the 4 vectors, then the list of vectors. The vector for alignment is provided:
### super heros venn

```r
names(superhero_info)
```

```
##  [1] "name"       "gender"     "eye_color"  "race"       "hair_color"
##  [6] "height"     "publisher"  "skin_color" "alignment"  "weight"
```


```r
 #evil

evil_vec <- superhero_info %>%
  filter(alignment == "bad")%>%
  pull(name)

# red eyes

red_eye_vec <- superhero_info %>% 
  filter(eye_color=="red") %>% 
  pull(name)

# male

male_vec <- superhero_info %>% 
  filter(gender=="male") %>% 
  pull(name)

# bald

bald_vec <- superhero_info %>% 
  filter(hair_color=="bald") %>% 
  pull(name)
```

Your list of vectors will look something like this:

```r
questions_list <- list(evil_vec, red_eye_vec, male_vec, bald_vec)
questions_list
```

```
## [[1]]
##   [1] "Abomination"       "Abraxas"           "Absorbing Man"    
##   [4] "Air-Walker"        "Ajax"              "Alex Mercer"      
##   [7] "Alien"             "Amazo"             "Ammo"             
##  [10] "Angela"            "Annihilus"         "Anti-Monitor"     
##  [13] "Anti-Spawn"        "Apocalypse"        "Arclight"         
##  [16] "Atlas"             "Azazel"            "Bane"             
##  [19] "Beetle"            "Big Barda"         "Big Man"          
##  [22] "Billy Kincaid"     "Bird-Man"          "Bird-Man II"      
##  [25] "Black Abbott"      "Black Adam"        "Black Mamba"      
##  [28] "Black Manta"       "Blackout"          "Blackwing"        
##  [31] "Blizzard"          "Blizzard"          "Blizzard II"      
##  [34] "Blob"              "Bloodaxe"          "Bloodwraith"      
##  [37] "Boba Fett"         "Bomb Queen"        "Brainiac"         
##  [40] "Bullseye"          "Callisto"          "Carnage"          
##  [43] "Chameleon"         "Changeling"        "Cheetah"          
##  [46] "Cheetah II"        "Cheetah III"       "Chromos"          
##  [49] "Clock King"        "Cogliostro"        "Cottonmouth"      
##  [52] "Curse"             "Cy-Gor"            "Cyborg Superman"  
##  [55] "Darkseid"          "Darkside"          "Darth Maul"       
##  [58] "Darth Vader"       "Deadshot"          "Demogoblin"       
##  [61] "Destroyer"         "Diamondback"       "Doctor Doom"      
##  [64] "Doctor Doom II"    "Doctor Octopus"    "Doomsday"         
##  [67] "Doppelganger"      "Dormammu"          "Ego"              
##  [70] "Electro"           "Elle Bishop"       "Evil Deadpool"    
##  [73] "Evilhawk"          "Exodus"            "Fabian Cortez"    
##  [76] "Fallen One II"     "Faora"             "Fixer"            
##  [79] "Frenzy"            "General Zod"       "Giganta"          
##  [82] "Goblin Queen"      "Godzilla"          "Gog"              
##  [85] "Gorilla Grodd"     "Granny Goodness"   "Greedo"           
##  [88] "Green Goblin"      "Green Goblin II"   "Harley Quinn"     
##  [91] "Heat Wave"         "Hela"              "Hobgoblin"        
##  [94] "Hydro-Man"         "Iron Monger"       "Jigsaw"           
##  [97] "Joker"             "Junkpile"          "Kang"             
## [100] "Killer Croc"       "Killer Frost"      "King Shark"       
## [103] "Kingpin"           "Klaw"              "Kraven II"        
## [106] "Kraven the Hunter" "Kylo Ren"          "Lady Bullseye"    
## [109] "Lady Deathstrike"  "Leader"            "Lex Luthor"       
## [112] "Lightning Lord"    "Living Brain"      "Lizard"           
## [115] "Loki"              "Luke Campbell"     "Mach-IV"          
## [118] "Magneto"           "Magus"             "Mandarin"         
## [121] "Match"             "Maxima"            "Mephisto"         
## [124] "Metallo"           "Mister Freeze"     "Mister Knife"     
## [127] "Mister Mxyzptlk"   "Mister Sinister"   "Mister Zsasz"     
## [130] "MODOK"             "Moloch"            "Molten Man"       
## [133] "Moonstone"         "Morlun"            "Moses Magnum"     
## [136] "Mysterio"          "Mystique"          "Nebula"           
## [139] "Omega Red"         "Onslaught"         "Overtkill"        
## [142] "Ozymandias"        "Parademon"         "Penguin"          
## [145] "Plantman"          "Plastique"         "Poison Ivy"       
## [148] "Predator"          "Professor Zoom"    "Proto-Goblin"     
## [151] "Purple Man"        "Pyro"              "Ra's Al Ghul"     
## [154] "Razor-Fist II"     "Red Mist"          "Red Skull"        
## [157] "Redeemer II"       "Redeemer III"      "Rhino"            
## [160] "Rick Flag"         "Riddler"           "Sabretooth"       
## [163] "Sauron"            "Scarecrow"         "Scarlet Witch"    
## [166] "Scorpia"           "Scorpion"          "Sebastian Shaw"   
## [169] "Shocker"           "Siren"             "Siren II"         
## [172] "Siryn"             "Snake-Eyes"        "Solomon Grundy"   
## [175] "Spider-Carnage"    "Spider-Woman IV"   "Steppenwolf"      
## [178] "Stormtrooper"      "Superboy-Prime"    "Swamp Thing"      
## [181] "Swarm"             "Sylar"             "T-1000"           
## [184] "T-800"             "T-850"             "T-X"              
## [187] "Taskmaster"        "Thanos"            "Tiger Shark"      
## [190] "Tinkerer"          "Trigon"            "Two-Face"         
## [193] "Ultron"            "Utgard-Loki"       "Vanisher"         
## [196] "Vegeta"            "Venom"             "Venom II"         
## [199] "Venom III"         "Violator"          "Vulture"          
## [202] "Walrus"            "Warp"              "Weapon XI"        
## [205] "White Canary"      "Yellow Claw"       "Zoom"             
## 
## [[2]]
##  [1] "Amazo"              "Apocalypse"         "Black Abbott"      
##  [4] "Blackout"           "Blackwulf"          "Captain Planet"    
##  [7] "Copycat"            "Darkseid"           "Demogoblin"        
## [10] "Doomsday"           "Drax the Destroyer" "Etrigan"           
## [13] "Evilhawk"           "Fin Fang Foom"      "Fixer"             
## [16] "Gambit"             "Ghost Rider"        "Hawk"              
## [19] "Hellstorm"          "Jubilee"            "Killer Croc"       
## [22] "Kilowog"            "Klaw"               "Lizard"            
## [25] "Lobo"               "Machine Man"        "Man-Thing"         
## [28] "Martian Manhunter"  "Miss Martian"       "Mister Sinister"   
## [31] "Omega Red"          "Onslaught"          "Sasquatch"         
## [34] "Shadow King"        "Spider-Man"         "Spider-Woman IV"   
## [37] "Steppenwolf"        "Swamp Thing"        "T-800"             
## [40] "T-850"              "Thanos"             "Ultron"            
## [43] "Vision II"          "Warlock"            "Wonder Man"        
## [46] "Zoom"              
## 
## [[3]]
## character(0)
## 
## [[4]]
## character(0)
```

### 9. 
Let's make the venn diagram! use the code from lab as a reference. 

```r
# something like:
 ggVennDiagram(questions_list, category.names = c("evil_vec", "red_eye_vec", "male_vec", "bald_vec"))
```

![](lab14_hw_files/figure-html/unnamed-chunk-20-1.png)<!-- -->


### 10. Choose one intersection of the venn diagram that is interesting to you. Use dplyr to find the names of the superheros in that intersection. 


```r
superhero_info %>% 
  filter(eye_color=="red") %>% 
  filter(alignment=="bad") %>% 
  select(name)
```

```
## # A tibble: 23 × 1
##    name        
##    <chr>       
##  1 Amazo       
##  2 Apocalypse  
##  3 Black Abbott
##  4 Blackout    
##  5 Darkseid    
##  6 Demogoblin  
##  7 Doomsday    
##  8 Evilhawk    
##  9 Fixer       
## 10 Killer Croc 
## # … with 13 more rows
```

### 11. Make another venn diagram with the `superhero_info` data. What are your questions? ( At least 2!)
Do they have blue eyes? 
Are they human?


```r
blue_eye_vec <- superhero_info %>% 
  filter(eye_color=="blue") %>% 
  pull(name)

human_vec <- superhero_info %>% 
  filter(race=="human") %>% 
  pull(name)
```



```r
blue_human_list <- list(blue_eye_vec, human_vec)
```


```r
ggVennDiagram(blue_human_list, category.names = c("blue_eyes", "human"))
```

![](lab14_hw_files/figure-html/unnamed-chunk-24-1.png)<!-- -->

### 12.
What are some very common super powers? Lets make a word cloud with the `superhero_powers` data.

Use the provided code to make the frequency table, then make a word cloud with it. 
Remember, you can change `scale_size_area(max_size = 20)` to a different number if the words wont fit!


```r
head(superhero_powers)
```

```
## # A tibble: 6 × 168
##   hero_names  Agility `Accelerated Healing` `Lantern Power Ri…` `Dimensional A…`
##   <chr>       <lgl>   <lgl>                 <lgl>               <lgl>           
## 1 3-D Man     TRUE    FALSE                 FALSE               FALSE           
## 2 A-Bomb      FALSE   TRUE                  FALSE               FALSE           
## 3 Abe Sapien  TRUE    TRUE                  FALSE               FALSE           
## 4 Abin Sur    FALSE   FALSE                 TRUE                FALSE           
## 5 Abomination FALSE   TRUE                  FALSE               FALSE           
## 6 Abraxas     FALSE   FALSE                 FALSE               TRUE            
## # … with 163 more variables: `Cold Resistance` <lgl>, Durability <lgl>,
## #   Stealth <lgl>, `Energy Absorption` <lgl>, Flight <lgl>,
## #   `Danger Sense` <lgl>, `Underwater breathing` <lgl>, Marksmanship <lgl>,
## #   `Weapons Master` <lgl>, `Power Augmentation` <lgl>,
## #   `Animal Attributes` <lgl>, Longevity <lgl>, Intelligence <lgl>,
## #   `Super Strength` <lgl>, Cryokinesis <lgl>, Telepathy <lgl>,
## #   `Energy Armor` <lgl>, `Energy Blasts` <lgl>, Duplication <lgl>, …
```

```r
power_frequency <- superhero_powers %>% 
  select(-hero_names) %>%
  summarise_all(~(sum(.))) %>%
  pivot_longer(everything(), names_to = "power", values_to = "freq")
power_frequency
```

```
## # A tibble: 167 × 2
##    power                  freq
##    <chr>                 <int>
##  1 Agility                 242
##  2 Accelerated Healing     178
##  3 Lantern Power Ring       11
##  4 Dimensional Awareness    25
##  5 Cold Resistance          47
##  6 Durability              257
##  7 Stealth                 126
##  8 Energy Absorption        77
##  9 Flight                  212
## 10 Danger Sense             30
## # … with 157 more rows
```



```r
power_frequency %>% 
  ggplot(aes(
    label=power,
    size=freq,
    color=power))+
  geom_text_wordcloud()+
  scale_size_area(max_size = 7)+
  theme_minimal()
```

![](lab14_hw_files/figure-html/unnamed-chunk-27-1.png)<!-- -->
### 13.  
Who are some very powerful supers? 
 Lets make a different word cloud with the `superhero_powers` data. 
Use the provided code to make the frequency table, then make a word cloud with it.
You can use `hero_names` for the labels, and `sum_powers` for size. 


```r
power_quantity <- superhero_powers %>% 
  pivot_longer(-hero_names, names_to = "power", values_to = "yes_or_no") %>% 
  group_by(hero_names) %>% 
  mutate(sum_powers = sum(yes_or_no, na.rm=T)) %>%
  arrange(desc(sum_powers), hero_names, desc(yes_or_no))
power_quantity
```

```
## # A tibble: 111,389 × 4
## # Groups:   hero_names [667]
##    hero_names power                 yes_or_no sum_powers
##    <chr>      <chr>                 <lgl>          <int>
##  1 Spectre    Agility               TRUE              49
##  2 Spectre    Accelerated Healing   TRUE              49
##  3 Spectre    Dimensional Awareness TRUE              49
##  4 Spectre    Stealth               TRUE              49
##  5 Spectre    Energy Absorption     TRUE              49
##  6 Spectre    Flight                TRUE              49
##  7 Spectre    Marksmanship          TRUE              49
##  8 Spectre    Longevity             TRUE              49
##  9 Spectre    Intelligence          TRUE              49
## 10 Spectre    Super Strength        TRUE              49
## # … with 111,379 more rows
```

```r
#we have to trim down to only the top 50, or it will crowd the image!
power_quantity <- power_quantity %>% 
  ungroup %>%
  distinct(hero_names, sum_powers) %>%
  slice_max(sum_powers, n = 50)
power_quantity
```

```
## # A tibble: 51 × 2
##    hero_names        sum_powers
##    <chr>                  <int>
##  1 Spectre                   49
##  2 Amazo                     44
##  3 Living Tribunal           35
##  4 Martian Manhunter         35
##  5 Man of Miracles           34
##  6 Captain Marvel            33
##  7 T-X                       33
##  8 Galactus                  32
##  9 T-1000                    32
## 10 Mister Mxyzptlk           31
## # … with 41 more rows
```




```r
power_quantity %>% 
  ggplot(aes(
    label=hero_names,
    size=sum_powers,
    color=hero_names
  ))+
  geom_text_wordcloud()+
  scale_size_area(max_size = 5)
```

![](lab14_hw_files/figure-html/unnamed-chunk-29-1.png)<!-- -->

## That's it! 🎉
Thanks for coding with us all winter! 
Now go finish your group project :) 

