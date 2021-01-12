---
title: "Lab 2 Homework"
author: "Jessica Rico"
date: "2021-01-11"
output:
  html_document: 
    theme: spacelab
    keep_md: yes
---

## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your final lab report should be organized, clean, and run free from errors. Remember, you must remove the `#` for the included code chunks to run. Be sure to add your name to the author header above.  

Make sure to use the formatting conventions of RMarkdown to make your report neat and clean!  

1. What is a vector in R?  

A vector is group of data elements that are the same type, like a numeric vector or a character vector.  

2. What is a data matrix in R?  

A data matrix is a joining of multiple data vectors into one. We saw this in the harry potter matrix that we worked with in part 2 of lab 2. 

3. Below are data collected by three scientists (Jill, Steve, Susan in order) measuring temperatures of eight hot springs. Run this code chunk to create the vectors.  

```r
spring_1 <- c(36.25, 35.40, 35.30)
spring_2 <- c(35.15, 35.35, 33.35)
spring_3 <- c(30.70, 29.65, 29.20)
spring_4 <- c(39.70, 40.05, 38.65)
spring_5 <- c(31.85, 31.40, 29.30)
spring_6 <- c(30.20, 30.65, 29.75)
spring_7 <- c(32.90, 32.50, 32.80)
spring_8 <- c(36.80, 36.45, 33.15)
```

4. Build a data matrix that has the springs as rows and the columns as scientists.  


```r
Temperatures <- c(spring_1, spring_2, spring_3, spring_4, spring_5, spring_6, spring_7, spring_8)
Temperatures
```

```
##  [1] 36.25 35.40 35.30 35.15 35.35 33.35 30.70 29.65 29.20 39.70 40.05 38.65
## [13] 31.85 31.40 29.30 30.20 30.65 29.75 32.90 32.50 32.80 36.80 36.45 33.15
```

```r
spring_matrix <- matrix(Temperatures, nrow=8, byrow=T)
spring_matrix
```

```
##       [,1]  [,2]  [,3]
## [1,] 36.25 35.40 35.30
## [2,] 35.15 35.35 33.35
## [3,] 30.70 29.65 29.20
## [4,] 39.70 40.05 38.65
## [5,] 31.85 31.40 29.30
## [6,] 30.20 30.65 29.75
## [7,] 32.90 32.50 32.80
## [8,] 36.80 36.45 33.15
```


5. The names of the springs are 1.Bluebell Spring, 2.Opal Spring, 3.Riverside Spring, 4.Too Hot Spring, 5.Mystery Spring, 6.Emerald Spring, 7.Black Spring, 8.Pearl Spring. Name the rows and columns in the data matrix. Start by making two new vectors with the names, then use `colnames()` and `rownames()` to name the columns and rows.


```r
springs <- c("Bluebell_spring", "Opal_Spring", "Riverside_Spring", "Too_Hot_Spring", "Mystery_spring", "Emerald_Spring", "Black_Spring", "Pearl_Spring")
springs
```

```
## [1] "Bluebell_spring"  "Opal_Spring"      "Riverside_Spring" "Too_Hot_Spring"  
## [5] "Mystery_spring"   "Emerald_Spring"   "Black_Spring"     "Pearl_Spring"
```

```r
Scientists <- c("Jill","Steve", "Susan")
Scientists
```

```
## [1] "Jill"  "Steve" "Susan"
```

```r
colnames(spring_matrix) <- Scientists
rownames(spring_matrix) <- springs

spring_matrix
```

```
##                   Jill Steve Susan
## Bluebell_spring  36.25 35.40 35.30
## Opal_Spring      35.15 35.35 33.35
## Riverside_Spring 30.70 29.65 29.20
## Too_Hot_Spring   39.70 40.05 38.65
## Mystery_spring   31.85 31.40 29.30
## Emerald_Spring   30.20 30.65 29.75
## Black_Spring     32.90 32.50 32.80
## Pearl_Spring     36.80 36.45 33.15
```


6. Calculate the mean temperature of all three springs.


```r
bs <- mean(spring_matrix[1,])
os <- mean(spring_matrix[2,])
rs <- mean(spring_matrix[3,])
ths <- mean(spring_matrix[4,])
ms <- mean(spring_matrix[5,])
es <- mean(spring_matrix[6,])
bls <- mean(spring_matrix[7,])
ps <- mean(spring_matrix[8,])

Mean_temperatures <- c(bs, os, rs, ths, ms, es, bls, ps)
Mean_temperatures
```

```
## [1] 35.65000 34.61667 29.85000 39.46667 30.85000 30.20000 32.73333 35.46667
```


7. Add this as a new column in the data matrix.  

```r
spring_matrix <- cbind(spring_matrix, Mean_temperatures)
spring_matrix
```

```
##                   Jill Steve Susan Mean_temperatures
## Bluebell_spring  36.25 35.40 35.30          35.65000
## Opal_Spring      35.15 35.35 33.35          34.61667
## Riverside_Spring 30.70 29.65 29.20          29.85000
## Too_Hot_Spring   39.70 40.05 38.65          39.46667
## Mystery_spring   31.85 31.40 29.30          30.85000
## Emerald_Spring   30.20 30.65 29.75          30.20000
## Black_Spring     32.90 32.50 32.80          32.73333
## Pearl_Spring     36.80 36.45 33.15          35.46667
```


8. Show Susan's value for Opal Spring only.

```r
spring_matrix[2,3]
```

```
## [1] 33.35
```


9. Calculate the mean for Jill's column only.  

```r
jill_only <- spring_matrix[,1]
mean(jill_only)
```

```
## [1] 34.19375
```


10. Use the data matrix to perform one calculation or operation of your interest.

```r
total<- colSums(spring_matrix)
total
```

```
##              Jill             Steve             Susan Mean_temperatures 
##          273.5500          271.4500          261.5000          268.8333
```


## Push your final code to GitHub!
Please be sure that you check the `keep md` file in the knit preferences.  
