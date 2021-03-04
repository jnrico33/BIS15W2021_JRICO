---
title: "Group"
author: "Jessica Rico"
date: "3/2/2021"
output: 
  
  html_document: 
    keep_md: yes
    toc: yes
    theme: spacelab
---




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
library(rmarkdown)
library(shiny)
library(shinydashboard)
```

```
## 
## Attaching package: 'shinydashboard'
```

```
## The following object is masked from 'package:graphics':
## 
##     box
```

```r
library(skimr)
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
library(here)
```

```
## here() starts at C:/Users/Jessica/Desktop/BIS15W2021_JRICO
```

```r
library(RColorBrewer)
library(paletteer)
library(ggthemes)
library(naniar)
```

```
## 
## Attaching package: 'naniar'
```

```
## The following object is masked from 'package:skimr':
## 
##     n_complete
```

```r
library(ggpubr)
options(scipen=999)
```

## Term GPA's by Grade


```r
gpa19 <- readr::read_csv("1920 Term gpa.csv")
```

```
## 
## -- Column specification --------------------------------------------------------
## cols(
##   Grade = col_double(),
##   `Term 1` = col_double(),
##   `Term 2` = col_double()
## )
```

```r
gpa19 <- janitor::clean_names(gpa19)
gpa19$grade <- as.factor(gpa19$grade)
gpa19
```

```
## # A tibble: 952 x 3
##    grade term_1 term_2
##    <fct>  <dbl>  <dbl>
##  1 8      NA      2.29
##  2 7       4      4   
##  3 6       2.43   2.43
##  4 8       3.14   3.29
##  5 6       3.29   3.14
##  6 7       3.71   3.71
##  7 7       3.71   4   
##  8 6       1.86   1.57
##  9 7       2.29   2   
## 10 6       3.57   3.71
## # ... with 942 more rows
```

```r
#This is the dataset which shows the term GPA's for the first semester of 2019/2020. Term_1= progress report grades, term_2=report card grades
```



```r
term_2_19 <- gpa19%>%
  select(grade, term_2)

term_2_19 %>%
   group_by(grade)%>%
  summarise(mean(term_2, na.rm=T), totaln=n(), na.rm=T)
```

```
## # A tibble: 3 x 4
##   grade `mean(term_2, na.rm = T)` totaln na.rm
## * <fct>                     <dbl>  <int> <lgl>
## 1 6                          2.85    325 TRUE 
## 2 7                          2.80    294 TRUE 
## 3 8                          2.71    333 TRUE
```


```r
eightgpa_19 <- term_2_19%>%
  filter(grade=="8")%>%
  group_by(term_2)%>%
  summarise(totaln=n())
eightgpa_19
```

```
## # A tibble: 27 x 2
##    term_2 totaln
##  *  <dbl>  <int>
##  1  0.429      1
##  2  0.571      2
##  3  0.714      1
##  4  1          4
##  5  1.14       2
##  6  1.29       5
##  7  1.43       9
##  8  1.57      10
##  9  1.71      10
## 10  1.86       9
## # ... with 17 more rows
```


```r
sevengpa_19 <- term_2_19%>%
  filter(grade=="7")%>%
  group_by(term_2)%>%
  summarise(totaln=n())
sevengpa_19
```

```
## # A tibble: 25 x 2
##    term_2 totaln
##  *  <dbl>  <int>
##  1  0.714      1
##  2  0.857      3
##  3  1.14       2
##  4  1.29       4
##  5  1.43       3
##  6  1.57       5
##  7  1.71      10
##  8  1.86       9
##  9  2          8
## 10  2.14      17
## # ... with 15 more rows
```

```r
sixgpa_19 <- term_2_19%>%
  filter(grade=="6")%>%
  group_by(term_2)%>%
  summarise(totaln=n())
