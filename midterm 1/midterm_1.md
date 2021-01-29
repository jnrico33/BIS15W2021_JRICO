---
title: "Midterm 1"
author: "Jessica Rico"
date: "2021-01-29"
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
library(skimr)
library(janitor)
```

## Questions
**1. (2 points) Briefly explain how R, RStudio, and GitHub work together to make work flows in data science transparent and repeatable. What is the advantage of using RMarkdown in this context?**  


```r
#R is a scripting language that is open sourced, RStudio is the interface where we can use R to code and GitHub  is a site where people can share and store their code. Github is a resource to also maybe find bits of code that can help you move along a project and also do the same for others who happen to find your code useful.

#Rmarkdown gives an advantage because it lets you see the code that was run in chunks along with the results of  the code and you can also edit it or manipulate it as you want/need. This also lets you run it for yourself as  well from a different computer so that you can verify it works and adjust it as needed. 
```



**2. (2 points) What are the three types of `data structures` that we have discussed? Why are we using data frames for BIS 15L?**



```r
#We have discussed vectors, data matrices and data frames so far. We are using data frames in this class because it can store multiple classes of data. 
```


In the midterm 1 folder there is a second folder called `data`. Inside the `data` folder, there is a .csv file called `ElephantsMF`. These data are from Phyllis Lee, Stirling University, and are related to Lee, P., et al. (2013), "Enduring consequences of early experiences: 40-year effects on survival and success among African elephants (Loxodonta africana)," Biology Letters, 9: 20130011. [kaggle](https://www.kaggle.com/mostafaelseidy/elephantsmf).  

**3. (2 points) Please load these data as a new object called `elephants`. Use the function(s) of your choice to get an idea of the structure of the data. Be sure to show the class of each variable.**


```r
elephants<- readr::read_csv("data/ElephantsMF.csv")
```

```
## 
## -- Column specification --------------------------------------------------------
## cols(
##   Age = col_double(),
##   Height = col_double(),
##   Sex = col_character()
## )
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

|skim_variable | n_missing| complete_rate|   mean|   sd|    p0|    p25|    p50|    p75|   p100|hist                                     |
|:-------------|---------:|-------------:|------:|----:|-----:|------:|------:|------:|------:|:----------------------------------------|
|Age           |         0|             1|  10.97|  8.4|  0.01|   4.58|   9.46|  16.50|  32.17|▆▇▂▂▂ |
|Height        |         0|             1| 187.68| 50.6| 75.46| 160.75| 200.00| 221.09| 304.06|▃▃▇▇▁ |

```r
names(elephants)
```

```
## [1] "Age"    "Height" "Sex"
```


**4. (2 points) Change the names of the variables to lower case and change the class of the variable `sex` to a factor.**


```r
elephants<- janitor::clean_names(elephants)
elephants$sex<- as.factor(elephants$sex)
elephants
```

```
## # A tibble: 288 x 3
##      age height sex  
##    <dbl>  <dbl> <fct>
##  1   1.4   120  M    
##  2  17.5   227  M    
##  3  12.8   235  M    
##  4  11.2   210  M    
##  5  12.7   220  M    
##  6  12.7   189  M    
##  7  12.2   225  M    
##  8  12.2   204  M    
##  9  28.2   266. M    
## 10  11.7   233  M    
## # ... with 278 more rows
```


**5. (2 points) How many male and female elephants are represented in the data?**


```r
tabyl(elephants, sex)
```

```
##  sex   n   percent
##    F 150 0.5208333
##    M 138 0.4791667
```


**6. (2 points) What is the average age all elephants in the data?**



```r
elephants%>%
  summarise(average_age= mean(age), total=n(), na.rm=TRUE)
```

```
## # A tibble: 1 x 3
##   average_age total na.rm
##         <dbl> <int> <lgl>
## 1        11.0   288 TRUE
```

```r
#The average age for all elephants in the data is 10.97 (could be rounded to 11 in use)
```


**7. (2 points) How does the average age and height of elephants compare by sex?**


