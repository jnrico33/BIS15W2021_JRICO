---
title: "Lab 13 Homework"
author: "Jessica Rico"
date: "2021-03-02"
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
## v ggplot2 3.3.3     v purrr   0.3.4
## v tibble  3.0.6     v dplyr   1.0.4
## v tidyr   1.1.2     v stringr 1.4.0
## v readr   1.4.0     v forcats 0.5.1
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
library(skimr)
```

## Data
The data for this assignment come from the [University of California Information Center](https://www.universityofcalifornia.edu/infocenter). Admissions data were collected for the years 2010-2019 for each UC campus. Admissions are broken down into three categories: applications, admits, and enrollees. The number of individuals in each category are presented by demographic.  

```r
UC_admit <- readr::read_csv("data/UC_admit.csv")
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
summary(UC_admit)
```

```
##     Campus           Academic_Yr     Category          Ethnicity        
##  Length:2160        Min.   :2010   Length:2160        Length:2160       
##  Class :character   1st Qu.:2012   Class :character   Class :character  
##  Mode  :character   Median :2014   Mode  :character   Mode  :character  
##                     Mean   :2014                                        
##                     3rd Qu.:2017                                        
##                     Max.   :2019                                        
##                                                                         
##    Perc FR          FilteredCountFR   
##  Length:2160        Min.   :     1.0  
##  Class :character   1st Qu.:   447.5  
##  Mode  :character   Median :  1837.0  
##                     Mean   :  7142.6  
##                     3rd Qu.:  6899.5  
##                     Max.   :113755.0  
##                     NA's   :1
```

```r
skim(UC_admit)
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
|Campus        |         0|             1|   5|  13|     0|        9|          0|
|Category      |         0|             1|   6|  10|     0|        3|          0|
|Ethnicity     |         0|             1|   3|  16|     0|        8|          0|
|Perc FR       |         1|             1|   5|   7|     0|     1293|          0|


**Variable type: numeric**

|skim_variable   | n_missing| complete_rate|    mean|       sd|   p0|    p25|    p50|    p75|   p100|hist                                     |
|:---------------|---------:|-------------:|-------:|--------:|----:|------:|------:|------:|------:|:----------------------------------------|
|Academic_Yr     |         0|             1| 2014.50|     2.87| 2010| 2012.0| 2014.5| 2017.0|   2019|▇▇▇▇▇ |
|FilteredCountFR |         1|             1| 7142.63| 13808.91|    1|  447.5| 1837.0| 6899.5| 113755|▇▁▁▁▁ |

**2. The president of UC has asked you to build a shiny app that shows admissions by ethnicity across all UC campuses. Your app should allow users to explore year, campus, and admit category as interactive variables. Use shiny dashboard and try to incorporate the aesthetics you have learned in ggplot to make the app neat and clean.**



```r
options(scipen = 999)
```


```r
UC_admit$Academic_Yr <- as.factor(UC_admit$Academic_Yr)
```


```r
ui <- dashboardPage(
  dashboardHeader(title = "University of California Admissions"),
  dashboardSidebar(),
  dashboardBody(
  selectInput("y", "Select Filter", choices = c("Academic_Yr", "Campus", "Category"), selected = "Academic_Yr"),
  plotOutput("plot", width = "500px", height = "400px"))
)

server <- function(input, output, session) { 
 output$plot <- renderPlot({
  UC_admit %>% 
     filter(Ethnicity!="All") %>% 
  ggplot(aes_string(x = "Ethnicity", y = "FilteredCountFR", fill=input$y)) +
  geom_col(position="dodge", alpha = 0.8, size=4)+
  theme_light(base_size = 18) + 
     labs(title= "University of California Admissions", x="Ethnicity", y="Number Students Admitted", fill = "Legend")+
     theme(axis.text.x = element_text(angle = 90, hjust = 1))
  })
  session$onSessionEnded(stopApp)
}

shinyApp(ui, server)
```

**3. Make alternate version of your app above by tracking enrollment at a campus over all of the represented years while allowing users to interact with campus, category, and ethnicity.**


```r
ui <- dashboardPage(
  dashboardHeader(title = "University of California"),
  dashboardSidebar(),
  dashboardBody(
  selectInput("y", "Select Filter", choices = c("Campus", "Category", "Ethnicity"), selected = "Campus"),
  plotOutput("plot", width = "500px", height = "400px"))
)
server <- function(input, output, session) { 
 output$plot <- renderPlot({
  UC_admit %>% 
  ggplot(aes_string(x = "Academic_Yr", y = "FilteredCountFR", fill=input$y)) +
  geom_col(position="dodge", alpha = 0.8, size=4)+
  theme_light(base_size = 18) + 
     labs(title= "University of California Admissions", x="Academic Year", y="Number Students Admitted", fill = "Legend")+
     theme(axis.text.x = element_text(angle = 90, hjust = 1))
  })
  session$onSessionEnded(stopApp)
  }
shinyApp(ui, server)
```

`<div style="width: 100% ; height: 400px ; text-align: center; box-sizing: border-box; -moz-box-sizing: border-box; -webkit-box-sizing: border-box;" class="muted well">Shiny applications not supported in static R Markdown documents</div>`{=html}

## Push your final code to GitHub!
Please be sure that you check the `keep md` file in the knit preferences. 
