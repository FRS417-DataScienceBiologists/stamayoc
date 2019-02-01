---
title: "Lab 2 Homework"
author: "Sara Tamayo-Correa"
date: "Winter 2019"
output:
  html_document:
    keep_md: yes
    theme: spacelab
  pdf_document:
    toc: no
---

## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to our [GitHub repository](https://github.com/FRS417-DataScienceBiologists). I will randomly select a few examples of student work at the start of each session to use as examples so be sure that your code is working to the best of your ability.

## Load the tidyverse

```r
library("tidyverse")
```

```
## ── Attaching packages ─────────────────────────────────────────────────────────────────── tidyverse 1.2.1 ──
```

```
## ✔ ggplot2 3.1.0     ✔ purrr   0.2.5
## ✔ tibble  2.0.0     ✔ dplyr   0.7.8
## ✔ tidyr   0.8.2     ✔ stringr 1.3.1
## ✔ readr   1.3.1     ✔ forcats 0.3.0
```

```
## ── Conflicts ────────────────────────────────────────────────────────────────────── tidyverse_conflicts() ──
## ✖ dplyr::filter() masks stats::filter()
## ✖ dplyr::lag()    masks stats::lag()
```

## Mammals Sleep
For this assignment, we are going to use built-in data on mammal sleep patterns.  

```r
msleep
```

```
## # A tibble: 83 x 11
##    name  genus vore  order conservation sleep_total sleep_rem sleep_cycle
##    <chr> <chr> <chr> <chr> <chr>              <dbl>     <dbl>       <dbl>
##  1 Chee… Acin… carni Carn… lc                  12.1      NA        NA    
##  2 Owl … Aotus omni  Prim… <NA>                17         1.8      NA    
##  3 Moun… Aplo… herbi Rode… nt                  14.4       2.4      NA    
##  4 Grea… Blar… omni  Sori… lc                  14.9       2.3       0.133
##  5 Cow   Bos   herbi Arti… domesticated         4         0.7       0.667
##  6 Thre… Brad… herbi Pilo… <NA>                14.4       2.2       0.767
##  7 Nort… Call… carni Carn… vu                   8.7       1.4       0.383
##  8 Vesp… Calo… <NA>  Rode… <NA>                 7        NA        NA    
##  9 Dog   Canis carni Carn… domesticated        10.1       2.9       0.333
## 10 Roe … Capr… herbi Arti… lc                   3        NA        NA    
## # … with 73 more rows, and 3 more variables: awake <dbl>, brainwt <dbl>,
## #   bodywt <dbl>
```

1. From which publication are these data taken from? Don't do an internet search; show the code that you would use to find out in R.
To find things on are use ? and type the function/object/data you are looking for

```r
?sleep
```



2. Provide some summary information about the data to get you started; feel free to use the functions that you find most helpful.

```r
colnames(msleep)
```

```
##  [1] "name"         "genus"        "vore"         "order"       
##  [5] "conservation" "sleep_total"  "sleep_rem"    "sleep_cycle" 
##  [9] "awake"        "brainwt"      "bodywt"
```


```r
summary(msleep)
```

```
##      name              genus               vore          
##  Length:83          Length:83          Length:83         
##  Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character  
##                                                          
##                                                          
##                                                          
##                                                          
##     order           conservation        sleep_total      sleep_rem    
##  Length:83          Length:83          Min.   : 1.90   Min.   :0.100  
##  Class :character   Class :character   1st Qu.: 7.85   1st Qu.:0.900  
##  Mode  :character   Mode  :character   Median :10.10   Median :1.500  
##                                        Mean   :10.43   Mean   :1.875  
##                                        3rd Qu.:13.75   3rd Qu.:2.400  
##                                        Max.   :19.90   Max.   :6.600  
##                                                        NA's   :22     
##   sleep_cycle         awake          brainwt            bodywt        
##  Min.   :0.1167   Min.   : 4.10   Min.   :0.00014   Min.   :   0.005  
##  1st Qu.:0.1833   1st Qu.:10.25   1st Qu.:0.00290   1st Qu.:   0.174  
##  Median :0.3333   Median :13.90   Median :0.01240   Median :   1.670  
##  Mean   :0.4396   Mean   :13.57   Mean   :0.28158   Mean   : 166.136  
##  3rd Qu.:0.5792   3rd Qu.:16.15   3rd Qu.:0.12550   3rd Qu.:  41.750  
##  Max.   :1.5000   Max.   :22.10   Max.   :5.71200   Max.   :6654.000  
##  NA's   :51                       NA's   :27
```


