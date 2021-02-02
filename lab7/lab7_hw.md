---
title: "Lab 7 Homework"
author: "Jessica Rico"
date: "2021-02-02"
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
library(skimr)
```

## Data
**1. For this homework, we will use two different data sets. Please load `amniota` and `amphibio`.**  

`amniota` data:  
Myhrvold N, Baldridge E, Chan B, Sivam D, Freeman DL, Ernest SKM (2015). “An amniote life-history
database to perform comparative analyses with birds, mammals, and reptiles.” _Ecology_, *96*, 3109.
doi: 10.1890/15-0846.1 (URL: https://doi.org/10.1890/15-0846.1).

```r
amniota <- readr::read_csv("data/amniota.csv")
```

```
## 
## -- Column specification --------------------------------------------------------
## cols(
##   .default = col_double(),
##   class = col_character(),
##   order = col_character(),
##   family = col_character(),
##   genus = col_character(),
##   species = col_character(),
##   common_name = col_character()
## )
## i Use `spec()` for the full column specifications.
```

`amphibio` data:  
Oliveira BF, São-Pedro VA, Santos-Barrera G, Penone C, Costa GC (2017). “AmphiBIO, a global database
for amphibian ecological traits.” _Scientific Data_, *4*, 170123. doi: 10.1038/sdata.2017.123 (URL:
https://doi.org/10.1038/sdata.2017.123).

```r
amphibio <- readr::read_csv("data/amphibio.csv")
```

```
## 
## -- Column specification --------------------------------------------------------
## cols(
##   .default = col_double(),
##   id = col_character(),
##   Order = col_character(),
##   Family = col_character(),
##   Genus = col_character(),
##   Species = col_character(),
##   Seeds = col_logical(),
##   OBS = col_logical()
## )
## i Use `spec()` for the full column specifications.
```

```
## Warning: 125 parsing failures.
##  row col           expected                                                           actual                file
## 1410 OBS 1/0/T/F/TRUE/FALSE Identified as P. appendiculata in Boquimpani-Freitas et al. 2002 'data/amphibio.csv'
## 1416 OBS 1/0/T/F/TRUE/FALSE Identified as T. miliaris in Giaretta and Facure 2004            'data/amphibio.csv'
## 1447 OBS 1/0/T/F/TRUE/FALSE Considered endangered by Soto-Azat et al. 2013                   'data/amphibio.csv'
## 1448 OBS 1/0/T/F/TRUE/FALSE Considered extinct by Soto-Azat et al. 2013                      'data/amphibio.csv'
## 1471 OBS 1/0/T/F/TRUE/FALSE nomem dubitum                                                    'data/amphibio.csv'
## .... ... .................. ................................................................ ...................
## See problems(...) for more details.
```

## Questions  
**2. Do some exploratory analysis of the `amniota` data set. Use the function(s) of your choice. Try to get an idea of how NA's are represented in the data.**  


```r
glimpse(amniota)
```

```
## Rows: 21,322
## Columns: 36
## $ class                                 <chr> "Aves", "Aves", "Aves", "Aves...
## $ order                                 <chr> "Accipitriformes", "Accipitri...
## $ family                                <chr> "Accipitridae", "Accipitridae...
## $ genus                                 <chr> "Accipiter", "Accipiter", "Ac...
## $ species                               <chr> "albogularis", "badius", "bic...
## $ subspecies                            <dbl> -999, -999, -999, -999, -999,...
## $ common_name                           <chr> "Pied Goshawk", "Shikra", "Bi...
## $ female_maturity_d                     <dbl> -999.000, 363.468, -999.000, ...
## $ litter_or_clutch_size_n               <dbl> -999.000, 3.250, 2.700, -999....
## $ litters_or_clutches_per_y             <dbl> -999, 1, -999, -999, 1, -999,...
## $ adult_body_mass_g                     <dbl> 251.500, 140.000, 345.000, 14...
## $ maximum_longevity_y                   <dbl> -999.00000, -999.00000, -999....
## $ gestation_d                           <dbl> -999, -999, -999, -999, -999,...
## $ weaning_d                             <dbl> -999, -999, -999, -999, -999,...
## $ birth_or_hatching_weight_g            <dbl> -999, -999, -999, -999, -999,...
## $ weaning_weight_g                      <dbl> -999, -999, -999, -999, -999,...
## $ egg_mass_g                            <dbl> -999.00, 21.00, 32.00, -999.0...
## $ incubation_d                          <dbl> -999.00, 30.00, -999.00, -999...
## $ fledging_age_d                        <dbl> -999.00, 32.00, -999.00, -999...
## $ longevity_y                           <dbl> -999.00000, -999.00000, -999....
## $ male_maturity_d                       <dbl> -999, -999, -999, -999, -999,...
## $ inter_litter_or_interbirth_interval_y <dbl> -999, -999, -999, -999, -999,...
## $ female_body_mass_g                    <dbl> 352.500, 168.500, 390.000, -9...
## $ male_body_mass_g                      <dbl> 223.000, 125.000, 212.000, 14...
## $ no_sex_body_mass_g                    <dbl> -999.0, 123.0, -999.0, -999.0...
## $ egg_width_mm                          <dbl> -999, -999, -999, -999, -999,...
## $ egg_length_mm                         <dbl> -999, -999, -999, -999, -999,...
## $ fledging_mass_g                       <dbl> -999, -999, -999, -999, -999,...
## $ adult_svl_cm                          <dbl> -999.00, 30.00, 39.50, -999.0...
## $ male_svl_cm                           <dbl> -999, -999, -999, -999, -999,...
## $ female_svl_cm                         <dbl> -999, -999, -999, -999, -999,...
## $ birth_or_hatching_svl_cm              <dbl> -999, -999, -999, -999, -999,...
## $ female_svl_at_maturity_cm             <dbl> -999, -999, -999, -999, -999,...
## $ female_body_mass_at_maturity_g        <dbl> -999, -999, -999, -999, -999,...
## $ no_sex_svl_cm                         <dbl> -999, -999, -999, -999, -999,...
## $ no_sex_maturity_d                     <dbl> -999, -999, -999, -999, -999,...
```

**3. Do some exploratory analysis of the `amphibio` data set. Use the function(s) of your choice. Try to get an idea of how NA's are represented in the data.**  


```r
summary(amphibio)
```

```
##       id               Order              Family             Genus          
##  Length:6776        Length:6776        Length:6776        Length:6776       
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##                                                                             
##    Species               Fos            Ter            Aqu            Arb      
##  Length:6776        Min.   :1      Min.   :1      Min.   :1      Min.   :1     
##  Class :character   1st Qu.:1      1st Qu.:1      1st Qu.:1      1st Qu.:1     
##  Mode  :character   Median :1      Median :1      Median :1      Median :1     
##                     Mean   :1      Mean   :1      Mean   :1      Mean   :1     
##                     3rd Qu.:1      3rd Qu.:1      3rd Qu.:1      3rd Qu.:1     
##                     Max.   :1      Max.   :1      Max.   :1      Max.   :1     
##                     NA's   :6053   NA's   :1104   NA's   :2810   NA's   :4347  
##      Leaves        Flowers      Seeds             Fruits         Arthro    
##  Min.   :1      Min.   :1      Mode:logical   Min.   :1      Min.   :1     
##  1st Qu.:1      1st Qu.:1      TRUE:4         1st Qu.:1      1st Qu.:1     
##  Median :1      Median :1      NA's:6772      Median :1      Median :1     
##  Mean   :1      Mean   :1                     Mean   :1      Mean   :1     
##  3rd Qu.:1      3rd Qu.:1                     3rd Qu.:1      3rd Qu.:1     
##  Max.   :1      Max.   :1                     Max.   :1      Max.   :1     
##  NA's   :6752   NA's   :6772                  NA's   :6774   NA's   :5534  
##       Vert           Diu            Noc           Crepu         Wet_warm   
##  Min.   :1      Min.   :1      Min.   :1      Min.   :1      Min.   :1     
##  1st Qu.:1      1st Qu.:1      1st Qu.:1      1st Qu.:1      1st Qu.:1     
##  Median :1      Median :1      Median :1      Median :1      Median :1     
##  Mean   :1      Mean   :1      Mean   :1      Mean   :1      Mean   :1     
##  3rd Qu.:1      3rd Qu.:1      3rd Qu.:1      3rd Qu.:1      3rd Qu.:1     
##  Max.   :1      Max.   :1      Max.   :1      Max.   :1      Max.   :1     
##  NA's   :6657   NA's   :5876   NA's   :5156   NA's   :6608   NA's   :5997  
##     Wet_cold       Dry_warm       Dry_cold     Body_mass_g      
##  Min.   :1      Min.   :1      Min.   :1      Min.   :    0.16  
##  1st Qu.:1      1st Qu.:1      1st Qu.:1      1st Qu.:    2.60  
##  Median :1      Median :1      Median :1      Median :    9.29  
##  Mean   :1      Mean   :1      Mean   :1      Mean   :   94.56  
##  3rd Qu.:1      3rd Qu.:1      3rd Qu.:1      3rd Qu.:   31.82  
##  Max.   :1      Max.   :1      Max.   :1      Max.   :26000.00  
##  NA's   :6625   NA's   :6572   NA's   :6735   NA's   :6185      
##  Age_at_maturity_min_y Age_at_maturity_max_y  Body_size_mm    
##  Min.   :0.25          Min.   : 0.300        Min.   :   8.40  
##  1st Qu.:1.00          1st Qu.: 2.000        1st Qu.:  29.00  
##  Median :2.00          Median : 3.000        Median :  43.00  
##  Mean   :2.14          Mean   : 2.964        Mean   :  66.65  
##  3rd Qu.:3.00          3rd Qu.: 4.000        3rd Qu.:  69.15  
##  Max.   :7.00          Max.   :12.000        Max.   :1520.00  
##  NA's   :6392          NA's   :6392          NA's   :1549     
##  Size_at_maturity_min_mm Size_at_maturity_max_mm Longevity_max_y 
##  Min.   :  8.80          Min.   : 10.10          Min.   :  0.17  
##  1st Qu.: 27.50          1st Qu.: 32.00          1st Qu.:  6.00  
##  Median : 43.00          Median : 50.00          Median : 10.00  
##  Mean   : 56.63          Mean   : 67.46          Mean   : 11.68  
##  3rd Qu.: 58.00          3rd Qu.: 75.50          3rd Qu.: 15.00  
##  Max.   :350.00          Max.   :400.00          Max.   :121.80  
##  NA's   :6529            NA's   :6528            NA's   :6417    
##  Litter_size_min_n Litter_size_max_n Reproductive_output_y
##  Min.   :    1.0   Min.   :    1     Min.   : 0.080       
##  1st Qu.:   18.0   1st Qu.:   30     1st Qu.: 1.000       
##  Median :   80.0   Median :  186     Median : 1.000       
##  Mean   :  530.9   Mean   : 1034     Mean   : 1.034       
##  3rd Qu.:  300.0   3rd Qu.:  700     3rd Qu.: 1.000       
##  Max.   :25000.0   Max.   :45054     Max.   :15.000       
##  NA's   :5153      NA's   :5153      NA's   :2344         
##  Offspring_size_min_mm Offspring_size_max_mm      Dir              Lar        
##  Min.   : 0.200        Min.   : 0.400        Min.   :0.0000   Min.   :0.0000  
##  1st Qu.: 1.400        1st Qu.: 1.600        1st Qu.:0.0000   1st Qu.:0.0000  
##  Median : 2.000        Median : 2.300        Median :0.0000   Median :1.0000  
##  Mean   : 2.455        Mean   : 2.857        Mean   :0.2984   Mean   :0.6921  
##  3rd Qu.: 3.000        3rd Qu.: 3.500        3rd Qu.:1.0000   3rd Qu.:1.0000  
##  Max.   :20.000        Max.   :25.000        Max.   :1.0000   Max.   :1.0000  
##  NA's   :5446          NA's   :5446          NA's   :1079     NA's   :1079    
##       Viv           OBS         
##  Min.   :0.0000   Mode:logical  
##  1st Qu.:0.0000   NA's:6776     
##  Median :0.0000                 
##  Mean   :0.0095                 
##  3rd Qu.:0.0000                 
##  Max.   :1.0000                 
##  NA's   :1079
```

**4. How many total NA's are in each data set? Do these values make sense? Are NA's represented by values?**   


```r
amphibio %>% 
  summarize(total_amphibio_NA=sum(is.na(amphibio)))
