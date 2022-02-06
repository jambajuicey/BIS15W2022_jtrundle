---
title: "Lab 9 Homework"
author: "Julie Trundle"
date: "2022-02-06"
output:
  html_document: 
    theme: spacelab
    keep_md: yes
---



## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your final lab report should be organized, clean, and run free from errors. Remember, you must remove the `#` for the included code chunks to run. Be sure to add your name to the author header above.  

Make sure to use the formatting conventions of RMarkdown to make your report neat and clean!  

## Load the libraries

```r
library(tidyverse)
library(janitor)
library(here)
library(naniar)
library(dplyr)
```

For this homework, we will take a departure from biological data and use data about California colleges. These data are a subset of the national college scorecard (https://collegescorecard.ed.gov/data/). Load the `ca_college_data.csv` as a new object called `colleges`.

```r
college <- readr::read_csv("data/ca_college_data.csv") 
```

```
## Rows: 341 Columns: 10
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr (4): INSTNM, CITY, STABBR, ZIP
## dbl (6): ADM_RATE, SAT_AVG, PCIP26, COSTT4_A, C150_4_POOLED, PFTFTUG1_EF
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

```r
college
```

```
## # A tibble: 341 × 10
##    INSTNM      CITY  STABBR ZIP   ADM_RATE SAT_AVG PCIP26 COSTT4_A C150_4_POOLED
##    <chr>       <chr> <chr>  <chr>    <dbl>   <dbl>  <dbl>    <dbl>         <dbl>
##  1 Grossmont … El C… CA     9202…       NA      NA 0.0016     7956        NA    
##  2 College of… Visa… CA     9327…       NA      NA 0.0066     8109        NA    
##  3 College of… San … CA     9440…       NA      NA 0.0038     8278        NA    
##  4 Ventura Co… Vent… CA     9300…       NA      NA 0.0035     8407        NA    
##  5 Oxnard Col… Oxna… CA     9303…       NA      NA 0.0085     8516        NA    
##  6 Moorpark C… Moor… CA     9302…       NA      NA 0.0151     8577        NA    
##  7 Skyline Co… San … CA     9406…       NA      NA 0          8580         0.233
##  8 Glendale C… Glen… CA     9120…       NA      NA 0.002      9181        NA    
##  9 Citrus Col… Glen… CA     9174…       NA      NA 0.0021     9281        NA    
## 10 Fresno Cit… Fres… CA     93741       NA      NA 0.0324     9370        NA    
## # … with 331 more rows, and 1 more variable: PFTFTUG1_EF <dbl>
```


The variables are a bit hard to decipher, here is a key:  

INSTNM: Institution name  
CITY: California city  
STABBR: Location state  
ZIP: Zip code  
ADM_RATE: Admission rate  
SAT_AVG: SAT average score  
PCIP26: Percentage of degrees awarded in Biological And Biomedical Sciences  
COSTT4_A: Annual cost of attendance  
C150_4_POOLED: 4-year completion rate  
PFTFTUG1_EF: Percentage of undergraduate students who are first-time, full-time degree/certificate-seeking undergraduate students  

1. Use your preferred function(s) to have a look at the data and get an idea of its structure. Make sure you summarize NA's and determine whether or not the data are tidy. You may also consider dealing with any naming issues.


```r
college
```

```
## # A tibble: 341 × 10
##    INSTNM      CITY  STABBR ZIP   ADM_RATE SAT_AVG PCIP26 COSTT4_A C150_4_POOLED
##    <chr>       <chr> <chr>  <chr>    <dbl>   <dbl>  <dbl>    <dbl>         <dbl>
##  1 Grossmont … El C… CA     9202…       NA      NA 0.0016     7956        NA    
##  2 College of… Visa… CA     9327…       NA      NA 0.0066     8109        NA    
##  3 College of… San … CA     9440…       NA      NA 0.0038     8278        NA    
##  4 Ventura Co… Vent… CA     9300…       NA      NA 0.0035     8407        NA    
##  5 Oxnard Col… Oxna… CA     9303…       NA      NA 0.0085     8516        NA    
##  6 Moorpark C… Moor… CA     9302…       NA      NA 0.0151     8577        NA    
##  7 Skyline Co… San … CA     9406…       NA      NA 0          8580         0.233
##  8 Glendale C… Glen… CA     9120…       NA      NA 0.002      9181        NA    
##  9 Citrus Col… Glen… CA     9174…       NA      NA 0.0021     9281        NA    
## 10 Fresno Cit… Fres… CA     93741       NA      NA 0.0324     9370        NA    
## # … with 331 more rows, and 1 more variable: PFTFTUG1_EF <dbl>
```


```r
glimpse(college)
```

```
## Rows: 341
## Columns: 10
## $ INSTNM        <chr> "Grossmont College", "College of the Sequoias", "College…
## $ CITY          <chr> "El Cajon", "Visalia", "San Mateo", "Ventura", "Oxnard",…
## $ STABBR        <chr> "CA", "CA", "CA", "CA", "CA", "CA", "CA", "CA", "CA", "C…
## $ ZIP           <chr> "92020-1799", "93277-2214", "94402-3784", "93003-3872", …
## $ ADM_RATE      <dbl> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, …
## $ SAT_AVG       <dbl> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, …
## $ PCIP26        <dbl> 0.0016, 0.0066, 0.0038, 0.0035, 0.0085, 0.0151, 0.0000, …
## $ COSTT4_A      <dbl> 7956, 8109, 8278, 8407, 8516, 8577, 8580, 9181, 9281, 93…
## $ C150_4_POOLED <dbl> NA, NA, NA, NA, NA, NA, 0.2334, NA, NA, NA, NA, 0.1704, …
## $ PFTFTUG1_EF   <dbl> 0.3546, 0.5413, 0.3567, 0.3824, 0.2753, 0.4286, 0.2307, …
```


```r
naniar::miss_var_summary(college)
```

```
## # A tibble: 10 × 3
##    variable      n_miss pct_miss
##    <chr>          <int>    <dbl>
##  1 SAT_AVG          276     80.9
##  2 ADM_RATE         240     70.4
##  3 C150_4_POOLED    221     64.8
##  4 COSTT4_A         124     36.4
##  5 PFTFTUG1_EF       53     15.5
##  6 PCIP26            35     10.3
##  7 INSTNM             0      0  
##  8 CITY               0      0  
##  9 STABBR             0      0  
## 10 ZIP                0      0
```


```r
college <- clean_names(college)
```


```r
names(college)
```

```
##  [1] "instnm"        "city"          "stabbr"        "zip"          
##  [5] "adm_rate"      "sat_avg"       "pcip26"        "costt4_a"     
##  [9] "c150_4_pooled" "pftftug1_ef"
```

2. Which cities in California have the highest number of colleges?

```r
college_by_city <- college %>% 
  group_by(city) %>% 
  summarise(n_colleges=n_distinct(instnm)) %>% 
  arrange(desc(n_colleges))
college_by_city
```

```
## # A tibble: 161 × 2
##    city          n_colleges
##    <chr>              <int>
##  1 Los Angeles           24
##  2 San Diego             18
##  3 San Francisco         15
##  4 Sacramento            10
##  5 Berkeley               9
##  6 Oakland                9
##  7 Claremont              7
##  8 Pasadena               6
##  9 Fresno                 5
## 10 Irvine                 5
## # … with 151 more rows
```
Los Angeles, San Diego, San Francisco and Sacramento have the highest number of colleges in the state of California. 
3. Based on your answer to #2, make a plot that shows the number of colleges in the top 10 cities.

```r
college_by_city %>% 
  top_n(+10, n_colleges) %>% 
  ggplot(aes(x=city, y=n_colleges))+
  geom_col()+
  coord_flip()
```

![](lab9_hw_files/figure-html/unnamed-chunk-9-1.png)<!-- -->

4. The column `COSTT4_A` is the annual cost of each institution. Which city has the highest average cost? Where is it located?


```r
names(college)
```

```
##  [1] "instnm"        "city"          "stabbr"        "zip"          
##  [5] "adm_rate"      "sat_avg"       "pcip26"        "costt4_a"     
##  [9] "c150_4_pooled" "pftftug1_ef"
```


```r
college %>% 
  select(instnm, city, stabbr, costt4_a) %>% 
  group_by(city) %>% 
  summarise(mean_cost=mean(costt4_a, na.rm=T)) %>% 
  arrange(desc(mean_cost))
```

```
## # A tibble: 161 × 2
##    city                mean_cost
##    <chr>                   <dbl>
##  1 Claremont               66498
##  2 Malibu                  66152
##  3 Valencia                64686
##  4 Orange                  64501
##  5 Redlands                61542
##  6 Moraga                  61095
##  7 Atherton                56035
##  8 Thousand Oaks           54373
##  9 Rancho Palos Verdes     50758
## 10 La Verne                50603
## # … with 151 more rows
```
Claremont, CA has the highest average cost of attending an institution. 

5. Based on your answer to #4, make a plot that compares the cost of the individual colleges in the most expensive city. Bonus! Add UC Davis here to see how it compares :>).


```r
college %>% 
  filter(city=="Claremont")
```

```
## # A tibble: 7 × 10
##   instnm      city  stabbr zip   adm_rate sat_avg  pcip26 costt4_a c150_4_pooled
##   <chr>       <chr> <chr>  <chr>    <dbl>   <dbl>   <dbl>    <dbl>         <dbl>
## 1 Pomona Col… Clar… CA     9171…   0.0944    1442  0.171     64870         0.957
## 2 Pitzer Col… Clar… CA     9171…   0.137       NA  0.0888    65880         0.888
## 3 Scripps Co… Clar… CA     9171…   0.299     1353  0.152     66060         0.871
## 4 Claremont … Clar… CA     9171…   0.0944    1413  0.0681    66325         0.924
## 5 Harvey Mud… Clar… CA     91711   0.129     1496  0.0674    69355         0.925
## 6 Claremont … Clar… CA     9171…  NA           NA NA            NA        NA    
## 7 Claremont … Clar… CA     9171…  NA           NA NA            NA        NA    
## # … with 1 more variable: pftftug1_ef <dbl>
```


```r
college %>% 
  filter(city=="Claremont") %>% 
  select(instnm, costt4_a) %>% 
  filter(costt4_a!="NA") %>% 
  ggplot(aes(x=instnm, y=costt4_a))+
  geom_col()+
  coord_flip()
```

![](lab9_hw_files/figure-html/unnamed-chunk-13-1.png)<!-- -->

6. The column `ADM_RATE` is the admissions rate by college and `C150_4_POOLED` is the four-year completion rate. Use a scatterplot to show the relationship between these two variables. What do you think this means?

```r
college %>% 
  ggplot(aes(x=adm_rate, y=c150_4_pooled))+
  geom_point()+
  geom_smooth(method = lm, se=T)
```

```
## `geom_smooth()` using formula 'y ~ x'
```

```
## Warning: Removed 251 rows containing non-finite values (stat_smooth).
```

```
## Warning: Removed 251 rows containing missing values (geom_point).
```

![](lab9_hw_files/figure-html/unnamed-chunk-14-1.png)<!-- -->
This scatterplot shows that generally schools with a lower admission rate have a higher four-year completion rate, and schools with a higher admission rate have a lower four-year completion rate. 

7. Is there a relationship between cost and four-year completion rate? (You don't need to do the stats, just produce a plot). What do you think this means?

```r
college %>% 
  ggplot(aes(x=costt4_a, y=c150_4_pooled))+
  geom_point()+
  geom_smooth(method=lm)
```

```
## `geom_smooth()` using formula 'y ~ x'
```

```
## Warning: Removed 225 rows containing non-finite values (stat_smooth).
```

```
## Warning: Removed 225 rows containing missing values (geom_point).
```

![](lab9_hw_files/figure-html/unnamed-chunk-15-1.png)<!-- -->
There is a slight correlation between cost and four-year completion rate- the higher the cost the higher the completion rate -but it is not a strong correlation. 

8. The column titled `INSTNM` is the institution name. We are only interested in the University of California colleges. Make a new data frame that is restricted to UC institutions. You can remove `Hastings College of Law` and `UC San Francisco` as we are only interested in undergraduate institutions.


```r
view(college)
```


```r
UC_colleges <- college %>% 
  filter(grepl("University of California", instnm))
UC_colleges
```

```
## # A tibble: 10 × 10
##    instnm      city  stabbr zip   adm_rate sat_avg pcip26 costt4_a c150_4_pooled
##    <chr>       <chr> <chr>  <chr>    <dbl>   <dbl>  <dbl>    <dbl>         <dbl>
##  1 University… La J… CA     92093    0.357    1324  0.216    31043         0.872
##  2 University… Irvi… CA     92697    0.406    1206  0.107    31198         0.876
##  3 University… Rive… CA     92521    0.663    1078  0.149    31494         0.73 
##  4 University… Los … CA     9009…    0.180    1334  0.155    33078         0.911
##  5 University… Davis CA     9561…    0.423    1218  0.198    33904         0.850
##  6 University… Sant… CA     9506…    0.578    1201  0.193    34608         0.776
##  7 University… Berk… CA     94720    0.169    1422  0.105    34924         0.916
##  8 University… Sant… CA     93106    0.358    1281  0.108    34998         0.816
##  9 University… San … CA     9410…   NA          NA NA           NA        NA    
## 10 University… San … CA     9414…   NA          NA NA           NA        NA    
## # … with 1 more variable: pftftug1_ef <dbl>
```
Remove `Hastings College of Law` and `UC San Francisco` and store the final data frame as a new object `univ_calif_final`.

```r
univ_calif_final <- UC_colleges %>% 
  filter(instnm!="University of California-Hastings College of Law") %>% 
  filter(instnm!="University of California-San Francisco")
univ_calif_final
```

```
## # A tibble: 8 × 10
##   instnm       city  stabbr zip   adm_rate sat_avg pcip26 costt4_a c150_4_pooled
##   <chr>        <chr> <chr>  <chr>    <dbl>   <dbl>  <dbl>    <dbl>         <dbl>
## 1 University … La J… CA     92093    0.357    1324  0.216    31043         0.872
## 2 University … Irvi… CA     92697    0.406    1206  0.107    31198         0.876
## 3 University … Rive… CA     92521    0.663    1078  0.149    31494         0.73 
## 4 University … Los … CA     9009…    0.180    1334  0.155    33078         0.911
## 5 University … Davis CA     9561…    0.423    1218  0.198    33904         0.850
## 6 University … Sant… CA     9506…    0.578    1201  0.193    34608         0.776
## 7 University … Berk… CA     94720    0.169    1422  0.105    34924         0.916
## 8 University … Sant… CA     93106    0.358    1281  0.108    34998         0.816
## # … with 1 more variable: pftftug1_ef <dbl>
```

Use `separate()` to separate institution name into two new columns "UNIV" and "CAMPUS".

```r
univ_calif_final2 <- univ_calif_final %>% 
  separate(instnm, into = c("univ", "campus"), sep = "-")
univ_calif_final2
```

```
## # A tibble: 8 × 11
##   univ  campus city  stabbr zip   adm_rate sat_avg pcip26 costt4_a c150_4_pooled
##   <chr> <chr>  <chr> <chr>  <chr>    <dbl>   <dbl>  <dbl>    <dbl>         <dbl>
## 1 Univ… San D… La J… CA     92093    0.357    1324  0.216    31043         0.872
## 2 Univ… Irvine Irvi… CA     92697    0.406    1206  0.107    31198         0.876
## 3 Univ… River… Rive… CA     92521    0.663    1078  0.149    31494         0.73 
## 4 Univ… Los A… Los … CA     9009…    0.180    1334  0.155    33078         0.911
## 5 Univ… Davis  Davis CA     9561…    0.423    1218  0.198    33904         0.850
## 6 Univ… Santa… Sant… CA     9506…    0.578    1201  0.193    34608         0.776
## 7 Univ… Berke… Berk… CA     94720    0.169    1422  0.105    34924         0.916
## 8 Univ… Santa… Sant… CA     93106    0.358    1281  0.108    34998         0.816
## # … with 1 more variable: pftftug1_ef <dbl>
```

9. The column `ADM_RATE` is the admissions rate by campus. Which UC has the lowest and highest admissions rates? Produce a numerical summary and an appropriate plot.

```r
univ_calif_final2 %>% 
  select(univ, campus, adm_rate) %>% 
  arrange(adm_rate)
```

```
## # A tibble: 8 × 3
##   univ                     campus        adm_rate
##   <chr>                    <chr>            <dbl>
## 1 University of California Berkeley         0.169
## 2 University of California Los Angeles      0.180
## 3 University of California San Diego        0.357
## 4 University of California Santa Barbara    0.358
## 5 University of California Irvine           0.406
## 6 University of California Davis            0.423
## 7 University of California Santa Cruz       0.578
## 8 University of California Riverside        0.663
```

UC Berkeley has the lowest admission rate, UC Riverside has the highest. 


```r
univ_calif_final2 %>% 
  ggplot(aes(x=campus, y=adm_rate))+
  geom_col()
```

![](lab9_hw_files/figure-html/unnamed-chunk-21-1.png)<!-- -->

10. If you wanted to get a degree in biological or biomedical sciences, which campus confers the majority of these degrees? Produce a numerical summary and an appropriate plot.


```r
names(college)
```

```
##  [1] "instnm"        "city"          "stabbr"        "zip"          
##  [5] "adm_rate"      "sat_avg"       "pcip26"        "costt4_a"     
##  [9] "c150_4_pooled" "pftftug1_ef"
```


```r
college %>% 
  select(instnm, pcip26) %>% 
  arrange(desc(pcip26))
```

```
## # A tibble: 341 × 2
##    instnm                                            pcip26
##    <chr>                                              <dbl>
##  1 University of California-San Diego                 0.216
##  2 University of California-Davis                     0.198
##  3 University of California-Santa Cruz                0.193
##  4 Pomona College                                     0.171
##  5 University of California-Los Angeles               0.155
##  6 Scripps College                                    0.152
##  7 University of California-Riverside                 0.149
##  8 Occidental College                                 0.137
##  9 Southern California University of Health Sciences  0.136
## 10 Notre Dame de Namur University                     0.135
## # … with 331 more rows
```


```r
college %>% 
  select(instnm, pcip26) %>% 
  top_n(+10, pcip26) %>% 
  ggplot(aes(x=instnm, y=pcip26))+
  geom_col()+
  coord_flip()
```

![](lab9_hw_files/figure-html/unnamed-chunk-24-1.png)<!-- -->

## Knit Your Output and Post to [GitHub](https://github.com/FRS417-DataScienceBiologists)