3. Make a new data frame focused on body weight, but be sure to indicate the common name and genus of each mammal. Sort the data in descending order by body weight.

```r
New_data <- select(msleep, genus, name, bodywt) %>% 
  arrange(desc(bodywt))
New_data
```

```
## # A tibble: 83 x 3
##    genus         name                 bodywt
##    <chr>         <chr>                 <dbl>
##  1 Loxodonta     African elephant      6654 
##  2 Elephas       Asian elephant        2547 
##  3 Giraffa       Giraffe                900.
##  4 Globicephalus Pilot whale            800 
##  5 Bos           Cow                    600 
##  6 Equus         Horse                  521 
##  7 Tapirus       Brazilian tapir        208.
##  8 Equus         Donkey                 187 
##  9 Tursiops      Bottle-nosed dolphin   173.
## 10 Panthera      Tiger                  163.
## # … with 73 more rows
```



4. We are interested in two groups; small and large mammals. Let's define small as less than or equal to 1kg body weight and large as greater than or equal to 200kg body weight. For our study, we are interested in body weight and sleep total Make two new dataframes (large and small) based on these parameters. Sort the data in descending order by body weight.

```r
Small_mammals <- select( msleep, genus, name, bodywt, sleep_total) %>% 
  filter(bodywt<=1) %>% 
  arrange(desc(bodywt))
Small_mammals
```

```
## # A tibble: 36 x 4
##    genus        name                      bodywt sleep_total
##    <chr>        <chr>                      <dbl>       <dbl>
##  1 Cricetomys   African giant pouched rat  1             8.3
##  2 Spermophilus Arctic ground squirrel     0.92         16.6
##  3 Tenrec       Tenrec                     0.9          15.6
##  4 Erinaceus    European hedgehog          0.77         10.1
##  5 Saimiri      Squirrel monkey            0.743         9.6
##  6 Cavis        Guinea pig                 0.728         9.4
##  7 Paraechinus  Desert hedgehog            0.55         10.3
##  8 Aotus        Owl monkey                 0.48         17  
##  9 Chinchilla   Chinchilla                 0.42         12.5
## 10 Lutreolina   Thick-tailed opposum       0.37         19.4
## # … with 26 more rows
```

```r
Large_mammals <- select(msleep, genus, name, bodywt, sleep_total) %>% 
  filter(bodywt>=200) %>% 
  arrange(desc(bodywt))
Large_mammals
```

```
## # A tibble: 7 x 4
##   genus         name             bodywt sleep_total
##   <chr>         <chr>             <dbl>       <dbl>
## 1 Loxodonta     African elephant  6654          3.3
## 2 Elephas       Asian elephant    2547          3.9
## 3 Giraffa       Giraffe            900.         1.9
## 4 Globicephalus Pilot whale        800          2.7
## 5 Bos           Cow                600          4  
## 6 Equus         Horse              521          2.9
## 7 Tapirus       Brazilian tapir    208.         4.4
```





5. Let's try to figure out if large mammals sleep, on average, longer than small mammals. What is the average sleep duration for large mammals as we have defined them?

```r
mean(Large_mammals$sleep_total)
```

```
## [1] 3.3
```



6. What is the average sleep duration for small mammals as we have defined them?

```r
mean(Small_mammals$sleep_total)
```

```
## [1] 12.65833
```



7. Which animals sleep at least 18 hours per day? Be sure to show the name, genus, order, and sleep total. Sort by order and sleep total.



```r
Animal_Sleep <- select(msleep, name, genus, order, sleep_total) %>% 
  filter(sleep_total>=18) %>% 
  arrange(order, sleep_total)  
Animal_Sleep
```

```
## # A tibble: 5 x 4
##   name                   genus      order           sleep_total
##   <chr>                  <chr>      <chr>                 <dbl>
## 1 Big brown bat          Eptesicus  Chiroptera             19.7
## 2 Little brown bat       Myotis     Chiroptera             19.9
## 3 Giant armadillo        Priodontes Cingulata              18.1
## 4 North American Opossum Didelphis  Didelphimorphia        18  
## 5 Thick-tailed opposum   Lutreolina Didelphimorphia        19.4
```



## Push your final code to [GitHub](https://github.com/FRS417-DataScienceBiologists)