```

```
## # A tibble: 1 x 1
##   total_amphibio_NA
##               <int>
## 1            170691
```


```r
amphibio %>% 
  purrr::map_df(~ sum(is.na(.))) %>% 
  pivot_longer(everything(),
    names_to= "variables",
    values_to = "num_nas") %>% 
  arrange(desc(num_nas))
```

```
## # A tibble: 38 x 2
##    variables num_nas
##    <chr>       <int>
##  1 OBS          6776
##  2 Fruits       6774
##  3 Flowers      6772
##  4 Seeds        6772
##  5 Leaves       6752
##  6 Dry_cold     6735
##  7 Vert         6657
##  8 Wet_cold     6625
##  9 Crepu        6608
## 10 Dry_warm     6572
## # ... with 28 more rows
```

**5. Make any necessary replacements in the data such that all NA's appear as "NA".**   

```r
amniota_v2 <- amniota %>% 
  na_if("-999") %>% 
  na_if("-30258.711")
```


```r
amniota_v2 %>% 
  summarize(amniota_v2_NA_tot=sum(is.na(amniota_v2)))
```

```
## # A tibble: 1 x 1
##   amniota_v2_NA_tot
##               <int>
## 1            528200
```

**6. Use the package `naniar` to produce a summary, including percentages, of missing data in each column for the `amniota` data.**  

```r
naniar::miss_var_summary(amniota_v2)
```

```
## # A tibble: 36 x 3
##    variable                       n_miss pct_miss
##    <chr>                           <int>    <dbl>
##  1 subspecies                      21322    100  
##  2 female_body_mass_at_maturity_g  21318    100. 
##  3 female_svl_at_maturity_cm       21120     99.1
##  4 fledging_mass_g                 21111     99.0
##  5 male_svl_cm                     21040     98.7
##  6 no_sex_maturity_d               20860     97.8
##  7 egg_width_mm                    20727     97.2
##  8 egg_length_mm                   20702     97.1
##  9 weaning_weight_g                20258     95.0
## 10 female_svl_cm                   20242     94.9
## # ... with 26 more rows
```

**7. Use the package `naniar` to produce a summary, including percentages, of missing data in each column for the `amphibio` data.**

```r
naniar::miss_var_summary(amphibio)
```

```
## # A tibble: 38 x 3
##    variable n_miss pct_miss
##    <chr>     <int>    <dbl>
##  1 OBS        6776    100  
##  2 Fruits     6774    100. 
##  3 Flowers    6772     99.9
##  4 Seeds      6772     99.9
##  5 Leaves     6752     99.6
##  6 Dry_cold   6735     99.4
##  7 Vert       6657     98.2
##  8 Wet_cold   6625     97.8
##  9 Crepu      6608     97.5
## 10 Dry_warm   6572     97.0
## # ... with 28 more rows
```

**8. For the `amniota` data, calculate the number of NAs in the `egg_mass_g` column sorted by taxonomic class; i.e. how many NA's are present in the `egg_mass_g` column in birds, mammals, and reptiles? Does this results make sense biologically? How do these results affect your interpretation of NA's?**  


```r
amniota_v2 %>% 
  select(class, egg_mass_g) %>% 
  group_by(class) %>% 
  naniar::miss_var_summary(order=T)
