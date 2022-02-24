---
title: "Lab 12 Homework"
author: "Julie Trundle"
date: "2022-02-23"
output:
  html_document: 
    theme: spacelab
    keep_md: yes
---



## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your final lab report should be organized, clean, and run free from errors. Remember, you must remove the `#` for the included code chunks to run. Be sure to add your name to the author header above. For any included plots, make sure they are clearly labeled. You are free to use any plot type that you feel best communicates the results of your analysis.  

Make sure to use the formatting conventions of RMarkdown to make your report neat and clean!  


```r
#install.packages("shiny")
#install.packages("shinydashboard")
```

## Load the libraries

```r
library(tidyverse)
library(janitor)
library(here)
library(ggmap)
library(dplyr )
```


```r
#library(albersusa)
```

## Load the Data
We will use two separate data sets for this homework.  

1. The first [data set](https://rcweb.dartmouth.edu/~f002d69/workshops/index_rspatial.html) represent sightings of grizzly bears (Ursos arctos) in Alaska.  
2. The second data set is from Brandell, Ellen E (2021), Serological dataset and R code for: Patterns and processes of pathogen exposure in gray wolves across North America, Dryad, [Dataset](https://doi.org/10.5061/dryad.5hqbzkh51).  

1. Load the `grizzly` data and evaluate its structure. As part of this step, produce a summary that provides the range of latitude and longitude so you can build an appropriate bounding box.


```r
grizzly <- readr::read_csv("data/bear-sightings.csv")
```

```
## Rows: 494 Columns: 3
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## dbl (3): bear.id, longitude, latitude
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

```r
grizzly
```

```
## # A tibble: 494 × 3
##    bear.id longitude latitude
##      <dbl>     <dbl>    <dbl>
##  1       7     -149.     62.7
##  2      57     -153.     58.4
##  3      69     -145.     62.4
##  4      75     -153.     59.9
##  5     104     -143.     61.1
##  6     108     -150.     62.9
##  7     115     -152.     68.0
##  8     116     -147.     62.6
##  9     125     -157.     60.2
## 10     135     -156.     58.9
## # … with 484 more rows
```


```r
glimpse(grizzly)
```

```
## Rows: 494
## Columns: 3
## $ bear.id   <dbl> 7, 57, 69, 75, 104, 108, 115, 116, 125, 135, 137, 162, 185, …
## $ longitude <dbl> -148.9560, -152.6228, -144.9374, -152.8485, -143.2948, -149.…
## $ latitude  <dbl> 62.65822, 58.35064, 62.38227, 59.90122, 61.07311, 62.91605, …
```


```r
summary(grizzly)
```

```
##     bear.id       longitude         latitude    
##  Min.   :   7   Min.   :-166.2   Min.   :55.02  
##  1st Qu.:2569   1st Qu.:-154.2   1st Qu.:58.13  
##  Median :4822   Median :-151.0   Median :60.97  
##  Mean   :4935   Mean   :-149.1   Mean   :61.41  
##  3rd Qu.:7387   3rd Qu.:-145.6   3rd Qu.:64.13  
##  Max.   :9996   Max.   :-131.3   Max.   :70.37
```

2. Use the range of the latitude and longitude to build an appropriate bounding box for your map.


```r
lat <- c(55.02, 70.37)
long <- c(-166.2, -131.3)
bbox <- make_bbox(long, lat, f=0.05)
```

3. Load a map from `stamen` in a terrain style projection and display the map.


```r
map1 <- get_map(bbox, maptype = "terrain", source = "stamen")
```

```
## Map tiles by Stamen Design, under CC BY 3.0. Data by OpenStreetMap, under ODbL.
```


```r
ggmap(map1)
```

![](lab12_hw_files/figure-html/unnamed-chunk-9-1.png)<!-- -->

4. Build a final map that overlays the recorded observations of grizzly bears in Alaska.


```r
ggmap(map1)+
  geom_point(data = grizzly, aes(longitude, latitude))+
  labs(x="Longitude", y="Latitude", title="Grizzly Bears in Alaska")
```

![](lab12_hw_files/figure-html/unnamed-chunk-10-1.png)<!-- -->

Let's switch to the wolves data. Brandell, Ellen E (2021), Serological dataset and R code for: Patterns and processes of pathogen exposure in gray wolves across North America, Dryad, [Dataset](https://doi.org/10.5061/dryad.5hqbzkh51).  

5. Load the data and evaluate its structure.  


```r
wolves <- readr::read_csv("data/wolves_data/wolves_dataset.csv")
```

```
## Rows: 1986 Columns: 23
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr  (4): pop, age.cat, sex, color
## dbl (19): year, lat, long, habitat, human, pop.density, pack.size, standard....
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

```r
wolves
```

```
## # A tibble: 1,986 × 23
##    pop     year age.cat sex   color   lat  long habitat human pop.density
##    <chr>  <dbl> <chr>   <chr> <chr> <dbl> <dbl>   <dbl> <dbl>       <dbl>
##  1 AK.PEN  2006 S       F     G      57.0 -158.    254.  10.4           8
##  2 AK.PEN  2006 S       M     G      57.0 -158.    254.  10.4           8
##  3 AK.PEN  2006 A       F     G      57.0 -158.    254.  10.4           8
##  4 AK.PEN  2006 S       M     B      57.0 -158.    254.  10.4           8
##  5 AK.PEN  2006 A       M     B      57.0 -158.    254.  10.4           8
##  6 AK.PEN  2006 A       M     G      57.0 -158.    254.  10.4           8
##  7 AK.PEN  2006 A       F     G      57.0 -158.    254.  10.4           8
##  8 AK.PEN  2006 P       M     G      57.0 -158.    254.  10.4           8
##  9 AK.PEN  2006 S       F     G      57.0 -158.    254.  10.4           8
## 10 AK.PEN  2006 P       M     G      57.0 -158.    254.  10.4           8
## # … with 1,976 more rows, and 13 more variables: pack.size <dbl>,
## #   standard.habitat <dbl>, standard.human <dbl>, standard.pop <dbl>,
## #   standard.packsize <dbl>, standard.latitude <dbl>, standard.longitude <dbl>,
## #   cav.binary <dbl>, cdv.binary <dbl>, cpv.binary <dbl>, chv.binary <dbl>,
## #   neo.binary <dbl>, toxo.binary <dbl>
```

6. How many distinct wolf populations are included in this study? Make a new object that restricts the data to the wolf populations in the lower 48 US states.


```r
wolves %>% 
  count(pop)
```

```
## # A tibble: 17 × 2
##    pop         n
##    <chr>   <int>
##  1 AK.PEN    100
##  2 BAN.JAS    96
##  3 BC        145
##  4 DENALI    154
##  5 ELLES      11
##  6 GTNP       60
##  7 INT.AK     35
##  8 MEXICAN   181
##  9 MI        102
## 10 MT        351
## 11 N.NWT      67
## 12 ONT        60
## 13 SE.AK      10
## 14 SNF        92
## 15 SS.NWT     34
## 16 YNP       383
## 17 YUCH      105
```

```r
low_wolves <- wolves %>% 
  filter(pop %in% c("GNTP", "MEXICAN", "MI", "MT", "SNF", "YNP"))
low_wolves
```

```
## # A tibble: 1,109 × 23
##    pop      year age.cat sex   color   lat  long habitat human pop.density
##    <chr>   <dbl> <chr>   <chr> <chr> <dbl> <dbl>   <dbl> <dbl>       <dbl>
##  1 MEXICAN  2006 P       F     G      33.9 -109.  11861. 2788.        3.99
##  2 MEXICAN  2006 P       F     G      33.9 -109.  11861. 2788.        3.99
##  3 MEXICAN  2006 P       F     G      33.9 -109.  11861. 2788.        3.99
##  4 MEXICAN  2006 P       F     G      33.9 -109.  11861. 2788.        3.99
##  5 MEXICAN  2006 P       F     G      33.9 -109.  11861. 2788.        3.99
##  6 MEXICAN  2006 P       M     G      33.9 -109.  11861. 2788.        3.99
##  7 MEXICAN  2006 P       M     G      33.9 -109.  11861. 2788.        3.99
##  8 MEXICAN  2006 P       M     G      33.9 -109.  11861. 2788.        3.99
##  9 MEXICAN  2006 P       M     G      33.9 -109.  11861. 2788.        3.99
## 10 MEXICAN  2006 P       M     G      33.9 -109.  11861. 2788.        3.99
## # … with 1,099 more rows, and 13 more variables: pack.size <dbl>,
## #   standard.habitat <dbl>, standard.human <dbl>, standard.pop <dbl>,
## #   standard.packsize <dbl>, standard.latitude <dbl>, standard.longitude <dbl>,
## #   cav.binary <dbl>, cdv.binary <dbl>, cpv.binary <dbl>, chv.binary <dbl>,
## #   neo.binary <dbl>, toxo.binary <dbl>
```

7. Use the range of the latitude and longitude to build an appropriate bounding box for your map.


```r
low_wolves %>% 
  select(lat, long) %>% 
  summary()
```

```
##       lat             long        
##  Min.   :33.89   Min.   :-110.99  
##  1st Qu.:44.60   1st Qu.:-110.99  
##  Median :44.60   Median :-110.55  
##  Mean   :43.96   Mean   :-106.70  
##  3rd Qu.:46.83   3rd Qu.:-109.17  
##  Max.   :47.75   Max.   : -86.82
```


```r
lat <- c(33.89, 47.75)
long <- c(-110.99, -86.82)
bbox <- make_bbox(long, lat, f=0.05)
```

8.  Load a map from `stamen` in a `terrain-lines` projection and display the map.


```r
map2 <- get_map(bbox, maptype = "terrain-lines", source = "stamen")
```

```
## Map tiles by Stamen Design, under CC BY 3.0. Data by OpenStreetMap, under ODbL.
```


```r
ggmap(map2)
```

![](lab12_hw_files/figure-html/unnamed-chunk-17-1.png)<!-- -->


9. Build a final map that overlays the recorded observations of wolves in the lower 48 states.


```r
ggmap(map2)+
  geom_point(low_wolves, mapping = aes(x=long, y=lat), size=2)+
  labs(x="Longitude", y="Latitude", title="Wolves in the Lower US")
```

![](lab12_hw_files/figure-html/unnamed-chunk-18-1.png)<!-- -->

10. Use the map from #9 above, but add some aesthetics. Try to `fill` and `color` by population.


```r
ggmap(map2)+
  geom_point(low_wolves, mapping = aes(x=long, y=lat, fill=pop, color=pop), size=2)+
  labs(x="Longitude", y="Latitude", title="Wolves in the Lower US")
```

![](lab12_hw_files/figure-html/unnamed-chunk-19-1.png)<!-- -->

## Push your final code to GitHub!
Please be sure that you check the `keep md` file in the knit preferences. 
