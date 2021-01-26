---
title: "Midterm 1"
author: "Chloe Tannous"
date: "2021-01-26"
output:
  html_document: 
    theme: spacelab
    keep_md: yes
---



## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your code should be organized, clean, and run free from errors. Be sure to **add your name** to the author header above. You may use any resources to answer these questions (including each other), but you may not post questions to Open Stacks or external help sites. There are 12 total questions.  

Make sure to use the formatting conventions of RMarkdown to make your report neat and clean!  

This exam is due by **12:00p on Thursday, January 28**.  

## Load the tidyverse
If you plan to use any other libraries to complete this assignment then you should load them here.

```r
library(tidyverse)
library(janitor)
```

## Questions
**1. (2 points) Briefly explain how R, RStudio, and GitHub work together to make work flows in data science transparent and repeatable. What is the advantage of using RMarkdown in this context?**  

R is the programming language that is used in Rstudio. Github is used to upload the R programs to the internet, to make them accesible for others to view and learn from. Rmarkdown makes the code easier to look as its formatted in a html file.

**2. (2 points) What are the three types of `data structures` that we have discussed? Why are we using data frames for BIS 15L?**
Data frames, vectors, and matrcies are the three data structures that have been discussed. The data frames are most frequently used because they are the easiest to work with, as they compile lots of observations. Additionally, using the dpylr functions make data frames more convient to use. 