```

```
## # A tibble: 3 x 4
## # Groups:   class [3]
##   class    variable   n_miss pct_miss
##   <chr>    <chr>       <int>    <dbl>
## 1 Aves     egg_mass_g   4914     50.1
## 2 Mammalia egg_mass_g   4953    100  
## 3 Reptilia egg_mass_g   6040     92.0
```

**9. The `amphibio` data have variables that classify species as fossorial (burrowing), terrestrial, aquatic, or arboreal.Calculate the number of NA's in each of these variables. Do you think that the authors intend us to think that there are NA's in these columns or could they represent something else? Explain.**

```r
amphibio %>% 
  select(Fos, Ter, Aqu, Arb) %>% 
  naniar::miss_var_summary(order = T)
```

```
## # A tibble: 4 x 3
##   variable n_miss pct_miss
##   <chr>     <int>    <dbl>
## 1 Fos        6053     89.3
## 2 Arb        4347     64.2
## 3 Aqu        2810     41.5
## 4 Ter        1104     16.3
```

**10. Now that we know how NA's are represented in the `amniota` data, how would you load the data such that the values which represent NA's are automatically converted?**

```r
amniota_final <- readr::read_csv(file = "data/amniota.csv", 
                  na = c("NA", " ", ".", "-999", "-30258.711")) #all NA, blank spaces, .,and -999 are treated as NA
