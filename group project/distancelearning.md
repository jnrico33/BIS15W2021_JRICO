---
title: "distancelearning"
author: "Jessica Rico"
date: "3/3/2021"
output: 
  html_document: 
    keep_md: yes
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


## Loading datasets


```r
att_rates <- readr::read_csv("att_rates.csv")
```

```
## 
## -- Column specification --------------------------------------------------------
## cols(
##   `Academic Year` = col_character(),
##   `Attendance Rate %` = col_double(),
##   student_add = col_double(),
##   student_drop = col_double()
## )
```

```r
att_rates <- janitor::clean_names(att_rates)
att_rates$academic_year <- as.factor(att_rates$academic_year)
att_rates
```

```
## # A tibble: 10 x 4
##    academic_year attendance_rate_percent student_add student_drop
##    <fct>                           <dbl>       <dbl>        <dbl>
##  1 2019_1                           96.0          22           12
##  2 2019_2                           96.4           8           22
##  3 2019_3                           95            15           13
##  4 2019_4                           93.7           8           19
##  5 2019_5                           93.6           8           19
##  6 2020_1                           88.6          20            6
##  7 2020_2                           88.6          17            6
##  8 2020_3                           94.3           7            4
##  9 2020_4                           94.6           8            6
## 10 2020_5                           92.5           1            6
```

```r
#This shows the attendance rate for attendance month 1-5 (2019_1= 2019 academic year, month 1). This also includes the number of students enrolled and withdrawn from the middle school during the respective month. 
```


```r
att_ratestidy <- att_rates%>%
  pivot_longer(-academic_year, 
               names_to = "academic_year_values", 
               values_to = "student_numbers")
att_ratestidy
```

```
## # A tibble: 30 x 3
##    academic_year academic_year_values    student_numbers
##    <fct>         <chr>                             <dbl>
##  1 2019_1        attendance_rate_percent            96.0
##  2 2019_1        student_add                        22  
##  3 2019_1        student_drop                       12  
##  4 2019_2        attendance_rate_percent            96.4
##  5 2019_2        student_add                         8  
##  6 2019_2        student_drop                       22  
##  7 2019_3        attendance_rate_percent            95  
##  8 2019_3        student_add                        15  
##  9 2019_3        student_drop                       13  
## 10 2019_4        attendance_rate_percent            93.7
## # ... with 20 more rows
```





```r
ui <- dashboardPage(
    dashboardHeader(title = "Attendance, Enrollment, and Withdrawals by Month"),
  dashboardSidebar(),
  dashboardBody(
  selectInput("x", "Select X variable", choices = c("academic_year_values"), selected = "academic_year"),
  plotOutput("plot", width = "800px", height = "400px"))
)



server <- function(input, output, session) { 
 output$plot <- renderPlot({
  att_ratestidy %>% 
  ggplot(aes_string(x = "academic_year", y = "student_numbers", fill=input$x)) +
  geom_col(position="dodge", alpha = 0.8, size=4)+
  theme_light(base_size = 18) + 
     labs(title= "Attendance Rates, Enrollment, and Withdrawals by Year and Month", x="Academic Year_Month", y="Student Numbers", fill = "Legend")+
     theme(axis.text.x = element_text(angle = 90, hjust = 1))
  })
  session$onSessionEnded(stopApp)
  }
shinyApp(ui, server)
```





```r
term_gpa <- readr::read_csv("term_gpas.csv")
```

```
## 
## -- Column specification --------------------------------------------------------
## cols(
##   Year = col_double(),
##   Grade = col_character(),
##   `Term 1` = col_character(),
##   `Term 2` = col_character()
## )
```

```r
term_gpa <- janitor::clean_names(term_gpa)
term_gpa
```

