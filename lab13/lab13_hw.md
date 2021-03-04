---
title: "Lab 13 Homework"
author: "Chloe Tannous"
date: "2021-03-03"
output:
  html_document: 
    theme: spacelab
    keep_md: yes
---



## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your final lab report should be organized, clean, and run free from errors. Remember, you must remove the `#` for the included code chunks to run. Be sure to add your name to the author header above. For any included plots, make sure they are clearly labeled. You are free to use any plot type that you feel best communicates the results of your analysis.  

Make sure to use the formatting conventions of RMarkdown to make your report neat and clean!  

## Libraries

```r
if (!require("tidyverse")) install.packages('tidyverse')
```

```
## Loading required package: tidyverse
```

```
## -- Attaching packages --------------------------------------- tidyverse 1.3.0 --
```

```
## √ ggplot2 3.3.3     √ purrr   0.3.4
## √ tibble  3.1.0     √ dplyr   1.0.4
## √ tidyr   1.1.3     √ stringr 1.4.0
## √ readr   1.4.0     √ forcats 0.5.1
```

```
## -- Conflicts ------------------------------------------ tidyverse_conflicts() --
## x dplyr::filter() masks stats::filter()
## x dplyr::lag()    masks stats::lag()
```


```r
library(tidyverse)
library(shiny)
library(shinydashboard)
library(janitor)
library(naniar)
library(paletteer)
```

