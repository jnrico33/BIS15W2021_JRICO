---
title: "Lab 11 Homework"
author: "Jessica Rico"
date: "2021-02-17"
output:
  html_document: 
    theme: spacelab
    keep_md: yes
---



## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your final lab report should be organized, clean, and run free from errors. Remember, you must remove the `#` for the included code chunks to run. Be sure to add your name to the author header above. For any included plots, make sure they are clearly labeled. You are free to use any plot type that you feel best communicates the results of your analysis.  

**In this homework, you should make use of the aesthetics you have learned. It's OK to be flashy!**

Make sure to use the formatting conventions of RMarkdown to make your report neat and clean!  

## Load the libraries

```r
library(tidyverse)
library(janitor)
library(here)
library(naniar)
library(ggthemes)
```

## Resources
The idea for this assignment came from [Rebecca Barter's](http://www.rebeccabarter.com/blog/2017-11-17-ggplot2_tutorial/) ggplot tutorial so if you get stuck this is a good place to have a look.  

## Gapminder
For this assignment, we are going to use the dataset [gapminder](https://cran.r-project.org/web/packages/gapminder/index.html). Gapminder includes information about economics, population, and life expectancy from countries all over the world. You will need to install it before use. This is the same data that we will use for midterm 2 so this is good practice.

```r
#install.packages("gapminder")
library("gapminder")
```

## Questions
The questions below are open-ended and have many possible solutions. Your approach should, where appropriate, include numerical summaries and visuals. Be creative; assume you are building an analysis that you would ultimately present to an audience of stakeholders. Feel free to try out different `geoms` if they more clearly present your results.  

**1. Use the function(s) of your choice to get an idea of the overall structure of the data frame, including its dimensions, column names, variable classes, etc. As part of this, determine how NA's are treated in the data.**  

```r
summary(gapminder)
```

```
##         country        continent        year         lifeExp     
##  Afghanistan:  12   Africa  :624   Min.   :1952   Min.   :23.60  
##  Albania    :  12   Americas:300   1st Qu.:1966   1st Qu.:48.20  
##  Algeria    :  12   Asia    :396   Median :1980   Median :60.71  
##  Angola     :  12   Europe  :360   Mean   :1980   Mean   :59.47  
##  Argentina  :  12   Oceania : 24   3rd Qu.:1993   3rd Qu.:70.85  
##  Australia  :  12                  Max.   :2007   Max.   :82.60  
##  (Other)    :1632                                                
##       pop              gdpPercap       
##  Min.   :6.001e+04   Min.   :   241.2  
##  1st Qu.:2.794e+06   1st Qu.:  1202.1  
##  Median :7.024e+06   Median :  3531.8  
##  Mean   :2.960e+07   Mean   :  7215.3  
##  3rd Qu.:1.959e+07   3rd Qu.:  9325.5  
##  Max.   :1.319e+09   Max.   :113523.1  
## 
```

```r
options(scipen = 999)
```



```r
naniar::miss_var_summary(gapminder)
```

```
## # A tibble: 6 x 3
##   variable  n_miss pct_miss
##   <chr>      <int>    <dbl>
## 1 country        0        0
## 2 continent      0        0
## 3 year           0        0
## 4 lifeExp        0        0
## 5 pop            0        0
## 6 gdpPercap      0        0
```

**2. Among the interesting variables in gapminder is life expectancy. How has global life expectancy changed between 1952 and 2007?**

```r
global_expectancy <- gapminder %>% 
  group_by(year) %>% 
  summarise(glob_life_expectancy=mean(lifeExp),
            .groups = 'keep')
global_expectancy
```

```
## # A tibble: 12 x 2
## # Groups:   year [12]
##     year glob_life_expectancy
##    <int>                <dbl>
##  1  1952                 49.1
##  2  1957                 51.5
##  3  1962                 53.6
##  4  1967                 55.7
##  5  1972                 57.6
##  6  1977                 59.6
##  7  1982                 61.5
##  8  1987                 63.2
##  9  1992                 64.2
## 10  1997                 65.0
## 11  2002                 65.7
## 12  2007                 67.0
```

```r
gapminder$year<-as.factor(gapminder$year)
```



```r
global_expectancy %>% 
  ggplot(aes(x=year, y=glob_life_expectancy, color=glob_life_expectancy))+
  geom_line(alpha=0.5)+
  geom_point(shape=8)+
  theme(axis.text.x = element_text(angle = 30, hjust = 1))+
  labs(title = "Global Life Expectancy [1952-2007]",
       x = "Year",
       y = "Life Expectancy (Global)")
```

![](lab11_hw_files/figure-html/unnamed-chunk-8-1.png)<!-- -->

**3. How do the distributions of life expectancy compare for the years 1952 and 2007?**

```r
gapminder %>% 
  group_by(year) %>% 
  filter(year==1952 | year==2007) %>% 
  summarize(min_lifeExp=min(lifeExp),
            max_lifeExp=max(lifeExp),
            med_lifeExp=median(lifeExp),
            .groups = 'keep')
```

```
## # A tibble: 2 x 4
## # Groups:   year [2]
##   year  min_lifeExp max_lifeExp med_lifeExp
##   <fct>       <dbl>       <dbl>       <dbl>
## 1 1952         28.8        72.7        45.1
## 2 2007         39.6        82.6        71.9
```

```r
gapminder %>% 
  group_by(year) %>% 
  filter(year==1952 | year==2007) %>% 
  ggplot(aes(x=year, y=lifeExp, fill=year))+
  geom_boxplot(color="ivory4")+
  labs(title = "Difference in Global Life Expectancy between 1952 and 2007",
       x = "Year",
       y = "LifeExp")
```

![](lab11_hw_files/figure-html/unnamed-chunk-10-1.png)<!-- -->

**4. Your answer above doesn't tell the whole story since life expectancy varies by region. Make a summary that shows the min, mean, and max life expectancy by continent for all years represented in the data.**

```r
gapminder %>% 
  group_by(continent, year) %>% 
  summarize(min_lifeExp=min(lifeExp),
            max_lifeExp=max(lifeExp),
            avg_lifeExp=mean(lifeExp),
            .groups = 'keep')
```

```
## # A tibble: 60 x 5
## # Groups:   continent, year [60]
##    continent year  min_lifeExp max_lifeExp avg_lifeExp
##    <fct>     <fct>       <dbl>       <dbl>       <dbl>
##  1 Africa    1952         30          52.7        39.1
##  2 Africa    1957         31.6        58.1        41.3
##  3 Africa    1962         32.8        60.2        43.3
##  4 Africa    1967         34.1        61.6        45.3
##  5 Africa    1972         35.4        64.3        47.5
##  6 Africa    1977         36.8        67.1        49.6
##  7 Africa    1982         38.4        69.9        51.6
##  8 Africa    1987         39.9        71.9        53.3
##  9 Africa    1992         23.6        73.6        53.6
## 10 Africa    1997         36.1        74.8        53.6
## # ... with 50 more rows
```

```r
gapminder %>% 
  group_by(continent, year) %>% 
  ggplot(aes(x=continent, y=lifeExp, color=continent))+
  geom_boxplot(alpha=0.6)+facet_wrap(~year)+
  theme(axis.text.x = element_text(angle = 35, hjust = 0.5))+
  labs(title = "Life Expectancy Across Continents by Year",
       x = "Continent",
       y = "lifeExp")
```

![](lab11_hw_files/figure-html/unnamed-chunk-12-1.png)<!-- -->

**5. How has life expectancy changed between 1952-2007 for each continent?**

```r
gapminder %>% 
  group_by(continent, year) %>% 
  summarize(min_lifeExp=min(lifeExp),
            max_lifeExp=max(lifeExp),
            avg_lifeExp=mean(lifeExp),
            .groups = 'keep') %>% 
  ggplot(aes(x=year, y=avg_lifeExp, group=1))+
  geom_line(color="royalblue")+
  facet_wrap(~continent)+
  theme(axis.text.x = element_text(angle=70, hjust=0.5))+
  labs(title = "Average Life Expectancy Per Continent [1952-2007]",
       x = "Year",
       y = "Average Life Expectancy")
```

![](lab11_hw_files/figure-html/unnamed-chunk-13-1.png)<!-- -->

**6. We are interested in the relationship between per capita GDP and life expectancy; i.e. does having more money help you live longer?**

```r
gapminder %>% 
  group_by(country, continent) %>%
  summarize(avg_life_exp=mean(lifeExp),
            avg_gdp=mean(gdpPercap),.groups = 'keep') %>% 
  ggplot(aes(x=avg_life_exp, y=avg_gdp, color=continent))+
  theme_bw()+
  geom_point(alpha=1, size=0.75)+
  labs(title = "Per Capita GDP vs Life Expectancy by Continent",
       x = "Mean Life Expectancy [Years]",
       y = "Mean Per Capita GDP")
```

![](lab11_hw_files/figure-html/unnamed-chunk-14-1.png)<!-- -->

**7. Which countries have had the largest population growth since 1952?**

```r
gapminder %>% 
  select(year, pop, country) %>% 
  filter(year==1952 | year==2007) %>% 
  group_by(country) %>% 
  summarize(large_pop_growth=diff(pop),.groups = 'keep') %>% 
  arrange(desc(large_pop_growth)) %>% 
  head(n=5)
```

```
## # A tibble: 5 x 2
## # Groups:   country [5]
##   country       large_pop_growth
##   <fct>                    <int>
## 1 China                762419569
## 2 India                738396331
## 3 United States        143586947
## 4 Indonesia            141495000
## 5 Brazil               133408087
```

**8. Use your results from the question above to plot population growth for the top five countries since 1952.**

```r
gapminder %>% 
  filter(year==1952 | year==2007) %>% 
  group_by(country) %>% 
  summarize(large_pop_growth=diff(pop),.groups = 'keep') %>% 
  arrange(desc(large_pop_growth)) %>% 
  head(n=5) %>% 
  ggplot(aes(x=country, y=large_pop_growth, fill=country))+
  geom_col()+coord_flip()+
  scale_fill_brewer(palette = "Blues")+
  theme_light()+
  labs(title = "5 Countries with Largest Population Growth Since 1952",
       x = "Country",
       y = "Population Growth")
```

![](lab11_hw_files/figure-html/unnamed-chunk-16-1.png)<!-- -->

**9. How does per-capita GDP growth compare between these same five countries?**

```r
gapminder %>% 
  filter(country=="China"| country=="India"|country=="United States"|country=="Indonesia"|country=="Brazil") %>%
  select(country, year, gdpPercap) %>% 
  ggplot(aes(x=year, y=gdpPercap, color=country, group=country))+
  geom_line()+
  geom_point(shape=8)+
  theme_stata()+
  scale_color_brewer(palette = "Set1")+
  labs(title="Per-Capita GDP per year For 5 Countries with Largest Population Growth",
       x = "Year",
       y = "GDP per Capita")
```

![](lab11_hw_files/figure-html/unnamed-chunk-17-1.png)<!-- -->

**10. Make one plot of your choice that uses faceting!**

```r
gapminder %>% 
  filter(country=="China"| country=="India"|country=="United States"|country=="Indonesia"|country=="Brazil") %>%
  ggplot(aes(x=lifeExp))+
  geom_density(alpha=0.5)+
  theme_tufte()+
  facet_wrap(~country)+
  labs(title = "Density of Life Expectancy Across the 5 Countries with Largest Pop. Growth",
       x = "Life Expectancy [Years]",
       y = "Density")
```

![](lab11_hw_files/figure-html/unnamed-chunk-18-1.png)<!-- -->

## Push your final code to GitHub!
Please be sure that you check the `keep md` file in the knit preferences. 