```r
elephants%>%
  group_by(sex)%>%
  summarise(avg_age= mean(age), avg_height= mean(height), total=n(), na.rm=TRUE)
```

```
## `summarise()` ungrouping output (override with `.groups` argument)
```

```
## # A tibble: 2 x 5
##   sex   avg_age avg_height total na.rm
##   <fct>   <dbl>      <dbl> <int> <lgl>
## 1 F       12.8        190.   150 TRUE 
## 2 M        8.95       185.   138 TRUE
```

```r
#On average, females are both older and taller than males. 
```


**8. (2 points) How does the average height of elephants compare by sex for individuals over 25 years old. Include the min and max height as well as the number of individuals in the sample as part of your analysis.**



```r
elephants%>%
  group_by(sex)%>%
  filter(age>25)%>%
  summarise(aver_height= mean(height), min_height= min(height), max_height= max(height), total=n(), na.rm=TRUE)
```

```
## `summarise()` ungrouping output (override with `.groups` argument)
```

```
## # A tibble: 2 x 6
##   sex   aver_height min_height max_height total na.rm
##   <fct>       <dbl>      <dbl>      <dbl> <int> <lgl>
## 1 F            233.       206.       278.    25 TRUE 
## 2 M            273.       237.       304.     8 TRUE
```

```r
#This time we see that for elephants over 25 years old, males have larger average height, min height and max     height values. There are also only 8 males in this over 25 age category while there are 25 females. 
```