## Data
The data for this assignment come from the [University of California Information Center](https://www.universityofcalifornia.edu/infocenter). Admissions data were collected for the years 2010-2019 for each UC campus. Admissions are broken down into three categories: applications, admits, and enrollees. The number of individuals in each category are presented by demographic.  

```r
UC_admit <- readr::read_csv("data/UC_admit.csv") %>%
  clean_names()
```

```
## 
## -- Column specification --------------------------------------------------------
## cols(
##   Campus = col_character(),
##   Academic_Yr = col_double(),
##   Category = col_character(),
##   Ethnicity = col_character(),
##   `Perc FR` = col_character(),
##   FilteredCountFR = col_double()
## )
```

**1. Use the function(s) of your choice to get an idea of the overall structure of the data frame, including its dimensions, column names, variable classes, etc. As part of this, determine if there are NA's and how they are treated.**  


```r
glimpse(UC_admit)
```

```
## Rows: 2,160
## Columns: 6
## $ campus            <chr> "Davis", "Davis", "Davis", "Davis", "Davis", "Davis"~
## $ academic_yr       <dbl> 2019, 2019, 2019, 2019, 2019, 2019, 2019, 2019, 2018~
## $ category          <chr> "Applicants", "Applicants", "Applicants", "Applicant~
## $ ethnicity         <chr> "International", "Unknown", "White", "Asian", "Chica~
## $ perc_fr           <chr> "21.16%", "2.51%", "18.39%", "30.76%", "22.44%", "0.~
## $ filtered_count_fr <dbl> 16522, 1959, 14360, 24024, 17526, 277, 3425, 78093, ~
```

```r
names(UC_admit)
```

```
## [1] "campus"            "academic_yr"       "category"         
## [4] "ethnicity"         "perc_fr"           "filtered_count_fr"
```

```r
skimr::skim(UC_admit)
```


Table: Data summary

|                         |         |
|:------------------------|:--------|
|Name                     |UC_admit |
|Number of rows           |2160     |
|Number of columns        |6        |
|_______________________  |         |
|Column type frequency:   |         |
|character                |4        |
|numeric                  |2        |
|________________________ |         |
|Group variables          |None     |


**Variable type: character**

|skim_variable | n_missing| complete_rate| min| max| empty| n_unique| whitespace|
|:-------------|---------:|-------------:|---:|---:|-----:|--------:|----------:|
|campus        |         0|             1|   5|  13|     0|        9|          0|
|category      |         0|             1|   6|  10|     0|        3|          0|
|ethnicity     |         0|             1|   3|  16|     0|        8|          0|
|perc_fr       |         1|             1|   5|   7|     0|     1293|          0|


**Variable type: numeric**

|skim_variable     | n_missing| complete_rate|    mean|       sd|   p0|    p25|    p50|    p75|   p100|hist  |
|:-----------------|---------:|-------------:|-------:|--------:|----:|------:|------:|------:|------:|:-----|
|academic_yr       |         0|             1| 2014.50|     2.87| 2010| 2012.0| 2014.5| 2017.0|   2019|▇▇▇▇▇ |
|filtered_count_fr |         1|             1| 7142.63| 13808.91|    1|  447.5| 1837.0| 6899.5| 113755|▇▁▁▁▁ |

```r
summary(UC_admit)
```

```
##     campus           academic_yr     category          ethnicity        
##  Length:2160        Min.   :2010   Length:2160        Length:2160       
##  Class :character   1st Qu.:2012   Class :character   Class :character  
##  Mode  :character   Median :2014   Mode  :character   Mode  :character  
##                     Mean   :2014                                        
##                     3rd Qu.:2017                                        
##                     Max.   :2019                                        
##                                                                         
##    perc_fr          filtered_count_fr 
##  Length:2160        Min.   :     1.0  
##  Class :character   1st Qu.:   447.5  
##  Mode  :character   Median :  1837.0  
##                     Mean   :  7142.6  
##                     3rd Qu.:  6899.5  
##                     Max.   :113755.0  
##                     NA's   :1
```


**2. The president of UC has asked you to build a shiny app that shows admissions by ethnicity across all UC campuses. Your app should allow users to explore year, campus, and admit category as interactive variables. Use shiny dashboard and try to incorporate the aesthetics you have learned in ggplot to make the app neat and clean.**


```r
palette <- paletteer_d("miscpalettes::berry")
```

```r
ui <- dashboardPage(
  dashboardHeader(title = "Admissions By Ethnicity Across the UC Campuses"),
  dashboardSidebar(disable = T),
  dashboardBody(
  fluidRow(
  box(title = "Plot Options", width = 3,
  selectInput("x", "Select Variable",  choices=c("campus", "academic_yr", "category"), 
              selected = "campus"),
      hr(),
      helpText("Reference: [https://www.universityofcalifornia.edu/infocenter] Admissions data were collected for the years 2010-2019 for each UC campus.")
  ), 
  box(title = "Ethnicity", width = 6,
  plotOutput("plot", width = "600px", height = "500px")
  ) 
  ) 
  ) 
) 

server <- function(input, output, session) { 
  
  output$plot <- renderPlot({
  UC_admit %>% 
   ggplot(aes_string(x=input$x, y="filtered_count_fr", fill="ethnicity")) +
  geom_col()+
      scale_fill_manual(values = palette)+
      labs(title = "UC Admission Information",x=NULL,y="Number of Students")+
    theme_light(base_size = 18) +
      theme(axis.text.x = element_text(angle = 60, hjust = 1), plot.title = element_text(size = rel(2), hjust = .5))
  })
  

  session$onSessionEnded(stopApp)
  }

shinyApp(ui, server)
```

```
## PhantomJS not found. You can install it with webshot::install_phantomjs(). If it is installed, please make sure the phantomjs executable can be found via the PATH variable.
```

`<div style="width: 100% ; height: 400px ; text-align: center; box-sizing: border-box; -moz-box-sizing: border-box; -webkit-box-sizing: border-box;" class="muted well">Shiny applications not supported in static R Markdown documents</div>`{=html}


**3. Make alternate version of your app above by tracking enrollment at a campus over all of the represented years while allowing users to interact with campus, category, and ethnicity.**

```r
ui <- dashboardPage(
  dashboardHeader(title = "Enrollment"),
  dashboardSidebar(disable = T),
  dashboardBody(
  fluidRow(
  box(title = "Plot Options", width = 3,
  selectInput("x", "Select Variable",  choices=c("campus", "ethnicity", "category"), 
              selected = "campus"),
      hr(),
      helpText("Reference: [https://www.universityofcalifornia.edu/infocenter] Admissions data were collected for the years 2010-2019 for each UC campus.")
  ), 
  box(title = "Enrollment in the UC System", width = 6,
  plotOutput("plot", width = "600px", height = "500px")
  ) 
  ) 
  ) 
) 

server <- function(input, output, session) { 
  
  output$plot <- renderPlot({
  UC_admit %>% 
  ggplot(aes_string(x = "academic_yr", y="filtered_count_fr",fill = input$x)) +
  geom_col()+
      scale_fill_manual(values = palette)+
      labs(x="Year", y="# of Students")+
    theme_light(base_size = 18) +
      theme(axis.text.x = element_text(angle = 60, hjust = 1), plot.title = element_text(size = rel(2), hjust = .5))
  })
  

  session$onSessionEnded(stopApp)
  }

shinyApp(ui, server)
```

`<div style="width: 100% ; height: 400px ; text-align: center; box-sizing: border-box; -moz-box-sizing: border-box; -webkit-box-sizing: border-box;" class="muted well">Shiny applications not supported in static R Markdown documents</div>`{=html}


## Push your final code to GitHub!
Please be sure that you check the `keep md` file in the knit preferences. 