sixgpa_19
```

```
## # A tibble: 25 x 2
##    term_2 totaln
##  *  <dbl>  <int>
##  1  0.571      2
##  2  0.857      1
##  3  1          4
##  4  1.29       1
##  5  1.43       2
##  6  1.57      11
##  7  1.71       5
##  8  1.86      12
##  9  2         16
## 10  2.14      13
## # ... with 15 more rows
```





```r
gpa20 <- readr::read_csv("2021TermGPA.csv")
```

```
## 
## -- Column specification --------------------------------------------------------
## cols(
##   Grade = col_double(),
##   `Term 1` = col_double(),
##   `Term 2` = col_double()
## )
```

```r
gpa20 <- janitor::clean_names(gpa20)
gpa20$grade <- as.factor(gpa20$grade)
gpa20
```

```
## # A tibble: 935 x 3
##    grade term_1 term_2
##    <fct>  <dbl>  <dbl>
##  1 8      3.83   4    
##  2 6      2.29   2.57 
##  3 7      1      0.833
##  4 7      1.14   1.29 
##  5 8      2.33   2    
##  6 8      3      3.57 
##  7 7      1.29   1.29 
##  8 8      0.571  1.14 
##  9 6      3.57   3.43 
## 10 7      2.83   3.14 
## # ... with 925 more rows
```

```r
#This is the dataset which shows the term GPA's for the first semester of 2020/2021. Term_1= progress report grades, term_2=report card grades
```




```r
term_2_20 <- gpa20%>%
  select(grade, term_2)

term_2_20 %>%
  group_by(grade)%>%
  summarise(mean(term_2, na.rm=T), totaln=n(), na.rm=T)
```

```
## # A tibble: 3 x 4
##   grade `mean(term_2, na.rm = T)` totaln na.rm
## * <fct>                     <dbl>  <int> <lgl>
## 1 6                          2.39    351 TRUE 
## 2 7                          2.29    308 TRUE 
## 3 8                          2.13    276 TRUE
```

```r
gpabyyear19 <- term_2_19%>%
  group_by(term_2)%>%
  summarise(totaln=n())
gpabyyear19
```

```
## # A tibble: 30 x 2
##    term_2 totaln
##  *  <dbl>  <int>
##  1  0.429      1
##  2  0.571      4
##  3  0.714      2
##  4  0.857      4
##  5  1          8
##  6  1.14       4
##  7  1.29      10
##  8  1.43      14
##  9  1.57      26
## 10  1.71      25
## # ... with 20 more rows
```



```r
gpabyyear20 <- term_2_20%>%
  group_by(term_2)%>%
  summarise(totaln=n())
gpabyyear20
```

```
## # A tibble: 45 x 2
##    term_2 totaln
##  *  <dbl>  <int>
##  1  0          4
##  2  0.143      3
##  3  0.167      2
##  4  0.2        1
##  5  0.286      5
##  6  0.333      3
##  7  0.4        2
##  8  0.429      9
##  9  0.5        2
## 10  0.571     17
## # ... with 35 more rows
```









```r
eightgpa_20 <- term_2_20%>%
  filter(grade=="8")%>%
  group_by(term_2)%>%
  summarise(totaln=n())
eightgpa_20
```

```
## # A tibble: 37 x 2
##    term_2 totaln
##  *  <dbl>  <int>
##  1  0          1
##  2  0.143      1
##  3  0.286      4
##  4  0.333      1
##  5  0.429      2
##  6  0.5        1
##  7  0.571      8
##  8  0.6        1
##  9  0.714      8
## 10  0.857      9
## # ... with 27 more rows
```




```r
sevengpa_20 <- term_2_20%>%
  filter(grade=="7")%>%
  group_by(term_2)%>%
  summarise(totaln=n())
sevengpa_20
```

```
## # A tibble: 35 x 2
##    term_2 totaln
##  *  <dbl>  <int>
##  1  0          2
##  2  0.143      1
##  3  0.167      1
##  4  0.286      1
##  5  0.333      2
##  6  0.4        1
##  7  0.429      6
##  8  0.571      5
##  9  0.667      2
## 10  0.714      4
## # ... with 25 more rows
```



```r
sixgpa_20 <- term_2_20%>%
  filter(grade=="6")%>%
  group_by(term_2)%>%
  summarise(totaln=n())