For the next series of questions, we will use data from a study on vertebrate community composition and impacts from defaunation in [Gabon, Africa](https://en.wikipedia.org/wiki/Gabon). One thing to notice is that the data include 24 separate transects. Each transect represents a path through different forest management areas.  

Reference: Koerner SE, Poulsen JR, Blanchard EJ, Okouyi J, Clark CJ. Vertebrate community composition and diversity declines along a defaunation gradient radiating from rural villages in Gabon. _Journal of Applied Ecology_. 2016. This paper, along with a description of the variables is included inside the midterm 1 folder.  


**9. (2 points) Load `IvindoData_DryadVersion.csv` and use the function(s) of your choice to get an idea of the overall structure. Change the variables `HuntCat` and `LandUse` to factors.**




```r
gabon_data <- readr::read_csv("data/IvindoData_DryadVersion.csv")
```

```
## 
## -- Column specification --------------------------------------------------------
## cols(
##   .default = col_double(),
##   HuntCat = col_character(),
##   LandUse = col_character()
## )
## i Use `spec()` for the full column specifications.
```

```r
skim(gabon_data)
```


Table: Data summary

|                         |           |
|:------------------------|:----------|
|Name                     |gabon_data |
|Number of rows           |24         |
|Number of columns        |26         |
|_______________________  |           |
|Column type frequency:   |           |
|character                |2          |
|numeric                  |24         |
|________________________ |           |
|Group variables          |None       |


**Variable type: character**

|skim_variable | n_missing| complete_rate| min| max| empty| n_unique| whitespace|
|:-------------|---------:|-------------:|---:|---:|-----:|--------:|----------:|
|HuntCat       |         0|             1|   4|   8|     0|        3|          0|
|LandUse       |         0|             1|   4|   7|     0|        3|          0|


**Variable type: numeric**

|skim_variable           | n_missing| complete_rate|  mean|    sd|    p0|   p25|   p50|   p75|  p100|hist                                     |
|:-----------------------|---------:|-------------:|-----:|-----:|-----:|-----:|-----:|-----:|-----:|:----------------------------------------|
|TransectID              |         0|             1| 13.50|  8.51|  1.00|  5.75| 14.50| 20.25| 27.00|▇▃▅▆▆ |
|Distance                |         0|             1| 11.88|  7.28|  2.70|  5.67|  9.72| 17.68| 26.76|▇▂▂▅▂ |
|NumHouseholds           |         0|             1| 37.88| 17.80| 13.00| 24.75| 29.00| 54.00| 73.00|▇▇▂▇▂ |
|Veg_Rich                |         0|             1| 14.83|  2.07| 10.88| 13.10| 14.94| 16.54| 18.75|▃▂▃▇▁ |
|Veg_Stems               |         0|             1| 32.80|  5.96| 23.44| 28.69| 32.44| 37.08| 47.56|▆▇▆▃▁ |
|Veg_liana               |         0|             1| 11.04|  3.29|  4.75|  9.03| 11.94| 13.25| 16.38|▃▂▃▇▃ |
|Veg_DBH                 |         0|             1| 46.09| 10.67| 28.45| 40.65| 43.90| 50.57| 76.48|▂▇▃▁▁ |
|Veg_Canopy              |         0|             1|  3.47|  0.35|  2.50|  3.25|  3.43|  3.75|  4.00|▁▁▇▅▇ |
|Veg_Understory          |         0|             1|  3.02|  0.34|  2.38|  2.88|  3.00|  3.17|  3.88|▂▆▇▂▁ |
|RA_Apes                 |         0|             1|  2.04|  3.03|  0.00|  0.00|  0.48|  3.82| 12.93|▇▂▁▁▁ |
|RA_Birds                |         0|             1| 58.64| 14.71| 31.56| 52.51| 57.89| 68.18| 85.03|▅▅▇▇▃ |
|RA_Elephant             |         0|             1|  0.54|  0.67|  0.00|  0.00|  0.36|  0.89|  2.30|▇▂▂▁▁ |
|RA_Monkeys              |         0|             1| 31.30| 12.38|  5.84| 22.70| 31.74| 39.88| 54.12|▂▅▃▇▂ |
|RA_Rodent               |         0|             1|  3.28|  1.47|  1.06|  2.05|  3.23|  4.09|  6.31|▇▅▇▃▃ |
|RA_Ungulate             |         0|             1|  4.17|  4.31|  0.00|  1.23|  2.54|  5.16| 13.86|▇▂▁▁▂ |
|Rich_AllSpecies         |         0|             1| 20.21|  2.06| 15.00| 19.00| 20.00| 22.00| 24.00|▁▁▇▅▁ |
|Evenness_AllSpecies     |         0|             1|  0.77|  0.05|  0.67|  0.75|  0.78|  0.81|  0.83|▃▁▅▇▇ |
|Diversity_AllSpecies    |         0|             1|  2.31|  0.15|  1.97|  2.25|  2.32|  2.43|  2.57|▂▃▇▆▅ |
|Rich_BirdSpecies        |         0|             1| 10.33|  1.24|  8.00| 10.00| 11.00| 11.00| 13.00|▃▅▇▁▁ |
|Evenness_BirdSpecies    |         0|             1|  0.71|  0.08|  0.56|  0.68|  0.72|  0.77|  0.82|▅▁▇▆▇ |
|Diversity_BirdSpecies   |         0|             1|  1.66|  0.20|  1.16|  1.60|  1.68|  1.78|  2.01|▂▂▅▇▃ |
|Rich_MammalSpecies      |         0|             1|  9.88|  1.68|  6.00|  9.00| 10.00| 11.00| 12.00|▂▂▃▅▇ |
|Evenness_MammalSpecies  |         0|             1|  0.75|  0.06|  0.62|  0.71|  0.74|  0.78|  0.86|▂▃▇▂▅ |
|Diversity_MammalSpecies |         0|             1|  1.70|  0.17|  1.38|  1.57|  1.70|  1.81|  2.06|▅▇▇▇▃ |

```r
gabon_data <- janitor::clean_names(gabon_data)
gabon_data$hunt_cat <- as.factor(gabon_data$hunt_cat)
gabon_data$land_use <- as.factor(gabon_data$land_use)
gabon_data
```

```
## # A tibble: 24 x 26
##    transect_id distance hunt_cat num_households land_use veg_rich veg_stems
##          <dbl>    <dbl> <fct>             <dbl> <fct>       <dbl>     <dbl>
##  1           1     7.14 Moderate             54 Park         16.7      31.2
##  2           2    17.3  None                 54 Park         15.8      37.4
##  3           2    18.3  None                 29 Park         16.9      32.3
##  4           3    20.8  None                 29 Logging      12.4      29.4
##  5           4    16.0  None                 29 Park         17.1      36  
##  6           5    17.5  None                 29 Park         16.5      29.2
##  7           6    24.1  None                 29 Park         14.8      31.2
##  8           7    19.8  None                 54 Logging      13.2      32.6
##  9           8     5.78 High                 25 Neither      12.6      23.7
## 10           9     5.13 High                 73 Logging      16        27.1
## # ... with 14 more rows, and 19 more variables: veg_liana <dbl>, veg_dbh <dbl>,
## #   veg_canopy <dbl>, veg_understory <dbl>, ra_apes <dbl>, ra_birds <dbl>,
## #   ra_elephant <dbl>, ra_monkeys <dbl>, ra_rodent <dbl>, ra_ungulate <dbl>,
## #   rich_all_species <dbl>, evenness_all_species <dbl>,
## #   diversity_all_species <dbl>, rich_bird_species <dbl>,
## #   evenness_bird_species <dbl>, diversity_bird_species <dbl>,
## #   rich_mammal_species <dbl>, evenness_mammal_species <dbl>,
## #   diversity_mammal_species <dbl>
```


**10. (4 points) For the transects with high and moderate hunting intensity, how does the average diversity of birds and mammals compare?**



```r
gabon_data%>%
  filter(hunt_cat!= "None")%>%
  group_by(hunt_cat)%>%
  summarise(avg_bird_diversity= mean(diversity_bird_species), avg_mammal_diversity= mean(diversity_mammal_species), total=n(), na.rm=TRUE)
```

```
## `summarise()` ungrouping output (override with `.groups` argument)
```

```
## # A tibble: 2 x 5
##   hunt_cat avg_bird_diversity avg_mammal_diversity total na.rm
##   <fct>                 <dbl>                <dbl> <int> <lgl>
## 1 High                   1.66                 1.74     7 TRUE 
## 2 Moderate               1.62                 1.68     8 TRUE
```

```r
#For transects with both high and moderate hunting intensity, the average mammal diversity is greater than that  of the average bird diversity. 
```


**11. (4 points) One of the conclusions in the study is that the relative abundance of animals drops off the closer you get to a village. Let's try to reconstruct this (without the statistics). How does the relative abundance (RA) of apes, birds, elephants, monkeys, rodents, and ungulates compare between sites that are less than 5km from a village to sites that are greater than 20km from a village? The variable `Distance` measures the distance of the transect from the nearest village. Hint: try using the `across` operator.**  




```r
gabon_data%>%
  filter(distance <5 | distance > 20)%>%
  group_by(distance<5)%>%
  summarise(across(contains("ra_"), mean, na.rm=TRUE))
```

```
## `summarise()` ungrouping output (override with `.groups` argument)
```

```
## # A tibble: 2 x 7
##   `distance < 5` ra_apes ra_birds ra_elephant ra_monkeys ra_rodent ra_ungulate
##   <lgl>            <dbl>    <dbl>       <dbl>      <dbl>     <dbl>       <dbl>
## 1 FALSE             7.21     44.5      0.557        40.1      2.68        4.98
## 2 TRUE              0.08     70.4      0.0967       24.1      3.66        1.59
```


**12. (4 points) Based on your interest, do one exploratory analysis on the `gabon` data of your choice. This analysis needs to include a minimum of two functions in `dplyr.`**


```r
names(gabon_data)
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
gabon_data%>%
  select(land_use, veg_rich, veg_canopy, veg_understory) %>% 
  filter(land_use!="Neither")%>%
  group_by(land_use)%>%
  summarise(across(contains("veg"), mean,  total=n(), , na.rm=TRUE))%>%
  arrange(desc(veg_rich))
```

```
## `summarise()` ungrouping output (override with `.groups` argument)
```

```
## # A tibble: 2 x 4
##   land_use veg_rich veg_canopy veg_understory
##   <fct>       <dbl>      <dbl>          <dbl>
## 1 Park         16.3       3.60           3.04
## 2 Logging      14.4       3.50           3.00
```