In the midterm 1 folder there is a second folder called `data`. Inside the `data` folder, there is a .csv file called `ElephantsMF`. These data are from Phyllis Lee, Stirling University, and are related to Lee, P., et al. (2013), "Enduring consequences of early experiences: 40-year effects on survival and success among African elephants (Loxodonta africana)," Biology Letters, 9: 20130011. [kaggle](https://www.kaggle.com/mostafaelseidy/elephantsmf).  

**3. (2 points) Please load these data as a new object called `elephants`. Use the function(s) of your choice to get an idea of the structure of the data. Be sure to show the class of each variable.**

```r
elephants <- readr::read_csv("data/ElephantsMF.csv")
```

```
## 
## ── Column specification ────────────────────────────────────────────────────────
## cols(
##   Age = col_double(),
##   Height = col_double(),
##   Sex = col_character()
## )
```

```r
glimpse(elephants)
```

```
## Rows: 288
## Columns: 3
## $ Age    <dbl> 1.40, 17.50, 12.75, 11.17, 12.67, 12.67, 12.25, 12.17, 28.17, …
## $ Height <dbl> 120.00, 227.00, 235.00, 210.00, 220.00, 189.00, 225.00, 204.00…
## $ Sex    <chr> "M", "M", "M", "M", "M", "M", "M", "M", "M", "M", "M", "M", "M…
```
**4. (2 points) Change the names of the variables to lower case and change the class of the variable `sex` to a factor.**

```r
elephants <- clean_names(elephants)
names(elephants)
```

```
## [1] "age"    "height" "sex"
```

```r
elephants$sex <- as.factor(elephants$sex)
levels(elephants$sex)
```

```
## [1] "F" "M"
```
**5. (2 points) How many male and female elephants are represented in the data?**

```r
elephants %>%
  tabyl(sex)
```

```
##  sex   n   percent
##    F 150 0.5208333
##    M 138 0.4791667
```
There are 150 female elephants and 138 male elephants.

**6. (2 points) What is the average age all elephants in the data?**

```r
elephants %>%
  select(age) %>%
  summarise(average.age = mean(age))
```

```
## # A tibble: 1 x 1
##   average.age
##         <dbl>
## 1        11.0
```
The average age is 10.97

**7. (2 points) How does the average age and height of elephants compare by sex?**

```r
elephants %>%
  select(age, sex, height) %>%
  group_by(sex) %>%
  summarize(across(c(age, height), mean,na.rm = T))
```

```
## # A tibble: 2 x 3
##   sex     age height
## * <fct> <dbl>  <dbl>
## 1 F     12.8    190.
## 2 M      8.95   185.
```
Females are typically taller, and have a higher average age. 


**8. (2 points) How does the average height of elephants compare by sex for individuals over 25 years old. Include the min and max height as well as the number of individuals in the sample as part of your analysis.**

```r
elephants %>%
  select(age, sex, height) %>%
  filter(age > 25) %>%
  group_by(sex) %>%
  summarize(mean.height = mean(height),
            max.height = max(height),
            min.height = min(height)
        , n_sample=n())
```

```
## # A tibble: 2 x 5
##   sex   mean.height max.height min.height n_sample
## * <fct>       <dbl>      <dbl>      <dbl>    <int>
## 1 F            233.       278.       206.       25
## 2 M            273.       304.       237.        8
```
It seems like as elephants grow older, male elephants grow taller than female elephants. However, its important to realize that the sample size for male elephants is a bit smaller

For the next series of questions, we will use data from a study on vertebrate community composition and impacts from defaunation in [Gabon, Africa](https://en.wikipedia.org/wiki/Gabon). One thing to notice is that the data include 24 separate transects. Each transect represents a path through different forest management areas.  

Reference: Koerner SE, Poulsen JR, Blanchard EJ, Okouyi J, Clark CJ. Vertebrate community composition and diversity declines along a defaunation gradient radiating from rural villages in Gabon. _Journal of Applied Ecology_. 2016. This paper, along with a description of the variables is included inside the midterm 1 folder.  

**9. (2 points) Load `IvindoData_DryadVersion.csv` and use the function(s) of your choice to get an idea of the overall structure. Change the variables `HuntCat` and `LandUse` to factors.**

```r
vertebrate <- readr::read_csv("data/IvindoData_DryadVersion.csv")
```

```
## 
## ── Column specification ────────────────────────────────────────────────────────
## cols(
##   .default = col_double(),
##   HuntCat = col_character(),
##   LandUse = col_character()
## )
## ℹ Use `spec()` for the full column specifications.
```

```r
vertebrate <- clean_names(vertebrate)
names(vertebrate)
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
glimpse(vertebrate)
```

```
## Rows: 24
## Columns: 26
## $ transect_id              <dbl> 1, 2, 2, 3, 4, 5, 6, 7, 8, 9, 13, 14, 15, 16…
## $ distance                 <dbl> 7.14, 17.31, 18.32, 20.85, 15.95, 17.47, 24.…
## $ hunt_cat                 <chr> "Moderate", "None", "None", "None", "None", …
## $ num_households           <dbl> 54, 54, 29, 29, 29, 29, 29, 54, 25, 73, 46, …
## $ land_use                 <chr> "Park", "Park", "Park", "Logging", "Park", "…
## $ veg_rich                 <dbl> 16.67, 15.75, 16.88, 12.44, 17.13, 16.50, 14…
## $ veg_stems                <dbl> 31.20, 37.44, 32.33, 29.39, 36.00, 29.22, 31…
## $ veg_liana                <dbl> 5.78, 13.25, 4.75, 9.78, 13.25, 12.88, 8.38,…
## $ veg_dbh                  <dbl> 49.57, 34.59, 42.82, 36.62, 41.52, 44.07, 51…
## $ veg_canopy               <dbl> 3.78, 3.75, 3.43, 3.75, 3.88, 2.50, 4.00, 4.…
## $ veg_understory           <dbl> 2.89, 3.88, 3.00, 2.75, 3.25, 3.00, 2.38, 2.…
## $ ra_apes                  <dbl> 1.87, 0.00, 4.49, 12.93, 0.00, 2.48, 3.78, 6…
## $ ra_birds                 <dbl> 52.66, 52.17, 37.44, 59.29, 52.62, 38.64, 42…
## $ ra_elephant              <dbl> 0.00, 0.86, 1.33, 0.56, 1.00, 0.00, 1.11, 0.…
## $ ra_monkeys               <dbl> 38.59, 28.53, 41.82, 19.85, 41.34, 43.29, 46…
## $ ra_rodent                <dbl> 4.22, 6.04, 1.06, 3.66, 2.52, 1.83, 3.10, 1.…
## $ ra_ungulate              <dbl> 2.66, 12.41, 13.86, 3.71, 2.53, 13.75, 3.10,…
## $ rich_all_species         <dbl> 22, 20, 22, 19, 20, 22, 23, 19, 19, 19, 21, …
## $ evenness_all_species     <dbl> 0.793, 0.773, 0.740, 0.681, 0.811, 0.786, 0.…
## $ diversity_all_species    <dbl> 2.452, 2.314, 2.288, 2.006, 2.431, 2.429, 2.…
## $ rich_bird_species        <dbl> 11, 10, 11, 8, 8, 10, 11, 11, 11, 9, 11, 11,…
## $ evenness_bird_species    <dbl> 0.732, 0.704, 0.688, 0.559, 0.799, 0.771, 0.…
## $ diversity_bird_species   <dbl> 1.756, 1.620, 1.649, 1.162, 1.660, 1.775, 1.…
## $ rich_mammal_species      <dbl> 11, 10, 11, 11, 12, 12, 12, 8, 8, 10, 10, 11…
## $ evenness_mammal_species  <dbl> 0.736, 0.705, 0.650, 0.619, 0.736, 0.694, 0.…
## $ diversity_mammal_species <dbl> 1.764, 1.624, 1.558, 1.484, 1.829, 1.725, 1.…
```

```r
vertebrate$hunt_cat <- as.factor(vertebrate$hunt_cat)
levels(vertebrate$hunt_cat)
```

```
## [1] "High"     "Moderate" "None"
```

```r
vertebrate$land_use <- as.factor(vertebrate$land_use)
levels(vertebrate$land_use)
```

```
## [1] "Logging" "Neither" "Park"
```

**10. (4 points) For the transects with high and moderate hunting intensity, how does the average diversity of birds and mammals compare?**

```r
vertebrate %>%
  filter(hunt_cat != "None") %>%
  group_by(hunt_cat) %>%
  summarise(across(c(diversity_bird_species, diversity_mammal_species),mean, na.rm = T ))
```

```
## # A tibble: 2 x 3
##   hunt_cat diversity_bird_species diversity_mammal_species
## * <fct>                     <dbl>                    <dbl>
## 1 High                       1.66                     1.74
## 2 Moderate                   1.62                     1.68
```
The high intensity has a greater diveristy in bird species and diversity mammal species. 

**11. (4 points) One of the conclusions in the study is that the relative abundance of animals drops off the closer you get to a village. Let's try to reconstruct this (without the statistics). How does the relative abundance (RA) of apes, birds, elephants, monkeys, rodents, and ungulates compare between sites that are less than 5km from a village to sites that are greater than 20km from a village? The variable `Distance` measures the distance of the transect from the nearest village. Hint: try using the `across` operator.**  

```r
vertebrate %>%
  mutate(r.distance = case_when( distance < 5 ~ "Less than 5 km", distance > 20 ~ "Greater than 20km")) %>%
  group_by(r.distance) %>%
    filter(!is.na(r.distance)) %>%
  summarise(across(starts_with("ra"), mean, na.rm = T))
```

```
## # A tibble: 2 x 7
##   r.distance       ra_apes ra_birds ra_elephant ra_monkeys ra_rodent ra_ungulate
##   <chr>              <dbl>    <dbl>       <dbl>      <dbl>     <dbl>       <dbl>
## 1 Greater than 20…    7.21     44.5      0.557        40.1      2.68        4.98
## 2 Less than 5 km      0.08     70.4      0.0967       24.1      3.66        1.59
```
It seems like the apes have a higher RA value the farther they are from the village.

**12. (4 points) Based on your interest, do one exploratory analysis on the `gabon` data of your choice. This analysis needs to include a minimum of two functions in `dplyr.`**

```r
vertebrate %>%
 summarise(across(starts_with("ra"), max, na.rm = T))
```

```
## # A tibble: 1 x 6
##   ra_apes ra_birds ra_elephant ra_monkeys ra_rodent ra_ungulate
##     <dbl>    <dbl>       <dbl>      <dbl>     <dbl>       <dbl>
## 1    12.9     85.0         2.3       54.1      6.31        13.9
```

My code shows the maximum values across all the relative abudance values. From this you can see that monkeys have the highest relative abudance rate.
