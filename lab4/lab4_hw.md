---
title: "Lab 4 Homework"
author: "Chloe Tannous"
date: "2021-01-24"
output:
  html_document: 
    theme: spacelab
    keep_md: yes
---



## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your final lab report should be organized, clean, and run free from errors. Remember, you must remove the `#` for the included code chunks to run. Be sure to add your name to the author header above.  

Make sure to use the formatting conventions of RMarkdown to make your report neat and clean!  

## Load the tidyverse

```r
library(tidyverse)
```

## Data
For the homework, we will use data about vertebrate home range sizes. The data are in the class folder, but the reference is below.  

**Database of vertebrate home range sizes.**  
Reference: Tamburello N, Cote IM, Dulvy NK (2015) Energy and the scaling of animal space use. The American Naturalist 186(2):196-211. http://dx.doi.org/10.1086/682070.  
Data: http://datadryad.org/resource/doi:10.5061/dryad.q5j65/1  

**1. Load the data into a new object called `homerange`.**

```r
homerange <-readr::read_csv("data/Tamburelloetal_HomeRangeDatabase.csv")
```

```
## Parsed with column specification:
## cols(
##   .default = col_character(),
##   mean.mass.g = col_double(),
##   log10.mass = col_double(),
##   mean.hra.m2 = col_double(),
##   log10.hra = col_double(),
##   preymass = col_double(),
##   log10.preymass = col_double(),
##   PPMR = col_double()
## )
```

```
## See spec(...) for full column specifications.
```

**2. Explore the data. Show the dimensions, column names, classes for each variable, and a statistical summary. Keep these as separate code chunks.**  

```r
dim(homerange)
```

```
## [1] 569  24
```


```r
colnames(homerange)
```

```
##  [1] "taxon"                      "common.name"               
##  [3] "class"                      "order"                     
##  [5] "family"                     "genus"                     
##  [7] "species"                    "primarymethod"             
##  [9] "N"                          "mean.mass.g"               
## [11] "log10.mass"                 "alternative.mass.reference"
## [13] "mean.hra.m2"                "log10.hra"                 
## [15] "hra.reference"              "realm"                     
## [17] "thermoregulation"           "locomotion"                
## [19] "trophic.guild"              "dimension"                 
## [21] "preymass"                   "log10.preymass"            
## [23] "PPMR"                       "prey.size.reference"
```

```r
str(homerange)
```

```
## Classes 'spec_tbl_df', 'tbl_df', 'tbl' and 'data.frame':	569 obs. of  24 variables:
##  $ taxon                     : chr  "lake fishes" "river fishes" "river fishes" "river fishes" ...
##  $ common.name               : chr  "american eel" "blacktail redhorse" "central stoneroller" "rosyside dace" ...
##  $ class                     : chr  "actinopterygii" "actinopterygii" "actinopterygii" "actinopterygii" ...
##  $ order                     : chr  "anguilliformes" "cypriniformes" "cypriniformes" "cypriniformes" ...
##  $ family                    : chr  "anguillidae" "catostomidae" "cyprinidae" "cyprinidae" ...
##  $ genus                     : chr  "anguilla" "moxostoma" "campostoma" "clinostomus" ...
##  $ species                   : chr  "rostrata" "poecilura" "anomalum" "funduloides" ...
##  $ primarymethod             : chr  "telemetry" "mark-recapture" "mark-recapture" "mark-recapture" ...
##  $ N                         : chr  "16" NA "20" "26" ...
##  $ mean.mass.g               : num  887 562 34 4 4 ...
##  $ log10.mass                : num  2.948 2.75 1.531 0.602 0.602 ...
##  $ alternative.mass.reference: chr  NA NA NA NA ...
##  $ mean.hra.m2               : num  282750 282.1 116.1 125.5 87.1 ...
##  $ log10.hra                 : num  5.45 2.45 2.06 2.1 1.94 ...
##  $ hra.reference             : chr  "Minns, C. K. 1995. Allometry of home range size in lake and river fishes. Canadian Journal of Fisheries and Aquatic Sciences 52 "Minns, C. K. 1995. Allometry of home range size in lake and river fishes. Canadian Journal of Fisheries and Aquatic Sciences 52 "Minns, C. K. 1995. Allometry of home range size in lake and river fishes. Canadian Journal of Fisheries and Aquatic Sciences 52 "Minns, C. K. 1995. Allometry of home range size in lake and river fishes. Canadian Journal of Fisheries and Aquatic Sciences 52 ...
##  $ realm                     : chr  "aquatic" "aquatic" "aquatic" "aquatic" ...
##  $ thermoregulation          : chr  "ectotherm" "ectotherm" "ectotherm" "ectotherm" ...
##  $ locomotion                : chr  "swimming" "swimming" "swimming" "swimming" ...
##  $ trophic.guild             : chr  "carnivore" "carnivore" "carnivore" "carnivore" ...
##  $ dimension                 : chr  "3D" "2D" "2D" "2D" ...
##  $ preymass                  : num  NA NA NA NA NA NA 1.39 NA NA NA ...
##  $ log10.preymass            : num  NA NA NA NA NA ...
##  $ PPMR                      : num  NA NA NA NA NA NA 530 NA NA NA ...
##  $ prey.size.reference       : chr  NA NA NA NA ...
##  - attr(*, "spec")=
##   .. cols(
##   ..   taxon = col_character(),
##   ..   common.name = col_character(),
##   ..   class = col_character(),
##   ..   order = col_character(),
##   ..   family = col_character(),
##   ..   genus = col_character(),
##   ..   species = col_character(),
##   ..   primarymethod = col_character(),
##   ..   N = col_character(),
##   ..   mean.mass.g = col_double(),
##   ..   log10.mass = col_double(),
##   ..   alternative.mass.reference = col_character(),
##   ..   mean.hra.m2 = col_double(),
##   ..   log10.hra = col_double(),
##   ..   hra.reference = col_character(),
##   ..   realm = col_character(),
##   ..   thermoregulation = col_character(),
##   ..   locomotion = col_character(),
##   ..   trophic.guild = col_character(),
##   ..   dimension = col_character(),
##   ..   preymass = col_double(),
##   ..   log10.preymass = col_double(),
##   ..   PPMR = col_double(),
##   ..   prey.size.reference = col_character()
##   .. )
```

