---
title: "Lab 3 Homework"
author: "Sara Tamayo-Correa"
date: "Winter 2019"
output:
  html_document:
    keep_md: yes
    theme: spacelab
---



## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code, keep track of your versions using git, and push your final work to our [GitHub repository](https://github.com/FRS417-DataScienceBiologists). I will randomly select a few examples of student work at the start of each session to use as examples so be sure that your code is working to the best of your ability.

## Load the libraries

```r
library(tidyverse)
```

For this assignment we are going to work with a large dataset from the [United Nations Food and Agriculture Organization](http://www.fao.org/about/en/) on world fisheries. The data are messy, so for this assignment I am going to provide some help. The code I use will likely be useful in the future so keep it handy. First, load the data. **Read** the error message.  


```r
fisheries <- readr::read_csv(file = "FAO_1950to2012_111914.csv") 
```

```
## Warning: Duplicated column names deduplicated: 'Species (ISSCAAP group)'
## => 'Species (ISSCAAP group)_1' [4], 'Species (ASFIS species)' => 'Species
## (ASFIS species)_1' [5], 'Species (ASFIS species)' => 'Species (ASFIS
## species)_2' [6]
```

```
## Parsed with column specification:
## cols(
##   .default = col_character(),
##   `Species (ISSCAAP group)` = col_double(),
##   `Fishing area (FAO major fishing area)` = col_double()
## )
```

```
## See spec(...) for full column specifications.
```


1. Do you see any potential problems with the column names? Does the error message now make more sense?  
*Column names are duplicated so R manipulated them to unique names*


2. The `make.names()` command is helpful when there are issues with column names. Notice that although the names are still cumbersome, much of the problemtatic syntax is removed.

```r
names(fisheries) = make.names(names(fisheries), unique=T)
names(fisheries)
```

```
##  [1] "Country..Country."                    
##  [2] "Species..ASFIS.species."              
##  [3] "Species..ISSCAAP.group."              
##  [4] "Species..ISSCAAP.group._1"            
##  [5] "Species..ASFIS.species._1"            
##  [6] "Species..ASFIS.species._2"            
##  [7] "Fishing.area..FAO.major.fishing.area."
##  [8] "Measure..Measure."                    
##  [9] "X1950"                                
## [10] "X1951"                                
## [11] "X1952"                                
## [12] "X1953"                                
## [13] "X1954"                                
## [14] "X1955"                                
## [15] "X1956"                                
## [16] "X1957"                                
## [17] "X1958"                                
## [18] "X1959"                                
## [19] "X1960"                                
## [20] "X1961"                                
## [21] "X1962"                                
## [22] "X1963"                                
## [23] "X1964"                                
## [24] "X1965"                                
## [25] "X1966"                                
## [26] "X1967"                                
## [27] "X1968"                                
## [28] "X1969"                                
## [29] "X1970"                                
## [30] "X1971"                                
## [31] "X1972"                                
## [32] "X1973"                                
## [33] "X1974"                                
## [34] "X1975"                                
## [35] "X1976"                                
## [36] "X1977"                                
## [37] "X1978"                                
## [38] "X1979"                                
## [39] "X1980"                                
## [40] "X1981"                                
## [41] "X1982"                                
## [42] "X1983"                                
## [43] "X1984"                                
## [44] "X1985"                                
## [45] "X1986"                                
## [46] "X1987"                                
## [47] "X1988"                                
## [48] "X1989"                                
## [49] "X1990"                                
## [50] "X1991"                                
## [51] "X1992"                                
## [52] "X1993"                                
## [53] "X1994"                                
## [54] "X1995"                                
## [55] "X1996"                                
## [56] "X1997"                                
## [57] "X1998"                                
## [58] "X1999"                                
## [59] "X2000"                                
## [60] "X2001"                                
## [61] "X2002"                                
## [62] "X2003"                                
## [63] "X2004"                                
## [64] "X2005"                                
## [65] "X2006"                                
## [66] "X2007"                                
## [67] "X2008"                                
## [68] "X2009"                                
## [69] "X2010"                                
## [70] "X2011"                                
## [71] "X2012"
```

3. Let's rename the columns. Use `rename()` to adjust the names as follows. Double check to make sure the rename worked correctly. Make sure to replace the old fisheries object with a new one so you can keep the column names.
+ country     = Country..Country.  
+ commname    = Species..ASFIS.species.  
+ sciname     = Species..ASFIS.species._2  
+ spcode      = Species..ASFIS.species._1  
+ spgroup     = Species..ISSCAAP.group.  
+ spgroupname = Species..ISSCAAP.group._1  
+ region      = Fishing.area..FAO.major.fishing.area.  
+ unit        = Measure..Measure.  


```r
fisheries <- 
fisheries %>% 
  rename(country= Country..Country., commname= Species..ASFIS.species., sciname= Species..ASFIS.species._2, spcode= Species..ASFIS.species._1, spgroup= Species..ISSCAAP.group. , spgroupname= Species..ISSCAAP.group._1, region= Fishing.area..FAO.major.fishing.area., unit= Measure..Measure.)
fisheries
```

```
## # A tibble: 17,692 x 71
##    country commname spgroup spgroupname spcode sciname region unit  X1950
##    <chr>   <chr>      <dbl> <chr>       <chr>  <chr>    <dbl> <chr> <chr>
##  1 Albania Angelsh…      38 Sharks, ra… 10903… Squati…     37 Quan… ...  
##  2 Albania Atlanti…      36 Tunas, bon… 17501… Sarda …     37 Quan… ...  
##  3 Albania Barracu…      37 Miscellane… 17710… Sphyra…     37 Quan… ...  
##  4 Albania Blue an…      45 Shrimps, p… 22802… Ariste…     37 Quan… ...  
##  5 Albania Blue wh…      32 Cods, hake… 14804… Microm…     37 Quan… ...  
##  6 Albania Bluefish      37 Miscellane… 17020… Pomato…     37 Quan… ...  
##  7 Albania Bogue         33 Miscellane… 17039… Boops …     37 Quan… ...  
##  8 Albania Caramot…      45 Shrimps, p… 22801… Penaeu…     37 Quan… ...  
##  9 Albania Catshar…      38 Sharks, ra… 10801… Scylio…     37 Quan… ...  
## 10 Albania Common …      57 Squids, cu… 32102… Sepia …     37 Quan… ...  
## # … with 17,682 more rows, and 62 more variables: X1951 <chr>,
## #   X1952 <chr>, X1953 <chr>, X1954 <chr>, X1955 <chr>, X1956 <chr>,
## #   X1957 <chr>, X1958 <chr>, X1959 <chr>, X1960 <chr>, X1961 <chr>,
## #   X1962 <chr>, X1963 <chr>, X1964 <chr>, X1965 <chr>, X1966 <chr>,
## #   X1967 <chr>, X1968 <chr>, X1969 <chr>, X1970 <chr>, X1971 <chr>,
## #   X1972 <chr>, X1973 <chr>, X1974 <chr>, X1975 <chr>, X1976 <chr>,
## #   X1977 <chr>, X1978 <chr>, X1979 <chr>, X1980 <chr>, X1981 <chr>,
## #   X1982 <chr>, X1983 <chr>, X1984 <chr>, X1985 <chr>, X1986 <chr>,
## #   X1987 <chr>, X1988 <chr>, X1989 <chr>, X1990 <chr>, X1991 <chr>,
## #   X1992 <chr>, X1993 <chr>, X1994 <chr>, X1995 <chr>, X1996 <chr>,
## #   X1997 <chr>, X1998 <chr>, X1999 <chr>, X2000 <chr>, X2001 <chr>,
## #   X2002 <chr>, X2003 <chr>, X2004 <chr>, X2005 <chr>, X2006 <chr>,
## #   X2007 <chr>, X2008 <chr>, X2009 <chr>, X2010 <chr>, X2011 <chr>,
## #   X2012 <chr>
```


4. Are these data tidy? Why or why not, and, how do you know?
These data is not tidy yet because the years are separate vectors that have no data in them. They should be one vector, and the years are the data.

5. We need to tidy the data using `gather()`. The code below will not run because it is commented (#) out. I have added a bit of code that will prevent you from needing to type in each year from 1950-2012 but you need to complete the remainder `QQQ` and remove the `#`.

```r
fisheries_tidy <- 
  fisheries %>% 
 gather(num_range('X',1950:2012), key= "year", value="catch")
fisheries
```

```
## # A tibble: 17,692 x 71
##    country commname spgroup spgroupname spcode sciname region unit  X1950
##    <chr>   <chr>      <dbl> <chr>       <chr>  <chr>    <dbl> <chr> <chr>
##  1 Albania Angelsh…      38 Sharks, ra… 10903… Squati…     37 Quan… ...  
##  2 Albania Atlanti…      36 Tunas, bon… 17501… Sarda …     37 Quan… ...  
##  3 Albania Barracu…      37 Miscellane… 17710… Sphyra…     37 Quan… ...  
##  4 Albania Blue an…      45 Shrimps, p… 22802… Ariste…     37 Quan… ...  
##  5 Albania Blue wh…      32 Cods, hake… 14804… Microm…     37 Quan… ...  
##  6 Albania Bluefish      37 Miscellane… 17020… Pomato…     37 Quan… ...  
##  7 Albania Bogue         33 Miscellane… 17039… Boops …     37 Quan… ...  
##  8 Albania Caramot…      45 Shrimps, p… 22801… Penaeu…     37 Quan… ...  
##  9 Albania Catshar…      38 Sharks, ra… 10801… Scylio…     37 Quan… ...  
## 10 Albania Common …      57 Squids, cu… 32102… Sepia …     37 Quan… ...  
## # … with 17,682 more rows, and 62 more variables: X1951 <chr>,
## #   X1952 <chr>, X1953 <chr>, X1954 <chr>, X1955 <chr>, X1956 <chr>,
## #   X1957 <chr>, X1958 <chr>, X1959 <chr>, X1960 <chr>, X1961 <chr>,
## #   X1962 <chr>, X1963 <chr>, X1964 <chr>, X1965 <chr>, X1966 <chr>,
## #   X1967 <chr>, X1968 <chr>, X1969 <chr>, X1970 <chr>, X1971 <chr>,
## #   X1972 <chr>, X1973 <chr>, X1974 <chr>, X1975 <chr>, X1976 <chr>,
## #   X1977 <chr>, X1978 <chr>, X1979 <chr>, X1980 <chr>, X1981 <chr>,
## #   X1982 <chr>, X1983 <chr>, X1984 <chr>, X1985 <chr>, X1986 <chr>,
## #   X1987 <chr>, X1988 <chr>, X1989 <chr>, X1990 <chr>, X1991 <chr>,
## #   X1992 <chr>, X1993 <chr>, X1994 <chr>, X1995 <chr>, X1996 <chr>,
## #   X1997 <chr>, X1998 <chr>, X1999 <chr>, X2000 <chr>, X2001 <chr>,
## #   X2002 <chr>, X2003 <chr>, X2004 <chr>, X2005 <chr>, X2006 <chr>,
## #   X2007 <chr>, X2008 <chr>, X2009 <chr>, X2010 <chr>, X2011 <chr>,
## #   X2012 <chr>
```

6. Use `glimpse()` to look at the categories of the variables. Pay particular attention to `year` and `catch`. What do you notice?  
-some of the data points are missing

```r
glimpse(fisheries)
```

```
## Observations: 17,692
## Variables: 71
## $ country     <chr> "Albania", "Albania", "Albania", "Albania", "Albania…
## $ commname    <chr> "Angelsharks, sand devils nei", "Atlantic bonito", "…
## $ spgroup     <dbl> 38, 36, 37, 45, 32, 37, 33, 45, 38, 57, 33, 57, 31, …
## $ spgroupname <chr> "Sharks, rays, chimaeras", "Tunas, bonitos, billfish…
## $ spcode      <chr> "10903XXXXX", "1750100101", "17710001XX", "228020310…
## $ sciname     <chr> "Squatinidae", "Sarda sarda", "Sphyraena spp", "Aris…
## $ region      <dbl> 37, 37, 37, 37, 37, 37, 37, 37, 37, 37, 37, 37, 37, …
## $ unit        <chr> "Quantity (tonnes)", "Quantity (tonnes)", "Quantity …
## $ X1950       <chr> "...", "...", "...", "...", "...", "...", "...", "..…
## $ X1951       <chr> "...", "...", "...", "...", "...", "...", "...", "..…
## $ X1952       <chr> "...", "...", "...", "...", "...", "...", "...", "..…
## $ X1953       <chr> "...", "...", "...", "...", "...", "...", "...", "..…
## $ X1954       <chr> "...", "...", "...", "...", "...", "...", "...", "..…
## $ X1955       <chr> "...", "...", "...", "...", "...", "...", "...", "..…
## $ X1956       <chr> "...", "...", "...", "...", "...", "...", "...", "..…
## $ X1957       <chr> "...", "...", "...", "...", "...", "...", "...", "..…
## $ X1958       <chr> "...", "...", "...", "...", "...", "...", "...", "..…
## $ X1959       <chr> "...", "...", "...", "...", "...", "...", "...", "..…
## $ X1960       <chr> "...", "...", "...", "...", "...", "...", "...", "..…
## $ X1961       <chr> "...", "...", "...", "...", "...", "...", "...", "..…
## $ X1962       <chr> "...", "...", "...", "...", "...", "...", "...", "..…
## $ X1963       <chr> "...", "...", "...", "...", "...", "...", "...", "..…
## $ X1964       <chr> "...", "...", "...", "...", "...", "...", "...", "..…
## $ X1965       <chr> "...", "...", "...", "...", "...", "...", "...", "..…
## $ X1966       <chr> "...", "...", "...", "...", "...", "...", "...", "..…
## $ X1967       <chr> "...", "...", "...", "...", "...", "...", "...", "..…
## $ X1968       <chr> "...", "...", "...", "...", "...", "...", "...", "..…
## $ X1969       <chr> "...", "...", "...", "...", "...", "...", "...", "..…
## $ X1970       <chr> "...", "...", "...", "...", "...", "...", "-", "..."…
## $ X1971       <chr> "...", "...", "...", "...", "...", "...", "-", "..."…
## $ X1972       <chr> "...", "...", "...", "...", "...", "...", "-", "..."…
## $ X1973       <chr> "...", "...", "...", "...", "...", "...", "-", "..."…
## $ X1974       <chr> "...", "...", "...", "...", "...", "...", "-", "..."…
## $ X1975       <chr> "...", "...", "...", "...", "...", "...", "-", "..."…
## $ X1976       <chr> "...", "...", "...", "...", "...", "...", "-", "..."…
## $ X1977       <chr> "...", "...", "...", "...", "...", "...", "-", "..."…
## $ X1978       <chr> "...", "...", "...", "...", "...", "...", "-", "..."…
## $ X1979       <chr> "...", "...", "...", "...", "...", "...", "-", "..."…
## $ X1980       <chr> "...", "...", "...", "...", "...", "...", "-", "..."…
## $ X1981       <chr> "...", "...", "...", "...", "...", "...", "...", "..…
## $ X1982       <chr> "...", "...", "...", "...", "...", "...", "...", "..…
## $ X1983       <chr> "...", "...", "...", "...", "...", "...", "559", "..…
## $ X1984       <chr> "...", "...", "...", "...", "...", "...", "392", "..…
## $ X1985       <chr> "...", "...", "...", "...", "...", "...", "406", "..…
## $ X1986       <chr> "...", "...", "...", "...", "...", "...", "499", "..…
## $ X1987       <chr> "...", "...", "...", "...", "...", "...", "564", "..…
## $ X1988       <chr> "...", "...", "...", "...", "...", "...", "724", "..…
## $ X1989       <chr> "...", "...", "...", "...", "...", "...", "583", "..…
## $ X1990       <chr> "...", "...", "...", "...", "...", "...", "754", "..…
## $ X1991       <chr> "...", "...", "...", "...", "...", "...", "283", "..…
## $ X1992       <chr> "...", "...", "...", "...", "...", "...", "196", "..…
## $ X1993       <chr> "...", "...", "...", "...", "...", "...", "150 F", "…
## $ X1994       <chr> "...", "...", "...", "...", "...", "...", "100 F", "…
## $ X1995       <chr> "0 0", "1", "...", "0 0", "0 0", "...", "52", "30", …
## $ X1996       <chr> "53", "2", "...", "3", "2", "...", "104", "8", "..."…
## $ X1997       <chr> "20", "0 0", "...", "0 0", "0 0", "...", "65", "4", …
## $ X1998       <chr> "31", "12", "...", "-", "-", "...", "220", "18", "..…
## $ X1999       <chr> "30", "30", "...", "-", "-", "...", "220", "18", "..…
## $ X2000       <chr> "30", "25", "2", "-", "-", "...", "220", "20", "..."…
## $ X2001       <chr> "16", "30", "...", "-", "-", "...", "120", "23", "..…
## $ X2002       <chr> "79", "24", "...", "34", "6", "...", "150", "84", ".…
## $ X2003       <chr> "1", "4", "...", "22", "...", "...", "84", "178", ".…
## $ X2004       <chr> "4", "2", "2", "15", "1", "2", "76", "285", "1", "70…
## $ X2005       <chr> "68", "23", "4", "12", "5", "6", "68", "150", "2", "…
## $ X2006       <chr> "55", "30", "7", "18", "8", "9", "86", "102", "4", "…
## $ X2007       <chr> "12", "19", "...", "...", "-", "-", "132", "18", "..…
## $ X2008       <chr> "23", "27", "...", "...", "-", "-", "132", "23", "..…
## $ X2009       <chr> "14", "21", "...", "...", "-", "-", "154", "20", "..…
## $ X2010       <chr> "78", "23", "7", "...", "-", "-", "80", "228", "..."…
## $ X2011       <chr> "12", "12", "...", "...", "-", "-", "88", "9", "..."…
## $ X2012       <chr> "5", "5", "...", "...", "-", "-", "129", "290", "...…
```




7. From question 6 you should see that there are a lot of entries that are missing. In R, these are referred to as NA's but they can be coded in different ways by the scientists in a given study. In order to make the data tidy, we need to deal with them. As a preview to our next lab, run the following code by removing the `#`. It removes the 'X' from the years and changes the `catch` column from a character into a numeric. This forces the blank entries to become NAs. The error "NAs introduced by coercion" indicates their replacement.

```r
fisheries_tidy <- 
  fisheries_tidy %>% 
  mutate(
    year= as.numeric(str_replace(year, 'X', '')), 
    catch= as.numeric(str_replace(catch, c(' F','...','-'), replacement = '')))
```

```
## Warning in evalq(as.numeric(str_replace(catch, c(" F", "...", "-"),
## replacement = "")), : NAs introduced by coercion
```


```r
fisheries_tidy
```

```
## # A tibble: 1,114,596 x 10
##    country commname spgroup spgroupname spcode sciname region unit   year
##    <chr>   <chr>      <dbl> <chr>       <chr>  <chr>    <dbl> <chr> <dbl>
##  1 Albania Angelsh…      38 Sharks, ra… 10903… Squati…     37 Quan…  1950
##  2 Albania Atlanti…      36 Tunas, bon… 17501… Sarda …     37 Quan…  1950
##  3 Albania Barracu…      37 Miscellane… 17710… Sphyra…     37 Quan…  1950
##  4 Albania Blue an…      45 Shrimps, p… 22802… Ariste…     37 Quan…  1950
##  5 Albania Blue wh…      32 Cods, hake… 14804… Microm…     37 Quan…  1950
##  6 Albania Bluefish      37 Miscellane… 17020… Pomato…     37 Quan…  1950
##  7 Albania Bogue         33 Miscellane… 17039… Boops …     37 Quan…  1950
##  8 Albania Caramot…      45 Shrimps, p… 22801… Penaeu…     37 Quan…  1950
##  9 Albania Catshar…      38 Sharks, ra… 10801… Scylio…     37 Quan…  1950
## 10 Albania Common …      57 Squids, cu… 32102… Sepia …     37 Quan…  1950
## # … with 1,114,586 more rows, and 1 more variable: catch <dbl>
```


8. Are the data tidy? Why?  
The data is tidy because now all of the years are in the same collumn and there's no longer missing data


9. You are a fisheries scientist studying cephalopod catch during 2008-2012. Identify the top five consumers (by country) of cephalopods (don't worry about species for now). Restrict the data frame only to our variables of interest.
##Cephalopods are squids, octopuses and cuttlefishes

```r
fisheries_tidy %>% 
  select(country, spgroupname,commname, year, catch) %>%
  filter(year== 2008:2012, spgroupname== "Squids, cuttlefishes, octopuses") %>% 
  arrange(desc(catch))
```

```
## Warning in year == 2008:2012: longer object length is not a multiple of
## shorter object length
```

```
## # A tibble: 745 x 5
##    country          spgroupname             commname            year  catch
##    <chr>            <chr>                   <chr>              <dbl>  <dbl>
##  1 Peru             Squids, cuttlefishes, … Jumbo flying squid  2011 404730
##  2 Japan            Squids, cuttlefishes, … Japanese flying s…  2009 218658
##  3 Chile            Squids, cuttlefishes, … Jumbo flying squid  2011 163495
##  4 China            Squids, cuttlefishes, … Jumbo flying squid  2010 142000
##  5 India            Squids, cuttlefishes, … Cephalopods nei     2012  82456
##  6 Russian Federat… Squids, cuttlefishes, … Schoolmaster gona…  2011  67678
##  7 Falkland Is.(Ma… Squids, cuttlefishes, … Patagonian squid    2012  64241
##  8 China            Squids, cuttlefishes, … Argentine shortfi…  2009  61400
##  9 Indonesia        Squids, cuttlefishes, … Common squids nei   2008  49565
## 10 Mexico           Squids, cuttlefishes, … Jumbo flying squid  2010  42893
## # … with 735 more rows
```



10. Let's be more specific. Who consumes the most `Common cuttlefish`? Store this as a new object `cuttle`.
##does not work when you put spgroupname, thus need to put scientific name in order to distinguish the entries

```r
cuttle <- fisheries_tidy %>% 
  select(country, commname, sciname, year, catch) %>%
  filter(year==2008:2012, commname=="Common cuttlefish") %>% 
  arrange(desc(catch))
```

```
## Warning in year == 2008:2012: longer object length is not a multiple of
## shorter object length
```

```r
cuttle
```

```
## # A tibble: 21 x 5
##    country  commname          sciname            year catch
##    <chr>    <chr>             <chr>             <dbl> <dbl>
##  1 France   Common cuttlefish Sepia officinalis  2009  8076
##  2 Tunisia  Common cuttlefish Sepia officinalis  2009  3924
##  3 Spain    Common cuttlefish Sepia officinalis  2012  1685
##  4 Greece   Common cuttlefish Sepia officinalis  2009  1493
##  5 Algeria  Common cuttlefish Sepia officinalis  2010   291
##  6 Croatia  Common cuttlefish Sepia officinalis  2011   105
##  7 Portugal Common cuttlefish Sepia officinalis  2008    74
##  8 France   Common cuttlefish Sepia officinalis  2008    67
##  9 Albania  Common cuttlefish Sepia officinalis  2008    62
## 10 Greece   Common cuttlefish Sepia officinalis  2010    57
## # … with 11 more rows
```



## Push your final code to [GitHub](https://github.com/FRS417-DataScienceBiologists)
Make sure that you push your code into the appropriate folder. Also, be sure that you have check the `keep md` file in the knit preferences.
