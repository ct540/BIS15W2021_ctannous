---
title: "Lab 10 Homework"
author: "Chloe Tannous"
date: "2021-02-15"
output:
  html_document: 
    theme: spacelab
    keep_md: yes
---



## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your final lab report should be organized, clean, and run free from errors. Remember, you must remove the `#` for the included code chunks to run. Be sure to add your name to the author header above. For any included plots, make sure they are clearly labeled. You are free to use any plot type that you feel best communicates the results of your analysis.  

Make sure to use the formatting conventions of RMarkdown to make your report neat and clean!  

## Load the libraries

```r
library(tidyverse)
library(janitor)
library(here)
library(naniar)
```

## Desert Ecology
For this assignment, we are going to use a modified data set on [desert ecology](http://esapubs.org/archive/ecol/E090/118/). The data are from: S. K. Morgan Ernest, Thomas J. Valone, and James H. Brown. 2009. Long-term monitoring and experimental manipulation of a Chihuahuan Desert ecosystem near Portal, Arizona, USA. Ecology 90:1708.

```r
deserts <- read_csv(here("lab10", "data", "surveys_complete.csv"))
```

```
## 
## -- Column specification --------------------------------------------------------
## cols(
##   record_id = col_double(),
##   month = col_double(),
##   day = col_double(),
##   year = col_double(),
##   plot_id = col_double(),
##   species_id = col_character(),
##   sex = col_character(),
##   hindfoot_length = col_double(),
##   weight = col_double(),
##   genus = col_character(),
##   species = col_character(),
##   taxa = col_character(),
##   plot_type = col_character()
## )
```

1. Use the function(s) of your choice to get an idea of its structure, including how NA's are treated. Are the data tidy?  


```r
deserts
```

```
## # A tibble: 34,786 x 13
##    record_id month   day  year plot_id species_id sex   hindfoot_length weight
##        <dbl> <dbl> <dbl> <dbl>   <dbl> <chr>      <chr>           <dbl>  <dbl>
##  1         1     7    16  1977       2 NL         M                  32     NA
##  2         2     7    16  1977       3 NL         M                  33     NA
##  3         3     7    16  1977       2 DM         F                  37     NA
##  4         4     7    16  1977       7 DM         M                  36     NA
##  5         5     7    16  1977       3 DM         M                  35     NA
##  6         6     7    16  1977       1 PF         M                  14     NA
##  7         7     7    16  1977       2 PE         F                  NA     NA
##  8         8     7    16  1977       1 DM         M                  37     NA
##  9         9     7    16  1977       1 DM         F                  34     NA
## 10        10     7    16  1977       6 PF         F                  20     NA
## # ... with 34,776 more rows, and 4 more variables: genus <chr>, species <chr>,
## #   taxa <chr>, plot_type <chr>
```

```r
skimr::skim(deserts)
```


Table: Data summary

|                         |        |
|:------------------------|:-------|
|Name                     |deserts |
|Number of rows           |34786   |
|Number of columns        |13      |
|_______________________  |        |
|Column type frequency:   |        |
|character                |6       |
|numeric                  |7       |
|________________________ |        |
|Group variables          |None    |


**Variable type: character**

|skim_variable | n_missing| complete_rate| min| max| empty| n_unique| whitespace|
|:-------------|---------:|-------------:|---:|---:|-----:|--------:|----------:|
|species_id    |         0|          1.00|   2|   2|     0|       48|          0|
|sex           |      1748|          0.95|   1|   1|     0|        2|          0|
|genus         |         0|          1.00|   6|  16|     0|       26|          0|
|species       |         0|          1.00|   3|  15|     0|       40|          0|
|taxa          |         0|          1.00|   4|   7|     0|        4|          0|
|plot_type     |         0|          1.00|   7|  25|     0|        5|          0|


**Variable type: numeric**

|skim_variable   | n_missing| complete_rate|     mean|       sd|   p0|     p25|     p50|      p75|  p100|hist  |
|:---------------|---------:|-------------:|--------:|--------:|----:|-------:|-------:|--------:|-----:|:-----|
|record_id       |         0|          1.00| 17804.20| 10229.68|    1| 8964.25| 17761.5| 26654.75| 35548|▇▇▇▇▇ |
|month           |         0|          1.00|     6.47|     3.40|    1|    4.00|     6.0|    10.00|    12|▇▆▆▅▇ |
|day             |         0|          1.00|    16.10|     8.25|    1|    9.00|    16.0|    23.00|    31|▆▇▇▇▆ |
|year            |         0|          1.00|  1990.50|     7.47| 1977| 1984.00|  1990.0|  1997.00|  2002|▇▆▇▇▇ |
|plot_id         |         0|          1.00|    11.34|     6.79|    1|    5.00|    11.0|    17.00|    24|▇▆▇▆▅ |
|hindfoot_length |      3348|          0.90|    29.29|     9.56|    2|   21.00|    32.0|    36.00|    70|▁▇▇▁▁ |
|weight          |      2503|          0.93|    42.67|    36.63|    4|   20.00|    37.0|    48.00|   280|▇▁▁▁▁ |

```r
naniar::miss_var_summary(deserts)
```

```
## # A tibble: 13 x 3
##    variable        n_miss pct_miss
##    <chr>            <int>    <dbl>
##  1 hindfoot_length   3348     9.62
##  2 weight            2503     7.20
##  3 sex               1748     5.03
##  4 record_id            0     0   
##  5 month                0     0   
##  6 day                  0     0   
##  7 year                 0     0   
##  8 plot_id              0     0   
##  9 species_id           0     0   
## 10 genus                0     0   
## 11 species              0     0   
## 12 taxa                 0     0   
## 13 plot_type            0     0
```

2. How many genera and species are represented in the data? What are the total number of observations? Which species is most/ least frequently sampled in the study?

```r
deserts %>%
  summarize(genus_n = n_distinct (genus), species_n = n_distinct(species), observations = n())
```

```
## # A tibble: 1 x 3
##   genus_n species_n observations
##     <int>     <int>        <int>
## 1      26        40        34786
```

```r
deserts%>%
  tabyl(species)%>%
  arrange(desc(n))
```

```
##          species     n      percent
##         merriami 10596 3.046053e-01
##     penicillatus  3123 8.977750e-02
##            ordii  3027 8.701777e-02
##          baileyi  2891 8.310815e-02
##        megalotis  2609 7.500144e-02
##      spectabilis  2504 7.198298e-02
##         torridus  2249 6.465245e-02
##           flavus  1597 4.590927e-02
##         eremicus  1299 3.734261e-02
##         albigula  1252 3.599149e-02
##      leucogaster  1006 2.891968e-02
##      maniculatus   899 2.584373e-02
##          harrisi   437 1.256253e-02
##        bilineata   303 8.710401e-03
##        spilosoma   248 7.129305e-03
##         hispidus   179 5.145748e-03
##              sp.    86 2.472259e-03
##        audubonii    75 2.156040e-03
##       fulvescens    75 2.156040e-03
##  brunneicapillus    50 1.437360e-03
##          taylori    46 1.322371e-03
##      fulviventer    43 1.236129e-03
##     ochrognathus    43 1.236129e-03
##        chlorurus    39 1.121141e-03
##         leucopus    36 1.034899e-03
##         squamata    16 4.599552e-04
##      melanocorys    13 3.737136e-04
##      intermedius     9 2.587248e-04
##        gramineus     8 2.299776e-04
##         montanus     8 2.299776e-04
##           fuscus     5 1.437360e-04
##        undulatus     5 1.437360e-04
##       leucophrys     2 5.749439e-05
##       savannarum     2 5.749439e-05
##           clarki     1 2.874720e-05
##       scutalatus     1 2.874720e-05
##     tereticaudus     1 2.874720e-05
##           tigris     1 2.874720e-05
##        uniparens     1 2.874720e-05
##          viridis     1 2.874720e-05
```
Merriami is the most represented. The least represented are clarki, scutalatus, tereticaudus, tigris, uniparens, viridis

3. What is the proportion of taxa included in this study? Show a table and plot that reflects this count.

```r
deserts %>%
  count(taxa)
```

```
## # A tibble: 4 x 2
##   taxa        n
## * <chr>   <int>
## 1 Bird      450
## 2 Rabbit     75
## 3 Reptile    14
## 4 Rodent  34247
```

```r
deserts %>%
  ggplot(aes(x = taxa)) + geom_bar()+
  scale_y_log10() +
  labs(title = "Taxa in the Deserts",
       x = "Taxa",
       y= "Count (log10)") +
 theme(plot.title = element_text(size = rel(4), hjust = 0.1))
```

![](lab10_hw_files/figure-html/unnamed-chunk-9-1.png)<!-- -->
4. For the taxa included in the study, use the fill option to show the proportion of individuals sampled by `plot_type.`

```r
deserts %>%
  ggplot(aes(x = taxa, fill = plot_type)) + geom_bar()+
  scale_y_log10() +
  labs(title = "Taxa in the Deserts",
       x = "Taxa",
       y= "Count (log10)")
```

![](lab10_hw_files/figure-html/unnamed-chunk-10-1.png)<!-- -->
5. What is the range of weight for each species included in the study? Remove any observations of weight that are NA so they do not show up in the plot.

```r
deserts %>% 
  filter(!is.na(weight)) %>% 
  ggplot(aes(x = species, y = weight, color = species)) +
  geom_boxplot() +
  theme(axis.text.x = element_text(angle = 60, hjust = 1), plot.title = element_text(size = rel(2), hjust = .5)) +
  labs(title = "Weight Range of Each Species",
       x = "Species",
       y= "Weight")
```

![](lab10_hw_files/figure-html/unnamed-chunk-11-1.png)<!-- -->

6. Add another layer to your answer from #4 using `geom_point` to get an idea of how many measurements were taken for each species.

```r
deserts %>% 
  filter(!is.na(weight)) %>% 
  ggplot(aes(x = species, y = weight, color = species)) +
  geom_boxplot() +
  coord_flip() +
  geom_point(size = .2) +
  theme(axis.text.x = element_text(angle = 60, hjust = 1), plot.title = element_text(size = rel(2), hjust = .5)) +
  labs(title = "Weight Range of Each Species",
       x = "Species",
       y= "Weight")
```

![](lab10_hw_files/figure-html/unnamed-chunk-12-1.png)<!-- -->

7. [Dipodomys merriami](https://en.wikipedia.org/wiki/Merriam's_kangaroo_rat) is the most frequently sampled animal in the study. How have the number of observations of this species changed over the years included in the study?


```r
deserts %>% 
  filter(genus=="Dipodomys", species=="merriami") %>% 
  ggplot(aes(x=year))+
  geom_bar()+ 
  theme(axis.text.x = element_text(angle = 60, hjust = 1), plot.title = element_text(size = rel(2), hjust = .5)) +
  labs(title = "Merriami Weight Fluctuation",
       x = "Year",
       y= "Weight")
```

![](lab10_hw_files/figure-html/unnamed-chunk-13-1.png)<!-- -->

8. What is the relationship between `weight` and `hindfoot` length? Consider whether or not over plotting is an issue.

```r
deserts %>% 
  filter(weight!="NA") %>%
  ggplot(aes(x=weight, y= hindfoot_length)) + 
  geom_point(size = .02, na.rm = T) +
  labs(title="Elephant Weight vs. Hindfoot Lenghth",
       x="Weight",
       y="Hindfoot Length")
```

![](lab10_hw_files/figure-html/unnamed-chunk-14-1.png)<!-- -->

<style>
div.blue { background-color:#e6f0ff; border-radius: 5px; padding: 20px;}
</style>
<div class = "blue">

9. Which two species have, on average, the highest weight? Once you have identified them, make a new column that is a ratio of `weight` to `hindfoot_length`. Make a plot that shows the range of this new ratio and fill by sex.

```r
deserts %>% 
  filter(weight!="NA") %>% 
  group_by(species) %>% 
  summarise(avg_weight=mean(weight, na.rm=T)) %>% 
  arrange(desc(avg_weight))
```

```
## # A tibble: 22 x 2
##    species      avg_weight
##    <chr>             <dbl>
##  1 albigula          159. 
##  2 spectabilis       120. 
##  3 spilosoma          93.5
##  4 hispidus           65.6
##  5 fulviventer        58.9
##  6 ochrognathus       55.4
##  7 ordii              48.9
##  8 merriami           43.2
##  9 baileyi            31.7
## 10 leucogaster        31.6
## # ... with 12 more rows
```

```r
deserts_avg <- deserts %>% 
  filter(species == "albigula" | species == "spectabilis") %>% 
  filter(!is.na(weight)) %>% 
   filter(!is.na(sex)) %>% 
  filter(!is.na(hindfoot_length)) %>% 
  mutate(weight_hindfoot_ratio = weight/hindfoot_length)
deserts_avg
```

```
## # A tibble: 3,068 x 14
##    record_id month   day  year plot_id species_id sex   hindfoot_length weight
##        <dbl> <dbl> <dbl> <dbl>   <dbl> <chr>      <chr>           <dbl>  <dbl>
##  1       357    11    12  1977       9 DS         F                  50    117
##  2       362    11    12  1977       1 DS         F                  51    121
##  3       367    11    12  1977      20 DS         M                  51    115
##  4       377    11    12  1977       9 DS         F                  48    120
##  5       381    11    13  1977      17 DS         F                  48    118
##  6       383    11    13  1977      11 DS         F                  52    126
##  7       385    11    13  1977      17 DS         M                  50    132
##  8       392    11    13  1977      11 DS         F                  53    122
##  9       394    11    13  1977       4 DS         F                  48    107
## 10       398    11    13  1977       4 DS         F                  50    115
## # ... with 3,058 more rows, and 5 more variables: genus <chr>, species <chr>,
## #   taxa <chr>, plot_type <chr>, weight_hindfoot_ratio <dbl>
```
</div>

10. Make one plot of your choice! Make sure to include at least two of the aesthetics options you have learned.

```r
deserts %>%
  filter( taxa == "Reptile") %>%
  ggplot(aes(x=genus, fill = species)) +
  geom_bar() +
  theme(axis.text.x = element_text(angle = 60, hjust = 1), plot.title = element_text(size = rel(2), hjust = .5)) +
  labs(title = "Genus Breakdown of Reptiles by Species",
       x = "Genus",
       y= "Count")
```

![](lab10_hw_files/figure-html/unnamed-chunk-17-1.png)<!-- -->

## Push your final code to GitHub!
Please be sure that you check the `keep md` file in the knit preferences. 