```r
glimpse(homerange)
```

```
## Observations: 569
## Variables: 24
## $ taxon                      <chr> "lake fishes", "river fishes", "river fish…
## $ common.name                <chr> "american eel", "blacktail redhorse", "cen…
## $ class                      <chr> "actinopterygii", "actinopterygii", "actin…
## $ order                      <chr> "anguilliformes", "cypriniformes", "cyprin…
## $ family                     <chr> "anguillidae", "catostomidae", "cyprinidae…
## $ genus                      <chr> "anguilla", "moxostoma", "campostoma", "cl…
## $ species                    <chr> "rostrata", "poecilura", "anomalum", "fund…
## $ primarymethod              <chr> "telemetry", "mark-recapture", "mark-recap…
## $ N                          <chr> "16", NA, "20", "26", "17", "5", "2", "2",…
## $ mean.mass.g                <dbl> 887.00, 562.00, 34.00, 4.00, 4.00, 3525.00…
## $ log10.mass                 <dbl> 2.9479236, 2.7497363, 1.5314789, 0.6020600…
## $ alternative.mass.reference <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA…
## $ mean.hra.m2                <dbl> 282750.00, 282.10, 116.11, 125.50, 87.10, …
## $ log10.hra                  <dbl> 5.4514026, 2.4504031, 2.0648696, 2.0986437…
## $ hra.reference              <chr> "Minns, C. K. 1995. Allometry of home rang…
## $ realm                      <chr> "aquatic", "aquatic", "aquatic", "aquatic"…
## $ thermoregulation           <chr> "ectotherm", "ectotherm", "ectotherm", "ec…
## $ locomotion                 <chr> "swimming", "swimming", "swimming", "swimm…
## $ trophic.guild              <chr> "carnivore", "carnivore", "carnivore", "ca…
## $ dimension                  <chr> "3D", "2D", "2D", "2D", "2D", "2D", "2D", …
## $ preymass                   <dbl> NA, NA, NA, NA, NA, NA, 1.39, NA, NA, NA, …
## $ log10.preymass             <dbl> NA, NA, NA, NA, NA, NA, 0.1430148, NA, NA,…
## $ PPMR                       <dbl> NA, NA, NA, NA, NA, NA, 530, NA, NA, NA, N…
## $ prey.size.reference        <chr> NA, NA, NA, NA, NA, NA, "Brose U, et al. 2…
```

**3. Change the class of the variables `taxon` and `order` to factors and display their levels.**  

```r
homerange$taxon <- as.factor(homerange$taxon)
levels(homerange$taxon)
```