```

```
## 
## -- Column specification --------------------------------------------------------
## cols(
##   .default = col_double(),
##   class = col_character(),
##   order = col_character(),
##   family = col_character(),
##   genus = col_character(),
##   species = col_character(),
##   subspecies = col_logical(),
##   common_name = col_character(),
##   gestation_d = col_logical(),
##   weaning_d = col_logical(),
##   weaning_weight_g = col_logical(),
##   male_svl_cm = col_logical(),
##   female_svl_cm = col_logical(),
##   birth_or_hatching_svl_cm = col_logical(),
##   female_svl_at_maturity_cm = col_logical(),
##   female_body_mass_at_maturity_g = col_logical(),
##   no_sex_svl_cm = col_logical()
## )
## i Use `spec()` for the full column specifications.
```

```
## Warning: 13577 parsing failures.
##  row                      col           expected actual               file
## 9803 birth_or_hatching_svl_cm 1/0/T/F/TRUE/FALSE    4.7 'data/amniota.csv'
## 9804 birth_or_hatching_svl_cm 1/0/T/F/TRUE/FALSE    4.7 'data/amniota.csv'
## 9805 birth_or_hatching_svl_cm 1/0/T/F/TRUE/FALSE    4.7 'data/amniota.csv'
## 9806 birth_or_hatching_svl_cm 1/0/T/F/TRUE/FALSE    4.7 'data/amniota.csv'
## 9807 birth_or_hatching_svl_cm 1/0/T/F/TRUE/FALSE    4.7 'data/amniota.csv'
## .... ........................ .................. ...... ..................
## See problems(...) for more details.
```

## Push your final code to GitHub!
Please be sure that you check the `keep md` file in the knit preferences.  