sixgpa_20
```

```
## # A tibble: 38 x 2
##    term_2 totaln
##  *  <dbl>  <int>
##  1  0          1
##  2  0.143      1
##  3  0.167      1
##  4  0.2        1
##  5  0.4        1
##  6  0.429      1
##  7  0.5        1
##  8  0.571      4
##  9  0.714      3
## 10  0.8        2
## # ... with 28 more rows
```






## Attendance Rates by Year

```r
attendancerate19 <- readr::read_csv("1920 attendance rates.csv")
```

```
## 
## -- Column specification --------------------------------------------------------
## cols(
##   `Academic Year 19/20` = col_character(),
##   `Attendance Rate %` = col_double()
## )
```

```r
attendancerate19 <- janitor::clean_names(attendancerate19)
attendancerate19
```

```
## # A tibble: 5 x 2
##   academic_year_19_20 attendance_rate_percent
##   <chr>                                 <dbl>
## 1 Month 1                                96.0
## 2 Month 2                                96.4
## 3 Month 3                                95  
## 4 Month 4                                93.7
## 5 Month 5                                93.6
```



```r
attendancerate20 <- readr::read_csv("2021 attendance rates.csv")
```

```
## 
## -- Column specification --------------------------------------------------------
## cols(
##   `Academic Year 20/21` = col_character(),
##   `Attendance Rate %` = col_double()
## )
```

```r
attendancerate20 <- janitor:: clean_names(attendancerate20)
attendancerate20
```

```
## # A tibble: 5 x 2
##   academic_year_20_21 attendance_rate_percent
##   <chr>                                 <dbl>
## 1 Month 1                                88.6
## 2 Month 2                                88.6
## 3 Month 3                                94.3
## 4 Month 4                                94.6
## 5 Month 5                                92.5
```

## Attendance Reasons by Year


```r
reason19 <- readr::read_csv("1920 attendance reason report.csv")
```

```
## Warning: Missing column names filled in: 'X1' [1]
```

```
## 
## -- Column specification --------------------------------------------------------
## cols(
##   X1 = col_character(),
##   Dr = col_double(),
##   ILL = col_double(),
##   EXC = col_double(),
##   UNV = col_double(),
##   VUN = col_double(),
##   OTH = col_double()
## )
```

```r
reason19tidy <- reason19%>% 
  pivot_longer(-X1, # does not pivot
               names_to = "attendance_code", 
               values_to = "occurence"
               )%>%
  filter(X1=="Periods")%>%
  select(attendance_code, occurence)%>%
  arrange(desc(occurence))
reason19tidy
```

```
## # A tibble: 6 x 2
##   attendance_code occurence
##   <chr>               <dbl>
## 1 UNV                 12433
## 2 ILL                  7484
## 3 Dr                   3226
## 4 OTH                  1833
## 5 EXC                  1822
## 6 VUN                   923
```



```r
reason20 <- readr::read_csv("2021 attendance reason report.csv")
```

```
## Warning: Missing column names filled in: 'X1' [1]
```

```
## 
## -- Column specification --------------------------------------------------------
## cols(
##   X1 = col_character(),
##   DR = col_double(),
##   ILL = col_double(),
##   EXC = col_double(),
##   AVP = col_double(),
##   OTH = col_double(),
##   VUN = col_double(),
##   ILL19 = col_double(),
##   NWFA = col_double(),
##   DAC = col_double(),
##   NCA = col_double()
## )
```

```r
reason20tidy <- reason20%>% 
  pivot_longer(-X1, # does not pivot
               names_to = "attendance_code", 
               values_to = "occurence"
               )%>%
  filter(X1=="Periods")%>%
  select(attendance_code, occurence)%>%
  arrange(desc(occurence))
reason20tidy
```

```
## # A tibble: 10 x 2
##    attendance_code occurence
##    <chr>               <dbl>
##  1 AVP                 73543
##  2 NWFA                 1625
##  3 EXC                   970
##  4 ILL19                 588
##  5 NCA                   515
##  6 OTH                   457
##  7 ILL                   246
##  8 DR                    243
##  9 DAC                    99
## 10 VUN                    63
```