```
## [1] "birds"         "lake fishes"   "lizards"       "mammals"      
## [5] "marine fishes" "river fishes"  "snakes"        "tortoises"    
## [9] "turtles"
```

```r
homerange$order <- as.factor(homerange$order)
levels(homerange$order)
```

```
##  [1] "accipitriformes"    "afrosoricida"       "anguilliformes"    
##  [4] "anseriformes"       "apterygiformes"     "artiodactyla"      
##  [7] "caprimulgiformes"   "carnivora"          "charadriiformes"   
## [10] "columbidormes"      "columbiformes"      "coraciiformes"     
## [13] "cuculiformes"       "cypriniformes"      "dasyuromorpha"     
## [16] "dasyuromorpia"      "didelphimorphia"    "diprodontia"       
## [19] "diprotodontia"      "erinaceomorpha"     "esociformes"       
## [22] "falconiformes"      "gadiformes"         "galliformes"       
## [25] "gruiformes"         "lagomorpha"         "macroscelidea"     
## [28] "monotrematae"       "passeriformes"      "pelecaniformes"    
## [31] "peramelemorphia"    "perciformes"        "perissodactyla"    
## [34] "piciformes"         "pilosa"             "proboscidea"       
## [37] "psittaciformes"     "rheiformes"         "roden"             
## [40] "rodentia"           "salmoniformes"      "scorpaeniformes"   
## [43] "siluriformes"       "soricomorpha"       "squamata"          
## [46] "strigiformes"       "struthioniformes"   "syngnathiformes"   
## [49] "testudines"         "tetraodontiformes\xa0" "tinamiformes"
```

**4. What taxa are represented in the `homerange` data frame? Make a new data frame `taxa` that is restricted to taxon, common name, class, order, family, genus, species.**  

```r
taxa <- data.frame(select(homerange, "taxon","common.name", "class", "order", "family", "genus", "species"))
glimpse(taxa)
```

```
## Observations: 569
## Variables: 7
## $ taxon       <fct> lake fishes, river fishes, river fishes, river fishes, ri…
## $ common.name <chr> "american eel", "blacktail redhorse", "central stonerolle…
## $ class       <chr> "actinopterygii", "actinopterygii", "actinopterygii", "ac…
## $ order       <fct> anguilliformes, cypriniformes, cypriniformes, cypriniform…
## $ family      <chr> "anguillidae", "catostomidae", "cyprinidae", "cyprinidae"…
## $ genus       <chr> "anguilla", "moxostoma", "campostoma", "clinostomus", "rh…
## $ species     <chr> "rostrata", "poecilura", "anomalum", "funduloides", "cata…
```
**5. The variable `taxon` identifies the large, common name groups of the species represented in `homerange`. Make a table the shows the counts for each of these `taxon`.**  

```r
table(homerange$taxon)
```

```
## 
##         birds   lake fishes       lizards       mammals marine fishes 
##           140             9            11           238            90 
##  river fishes        snakes     tortoises       turtles 
##            14            41            12            14
```
**6. The species in `homerange` are also classified into trophic guilds. How many species are represented in each trophic guild.**  

```r
table(homerange$trophic.guild)
```

```
## 
## carnivore herbivore 
##       342       227
```
**7. Make two new data frames, one which is restricted to carnivores and another that is restricted to herbivores.**  

```r
carnivores <- subset.data.frame(homerange, trophic.guild == "carnivore")
summary(carnivores)
```