```
## # A tibble: 1,888 x 4
##     year grade term_1 term_2
##    <dbl> <chr> <chr>  <chr> 
##  1  2019 8     <NA>   2.286 
##  2  2019 7     4      4     
##  3  2019 6     2.429  2.429 
##  4  2019 8     3.143  3.286 
##  5  2019 6     3.286  3.143 
##  6  2019 7     3.714  3.714 
##  7  2019 7     3.714  4     
##  8  2019 6     1.857  1.571 
##  9  2019 7     2.286  2     
## 10  2019 6     3.571  3.714 
## # ... with 1,878 more rows
```





```r
term_2_gpa <- term_gpa%>%
  select(year, grade, term_2)%>%
  filter(grade!="Grade")
term_2_gpa
```

```
## # A tibble: 1,887 x 3
##     year grade term_2
##    <dbl> <chr> <chr> 
##  1  2019 8     2.286 
##  2  2019 7     4     
##  3  2019 6     2.429 
##  4  2019 8     3.286 
##  5  2019 6     3.143 
##  6  2019 7     3.714 
##  7  2019 7     4     
##  8  2019 6     1.571 
##  9  2019 7     2     
## 10  2019 6     3.714 
## # ... with 1,877 more rows
```

```r
term_2_gpa$term_2<-as.numeric(term_2_gpa$term_2)

term_2_gpa%>%
  group_by(grade, year)%>%
  summarise(mean(term_2, na.rm=T), totaln=n(), na.rm=T)
```

```
## `summarise()` has grouped output by 'grade'. You can override using the `.groups` argument.
```

```
## # A tibble: 6 x 5
## # Groups:   grade [3]
##   grade  year `mean(term_2, na.rm = T)` totaln na.rm
##   <chr> <dbl>                     <dbl>  <int> <lgl>
## 1 6      2019                      2.85    325 TRUE 
## 2 6      2020                      2.39    351 TRUE 
## 3 7      2019                      2.80    294 TRUE 
## 4 7      2020                      2.29    308 TRUE 
## 5 8      2019                      2.71    333 TRUE 
## 6 8      2020                      2.13    276 TRUE
```





```r
att_reason <- readr::read_csv("att_reason_reports.csv")
```

```
## 
## -- Column specification --------------------------------------------------------
## cols(
##   count_type = col_character(),
##   year = col_double(),
##   DR = col_double(),
##   ILL = col_double(),
##   EXC = col_double(),
##   UNV = col_double(),
##   VUN = col_double(),
##   OTH = col_double(),
##   AVP = col_double(),
##   ILL19 = col_double(),
##   NWFA = col_double(),
##   DAC = col_double(),
##   NCA = col_double()
## )
```

```r
att_reason
```

```
## # A tibble: 4 x 13
##   count_type  year     DR    ILL   EXC    UNV    VUN    OTH    AVP ILL19  NWFA
##   <chr>      <dbl>  <dbl>  <dbl> <dbl>  <dbl>  <dbl>  <dbl>  <dbl> <dbl> <dbl>
## 1 periods     2020  243    246    970      0   63     457   73543  588    1625
## 2 days        2020   32.5   32.5  129.     0    8.75   61.0  9660.  78.0     0
## 3 periods     2019 3226   7484   1822  12433  923    1833       0    0       0
## 4 days        2019  427.   100.   244.  1663. 122.    244.      0    0       0
## # ... with 2 more variables: DAC <dbl>, NCA <dbl>
```





```r
names(att_reason)
```

```
##  [1] "count_type" "year"       "DR"         "ILL"        "EXC"       
##  [6] "UNV"        "VUN"        "OTH"        "AVP"        "ILL19"     
## [11] "NWFA"       "DAC"        "NCA"
```



```r
#att_reason$year <- as.character(att_reason$year)

#att_reasontidy <- att_reason%>%
  #pivot_longer(-count_type,
      #         -year, # does not pivot
     #          names_to = "attendance_code", 
    #           values_to = "occurence"
   #            )%>%
  #filter(count_type=="periods")%>%
 # select(attendance_code, occurence)%>%
#  arrange(desc(occurence))
#att_reasontidy
```