```
##            taxon     common.name           class                     order   
##  birds        :116   Length:342         Length:342         perciformes  :68  
##  mammals      : 80   Class :character   Class :character   passeriformes:64  
##  marine fishes: 70   Mode  :character   Mode  :character   carnivora    :52  
##  snakes       : 41                                         squamata     :41  
##  river fishes : 14                                         falconiformes:17  
##  turtles      : 12                                         testudines   :12  
##  (Other)      :  9                                         (Other)      :88  
##     family             genus             species          primarymethod     
##  Length:342         Length:342         Length:342         Length:342        
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##                                                                             
##       N              mean.mass.g          log10.mass     
##  Length:342         Min.   :     0.22   Min.   :-0.6576  
##  Class :character   1st Qu.:    23.06   1st Qu.: 1.3629  
##  Mode  :character   Median :   189.00   Median : 2.2765  
##                     Mean   :  2634.96   Mean   : 2.2383  
##                     3rd Qu.:   928.65   3rd Qu.: 2.9678  
##                     Max.   :112000.51   Max.   : 5.0492  
##                                                          
##  alternative.mass.reference  mean.hra.m2          log10.hra     
##  Length:342                 Min.   :        0   Min.   :-1.523  
##  Class :character           1st Qu.:     4047   1st Qu.: 3.607  
##  Mode  :character           Median :    35429   Median : 4.549  
##                             Mean   : 13039918   Mean   : 4.701  
##                             3rd Qu.:  1304933   3rd Qu.: 6.115  
##                             Max.   :815060788   Max.   : 8.911  
##                                                                 
##  hra.reference         realm           thermoregulation    locomotion       
##  Length:342         Length:342         Length:342         Length:342        
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##                                                                             
##  trophic.guild       dimension            preymass         log10.preymass   
##  Length:342         Length:342         Min.   :     0.67   Min.   :-0.1739  
##  Class :character   Class :character   1st Qu.:    20.02   1st Qu.: 1.3014  
##  Mode  :character   Mode  :character   Median :    53.75   Median : 1.7304  
##                                        Mean   :  3989.88   Mean   : 2.0188  
##                                        3rd Qu.:   363.35   3rd Qu.: 2.5603  
##                                        Max.   :130233.20   Max.   : 5.1147  
##                                        NA's   :275         NA's   :275      
##       PPMR         prey.size.reference
##  Min.   :  0.380   Length:342         
##  1st Qu.:  3.315   Class :character   
##  Median :  7.190   Mode  :character   
##  Mean   : 31.752                      
##  3rd Qu.: 15.966                      
##  Max.   :530.000                      
##  NA's   :275
```

```r
herbivores <- subset.data.frame(homerange, trophic.guild == "herbivore")
summary(herbivores)
```

```
##            taxon     common.name           class                    order   
##  mammals      :158   Length:227         Length:227         rodentia    :76  
##  birds        : 24   Class :character   Class :character   artiodactyla:39  
##  marine fishes: 20   Mode  :character   Mode  :character   perciformes :20  
##  tortoises    : 12                                         lagomorpha  :14  
##  lizards      : 11                                         testudines  :14  
##  turtles      :  2                                         diprodontia :12  
##  (Other)      :  0                                         (Other)     :52  
##     family             genus             species          primarymethod     
##  Length:227         Length:227         Length:227         Length:227        
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##                                                                             
##       N              mean.mass.g        log10.mass    
##  Length:227         Min.   :      2   Min.   :0.3979  
##  Class :character   1st Qu.:    148   1st Qu.:2.1715  
##  Mode  :character   Median :    927   Median :2.9671  
##                     Mean   :  82764   Mean   :3.1316  
##                     3rd Qu.:  10725   3rd Qu.:4.0304  
##                     Max.   :4000000   Max.   :6.6021  
##                                                       
##  alternative.mass.reference  mean.hra.m2          log10.hra     
##  Length:227                 Min.   :0.000e+00   Min.   :-1.301  
##  Class :character           1st Qu.:4.879e+03   1st Qu.: 3.688  
##  Mode  :character           Median :4.300e+04   Median : 4.633  
##                             Mean   :3.414e+07   Mean   : 4.721  
##                             3rd Qu.:9.430e+05   3rd Qu.: 5.974  
##                             Max.   :3.551e+09   Max.   : 9.550  
##                                                                 
##  hra.reference         realm           thermoregulation    locomotion       
##  Length:227         Length:227         Length:227         Length:227        
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##                                                                             
##  trophic.guild       dimension            preymass   log10.preymass
##  Length:227         Length:227         Min.   : NA   Min.   : NA   
##  Class :character   Class :character   1st Qu.: NA   1st Qu.: NA   
##  Mode  :character   Mode  :character   Median : NA   Median : NA   
##                                        Mean   :NaN   Mean   :NaN   
##                                        3rd Qu.: NA   3rd Qu.: NA   
##                                        Max.   : NA   Max.   : NA   
##                                        NA's   :227   NA's   :227   
##       PPMR     prey.size.reference
##  Min.   : NA   Length:227         
##  1st Qu.: NA   Class :character   
##  Median : NA   Mode  :character   
##  Mean   :NaN                      
##  3rd Qu.: NA                      
##  Max.   : NA                      
##  NA's   :227
```

**8. Do herbivores or carnivores have, on average, a larger `mean.hra.m2`? Remove any NAs from the data.**  

```r
mean(herbivores$mean.hra.m2, na.rm= T)
```

```
## [1] 34137012
```


```r
mean(carnivores$mean.hra.m2, na.rm= T)
```

```
## [1] 13039918
```

It seems that herbivores have a larger mean.hra.m2 value than carnivores do. 
**9. Make a new dataframe `deer` that is limited to the mean mass, log10 mass, family, genus, and species of deer in the database. The family for deer is cervidae. Arrange the data in descending order by log10 mass. Which is the largest deer? What is its common name?**  

```r
deer <- subset.data.frame(homerange, family == "cervidae")

arrange(select(deer, "mean.mass.g", "log10.mass", "family", "genus", "species"), desc(log10.mass))
```

```
## # A tibble: 12 x 5
##    mean.mass.g log10.mass family   genus      species    
##          <dbl>      <dbl> <chr>    <chr>      <chr>      
##  1     307227.       5.49 cervidae alces      alces      
##  2     234758.       5.37 cervidae cervus     elaphus    
##  3     102059.       5.01 cervidae rangifer   tarandus   
##  4      87884.       4.94 cervidae odocoileus virginianus
##  5      71450.       4.85 cervidae dama       dama       
##  6      62823.       4.80 cervidae axis       axis       
##  7      53864.       4.73 cervidae odocoileus hemionus   
##  8      35000.       4.54 cervidae ozotoceros bezoarticus
##  9      29450.       4.47 cervidae cervus     nippon     
## 10      24050.       4.38 cervidae capreolus  capreolus  
## 11      13500.       4.13 cervidae muntiacus  reevesi    
## 12       7500.       3.88 cervidae pudu       puda
```

```r
filter(homerange, genus== "alces" & species == "alces")
```

```
## # A tibble: 1 x 24
##   taxon common.name class order family genus species primarymethod N    
##   <fct> <chr>       <chr> <fct> <chr>  <chr> <chr>   <chr>         <chr>
## 1 mamm… moose       mamm… arti… cervi… alces alces   telemetry*    <NA> 
## # … with 15 more variables: mean.mass.g <dbl>, log10.mass <dbl>,
## #   alternative.mass.reference <chr>, mean.hra.m2 <dbl>, log10.hra <dbl>,
## #   hra.reference <chr>, realm <chr>, thermoregulation <chr>, locomotion <chr>,
## #   trophic.guild <chr>, dimension <chr>, preymass <dbl>, log10.preymass <dbl>,
## #   PPMR <dbl>, prey.size.reference <chr>
```
The largest deer is the Moose.
**10. As measured by the data, which snake species has the smallest homerange? Show all of your work, please. Look this species up online and tell me about it!** **Snake is found in taxon column**    

```r
snake <- subset.data.frame( homerange, taxon == "snakes")
arrange(snake, mean.hra.m2) # if you remove the desc then the output is more clear
```

```
## # A tibble: 41 x 24
##    taxon common.name class order family genus species primarymethod N    
##    <fct> <chr>       <chr> <fct> <chr>  <chr> <chr>   <chr>         <chr>
##  1 snak… namaqua dw… rept… squa… viper… bitis schnei… telemetry     11   
##  2 snak… eastern wo… rept… squa… colub… carp… viridis radiotag      10   
##  3 snak… butlers ga… rept… squa… colub… tham… butleri mark-recaptu… 1    
##  4 snak… western wo… rept… squa… colub… carp… vermis  radiotag      1    
##  5 snak… snubnosed … rept… squa… viper… vipe… latast… telemetry     7    
##  6 snak… chinese pi… rept… squa… viper… gloy… shedao… telemetry     16   
##  7 snak… ringneck s… rept… squa… colub… diad… puncta… mark-recaptu… <NA> 
##  8 snak… cottonmouth rept… squa… viper… agki… pisciv… telemetry     15   
##  9 snak… redbacked … rept… squa… colub… ooca… rufodo… telemetry     21   
## 10 snak… gopher sna… rept… squa… colub… pitu… cateni… telemetry     4    
## # … with 31 more rows, and 15 more variables: mean.mass.g <dbl>,
## #   log10.mass <dbl>, alternative.mass.reference <chr>, mean.hra.m2 <dbl>,
## #   log10.hra <dbl>, hra.reference <chr>, realm <chr>, thermoregulation <chr>,
## #   locomotion <chr>, trophic.guild <chr>, dimension <chr>, preymass <dbl>,
## #   log10.preymass <dbl>, PPMR <dbl>, prey.size.reference <chr>
```
The namaqua dwarf adder is the snake with the smallest homerange.It is a venomous snake found on the border between Namibia and South Africa.
## Push your final code to GitHub!
Please be sure that you check the `keep md` file in the knit preferences.   
