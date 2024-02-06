---
title: "Midterm 1 W24"
author: "Anastasia Karp"
date: "2024-02-06"
output:
  html_document: 
    keep_md: yes
  pdf_document: default
---

## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your code must be organized, clean, and run free from errors. Remember, you must remove the `#` for any included code chunks to run. Be sure to add your name to the author header above. 

Your code must knit in order to be considered. If you are stuck and cannot answer a question, then comment out your code and knit the document. You may use your notes, labs, and homework to help you complete this exam. Do not use any other resources- including AI assistance.  

Don't forget to answer any questions that are asked in the prompt!  

Be sure to push your completed midterm to your repository. This exam is worth 30 points.  

## Background
In the data folder, you will find data related to a study on wolf mortality collected by the National Park Service. You should start by reading the `README_NPSwolfdata.pdf` file. This will provide an abstract of the study and an explanation of variables.  

The data are from: Cassidy, Kira et al. (2022). Gray wolf packs and human-caused wolf mortality. [Dryad](https://doi.org/10.5061/dryad.mkkwh713f). 

## Load the libraries

```r
library("tidyverse")
library("janitor")
```

## Load the wolves data
In these data, the authors used `NULL` to represent missing values. I am correcting this for you below and using `janitor` to clean the column names.

```r
wolves <- read.csv("data/NPS_wolfmortalitydata.csv", na = c("NULL")) %>% clean_names()
```

## Questions
Problem 1. (1 point) Let's start with some data exploration. What are the variable (column) names?  

```r
colnames(wolves)
```

```
##  [1] "park"         "biolyr"       "pack"         "packcode"     "packsize_aug"
##  [6] "mort_yn"      "mort_all"     "mort_lead"    "mort_nonlead" "reprody1"    
## [11] "persisty1"
```


Problem 2. (1 point) Use the function of your choice to summarize the data and get an idea of its structure.

```r
glimpse(wolves)
```

```
## Rows: 864
## Columns: 11
## $ park         <chr> "DENA", "DENA", "DENA", "DENA", "DENA", "DENA", "DENA", "…
## $ biolyr       <int> 1996, 1991, 2017, 1996, 1992, 1994, 2007, 2007, 1995, 200…
## $ pack         <chr> "McKinley River1", "Birch Creek N", "Eagle Gorge", "East …
## $ packcode     <int> 89, 58, 71, 72, 74, 77, 101, 108, 109, 53, 63, 66, 70, 72…
## $ packsize_aug <dbl> 12, 5, 8, 13, 7, 6, 10, NA, 9, 8, 7, 11, 0, 19, 15, 12, 1…
## $ mort_yn      <int> 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, …
## $ mort_all     <int> 4, 2, 2, 2, 2, 2, 2, 2, 2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, …
## $ mort_lead    <int> 2, 2, 0, 0, 0, 0, 1, 2, 1, 1, 1, 0, 0, 0, 1, 1, 1, 0, 0, …
## $ mort_nonlead <int> 2, 0, 2, 2, 2, 2, 1, 0, 1, 0, 0, 1, 1, 1, 0, 0, 0, 1, 1, …
## $ reprody1     <int> 0, 0, NA, 1, NA, 0, 0, 1, 0, 1, 0, 1, 0, 1, 1, 1, 1, 1, 1…
## $ persisty1    <int> 0, 0, 1, 1, 1, 1, 0, 1, 0, 1, 0, 1, 0, 1, 1, 1, 1, 1, 1, …
```


```r
summary(wolves)
```

```
##      park               biolyr         pack              packcode     
##  Length:864         Min.   :1986   Length:864         Min.   :  2.00  
##  Class :character   1st Qu.:1999   Class :character   1st Qu.: 48.00  
##  Mode  :character   Median :2006   Mode  :character   Median : 86.50  
##                     Mean   :2005                      Mean   : 91.39  
##                     3rd Qu.:2012                      3rd Qu.:133.00  
##                     Max.   :2021                      Max.   :193.00  
##                                                                       
##   packsize_aug       mort_yn          mort_all         mort_lead      
##  Min.   : 0.000   Min.   :0.0000   Min.   : 0.0000   Min.   :0.00000  
##  1st Qu.: 5.000   1st Qu.:0.0000   1st Qu.: 0.0000   1st Qu.:0.00000  
##  Median : 8.000   Median :0.0000   Median : 0.0000   Median :0.00000  
##  Mean   : 8.789   Mean   :0.1956   Mean   : 0.3715   Mean   :0.09552  
##  3rd Qu.:12.000   3rd Qu.:0.0000   3rd Qu.: 0.0000   3rd Qu.:0.00000  
##  Max.   :37.000   Max.   :1.0000   Max.   :24.0000   Max.   :3.00000  
##  NA's   :55                                          NA's   :16       
##   mort_nonlead        reprody1        persisty1     
##  Min.   : 0.0000   Min.   :0.0000   Min.   :0.0000  
##  1st Qu.: 0.0000   1st Qu.:1.0000   1st Qu.:1.0000  
##  Median : 0.0000   Median :1.0000   Median :1.0000  
##  Mean   : 0.2641   Mean   :0.7629   Mean   :0.8865  
##  3rd Qu.: 0.0000   3rd Qu.:1.0000   3rd Qu.:1.0000  
##  Max.   :22.0000   Max.   :1.0000   Max.   :1.0000  
##  NA's   :12        NA's   :71       NA's   :9
```



Problem 3. (3 points) Which parks/ reserves are represented in the data? Don't just use the abstract, pull this information from the data.  


```r
wolves
```

```
##     park biolyr                          pack packcode packsize_aug mort_yn
## 1   DENA   1996               McKinley River1       89         12.0       1
## 2   DENA   1991                 Birch Creek N       58          5.0       1
## 3   DENA   2017                   Eagle Gorge       71          8.0       1
## 4   DENA   1996                     East Fork       72         13.0       1
## 5   DENA   1992                       Foraker       74          7.0       1
## 6   DENA   1994                  Headquarters       77          6.0       1
## 7   DENA   2007                         Pinto      101         10.0       1
## 8   DENA   2007                        Savage      108           NA       1
## 9   DENA   1995                       Savage1      109          9.0       1
## 10  DENA   2003                      100 Mile       53          8.0       1
## 11  DENA   2003                 Castle Rocks2       63          7.0       1
## 12  DENA   2007                       Chitsia       66         11.0       1
## 13  DENA   2003                  Death Valley       70          0.0       1
## 14  DENA   1988                     East Fork       72         19.0       1
## 15  DENA   1992                     East Fork       72         15.0       1
## 16  DENA   2004                     East Fork       72         12.0       1
## 17  DENA   2005                     East Fork       72         14.0       1
## 18  DENA   2006                     East Fork       72         15.0       1
## 19  DENA   2010                     East Fork       72         11.0       1
## 20  DENA   2014                     East Fork       72         17.0       1
## 21  DENA   2015                     East Fork       72          4.0       1
## 22  DENA   2016                     East Fork       72          3.0       1
## 23  DENA   1988                     Ewe Creek       73          5.0       1
## 24  DENA   2007                   Grant Creek       75          5.0       1
## 25  DENA   2012                   Grant Creek       75          5.0       1
## 26  DENA   2013                   Grant Creek       75          7.0       1
## 27  DENA   2008                    Hot Slough       80          8.0       1
## 28  DENA   2017               Iron Creek West       83          2.0       1
## 29  DENA   2014                   John Hansen       85          7.0       1
## 30  DENA   2006               Kantishna River       86          1.0       1
## 31  DENA   1992               McKinley River1       89          7.0       1
## 32  DENA   2007                       McLeod2       93          6.0       1
## 33  DENA   2003                   Mt Margaret       95         12.0       1
## 34  DENA   2008                   Mt Margaret       95          2.0       1
## 35  DENA   2009                   Mt Margaret       95          5.0       1
## 36  DENA   2003                   Muddy River       96          3.0       1
## 37  DENA   2016                        Myrtle       97          5.0       1
## 38  DENA   2013                  Nenana River       98          9.0       1
## 39  DENA   1997                   Pinto Creek      102           NA       1
## 40  DENA   1999                   Pinto Creek      102         14.0       1
## 41  DENA   2017                   Riley Creek      104         17.0       1
## 42  DENA   1994                       Savage1      109          9.0       1
## 43  DENA   2007                        Somber      111         11.0       1
## 44  DENA   1990                      Stampede      112          3.0       1
## 45  DENA   1991                      Stampede      112          2.0       1
## 46  DENA   2006                    Starr Lake      113          3.0       1
## 47  DENA   2003                  Straightaway      115         11.0       1
## 48  DENA   2009                       Tonzona      116          1.0       1
## 49  DENA   2000                     East Fork       72         10.0       1
## 50  DENA   2007                   Hauke Creek       76          4.0       1
## 51  DENA   1993                  Headquarters       77          9.0       1
## 52  DENA   1994                   Little Bear       87          8.0       1
## 53  DENA   2000                     Sanctuary      106          9.0       1
## 54  DENA   1997                      100 Mile       53          7.0       0
## 55  DENA   1998                      100 Mile       53         11.0       0
## 56  DENA   1999                      100 Mile       53         12.0       0
## 57  DENA   2000                      100 Mile       53         23.0       0
## 58  DENA   2001                      100 Mile       53          8.0       0
## 59  DENA   2002                      100 Mile       53         14.0       0
## 60  DENA   2004                      100 Mile       53          3.0       0
## 61  DENA   2004                       Bearpaw       54          5.0       0
## 62  DENA   2005                       Bearpaw       54          6.0       0
## 63  DENA   2006                       Bearpaw       54         10.0       0
## 64  DENA   2007                       Bearpaw       54         10.0       0
## 65  DENA   2008                       Bearpaw       54          5.0       0
## 66  DENA   2009                       Bearpaw       54          2.0       0
## 67  DENA   2010                       Bearpaw       54          6.0       0
## 68  DENA   2011                       Bearpaw       54          6.0       0
## 69  DENA   2012                       Bearpaw       54          8.0       0
## 70  DENA   2013                       Bearpaw       54          1.0       0
## 71  DENA   2014                       Bearpaw       54          2.0       0
## 72  DENA   2015                       Bearpaw       54          8.0       0
## 73  DENA   2016                       Bearpaw       54         16.0       0
## 74  DENA   2017                       Bearpaw       54         17.0       0
## 75  DENA   1987                      Bearpaw1       55         10.0       0
## 76  DENA   1996                   Beaver Fork       56          9.0       0
## 77  DENA   1997                   Beaver Fork       56          6.0       0
## 78  DENA   1998                   Beaver Fork       56          3.0       0
## 79  DENA   1999                   Beaver Fork       56          3.0       0
## 80  DENA   1987                   Birch Creek       57         11.0       0
## 81  DENA   1988                   Birch Creek       57         23.0       0
## 82  DENA   1989                   Birch Creek       57         15.0       0
## 83  DENA   1999                   Birch Hills       59          4.0       0
## 84  DENA   2000                   Birch Hills       59          0.0       0
## 85  DENA   2008                     Boot Lake       60          2.0       0
## 86  DENA   2009                     Boot Lake       60          3.0       0
## 87  DENA   2001                       Brooker       61          3.0       0
## 88  DENA   2002                       Brooker       61          2.0       0
## 89  DENA   1997                 Caribou Creek       62          4.0       0
## 90  DENA   2004                 Castle Rocks2       63          1.0       0
## 91  DENA   2006                 Castle Rocks3       64          6.0       0
## 92  DENA   2007                 Castle Rocks3       64          7.0       0
## 93  DENA   2008                 Castle Rocks3       64          2.0       0
## 94  DENA   1991                 Chilchukabena       65          6.0       0
## 95  DENA   2004                       Chitsia       66          4.0       0
## 96  DENA   2005                       Chitsia       66          8.0       0
## 97  DENA   2006                       Chitsia       66         11.0       0
## 98  DENA   2008                       Chitsia       66          7.0       0
## 99  DENA   1989              Chitsia Mountain       67          4.0       0
## 100 DENA   1990              Chitsia Mountain       67          8.0       0
## 101 DENA   1991              Chitsia Mountain       67         12.0       0
## 102 DENA   1992              Chitsia Mountain       67          9.0       0
## 103 DENA   1993              Chitsia Mountain       67          8.0       0
## 104 DENA   1994              Chitsia Mountain       67          4.0       0
## 105 DENA   1986                    Clearwater       68          6.0       0
## 106 DENA   1987                    Clearwater       68          6.0       0
## 107 DENA   1988                    Clearwater       68          4.0       0
## 108 DENA   1989                    Clearwater       68          8.0       0
## 109 DENA   1995                   Corner Lake       69          4.0       0
## 110 DENA   1996                   Corner Lake       69          7.0       0
## 111 DENA   1997                   Corner Lake       69          4.0       0
## 112 DENA   1998                   Corner Lake       69          1.0       0
## 113 DENA   2000                  Death Valley       70          2.0       0
## 114 DENA   2001                  Death Valley       70          3.0       0
## 115 DENA   2002                  Death Valley       70          2.0       0
## 116 DENA   2016                   Eagle Gorge       71          5.0       0
## 117 DENA   1986                     East Fork       72          9.0       0
## 118 DENA   1987                     East Fork       72          8.0       0
## 119 DENA   1989                     East Fork       72         27.0       0
## 120 DENA   1990                     East Fork       72         33.0       0
## 121 DENA   1991                     East Fork       72         16.0       0
## 122 DENA   1993                     East Fork       72          9.0       0
## 123 DENA   1994                     East Fork       72          9.0       0
## 124 DENA   1995                     East Fork       72         14.0       0
## 125 DENA   1997                     East Fork       72          7.0       0
## 126 DENA   1998                     East Fork       72          7.0       0
## 127 DENA   1999                     East Fork       72          9.0       0
## 128 DENA   2001                     East Fork       72         10.0       0
## 129 DENA   2002                     East Fork       72          4.0       0
## 130 DENA   2003                     East Fork       72          8.0       0
## 131 DENA   2007                     East Fork       72         15.0       0
## 132 DENA   2008                     East Fork       72         16.0       0
## 133 DENA   2009                     East Fork       72         12.0       0
## 134 DENA   2011                     East Fork       72         10.0       0
## 135 DENA   2012                     East Fork       72         13.0       0
## 136 DENA   2013                     East Fork       72         20.0       0
## 137 DENA   1987                     Ewe Creek       73          8.0       0
## 138 DENA   1989                       Foraker        4          7.0       0
## 139 DENA   1990                       Foraker       74          9.0       0
## 140 DENA   1991                       Foraker       74          8.0       0
## 141 DENA   1993                       Foraker       74           NA       0
## 142 DENA   1994                       Foraker       74          6.0       0
## 143 DENA   1995                       Foraker       74         12.0       0
## 144 DENA   1996                       Foraker       74           NA       0
## 145 DENA   1997                       Foraker       74          8.0       0
## 146 DENA   1998                       Foraker       74          8.0       0
## 147 DENA   1999                       Foraker       74          8.0       0
## 148 DENA   2000                       Foraker       74          9.0       0
## 149 DENA   2001                       Foraker       74          1.0       0
## 150 DENA   2002                   Grant Creek       75          2.0       0
## 151 DENA   2003                   Grant Creek       75          2.0       0
## 152 DENA   2004                   Grant Creek       75          5.0       0
## 153 DENA   2005                   Grant Creek       75          6.0       0
## 154 DENA   2006                   Grant Creek       75         13.0       0
## 155 DENA   2008                   Grant Creek       75          6.0       0
## 156 DENA   2009                   Grant Creek       75         14.0       0
## 157 DENA   2010                   Grant Creek       75         16.0       0
## 158 DENA   2011                   Grant Creek       75         13.0       0
## 159 DENA   2014                   Grant Creek       75          4.0       0
## 160 DENA   2015                   Grant Creek       75          6.0       0
## 161 DENA   2016                   Grant Creek       75         12.0       0
## 162 DENA   2017                   Grant Creek       75          5.0       0
## 163 DENA   2008                   Hauke Creek       76          0.0       0
## 164 DENA   1986                  Headquarters       77          2.0       0
## 165 DENA   1987                  Headquarters       77          2.0       0
## 166 DENA   1988                  Headquarters       77          7.0       0
## 167 DENA   1989                  Headquarters       77         14.0       0
## 168 DENA   1990                  Headquarters       77         11.0       0
## 169 DENA   1991                  Headquarters       77         10.0       0
## 170 DENA   1992                  Headquarters       77          5.0       0
## 171 DENA   2002                        Herron       78         10.0       0
## 172 DENA   2003                        Herron       78          7.0       0
## 173 DENA   2004                        Herron       78          8.0       0
## 174 DENA   1988                     Highpower       79          8.0       0
## 175 DENA   1989                     Highpower       79         10.0       0
## 176 DENA   1990                     Highpower       79         12.0       0
## 177 DENA   1991                     Highpower       79         11.0       0
## 178 DENA   2007                    Hot Slough       80          7.0       0
## 179 DENA   2009                    Hot Slough       80          7.0       0
## 180 DENA   2010                    Hot Slough       80          3.0       0
## 181 DENA   2011                    Hot Slough       80         10.0       0
## 182 DENA   2012                    Hot Slough       80          3.0       0
## 183 DENA   2013                    Hot Slough       80          3.0       0
## 184 DENA   2014                    Hot Slough       80          3.0       0
## 185 DENA   2015                    Hot Slough       80          0.0       0
## 186 DENA   2010                    Iron Creek       81          9.0       0
## 187 DENA   2011                    Iron Creek       81          8.0       0
## 188 DENA   2013               Iron Creek East       82          5.0       0
## 189 DENA   2014               Iron Creek East       82          2.0       0
## 190 DENA   2013               Iron Creek West       83          6.0       0
## 191 DENA   2014               Iron Creek West       83          6.0       0
## 192 DENA   2015               Iron Creek West       83         15.0       0
## 193 DENA   2016               Iron Creek West       83         10.0       0
## 194 DENA   1993                   Jenny Creek       84          6.0       0
## 195 DENA   2013                   John Hansen       85          8.0       0
## 196 DENA   2015                   John Hansen       85          3.0       0
## 197 DENA   2016                   John Hansen       85          8.0       0
## 198 DENA   2017                   John Hansen       85         15.0       0
## 199 DENA   2000               Kantishna River       86          6.0       0
## 200 DENA   2001               Kantishna River       86          6.0       0
## 201 DENA   2002               Kantishna River       86          5.0       0
## 202 DENA   2003               Kantishna River       86          8.0       0
## 203 DENA   2004               Kantishna River       86          2.0       0
## 204 DENA   2005               Kantishna River       86          7.0       0
## 205 DENA   2007               Kantishna River       86          1.0       0
## 206 DENA   2008               Kantishna River       86          5.0       0
## 207 DENA   2009               Kantishna River       86          6.0       0
## 208 DENA   2010               Kantishna River       86          8.0       0
## 209 DENA   1988                   Little Bear       87          7.0       0
## 210 DENA   1989                   Little Bear       87         12.0       0
## 211 DENA   1991                   Little Bear       87         23.0       0
## 212 DENA   1992                   Little Bear       87         12.0       0
## 213 DENA   1993                   Little Bear       87         12.0       0
## 214 DENA   1995                   Little Bear       87          0.0       0
## 215 DENA   2004                McKinley River       88          6.0       0
## 216 DENA   2005                McKinley River       88          2.0       0
## 217 DENA   2006                McKinley River       88          6.0       0
## 218 DENA   2007                McKinley River       88         10.0       0
## 219 DENA   1990               McKinley River1       89          8.0       0
## 220 DENA   1991               McKinley River1       89          9.0       0
## 221 DENA   1993               McKinley River1       89          3.0       0
## 222 DENA   1999               McKinley Slough       90          2.0       0
## 223 DENA   2000               McKinley Slough       90          3.0       0
## 224 DENA   2001               McKinley Slough       90          2.0       0
## 225 DENA   2002               McKinley Slough       90          7.0       0
## 226 DENA   2003               McKinley Slough       90          8.0       0
## 227 DENA   2004               McKinley Slough       90          8.0       0
## 228 DENA   2005               McKinley Slough       90         10.0       0
## 229 DENA   2006               McKinley Slough       90          8.0       0
## 230 DENA   2007               McKinley Slough       90         15.0       0
## 231 DENA   2009               McKinley Slough       90         15.0       0
## 232 DENA   2010               McKinley Slough       90         19.0       0
## 233 DENA   2011               McKinley Slough       90         15.0       0
## 234 DENA   2012               McKinley Slough       90          5.0       0
## 235 DENA   2013               McKinley Slough       90          4.0       0
## 236 DENA   2014               McKinley Slough       90          2.0       0
## 237 DENA   2015               McKinley Slough       90          2.0       0
## 238 DENA   2016               McKinley Slough       90          2.0       0
## 239 DENA   1987                        McLeod       91          7.0       0
## 240 DENA   1988                        McLeod       91         12.0       0
## 241 DENA   1989                        McLeod       91         12.0       0
## 242 DENA   1990                        McLeod       91         20.0       0
## 243 DENA   1991                        McLeod       91         13.0       0
## 244 DENA   1992                        McLeod       91         13.0       0
## 245 DENA   1993                        McLeod       91         15.0       0
## 246 DENA   1994                        McLeod       91         10.0       0
## 247 DENA   1995                        McLeod       91          7.0       0
## 248 DENA   1996                        McLeod       91         13.0       0
## 249 DENA   1997                        McLeod       91          6.0       0
## 250 DENA   1991                   McLeod West       92         11.0       0
## 251 DENA   2009                   Moose Creek       94          2.0       0
## 252 DENA   2010                   Moose Creek       94          0.0       0
## 253 DENA   2000                   Mt Margaret       95          3.0       0
## 254 DENA   2001                   Mt Margaret       95          6.0       0
## 255 DENA   2002                   Mt Margaret       95         10.0       0
## 256 DENA   2004                   Mt Margaret       95          7.0       0
## 257 DENA   2005                   Mt Margaret       95         10.0       0
## 258 DENA   2006                   Mt Margaret       95         11.0       0
## 259 DENA   2007                   Mt Margaret       95          7.0       0
## 260 DENA   2000                   Muddy River       96          2.0       0
## 261 DENA   2001                   Muddy River       96          4.0       0
## 262 DENA   2002                   Muddy River       96          2.0       0
## 263 DENA   2015                        Myrtle       97          6.0       0
## 264 DENA   2009                  Nenana River       98          6.0       0
## 265 DENA   2010                  Nenana River       98          5.0       0
## 266 DENA   2011                  Nenana River       98          7.0       0
## 267 DENA   2012                  Nenana River       98          8.0       0
## 268 DENA   2014                  Nenana River       98         14.0       0
## 269 DENA   1999                    North Fork       99          5.0       0
## 270 DENA   2000                    North Fork       99          4.0       0
## 271 DENA   2001                    North Fork       99          5.0       0
## 272 DENA   1998                   Otter Creek      100          7.0       0
## 273 DENA   1999                   Otter Creek      100          6.0       0
## 274 DENA   2009                    Otter Lake      100          2.0       0
## 275 DENA   2006                         Pinto      101          5.0       0
## 276 DENA   1998                   Pinto Creek      102          8.0       0
## 277 DENA   2000                   Pinto Creek      102         17.0       0
## 278 DENA   2001                   Pinto Creek      102         17.0       0
## 279 DENA   2002                   Pinto Creek      102         10.0       0
## 280 DENA   1988                  Pirate Creek      103          9.0       0
## 281 DENA   2014                   Riley Creek      104          6.0       0
## 282 DENA   2015                   Riley Creek      104          9.0       0
## 283 DENA   2016                   Riley Creek      104         14.0       0
## 284 DENA   2018              Riley Creek West      105          2.0       0
## 285 DENA   1995                     Sanctuary      106          5.0       0
## 286 DENA   1996                     Sanctuary      106         10.0       0
## 287 DENA   1997                     Sanctuary      106         15.0       0
## 288 DENA   1998                     Sanctuary      106          3.0       0
## 289 DENA   1999                     Sanctuary      106          6.0       0
## 290 DENA   1997                 Sandless Lake      107          6.0       0
## 291 DENA   2008                        Savage      108          6.0       0
## 292 DENA   1993                       Savage1      109          8.0       0
## 293 DENA   1991                Slippery Creek      110          5.0       0
## 294 DENA   1992                Slippery Creek      110          1.0       0
## 295 DENA   2008                        Somber      111          8.0       0
## 296 DENA   2009                        Somber      111          2.0       0
## 297 DENA   2010                        Somber      111          6.0       0
## 298 DENA   2012                        Somber      111          6.0       0
## 299 DENA   2013                        Somber      111          5.0       0
## 300 DENA   2014                        Somber      111          2.0       0
## 301 DENA   2015                        Somber      111          5.0       0
## 302 DENA   1988                      Stampede      112          7.0       0
## 303 DENA   1989                      Stampede      112         10.0       0
## 304 DENA   1992                      Stampede      112          2.0       0
## 305 DENA   1993                      Stampede      112          3.0       0
## 306 DENA   1994                      Stampede      112          7.0       0
## 307 DENA   1995                      Stampede      112          8.0       0
## 308 DENA   1997                      Stampede      112          2.0       0
## 309 DENA   1998                      Stampede      112          6.0       0
## 310 DENA   1999                      Stampede      112          3.0       0
## 311 DENA   2000                      Stampede      112          2.0       0
## 312 DENA   2001                      Stampede      112          2.0       0
## 313 DENA   2001                    Starr Lake      113          2.0       0
## 314 DENA   2002                    Starr Lake      113          2.0       0
## 315 DENA   2003                    Starr Lake      113         10.0       0
## 316 DENA   2004                    Starr Lake      113          7.0       0
## 317 DENA   2005                    Starr Lake      113          9.0       0
## 318 DENA   2007                    Starr Lake      113          6.0       0
## 319 DENA   2008                    Starr Lake      113          3.0       0
## 320 DENA   2009                    Starr Lake      113          6.0       0
## 321 DENA   2010                    Starr Lake      113          3.0       0
## 322 DENA   1995                         Stony      114          2.0       0
## 323 DENA   1996                         Stony      114          6.0       0
## 324 DENA   1997                         Stony      114          7.0       0
## 325 DENA   1998                         Stony      114          3.0       0
## 326 DENA   1999                         Stony      114          3.0       0
## 327 DENA   2000                  Straightaway      115         10.0       0
## 328 DENA   2001                  Straightaway      115          9.0       0
## 329 DENA   2002                  Straightaway      115         12.0       0
## 330 DENA   2004                  Straightaway      115          4.0       0
## 331 DENA   2005                  Turtle Hill       117          3.0       0
## 332 DENA   2006                  Turtle Hill       117          3.0       0
## 333 DENA   1992                  Turtle Hill1      118          8.0       0
## 334 DENA   1993                  Turtle Hill1      118          7.0       0
## 335 DENA   1996                   White Creek      119          5.0       0
## 336 DENA   1997                   White Creek      119          3.0       0
## 337 DENA   1998                   White Creek      119          4.0       0
## 338 DENA   1999                   White Creek      119          2.0       0
## 339 DENA   1987                   Windy Creek      120          8.0       0
## 340 DENA   1988                   Windy Creek      120          5.0       0
## 341 GNTP   2010                       Buffalo      158         16.8       1
## 342 GNTP   2012               Phantom Springs      165          9.6       1
## 343 GNTP   2014               Phantom Springs      165          6.0       1
## 344 GNTP   2017               Phantom Springs      165          2.4       1
## 345 GNTP   2008                      Antelope      157          9.6       1
## 346 GNTP   2006                       Buffalo      158         15.6       1
## 347 GNTP   2008                       Buffalo      158         10.8       1
## 348 GNTP   2009                       Buffalo      158         26.4       1
## 349 GNTP   2005                    Flat Creek      159          9.6       1
## 350 GNTP   2019                   Huckleberry      161         20.4       1
## 351 GNTP   2016             Lower Gros Ventre      162          8.4       1
## 352 GNTP   2018             Lower Gros Ventre      162          9.6       1
## 353 GNTP   2020             Lower Gros Ventre      162         12.0       1
## 354 GNTP   2010               Phantom Springs      165         10.8       1
## 355 GNTP   2011               Phantom Springs      165         15.6       1
## 356 GNTP   2015               Phantom Springs      165          3.6       1
## 357 GNTP   2012                 Pinnacle Peak      166         12.0       1
## 358 GNTP   2016                 Pinnacle Peak      166         15.6       1
## 359 GNTP   2017                 Pinnacle Peak      166         12.0       1
## 360 GNTP   2019                 Pinnacle Peak      166         12.0       1
## 361 GNTP   1999                         Teton      167          7.2       1
## 362 GNTP   2000                         Teton      167          4.8       1
## 363 GNTP   2003                         Teton      167         14.4       1
## 364 GNTP   2004                         Teton      167         15.6       1
## 365 GNTP   2009                      Antelope      157          9.6       0
## 366 GNTP   2010                      Antelope      157          4.8       0
## 367 GNTP   2007                       Buffalo      158         18.0       0
## 368 GNTP   2019       Horsetail Creek (Murie)      160          3.6       0
## 369 GNTP   2020       Horsetail Creek (Murie)      160          4.8       0
## 370 GNTP   2006                   Huckleberry      161          8.4       0
## 371 GNTP   2007                   Huckleberry      161          8.4       0
## 372 GNTP   2008                   Huckleberry      161          4.8       0
## 373 GNTP   2009                   Huckleberry      161          7.2       0
## 374 GNTP   2010                   Huckleberry      161         10.8       0
## 375 GNTP   2011                   Huckleberry      161          6.0       0
## 376 GNTP   2012                   Huckleberry      161          7.2       0
## 377 GNTP   2013                   Huckleberry      161         13.2       0
## 378 GNTP   2014                   Huckleberry      161          3.6       0
## 379 GNTP   2015                   Huckleberry      161         12.0       0
## 380 GNTP   2016                   Huckleberry      161         15.6       0
## 381 GNTP   2017                   Huckleberry      161          8.4       0
## 382 GNTP   2018                   Huckleberry      161         12.0       0
## 383 GNTP   2011             Lower Gros Ventre      162          3.6       0
## 384 GNTP   2012             Lower Gros Ventre      162          6.0       0
## 385 GNTP   2013             Lower Gros Ventre      162          2.4       0
## 386 GNTP   2014             Lower Gros Ventre      162          4.8       0
## 387 GNTP   2015             Lower Gros Ventre      162          6.0       0
## 388 GNTP   2017             Lower Gros Ventre      162          6.0       0
## 389 GNTP   2019             Lower Gros Ventre      162         14.4       0
## 390 GNTP   2012 Lower Slide Lake (781M Group)      163          2.4       0
## 391 GNTP   2013 Lower Slide Lake (781M Group)      163          2.4       0
## 392 GNTP   2014 Lower Slide Lake (781M Group)      163          2.4       0
## 393 GNTP   2005                 Pacific Creek      164         13.2       0
## 394 GNTP   2006                 Pacific Creek      164         10.8       0
## 395 GNTP   2007                 Pacific Creek      164         15.6       0
## 396 GNTP   2008                 Pacific Creek      164         15.6       0
## 397 GNTP   2009                 Pacific Creek      164         16.8       0
## 398 GNTP   2010                 Pacific Creek      164         14.4       0
## 399 GNTP   2008               Phantom Springs      165         10.8       0
## 400 GNTP   2009               Phantom Springs      165         10.8       0
## 401 GNTP   2013               Phantom Springs      165         13.2       0
## 402 GNTP   2016               Phantom Springs      165          4.8       0
## 403 GNTP   2007                 Pinnacle Peak      166          2.4       0
## 404 GNTP   2008                 Pinnacle Peak      166         12.0       0
## 405 GNTP   2009                 Pinnacle Peak      166         16.8       0
## 406 GNTP   2010                 Pinnacle Peak      166         13.2       0
## 407 GNTP   2011                 Pinnacle Peak      166         16.8       0
## 408 GNTP   2013                 Pinnacle Peak      166         14.4       0
## 409 GNTP   2014                 Pinnacle Peak      166         14.4       0
## 410 GNTP   2015                 Pinnacle Peak      166         15.6       0
## 411 GNTP   2018                 Pinnacle Peak      166          9.6       0
## 412 GNTP   1998                         Teton      167          2.4       0
## 413 GNTP   2001                         Teton      167         14.4       0
## 414 GNTP   2002                         Teton      167         16.8       0
## 415 GNTP   2005                         Teton      167         13.2       0
## 416 GNTP   2006                         Teton      167          3.6       0
## 417 GNTP   2020                 Wildcat Ridge      168           NA       0
## 418  VNP   1987                     tomcodbay      169          6.0       1
## 419  VNP   1988                     tomcodbay      169          6.0       1
## 420  VNP   1988                   cruiserlake      172           NA       1
## 421  VNP   2015                   Moose River      187           NA       1
## 422  VNP   2016                   Moose River      187           NA       1
## 423  VNP   2016                   Sheep Ranch      191           NA       1
## 424  VNP   2017                   Sheep Ranch      191           NA       1
## 425  VNP   2020                    Bowman Bay      182           NA       1
## 426  VNP   2020                    Fawn Creek      184           NA       1
## 427  VNP   1988                   locatorlake      170           NA       0
## 428  VNP   1989               middlepeninsula      171           NA       0
## 429  VNP   1990               middlepeninsula      171           NA       0
## 430  VNP   1990                   cruiserlake      172          5.0       0
## 431  VNP   1990               mooserivergrade      173           NA       0
## 432  VNP   1988                     brownsbay      174           NA       0
## 433  VNP   1990                     brownsbay      174           NA       0
## 434  VNP   1989                   nebraskabay      175          7.0       0
## 435  VNP   1990                   nebraskabay      175           NA       0
## 436  VNP   1988                     bluemoose      176          3.0       0
## 437  VNP   1989                       ratroot      180           NA       0
## 438  VNP   2014                     Ash River      181           NA       0
## 439  VNP   2015                     Ash River      181           NA       0
## 440  VNP   2015                    Bowman Bay      182           NA       0
## 441  VNP   2015                    Sand Point      190           NA       0
## 442  VNP   2016                     Ash River      181           NA       0
## 443  VNP   2016                    Bowman Bay      182           NA       0
## 444  VNP   2016                    Sand Point      190           NA       0
## 445  VNP   2017                     Ash River      181           NA       0
## 446  VNP   2017                    Bowman Bay      182           NA       0
## 447  VNP   2017                     Lightfoot      186           NA       0
## 448  VNP   2017                   Moose River      187           NA       0
## 449  VNP   2017                    Sand Point      190           NA       0
## 450  VNP   2018                    Bowman Bay      182           NA       0
## 451  VNP   2018                    Fawn Creek      184           NA       0
## 452  VNP   2018                     Lightfoot      186           NA       0
## 453  VNP   2018                   Moose River      187           NA       0
## 454  VNP   2018                    Sand Point      190           NA       0
## 455  VNP   2018                   Sheep Ranch      191           NA       0
## 456  VNP   2019                 Cranberry Bay      183           NA       0
## 457  VNP   2019                       Nashata      188           NA       0
## 458  VNP   2019                      Paradise      189           NA       0
## 459  VNP   2019                    Sand Point      190          5.0       0
## 460  VNP   2019                 Shoepack Lake      192          7.0       0
## 461  VNP   2020                     Half-Moon      185          7.0       0
## 462  VNP   2020                       Nashata      188           NA       0
## 463  VNP   2020                      Paradise      189          2.0       0
## 464  VNP   2020                    Sand Point      190          2.0       0
## 465  VNP   2020                      Windsong      193           NA       0
## 466  YNP   2009                    cottonwood       23         12.0       1
## 467  YNP   2016                         8mile       11         20.0       1
## 468  YNP   2017                        canyon       20          2.0       1
## 469  YNP   2012                      junction       33         11.0       1
## 470  YNP   2016                      junction       33         15.0       1
## 471  YNP   2011                     642Fgroup        5         10.0       1
## 472  YNP   2012                         8mile       11         15.0       1
## 473  YNP   2012                       bechler       16         12.0       1
## 474  YNP   2012                         lamar       34         13.0       1
## 475  YNP   2019                       phantom       41         18.0       1
## 476  YNP   2020                       phantom       41         16.0       1
## 477  YNP   2017                      prospect       42         10.0       1
## 478  YNP   2020                        wapiti       51         23.0       1
## 479  YNP   2018                    1118Fgroup        2          5.0       1
## 480  YNP   2020                    1155Mgroup        3          3.0       1
## 481  YNP   2014                         8mile       11         27.0       1
## 482  YNP   2017                     963Fgroup       13          4.0       1
## 483  YNP   2012                     blacktail       18         10.0       1
## 484  YNP   2014                        cougar       24         18.0       1
## 485  YNP   2017                        cougar       24          7.0       1
## 486  YNP   2014                      junction       33         13.0       1
## 487  YNP   2018                         lamar       34          8.0       1
## 488  YNP   2012                       mollies       38          6.0       1
## 489  YNP   2019                       mollies       38         12.0       1
## 490  YNP   2018                       phantom       41          5.0       1
## 491  YNP   2014                      prospect       42          7.0       1
## 492  YNP   2012                         snake       46          4.0       1
## 493  YNP   2017                         snake       46         13.0       1
## 494  YNP   2007                     yelldelta       52         22.0       1
## 495  YNP   1997                         druid       26          5.0       1
## 496  YNP   2019                      junction       33         21.0       1
## 497  YNP   2003                         agate       15         13.0       1
## 498  YNP   2009                     blacktail       18         13.0       1
## 499  YNP   2011                     blacktail       18         16.0       1
## 500  YNP   2013                        canyon       20         10.0       1
## 501  YNP   2015                        cougar       24          7.0       1
## 502  YNP   2000                         druid       26         27.0       1
## 503  YNP   2004                   gibbon/mary       29          9.0       1
## 504  YNP   2009                   gibbon/mary       29         26.0       1
## 505  YNP   2010                      grayling       30          3.0       1
## 506  YNP   2007                       mollies       38         15.0       1
## 507  YNP   2011                       mollies       38         23.0       1
## 508  YNP   2013                       mollies       38          8.0       1
## 509  YNP   1996                      nezperce       39          9.0       1
## 510  YNP   1997                      nezperce       39          7.0       1
## 511  YNP   2000                      nezperce       39         22.0       1
## 512  YNP   1995                          rose       44          9.0       1
## 513  YNP   1998                          rose       44         23.0       1
## 514  YNP   2007                        slough       45         22.0       1
## 515  YNP   2009                     682Mgroup        6           NA       0
## 516  YNP   2010                     682Mgroup        6          0.0       0
## 517  YNP   2009                     694Fgroup        8          0.0       0
## 518  YNP   2013                     755Mgroup        9          2.0       0
## 519  YNP   2011                         8mile       11         17.0       0
## 520  YNP   2013                         8mile       11         19.0       0
## 521  YNP   2015                         8mile       11         13.0       0
## 522  YNP   2017                         8mile       11         18.0       0
## 523  YNP   2018                         8mile       11         13.0       0
## 524  YNP   2020                         8mile       11         22.0       0
## 525  YNP   2002                         agate       15         10.0       0
## 526  YNP   2004                         agate       15         13.0       0
## 527  YNP   2005                         agate       15         14.0       0
## 528  YNP   2006                         agate       15         13.0       0
## 529  YNP   2007                         agate       15         20.0       0
## 530  YNP   2008                         agate       15         14.0       0
## 531  YNP   2009                         agate       15          4.0       0
## 532  YNP   2010                         agate       15          9.0       0
## 533  YNP   2011                         agate       15         13.0       0
## 534  YNP   2012                         agate       15          0.0       0
## 535  YNP   2002                       bechler       16          4.0       0
## 536  YNP   2004                       biscuit       17         11.0       0
## 537  YNP   2008                     blacktail       18           NA       0
## 538  YNP   2010                     blacktail       18         17.0       0
## 539  YNP   2002                   buffalofork       19          4.0       0
## 540  YNP   2003                   buffalofork       19          3.0       0
## 541  YNP   2008                        canyon       20          6.0       0
## 542  YNP   2009                        canyon       20          4.0       0
## 543  YNP   2010                        canyon       20          6.0       0
## 544  YNP   2011                        canyon       20          8.0       0
## 545  YNP   2012                        canyon       20          9.0       0
## 546  YNP   2014                        canyon       20          5.0       0
## 547  YNP   2015                        canyon       20          6.0       0
## 548  YNP   2016                        canyon       20          6.0       0
## 549  YNP   2021                     carnelian       21          0.0       0
## 550  YNP   2016                      cinnabar       22          3.0       0
## 551  YNP   2008                    cottonwood       23          4.0       0
## 552  YNP   2001                        cougar       24          6.0       0
## 553  YNP   2002                        cougar       24         11.0       0
## 554  YNP   2003                        cougar       24         14.0       0
## 555  YNP   2004                        cougar       24         15.0       0
## 556  YNP   2005                        cougar       24         14.0       0
## 557  YNP   2006                        cougar       24          4.0       0
## 558  YNP   2007                        cougar       24          7.0       0
## 559  YNP   2008                        cougar       24          5.0       0
## 560  YNP   2009                        cougar       24          6.0       0
## 561  YNP   2010                        cougar       24          4.0       0
## 562  YNP   2011                        cougar       24          7.0       0
## 563  YNP   2012                        cougar       24         11.0       0
## 564  YNP   2013                        cougar       24         14.0       0
## 565  YNP   2016                        cougar       24          8.0       0
## 566  YNP   2018                        cougar       24         10.0       0
## 567  YNP   2019                        cougar       24          6.0       0
## 568  YNP   2020                        cougar       24          6.0       0
## 569  YNP   2017                       crevice       25           NA       0
## 570  YNP   1996                         druid       26          5.0       0
## 571  YNP   1998                         druid       26          8.0       0
## 572  YNP   1999                         druid       26          9.0       0
## 573  YNP   2001                         druid       26         37.0       0
## 574  YNP   2002                         druid       26         16.0       0
## 575  YNP   2003                         druid       26         18.0       0
## 576  YNP   2004                         druid       26         13.0       0
## 577  YNP   2005                         druid       26          5.0       0
## 578  YNP   2006                         druid       26         15.0       0
## 579  YNP   2007                         druid       26         18.0       0
## 580  YNP   2008                         druid       26         21.0       0
## 581  YNP   2009                         druid       26         12.0       0
## 582  YNP   2010                         druid       26          0.0       0
## 583  YNP   2008                        everts       27          9.0       0
## 584  YNP   2009                        everts       27         12.0       0
## 585  YNP   2002                    geode/hell       28          9.0       0
## 586  YNP   2003                    geode/hell       28          9.0       0
## 587  YNP   2004                    geode/hell       28         12.0       0
## 588  YNP   2005                    geode/hell       28          7.0       0
## 589  YNP   2003                   gibbon/mary       29           NA       0
## 590  YNP   2005                   gibbon/mary       29         10.0       0
## 591  YNP   2006                   gibbon/mary       29         12.0       0
## 592  YNP   2007                   gibbon/mary       29         18.0       0
## 593  YNP   2008                   gibbon/mary       29         25.0       0
## 594  YNP   2010                   gibbon/mary       29          8.0       0
## 595  YNP   2011                   gibbon/mary       29         11.0       0
## 596  YNP   2012                   gibbon/mary       29          0.0       0
## 597  YNP   2009                      grayling       30          6.0       0
## 598  YNP   2004                        hayden       31          4.0       0
## 599  YNP   2005                        hayden       31          5.0       0
## 600  YNP   2006                        hayden       31          7.0       0
## 601  YNP   2007                        hayden       31          9.0       0
## 602  YNP   2019                         heart       32          2.0       0
## 603  YNP   2020                         heart       32          7.0       0
## 604  YNP   2015                      junction       33         19.0       0
## 605  YNP   2017                      junction       33          8.0       0
## 606  YNP   2018                      junction       33         11.0       0
## 607  YNP   2020                      junction       33         35.0       0
## 608  YNP   2010                         lamar       34          7.0       0
## 609  YNP   2011                         lamar       34         11.0       0
## 610  YNP   2014                         lamar       34          8.0       0
## 611  YNP   2015                         lamar       34         12.0       0
## 612  YNP   2016                         lamar       34          4.0       0
## 613  YNP   2017                         lamar       34          3.0       0
## 614  YNP   2008                          lava       35          5.0       0
## 615  YNP   2009                          lava       35          3.0       0
## 616  YNP   2010                          lava       35          1.0       0
## 617  YNP   1996                       leopold       36          5.0       0
## 618  YNP   1997                       leopold       36         10.0       0
## 619  YNP   1998                       leopold       36         12.0       0
## 620  YNP   1999                       leopold       36         11.0       0
## 621  YNP   2000                       leopold       36         15.0       0
## 622  YNP   2001                       leopold       36         14.0       0
## 623  YNP   2002                       leopold       36         16.0       0
## 624  YNP   2003                       leopold       36         21.0       0
## 625  YNP   2004                       leopold       36         28.0       0
## 626  YNP   2005                       leopold       36         26.0       0
## 627  YNP   2006                       leopold       36         20.0       0
## 628  YNP   2007                       leopold       36         19.0       0
## 629  YNP   2008                       leopold       36          7.0       0
## 630  YNP   1996                      lonestar       37          0.0       0
## 631  YNP   1995                       mollies       38          5.0       0
## 632  YNP   1996                       mollies       38          2.0       0
## 633  YNP   1997                       mollies       38          8.0       0
## 634  YNP   1998                       mollies       38         16.0       0
## 635  YNP   1999                       mollies       38         15.0       0
## 636  YNP   2000                       mollies       38          5.0       0
## 637  YNP   2001                       mollies       38         10.0       0
## 638  YNP   2002                       mollies       38         13.0       0
## 639  YNP   2003                       mollies       38          8.0       0
## 640  YNP   2004                       mollies       38          9.0       0
## 641  YNP   2005                       mollies       38          7.0       0
## 642  YNP   2006                       mollies       38         11.0       0
## 643  YNP   2008                       mollies       38         15.0       0
## 644  YNP   2009                       mollies       38         17.0       0
## 645  YNP   2010                       mollies       38         17.0       0
## 646  YNP   2014                       mollies       38         12.0       0
## 647  YNP   2015                       mollies       38         17.0       0
## 648  YNP   2016                       mollies       38         18.0       0
## 649  YNP   2017                       mollies       38         15.0       0
## 650  YNP   2018                       mollies       38         10.0       0
## 651  YNP   2020                       mollies       38          8.0       0
## 652  YNP   2006                      nezperce       39          0.0       0
## 653  YNP   1998                      nezperce       39          7.0       0
## 654  YNP   1999                      nezperce       39         12.0       0
## 655  YNP   2001                      nezperce       39         19.0       0
## 656  YNP   2002                      nezperce       39         18.0       0
## 657  YNP   2003                      nezperce       39         18.0       0
## 658  YNP   2004                      nezperce       39         14.0       0
## 659  YNP   2005                      nezperce       39         11.0       0
## 660  YNP   2006                         oxbow       40         12.0       0
## 661  YNP   2007                         oxbow       40         24.0       0
## 662  YNP   2008                         oxbow       40         20.0       0
## 663  YNP   2015                      prospect       42         13.0       0
## 664  YNP   2016                      prospect       42         12.0       0
## 665  YNP   2007                      quadrant       43           NA       0
## 666  YNP   2008                      quadrant       43          4.0       0
## 667  YNP   2009                      quadrant       43          7.0       0
## 668  YNP   2010                      quadrant       43          7.0       0
## 669  YNP   1996                          rose       44         11.0       0
## 670  YNP   1997                          rose       44         16.0       0
## 671  YNP   1999                          rose       44         22.0       0
## 672  YNP   2000                          rose       44         21.0       0
## 673  YNP   2001                          rose       44         10.0       0
## 674  YNP   2002                          rose       44         11.0       0
## 675  YNP   2003                        slough       45         10.0       0
## 676  YNP   2004                        slough       45         21.0       0
## 677  YNP   2005                        slough       45         15.0       0
## 678  YNP   2006                        slough       45          9.0       0
## 679  YNP   2004               specimen/silver       47          5.0       0
## 680  YNP   2010               specimen/silver       47          8.0       0
## 681  YNP   2000                          swan       48          7.0       0
## 682  YNP   2001                          swan       48          9.0       0
## 683  YNP   2002                          swan       48         18.0       0
## 684  YNP   2003                          swan       48         20.0       0
## 685  YNP   2004                          swan       48         10.0       0
## 686  YNP   2005                          swan       48          3.0       0
## 687  YNP   1996                     thorofare       49          2.0       0
## 688  YNP   1997                     thorofare       49          8.0       0
## 689  YNP   1998                     thorofare       49          0.0       0
## 690  YNP   2001                         tower       50          4.0       0
## 691  YNP   2002                         tower       50          2.0       0
## 692  YNP   2014                        wapiti       51          2.0       0
## 693  YNP   2016                        wapiti       51          9.0       0
## 694  YNP   2017                        wapiti       51         21.0       0
## 695  YNP   2018                        wapiti       51         18.0       0
## 696  YNP   2019                        wapiti       51         19.0       0
## 697  YNP   1995                     yelldelta       52          6.0       0
## 698  YNP   1996                     yelldelta       52          5.0       0
## 699  YNP   1997                     yelldelta       52          8.0       0
## 700  YNP   1998                     yelldelta       52          8.0       0
## 701  YNP   1999                     yelldelta       52          6.0       0
## 702  YNP   2000                     yelldelta       52         15.0       0
## 703  YNP   2001                     yelldelta       52         18.0       0
## 704  YNP   2002                     yelldelta       52         17.0       0
## 705  YNP   2003                     yelldelta       52         17.0       0
## 706  YNP   2004                     yelldelta       52         19.0       0
## 707  YNP   2005                     yelldelta       52         19.0       0
## 708  YNP   2006                     yelldelta       52         15.0       0
## 709  YNP   2009                     yelldelta       52          4.0       0
## 710  YNP   2010                     yelldelta       52          9.0       0
## 711  YNP   2011                     yelldelta       52         13.0       0
## 712  YNP   2012                     yelldelta       52         11.0       0
## 713  YNP   2013                     yelldelta       52         17.0       0
## 714 YUCH   2000                       70 Mile      121           NA       1
## 715 YUCH   2005                       70 Mile      121         14.0       1
## 716 YUCH   2008                       70 Mile      121          9.0       1
## 717 YUCH   2012                       70 Mile      121         24.0       1
## 718 YUCH   2008               Copper Mountain      124          7.0       1
## 719 YUCH   2009               Copper Mountain      124          3.0       1
## 720 YUCH   1994                    Cottonwood      125         16.0       1
## 721 YUCH   1996                    Cottonwood      125           NA       1
## 722 YUCH   2000                    Cottonwood      125          9.0       1
## 723 YUCH   2005                    Cottonwood      125         14.0       1
## 724 YUCH   2006               Crescent Creek2      127          3.0       1
## 725 YUCH   1996                Edwards Creek1      128          9.0       1
## 726 YUCH   2001                Edwards Creek2      129          5.0       1
## 727 YUCH   2007                Edwards Creek2      129          3.0       1
## 728 YUCH   2004                  Fisher Creek      130          6.0       1
## 729 YUCH   1996                    Flat Creek      131          2.0       1
## 730 YUCH   1993                Fourth of July      132          3.0       1
## 731 YUCH   2006                   Hanna Creek      134          6.0       1
## 732 YUCH   2002               Hard Luck Creek      135          0.0       1
## 733 YUCH   2011                    Lost Creek      137         13.0       1
## 734 YUCH   2013                    Lost Creek      137         11.0       1
## 735 YUCH   2000                Lower Charley1      138          1.0       1
## 736 YUCH   2010                Lower Charley2      139         10.0       1
## 737 YUCH   2014                   Sheep Bluff      144           NA       1
## 738 YUCH   2005                Step Mountain2      147          6.0       1
## 739 YUCH   2012                Step Mountain2      147          6.0       1
## 740 YUCH   1999                  Three Finger      149         13.0       1
## 741 YUCH   2000                  Three Finger      149         12.0       1
## 742 YUCH   1995              Washington Creek      152          4.0       1
## 743 YUCH   2009                Webber Creek 2      153           NA       1
## 744 YUCH   2012                   Woodchopper      155         10.0       1
## 745 YUCH   2012                    Yukon Fork      156         10.0       1
## 746 YUCH   2013                    Yukon Fork      156          2.0       1
## 747 YUCH   1997                  Three Finger      149          4.0       1
## 748 YUCH   2001                       70 Mile      121          9.0       0
## 749 YUCH   2002                       70 Mile      121         13.0       0
## 750 YUCH   2003                       70 Mile      121         12.0       0
## 751 YUCH   2004                       70 Mile      121          8.0       0
## 752 YUCH   2006                       70 Mile      121         11.0       0
## 753 YUCH   2007                       70 Mile      121         13.0       0
## 754 YUCH   2009                       70 Mile      121         12.0       0
## 755 YUCH   2010                       70 Mile      121         12.0       0
## 756 YUCH   2011                       70 Mile      121         18.0       0
## 757 YUCH   2006             Andrew Creek Pair      122          2.0       0
## 758 YUCH   2005              Birch Creek Pair      123          2.0       0
## 759 YUCH   2006               Copper Mountain      124          5.0       0
## 760 YUCH   2007               Copper Mountain      124          5.0       0
## 761 YUCH   1993                    Cottonwood      125         13.0       0
## 762 YUCH   1995                    Cottonwood      125         16.0       0
## 763 YUCH   1997                    Cottonwood      125          7.0       0
## 764 YUCH   1998                    Cottonwood      125         10.0       0
## 765 YUCH   1999                    Cottonwood      125         10.0       0
## 766 YUCH   2001                    Cottonwood      125          8.0       0
## 767 YUCH   2002                    Cottonwood      125          8.0       0
## 768 YUCH   2003                    Cottonwood      125          6.0       0
## 769 YUCH   2004                    Cottonwood      125         12.0       0
## 770 YUCH   1993          Crescent Creek1 Pair      126          2.0       0
## 771 YUCH   2007               Crescent Creek2      127          6.0       0
## 772 YUCH   1995                Edwards Creek1      128          6.0       0
## 773 YUCH   1997                Edwards Creek1      128         12.0       0
## 774 YUCH   1998                Edwards Creek1      128          1.0       0
## 775 YUCH   2002                Edwards Creek2      129          3.0       0
## 776 YUCH   2003                Edwards Creek2      129          6.0       0
## 777 YUCH   2004                Edwards Creek2      129          5.0       0
## 778 YUCH   2005                Edwards Creek2      129          4.0       0
## 779 YUCH   2006                Edwards Creek2      129          4.0       0
## 780 YUCH   2008                Edwards Creek2      129          6.0       0
## 781 YUCH   2000                  Fisher Creek      130         11.0       0
## 782 YUCH   2001                  Fisher Creek      130         14.0       0
## 783 YUCH   2002                  Fisher Creek      130          2.0       0
## 784 YUCH   2003                  Fisher Creek      130          4.0       0
## 785 YUCH   2005                  Fisher Creek      130          2.0       0
## 786 YUCH   1993                    Flat Creek      131          2.0       0
## 787 YUCH   1994                    Flat Creek      131          2.0       0
## 788 YUCH   1995                    Flat Creek      131          5.0       0
## 789 YUCH   1998                   Godge Creek      133          2.0       0
## 790 YUCH   1999                   Godge Creek      133          7.0       0
## 791 YUCH   2000                   Godge Creek      133          8.0       0
## 792 YUCH   2001                   Godge Creek      133          7.0       0
## 793 YUCH   2002                   Godge Creek      133         11.0       0
## 794 YUCH   1997               Hard Luck Creek      135          6.0       0
## 795 YUCH   1998               Hard Luck Creek      135          5.0       0
## 796 YUCH   1999               Hard Luck Creek      135          9.0       0
## 797 YUCH   2000               Hard Luck Creek      135         12.0       0
## 798 YUCH   2001               Hard Luck Creek      135         13.0       0
## 799 YUCH   1995               Kathul Mountain      136          6.0       0
## 800 YUCH   2008                    Lost Creek      137          8.0       0
## 801 YUCH   2009                    Lost Creek      137         12.0       0
## 802 YUCH   2010                    Lost Creek      137          7.0       0
## 803 YUCH   2012                    Lost Creek      137         10.0       0
## 804 YUCH   2014                    Lost Creek      137           NA       0
## 805 YUCH   1995                Lower Charley1      138          8.0       0
## 806 YUCH   1996                Lower Charley1      138         13.0       0
## 807 YUCH   1997                Lower Charley1      138         18.0       0
## 808 YUCH   1998                Lower Charley1      138         14.0       0
## 809 YUCH   1999                Lower Charley1      138         11.0       0
## 810 YUCH   2007                Lower Charley2      139          4.0       0
## 811 YUCH   2008                Lower Charley2      139          9.0       0
## 812 YUCH   2009                Lower Charley2      139          9.0       0
## 813 YUCH   2011                Lower Charley2      139          8.0       0
## 814 YUCH   2012                Lower Charley2      139          4.0       0
## 815 YUCH   1997                  Lower Kandik      140          2.0       0
## 816 YUCH   1998                  Lower Kandik      140          2.0       0
## 817 YUCH   1999                  Lower Kandik      140          5.0       0
## 818 YUCH   2000                  Lower Kandik      140          4.0       0
## 819 YUCH   2010                  Nation River      141         12.0       0
## 820 YUCH   2011                  Nation River      141          6.0       0
## 821 YUCH   2012                  Nation River      141          2.0       0
## 822 YUCH   2013                  Nation River      141          4.0       0
## 823 YUCH   1998                 Poverty Creek      142          2.0       0
## 824 YUCH   1999                 Poverty Creek      142          7.0       0
## 825 YUCH   2000                    Rock Creek      143          2.0       0
## 826 YUCH   2012                   Sheep Bluff      144          6.0       0
## 827 YUCH   2013                   Sheep Bluff      144          8.0       0
## 828 YUCH   2001                   Snowy Peak1      145          0.0       0
## 829 YUCH   2012                   Snowy Peak2      146          9.0       0
## 830 YUCH   2000                Step Mountain2      147          8.0       0
## 831 YUCH   2001                Step Mountain2      147          3.0       0
## 832 YUCH   2002                Step Mountain2      147         13.0       0
## 833 YUCH   2003                Step Mountain2      147         14.0       0
## 834 YUCH   2004                Step Mountain2      147          7.0       0
## 835 YUCH   2006                Step Mountain2      147          6.0       0
## 836 YUCH   2007                Step Mountain2      147          2.0       0
## 837 YUCH   2008                Step Mountain2      147         10.0       0
## 838 YUCH   2009                Step Mountain2      147          4.0       0
## 839 YUCH   2010                Step Mountain2      147          7.0       0
## 840 YUCH   2011                Step Mountain2      147          9.0       0
## 841 YUCH   2013                Step Mountain2      147         12.0       0
## 842 YUCH   2007                Sterling Creek      148          7.0       0
## 843 YUCH   1993                  Three Finger      149          4.0       0
## 844 YUCH   1994                  Three Finger      149          7.0       0
## 845 YUCH   1995                  Three Finger      149          3.0       0
## 846 YUCH   1996                  Three Finger      149          9.0       0
## 847 YUCH   1998                  Three Finger      149         11.0       0
## 848 YUCH   2001                  Three Finger      149         11.0       0
## 849 YUCH   2002                  Three Finger      149         10.0       0
## 850 YUCH   2003                  Three Finger      150         13.0       0
## 851 YUCH   2006        Upper Black River Pair      150          2.0       0
## 852 YUCH   2007        Upper Black River Pair      150          3.0       0
## 853 YUCH   2008        Upper Black River Pair      150          1.0       0
## 854 YUCH   2002                  Upper Kandik      151           NA       0
## 855 YUCH   2003                  Upper Kandik      151           NA       0
## 856 YUCH   1993              Washington Creek      152          2.0       0
## 857 YUCH   1994              Washington Creek      152          3.0       0
## 858 YUCH   1998                 Webber Creek1      154          3.0       0
## 859 YUCH   1999                 Webber Creek1      154         10.0       0
## 860 YUCH   2000                 Webber Creek1      154          5.0       0
## 861 YUCH   2011                   Woodchopper      155          6.0       0
## 862 YUCH   2013                   Woodchopper      155          0.0       0
## 863 YUCH   2011                    Yukon Fork      156          5.0       0
## 864 YUCH   2014                    Yukon Fork      156          2.0       0
##     mort_all mort_lead mort_nonlead reprody1 persisty1
## 1          4         2            2        0         0
## 2          2         2            0        0         0
## 3          2         0            2       NA         1
## 4          2         0            2        1         1
## 5          2         0            2       NA         1
## 6          2         0            2        0         1
## 7          2         1            1        0         0
## 8          2         2            0        1         1
## 9          2         1            1        0         0
## 10         1         1            0        1         1
## 11         1         1            0        0         0
## 12         1         0            1        1         1
## 13         1         0            1        0         0
## 14         1         0            1        1         1
## 15         1         1            0        1         1
## 16         1         1            0        1         1
## 17         1         1            0        1         1
## 18         1         0            1        1         1
## 19         1         0            1        1         1
## 20         1         0            1        0         1
## 21         1         0            1        1         1
## 22         1         1            0        0         0
## 23         1         0            1        0         1
## 24         1         0            1        1         1
## 25         1         0            1        1         1
## 26         1         0            1        1         1
## 27         1         0            1        1         1
## 28         1         0            1       NA         1
## 29         1         1            0        0         1
## 30         1         0            1        0         1
## 31         1         1            0        1         1
## 32         1         0            1        0         0
## 33         1         0            1        1         1
## 34         1         1            0        1         1
## 35         1         0            1        0         0
## 36         1         0            1        0         0
## 37         1         1            0        0         0
## 38         1         1            0        1         1
## 39         1         0            1        1         1
## 40         1         0            1        1         1
## 41         1         0            1       NA         1
## 42         1         1            0        1         1
## 43         1         0            1        1         1
## 44         1         0            1        1         1
## 45         1         1            0        1         1
## 46         1         1            0        1         1
## 47         1         1            0        0         1
## 48         1         0            1        0         0
## 49         1         0            1        1         1
## 50         1         1            0        1         1
## 51         1         1            0        1         1
## 52         1         0            1        0         1
## 53         1         1            0        0         0
## 54         0         0            0        1         1
## 55         0         0            0        1         1
## 56         0         0            0        1         1
## 57         0         0            0        1         1
## 58         0         0            0        1         1
## 59         0         0            0        1         1
## 60         0         0            0        0         0
## 61         0         0            0        1         1
## 62         0         0            0        1         1
## 63         0         0            0        1         1
## 64         0         0            0        1         1
## 65         0         0            0        0         1
## 66         0         0            0        1         1
## 67         0         0            0        1         1
## 68         0         0            0        1         1
## 69         0         0            0        1         1
## 70         0         0            0        0         1
## 71         0         0            0        1         1
## 72         0         0            0        1         1
## 73         0         0            0        1         1
## 74         0         0            0       NA         1
## 75         0         0            0        0         0
## 76         0         0            0        1         1
## 77         0         0            0        1         1
## 78         0         0            0        0         1
## 79         0         0            0        0         0
## 80         0         0            0        1         1
## 81         0         0            0        1         1
## 82         0         0            0        1         1
## 83         0         0            0        0         0
## 84         0         0            0       NA        NA
## 85         0         0            0        0         1
## 86         0         0            0        1         1
## 87         0         0            0        0         1
## 88         0         0            0        0         1
## 89         0         0            0        0         0
## 90         0         0            0        0        NA
## 91         0         0            0        1         1
## 92         0         0            0        1         1
## 93         0         0            0        0         0
## 94         0         0            0        1         1
## 95         0         0            0        1         1
## 96         0         0            0        1         1
## 97         0         0            0        1         1
## 98         0         0            0        0         0
## 99         0         0            0        1         1
## 100        0         0            0        1         1
## 101        0         0            0        1         1
## 102        0         0            0        1         1
## 103        0         0            0        0         1
## 104        0         0            0        0         0
## 105        0         0            0        1         1
## 106        0         0            0        0         1
## 107        0         0            0        1         1
## 108        0         0            0        0         0
## 109        0         0            0        1         1
## 110        0         0            0        1         1
## 111        0         0            0        1         1
## 112        0         0            0        0         0
## 113        0         0            0        0         1
## 114        0         0            0        0         1
## 115        0         0            0        1         1
## 116        0         0            0        1         1
## 117        0         0            0        1         1
## 118        0         0            0        1         1
## 119        0         0            0        1         1
## 120        0         0            0        1         1
## 121        0         0            0        1         1
## 122        0         0            0        1         1
## 123        0         0            0        1         1
## 124        0         0            0        1         1
## 125        0         0            0        1         1
## 126        0         0            0        1         1
## 127        0         0            0        1         1
## 128        0         0            0        1         1
## 129        0         0            0        1         1
## 130        0         0            0        1         1
## 131        0         0            0        1         1
## 132        0         0            0        1         1
## 133        0         0            0        1         1
## 134        0         0            0        1         1
## 135        0         0            0        1         1
## 136        0         0            0        1         1
## 137        0         0            0        1         1
## 138        0         0            0        1         1
## 139        0         0            0        1         1
## 140        0         0            0        1         1
## 141        0         0            0        1         1
## 142        0         0            0        1         1
## 143        0         0            0        1         1
## 144        0         0            0        1         1
## 145        0         0            0        1         1
## 146        0         0            0        1         1
## 147        0         0            0        1         1
## 148        0         0            0        1         1
## 149        0         0            0        0         0
## 150        0         0            0        0         1
## 151        0         0            0        1         1
## 152        0         0            0        1         1
## 153        0         0            0        1         1
## 154        0         0            0        1         1
## 155        0         0            0        1         1
## 156        0         0            0        1         1
## 157        0         0            0        1         1
## 158        0         0            0        1         1
## 159        0         0            0        1         1
## 160        0         0            0        1         1
## 161        0         0            0        0         1
## 162        0         0            0       NA        NA
## 163        0         0            0        0         0
## 164        0         0            0        0         1
## 165        0         0            0        1         1
## 166        0         0            0        1         1
## 167        0         0            0        1         1
## 168        0         0            0        1         1
## 169        0         0            0        1         1
## 170        0         0            0        1         1
## 171        0         0            0        0         1
## 172        0         0            0        0         1
## 173        0         0            0        0         0
## 174        0         0            0        1         1
## 175        0         0            0        1         1
## 176        0         0            0        1         1
## 177        0         0            0        1         1
## 178        0         0            0        1         1
## 179        0         0            0        0         1
## 180        0         0            0        1         1
## 181        0         0            0        1         1
## 182        0         0            0        0         1
## 183        0         0            0        0         1
## 184        0         0            0        1         1
## 185        0         0            0        0         0
## 186        0         0            0        0         1
## 187        0         0            0        0         0
## 188        0         0            0        1         1
## 189        0         0            0        0         0
## 190        0         0            0        1         1
## 191        0         0            0        1         1
## 192        0         0            0        1         1
## 193        0         0            0        0         1
## 194        0         0            0        1         1
## 195        0         0            0        1         1
## 196        0         0            0        1         1
## 197        0         0            0        1         1
## 198        0         0            0       NA         1
## 199        0         0            0        1         1
## 200        0         0            0        0         1
## 201        0         0            0        1         1
## 202        0         0            0        0         1
## 203        0         0            0        1         1
## 204        0         0            0        0         1
## 205        0         0            0        1         1
## 206        0         0            0        1         1
## 207        0         0            0        1         1
## 208        0         0            0        0        NA
## 209        0         0            0        1         1
## 210        0         0            0       NA         1
## 211        0         0            0        1         1
## 212        0         0            0        1         1
## 213        0         0            0        1         1
## 214        0         0            0        0         0
## 215        0         0            0        1         1
## 216        0         0            0        1         1
## 217        0         0            0        1         1
## 218        0         0            0        1         0
## 219        0         0            0        1         1
## 220        0         0            0        1         1
## 221        0         0            0       NA         1
## 222        0         0            0        1         1
## 223        0         0            0        0         1
## 224        0         0            0        1         1
## 225        0         0            0        1         1
## 226        0         0            0        1         1
## 227        0         0            0        1         1
## 228        0         0            0        1         1
## 229        0         0            0        1         1
## 230        0         0            0        1         1
## 231        0         0            0        1         1
## 232        0         0            0        1         1
## 233        0         0            0        1         1
## 234        0         0            0        0         1
## 235        0         0            0        0         1
## 236        0         0            0        0         1
## 237        0         0            0        0         1
## 238        0         0            0        0         0
## 239        0         0            0        1         1
## 240        0         0            0        1         1
## 241        0         0            0        1         1
## 242        0         0            0        1         1
## 243        0         0            0        1         1
## 244        0         0            0        1         1
## 245        0         0            0        1         1
## 246        0         0            0        0         1
## 247        0         0            0        1         1
## 248        0         0            0        1         1
## 249        0         0            0        0         0
## 250        0         0            0        1         1
## 251        0         0            0        1         1
## 252        0         0            0        0         0
## 253        0         0            0        1         1
## 254        0         0            0        1         1
## 255        0         0            0        1         1
## 256        0         0            0        1         1
## 257        0         0            0        1         1
## 258        0         0            0        1         1
## 259        0         0            0        1         1
## 260        0         0            0        0         1
## 261        0         0            0       NA         1
## 262        0         0            0       NA         1
## 263        0         0            0        1         1
## 264        0         0            0        1         1
## 265        0         0            0        1         1
## 266        0         0            0        1         1
## 267        0         0            0        0         1
## 268        0         0            0        1         1
## 269        0         0            0        0         1
## 270        0         0            0        1         1
## 271        0         0            0        0         0
## 272        0         0            0        1         1
## 273        0         0            0        1         1
## 274        0         0            0        1         1
## 275        0         0            0        1         1
## 276        0         0            0        1         1
## 277        0         0            0        1         1
## 278        0         0            0        1         1
## 279        0         0            0        1         1
## 280        0         0            0        0         0
## 281        0         0            0        1         1
## 282        0         0            0        1         1
## 283        0         0            0        1         1
## 284        0         0            0       NA         1
## 285        0         0            0        1         1
## 286        0         0            0        1         1
## 287        0         0            0        1         1
## 288        0         0            0        1         1
## 289        0         0            0        1         1
## 290        0         0            0        0         1
## 291        0         0            0        0         0
## 292        0         0            0        1         1
## 293        0         0            0        1         1
## 294        0         0            0        0         0
## 295        0         0            0        0         1
## 296        0         0            0        1         1
## 297        0         0            0       NA         1
## 298        0         0            0        0         1
## 299        0         0            0        0         1
## 300        0         0            0        0         1
## 301        0         0            0        1         1
## 302        0         0            0        1         1
## 303        0         0            0        0         1
## 304        0         0            0        1         1
## 305        0         0            0        1         1
## 306        0         0            0        1         1
## 307        0         0            0        1         1
## 308        0         0            0        1         1
## 309        0         0            0        1         1
## 310        0         0            0        1         1
## 311        0         0            0        1         1
## 312        0         0            0        0         0
## 313        0         0            0        0         1
## 314        0         0            0        1         1
## 315        0         0            0        1         1
## 316        0         0            0        1         1
## 317        0         0            0        0         1
## 318        0         0            0        1         1
## 319        0         0            0        1         1
## 320        0         0            0        1         1
## 321        0         0            0        0         0
## 322        0         0            0        1         1
## 323        0         0            0        1         1
## 324        0         0            0        0         1
## 325        0         0            0        1         1
## 326        0         0            0        0         0
## 327        0         0            0        0         1
## 328        0         0            0        1         1
## 329        0         0            0        1         1
## 330        0         0            0       NA         1
## 331        0         0            0        0         1
## 332        0         0            0        1         1
## 333        0         0            0        1         1
## 334        0         0            0       NA         1
## 335        0         0            0        0         1
## 336        0         0            0        1         1
## 337        0         0            0        1         1
## 338        0         0            0        1         1
## 339        0         0            0        1         1
## 340        0         0            0        1         1
## 341        2         0            2        0         0
## 342        4         1            3        1         1
## 343        2        NA           NA        0         1
## 344        2         1            1        0         0
## 345        1        NA           NA        1         1
## 346        1        NA           NA        1         1
## 347        1        NA           NA        1         1
## 348        1        NA           NA        0         1
## 349        1         0            1        0         0
## 350        4         1            3        0         0
## 351        1         1            0        0         1
## 352        3         2           NA        1         1
## 353        1        NA           NA        1         1
## 354        1         1            0        1         1
## 355        1         0            1        1         1
## 356        1         0            1        1         1
## 357        1         0            1        1         1
## 358        2         1           NA        1         1
## 359        1         1            0        1         1
## 360        2        NA           NA        1         0
## 361        1         1            0        0         1
## 362        1         1            0        1         1
## 363        2         0            2        1         1
## 364        1         0            1        1         1
## 365        0         0            0        0         1
## 366        0         0            0        0         0
## 367        0         0            0        1         1
## 368        0         0            0        1         1
## 369        0         0            0        1         1
## 370        0         0            0        1         1
## 371        0         0            0       NA         1
## 372        0         0            0       NA         1
## 373        0         0            0       NA         1
## 374        0         0            0        1         1
## 375        0         0            0        1         1
## 376        0         0            0        1         1
## 377        0         0            0        0         1
## 378        0         0            0        1         1
## 379        0         0            0        1         1
## 380        0         0            0        0         1
## 381        0         0            0        1         1
## 382        0         0            0        1         1
## 383        0         0            0        1         1
## 384        0         0            0        0         1
## 385        0         0            0        1         1
## 386        0         0            0        1         1
## 387        0         0            0        1         1
## 388        0         0            0        1         1
## 389        0         0            0        1         1
## 390        0         0            0        0         1
## 391        0         0            0        0         1
## 392        0         0            0        0         1
## 393        0         0            0        1         1
## 394        0         0            0        1         1
## 395        0         0            0        1         1
## 396        0         0            0        1         1
## 397        0         0            0        1         1
## 398        0         0            0        1         1
## 399        0         0            0        1         1
## 400        0         0            0        1         1
## 401        0         0            0        0         1
## 402        0         0            0        0         1
## 403        0         0            0        1         1
## 404        0         0            0        1         1
## 405        0         0            0        1         1
## 406        0         0            0        1         1
## 407        0         0            0        1         1
## 408        0         0            0        1         1
## 409        0         0            0        1         1
## 410        0         0            0        1         1
## 411        0         0            0        1         1
## 412        0         0            0        1         1
## 413        0         0            0        1         1
## 414        0         0            0        1         1
## 415        0         0            0        0         1
## 416        0         0            0        1         1
## 417        0         0            0        1         1
## 418        2        NA           NA        1         1
## 419        1        NA           NA       NA         1
## 420        1         1            0        1         1
## 421        1        NA            1       NA         1
## 422        1        NA            1       NA         1
## 423        1        NA            1       NA         1
## 424        2        NA            2       NA         1
## 425        1        NA            1       NA         0
## 426        1        NA            1       NA         0
## 427        0         0            0        0         1
## 428        0         0            0       NA         1
## 429        0         0            0       NA         1
## 430        0         0            0       NA         1
## 431        0         0            0       NA         1
## 432        0         0            0        1         1
## 433        0         0            0       NA         1
## 434        0         0            0       NA         1
## 435        0         0            0       NA         1
## 436        0         0            0       NA         1
## 437        0         0            0       NA         1
## 438        0         0            0       NA         1
## 439        0         0            0       NA         1
## 440        0         0            0       NA         1
## 441        0         0            0       NA         1
## 442        0         0            0       NA         1
## 443        0         0            0       NA         1
## 444        0         0            0       NA         1
## 445        0         0            0       NA         0
## 446        0         0            0       NA         1
## 447        0         0            0       NA         1
## 448        0         0            0       NA         1
## 449        0         0            0       NA         1
## 450        0         0            0        1         1
## 451        0         0            0       NA         1
## 452        0         0            0        1         1
## 453        0         0            0       NA         0
## 454        0         0            0        1         1
## 455        0         0            0       NA         1
## 456        0         0            0        1         1
## 457        0         0            0        0         1
## 458        0         0            0        1         1
## 459        0         0            0       NA         1
## 460        0         0            0       NA         1
## 461        0         0            0       NA         1
## 462        0         0            0       NA         1
## 463        0         0            0       NA         1
## 464        0         0            0       NA         1
## 465        0         0            0       NA         1
## 466        4         1            3        0         0
## 467        3         0            3        1         1
## 468        3         3            0        0         0
## 469        3         0            3        1         1
## 470        3         0            3        1         1
## 471        2         1            1        0         0
## 472        2         0            2        1         1
## 473        2         0            2        1         1
## 474        2         1            1        1         1
## 475        2         0            2        1         1
## 476        2         0            2        1         1
## 477        2         1            1        0         0
## 478        2         0            2        1         1
## 479        1         0            1        0         0
## 480        1         1            0        0         0
## 481        1         0            1        1         1
## 482        1         1            0        0         0
## 483        1         0            1        0         1
## 484        1         1            0        1         1
## 485        1         0            1        1         1
## 486        1         0            1        1         1
## 487        1         0            1        1         1
## 488        1         0            1        1         1
## 489        1         0            1        1         1
## 490        1         1            0        1         1
## 491        1         0            1        1         1
## 492        1         1            0        1         1
## 493        1        NA           NA        0         1
## 494        3         0            3        1         1
## 495        2         1            1        1         1
## 496        2         0            2        1         1
## 497        1         0            1        1         1
## 498        1         0            1        1         1
## 499        1         0            1        1         1
## 500        1         0            1        0         1
## 501        1         1            0        1         1
## 502        1         0            1        1         1
## 503        1         0            1        1         1
## 504        1         0            1        1         1
## 505        1         1            0       NA         0
## 506        1         0            1        1         1
## 507        1         0            1        0         1
## 508        1         0            1        1         1
## 509        1         0            1        1         1
## 510        1         0            1        1         1
## 511        1         0            1        1         1
## 512        1         0            1        1         1
## 513        1         0            1        1         1
## 514        1         1            0        1         1
## 515        0         0            0       NA         1
## 516        0         0            0       NA         0
## 517        0         0            0        0         0
## 518        0         0            0        1         0
## 519        0         0            0        1         1
## 520        0         0            0        1         1
## 521        0         0            0        1         1
## 522        0         0            0        1         1
## 523        0         0            0        1         1
## 524        0         0            0        1         1
## 525        0         0            0        1         1
## 526        0         0            0        1         1
## 527        0         0            0        1         1
## 528        0         0            0        1         1
## 529        0         0            0        1         1
## 530        0         0            0        1         1
## 531        0         0            0        1         1
## 532        0         0            0        1         1
## 533        0         0            0        1         1
## 534        0         0            0        0         0
## 535        0         0            0        1         1
## 536        0         0            0        0         1
## 537        0         0            0        1         1
## 538        0         0            0        1         1
## 539        0         0            0        1         1
## 540        0         0            0        0         0
## 541        0         0            0        1         1
## 542        0         0            0        1         1
## 543        0         0            0        1         1
## 544        0         0            0        1         1
## 545        0         0            0        1         1
## 546        0         0            0        1         1
## 547        0         0            0        1         1
## 548        0         0            0        1         1
## 549        0         0            0        0         0
## 550        0         0            0        1         1
## 551        0         0            0        1         1
## 552        0         0            0        1         1
## 553        0         0            0        1         1
## 554        0         0            0        1         1
## 555        0         0            0        1         1
## 556        0         0            0        1         1
## 557        0         0            0        1         1
## 558        0         0            0        0         1
## 559        0         0            0        1         1
## 560        0         0            0        1         1
## 561        0         0            0        1         1
## 562        0         0            0        1         1
## 563        0         0            0        1         1
## 564        0         0            0        1         1
## 565        0         0            0        1         1
## 566        0         0            0        1         1
## 567        0         0            0        1         1
## 568        0         0            0        1         1
## 569        0         0            0        1         1
## 570        0         0            0        1         1
## 571        0         0            0        1         1
## 572        0         0            0        1         1
## 573        0         0            0        1         1
## 574        0         0            0        1         1
## 575        0         0            0        1         1
## 576        0         0            0        1         1
## 577        0         0            0        1         1
## 578        0         0            0        1         1
## 579        0         0            0        1         1
## 580        0         0            0        1         1
## 581        0         0            0        0         0
## 582        0         0            0        0        NA
## 583        0         0            0        1         1
## 584        0         0            0        0         0
## 585        0         0            0        1         1
## 586        0         0            0        1         1
## 587        0         0            0        1         1
## 588        0         0            0        1         1
## 589        0         0            0        1         1
## 590        0         0            0        1         1
## 591        0         0            0        1         1
## 592        0         0            0        1         1
## 593        0         0            0        1         1
## 594        0         0            0        1         1
## 595        0         0            0        1         1
## 596        0         0            0        0         0
## 597        0         0            0       NA         1
## 598        0         0            0        1         1
## 599        0         0            0        1         1
## 600        0         0            0        1         1
## 601        0         0            0       NA         0
## 602        0         0            0        1         1
## 603        0         0            0        1         1
## 604        0         0            0        1         1
## 605        0         0            0        1         1
## 606        0         0            0        1         1
## 607        0         0            0        1         1
## 608        0         0            0        1         1
## 609        0         0            0        1         1
## 610        0         0            0        1         1
## 611        0         0            0        1         1
## 612        0         0            0        1         1
## 613        0         0            0        1         1
## 614        0         0            0        1         1
## 615        0         0            0        1         1
## 616        0         0            0        0         0
## 617        0         0            0        1         1
## 618        0         0            0        1         1
## 619        0         0            0        1         1
## 620        0         0            0        1         1
## 621        0         0            0        1         1
## 622        0         0            0        1         1
## 623        0         0            0        1         1
## 624        0         0            0        1         1
## 625        0         0            0        1         1
## 626        0         0            0        1         1
## 627        0         0            0        1         1
## 628        0         0            0        1         1
## 629        0         0            0        0         0
## 630        0         0            0        0         0
## 631        0         0            0        1         1
## 632        0         0            0        1         1
## 633        0         0            0        1         1
## 634        0         0            0        1         1
## 635        0         0            0        0         1
## 636        0         0            0        1         1
## 637        0         0            0        1         1
## 638        0         0            0        1         1
## 639        0         0            0        1         1
## 640        0         0            0        0         1
## 641        0         0            0        1         1
## 642        0         0            0        1         1
## 643        0         0            0        1         1
## 644        0         0            0        1         1
## 645        0         0            0        1         1
## 646        0         0            0        1         1
## 647        0         0            0        1         1
## 648        0         0            0        1         1
## 649        0         0            0        1         1
## 650        0         0            0        1         1
## 651        0         0            0        1         1
## 652        0         0            0        0        NA
## 653        0         0            0        1         1
## 654        0         0            0        1         1
## 655        0         0            0        1         1
## 656        0         0            0        1         1
## 657        0         0            0        1         1
## 658        0         0            0        1         1
## 659        0         0            0        0         0
## 660        0         0            0        1         1
## 661        0         0            0        1         1
## 662        0         0            0        0         0
## 663        0         0            0        1         1
## 664        0         0            0        1         1
## 665        0         0            0        1         1
## 666        0         0            0        1         1
## 667        0         0            0        1         1
## 668        0         0            0        0         1
## 669        0         0            0        1         1
## 670        0         0            0        1         1
## 671        0         0            0        1         1
## 672        0         0            0        1         1
## 673        0         0            0        1         1
## 674        0         0            0        1         1
## 675        0         0            0        1         1
## 676        0         0            0        1         1
## 677        0         0            0        1         1
## 678        0         0            0        1         1
## 679        0         0            0       NA         1
## 680        0         0            0        0         0
## 681        0         0            0        1         1
## 682        0         0            0        1         1
## 683        0         0            0        1         1
## 684        0         0            0        1         1
## 685        0         0            0        1         1
## 686        0         0            0        1         1
## 687        0         0            0        1         1
## 688        0         0            0        0         0
## 689        0         0            0       NA        NA
## 690        0         0            0        1         1
## 691        0         0            0        0         0
## 692        0         0            0        1         1
## 693        0         0            0        1         1
## 694        0         0            0        1         1
## 695        0         0            0        1         1
## 696        0         0            0        1         1
## 697        0         0            0        1         1
## 698        0         0            0        1         1
## 699        0         0            0        0         1
## 700        0         0            0        0         1
## 701        0         0            0        1         1
## 702        0         0            0        1         1
## 703        0         0            0        1         1
## 704        0         0            0        1         1
## 705        0         0            0        1         1
## 706        0         0            0        1         1
## 707        0         0            0        1         1
## 708        0         0            0        1         1
## 709        0         0            0        1         1
## 710        0         0            0        1         1
## 711        0         0            0        0         1
## 712        0         0            0        1         1
## 713        0         0            0        1         1
## 714        4         0            4        1         1
## 715        2         0            2        1         1
## 716        2         0            2        1         1
## 717       24         2           22        0         0
## 718        5         2            3        0         1
## 719        2         1            1        1         1
## 720        1         0            1        1         1
## 721        2         0            2        1         1
## 722        3         0            3        1         1
## 723       14         2           12        0         0
## 724        1         0            1        1         1
## 725        1         0            1        1         1
## 726        1         1            0        1         1
## 727        1         1            0        1         1
## 728        1         0            1        0         1
## 729        1         1            0        0         0
## 730        2         2            0        0         0
## 731        1         1            0        0         0
## 732        5         1            4        0         0
## 733        8         0            8        1         1
## 734       12         3            9        0         0
## 735        2         2            0        0         0
## 736        2         0            2        1         1
## 737       12         0           12        0         0
## 738        1         0            1        1         1
## 739        1         1            0        1         1
## 740        1         0            1        1         1
## 741        1         1            0        1         1
## 742        2         2            0        0         0
## 743        4         0            4        0         0
## 744        4         1            3        0         0
## 745       10         2            8        1         1
## 746        2         0            2        0         1
## 747        1         1            0        1         1
## 748        0         0            0        1         1
## 749        0         0            0        1         1
## 750        0         0            0        1         1
## 751        0         0            0        1         1
## 752        0         0            0        1         1
## 753        0         0            0        1         1
## 754        0         0            0        1         1
## 755        0         0            0        1         1
## 756        0         0            0        1         1
## 757        0         0            0        0         0
## 758        0         0            0       NA         1
## 759        0         0            0        1         1
## 760        0         0            0        1         1
## 761        0         0            0        1         1
## 762        0         0            0        1         1
## 763        0         0            0        1         1
## 764        0         0            0        1         1
## 765        0         0            0        1         1
## 766        0         0            0        1         1
## 767        0         0            0        0         1
## 768        0         0            0        1         1
## 769        0         0            0        1         1
## 770        0         0            0        0         0
## 771        0         0            0        0         0
## 772        0         0            0        1         1
## 773        0         0            0        0         1
## 774        0         0            0        0         0
## 775        0         0            0        1         1
## 776        0         0            0        0         1
## 777        0         0            0        0         1
## 778        0         0            0        0         1
## 779        0         0            0        0         1
## 780        0         0            0        0         0
## 781        0         0            0        1         1
## 782        0         0            0        0         1
## 783        0         0            0        1         1
## 784        0         0            0        1         1
## 785        0         0            0        0         0
## 786        0         0            0        0         1
## 787        0         0            0        1         1
## 788        0         0            0        1         1
## 789        0         0            0        1         1
## 790        0         0            0        1         1
## 791        0         0            0        1         1
## 792        0         0            0        1         1
## 793        0         0            0        0         1
## 794        0         0            0        1         1
## 795        0         0            0        1         1
## 796        0         0            0        1         1
## 797        0         0            0        1         1
## 798        0         0            0        0         1
## 799        0         0            0        1         1
## 800        0         0            0        1         1
## 801        0         0            0        1         1
## 802        0         0            0        1         1
## 803        0         0            0        1         1
## 804        0         0            0       NA        NA
## 805        0         0            0        1         1
## 806        0         0            0        1         1
## 807        0         0            0        1         1
## 808        0         0            0        1         1
## 809        0         0            0        0         1
## 810        0         0            0        1         1
## 811        0         0            0        1         1
## 812        0         0            0        1         1
## 813        0         0            0        1         1
## 814        0         0            0        1         1
## 815        0         0            0        1         1
## 816        0         0            0        1         1
## 817        0         0            0        1         1
## 818        0         0            0        1         1
## 819        0         0            0        1         1
## 820        0         0            0        1         1
## 821        0         0            0        1         1
## 822        0         0            0        0         1
## 823        0         0            0        1         1
## 824        0         0            0        1         1
## 825        0         0            0        1         1
## 826        0         0            0        1         1
## 827        0         0            0        1         1
## 828        0         0            0        0         0
## 829        0         0            0        1         1
## 830        0         0            0        1         1
## 831        0         0            0        1         1
## 832        0         0            0        1         1
## 833        0         0            0        1         1
## 834        0         0            0        1         1
## 835        0         0            0        0         1
## 836        0         0            0        1         1
## 837        0         0            0        1         1
## 838        0         0            0        1         1
## 839        0         0            0        1         1
## 840        0         0            0        0         1
## 841        0         0            0       NA         1
## 842        0         0            0        1         1
## 843        0         0            0        1         1
## 844        0         0            0        1         1
## 845        0         0            0        1         1
## 846        0         0            0        1         1
## 847        0         0            0        1         1
## 848        0         0            0        1         1
## 849        0         0            0        1         1
## 850        0         0            0        1         1
## 851        0         0            0        1         1
## 852        0         0            0        0         1
## 853        0         0            0        0         1
## 854        0         0            0       NA         1
## 855        0         0            0       NA         0
## 856        0         0            0        1         1
## 857        0         0            0        1         1
## 858        0         0            0        1         1
## 859        0         0            0        1         1
## 860        0         0            0        1         1
## 861        0         0            0        1         1
## 862        0         0            0       NA        NA
## 863        0         0            0        1         1
## 864        0         0            0       NA         1
```


```r
wolves$park <- as.factor(wolves$park)
```


```r
levels(wolves$park)
```

```
## [1] "DENA" "GNTP" "VNP"  "YNP"  "YUCH"
```


Problem 4. (4 points) Which park has the largest number of wolf packs?

```r
wolves %>% 
  group_by(park) %>% 
  summarise(total=n())  %>% 
  arrange(desc(total)) 
```

```
## # A tibble: 5 × 2
##   park  total
##   <fct> <int>
## 1 DENA    340
## 2 YNP     248
## 3 YUCH    151
## 4 GNTP     77
## 5 VNP      48
```
It looks like Denali National Park and Preserve has the largest number of wolf packs.


Problem 5. (4 points) Which park has the highest total number of human-caused mortalities `mort_all`?

```r
wolves %>% 
  group_by(park, mort_all) %>% 
  summarise(total=n()) %>% 
  arrange(desc(mort_all))
```

```
## `summarise()` has grouped output by 'park'. You can override using the
## `.groups` argument.
```

```
## # A tibble: 28 × 3
## # Groups:   park [5]
##    park  mort_all total
##    <fct>    <int> <int>
##  1 YUCH        24     1
##  2 YUCH        14     1
##  3 YUCH        12     2
##  4 YUCH        10     1
##  5 YUCH         8     1
##  6 YUCH         5     2
##  7 DENA         4     1
##  8 GNTP         4     2
##  9 YNP          4     1
## 10 YUCH         4     3
## # ℹ 18 more rows
```
Yukon-Charley Rivers National Preserve appears to have the highest total number of human-caused mortalities.


```r
#group_by(mort_all) %>% 
  #arrange(desc(mort_all))
```


The wolves in [Yellowstone National Park](https://www.nps.gov/yell/learn/nature/wolf-restoration.htm) are an incredible conservation success story. Let's focus our attention on this park.  

Problem 6. (2 points) Create a new object "ynp" that only includes the data from Yellowstone National Park.  

```r
ynp <- wolves %>% 
  filter(park=="YNP")
ynp
```

```
##     park biolyr            pack packcode packsize_aug mort_yn mort_all
## 1    YNP   2009      cottonwood       23           12       1        4
## 2    YNP   2016           8mile       11           20       1        3
## 3    YNP   2017          canyon       20            2       1        3
## 4    YNP   2012        junction       33           11       1        3
## 5    YNP   2016        junction       33           15       1        3
## 6    YNP   2011       642Fgroup        5           10       1        2
## 7    YNP   2012           8mile       11           15       1        2
## 8    YNP   2012         bechler       16           12       1        2
## 9    YNP   2012           lamar       34           13       1        2
## 10   YNP   2019         phantom       41           18       1        2
## 11   YNP   2020         phantom       41           16       1        2
## 12   YNP   2017        prospect       42           10       1        2
## 13   YNP   2020          wapiti       51           23       1        2
## 14   YNP   2018      1118Fgroup        2            5       1        1
## 15   YNP   2020      1155Mgroup        3            3       1        1
## 16   YNP   2014           8mile       11           27       1        1
## 17   YNP   2017       963Fgroup       13            4       1        1
## 18   YNP   2012       blacktail       18           10       1        1
## 19   YNP   2014          cougar       24           18       1        1
## 20   YNP   2017          cougar       24            7       1        1
## 21   YNP   2014        junction       33           13       1        1
## 22   YNP   2018           lamar       34            8       1        1
## 23   YNP   2012         mollies       38            6       1        1
## 24   YNP   2019         mollies       38           12       1        1
## 25   YNP   2018         phantom       41            5       1        1
## 26   YNP   2014        prospect       42            7       1        1
## 27   YNP   2012           snake       46            4       1        1
## 28   YNP   2017           snake       46           13       1        1
## 29   YNP   2007       yelldelta       52           22       1        3
## 30   YNP   1997           druid       26            5       1        2
## 31   YNP   2019        junction       33           21       1        2
## 32   YNP   2003           agate       15           13       1        1
## 33   YNP   2009       blacktail       18           13       1        1
## 34   YNP   2011       blacktail       18           16       1        1
## 35   YNP   2013          canyon       20           10       1        1
## 36   YNP   2015          cougar       24            7       1        1
## 37   YNP   2000           druid       26           27       1        1
## 38   YNP   2004     gibbon/mary       29            9       1        1
## 39   YNP   2009     gibbon/mary       29           26       1        1
## 40   YNP   2010        grayling       30            3       1        1
## 41   YNP   2007         mollies       38           15       1        1
## 42   YNP   2011         mollies       38           23       1        1
## 43   YNP   2013         mollies       38            8       1        1
## 44   YNP   1996        nezperce       39            9       1        1
## 45   YNP   1997        nezperce       39            7       1        1
## 46   YNP   2000        nezperce       39           22       1        1
## 47   YNP   1995            rose       44            9       1        1
## 48   YNP   1998            rose       44           23       1        1
## 49   YNP   2007          slough       45           22       1        1
## 50   YNP   2009       682Mgroup        6           NA       0        0
## 51   YNP   2010       682Mgroup        6            0       0        0
## 52   YNP   2009       694Fgroup        8            0       0        0
## 53   YNP   2013       755Mgroup        9            2       0        0
## 54   YNP   2011           8mile       11           17       0        0
## 55   YNP   2013           8mile       11           19       0        0
## 56   YNP   2015           8mile       11           13       0        0
## 57   YNP   2017           8mile       11           18       0        0
## 58   YNP   2018           8mile       11           13       0        0
## 59   YNP   2020           8mile       11           22       0        0
## 60   YNP   2002           agate       15           10       0        0
## 61   YNP   2004           agate       15           13       0        0
## 62   YNP   2005           agate       15           14       0        0
## 63   YNP   2006           agate       15           13       0        0
## 64   YNP   2007           agate       15           20       0        0
## 65   YNP   2008           agate       15           14       0        0
## 66   YNP   2009           agate       15            4       0        0
## 67   YNP   2010           agate       15            9       0        0
## 68   YNP   2011           agate       15           13       0        0
## 69   YNP   2012           agate       15            0       0        0
## 70   YNP   2002         bechler       16            4       0        0
## 71   YNP   2004         biscuit       17           11       0        0
## 72   YNP   2008       blacktail       18           NA       0        0
## 73   YNP   2010       blacktail       18           17       0        0
## 74   YNP   2002     buffalofork       19            4       0        0
## 75   YNP   2003     buffalofork       19            3       0        0
## 76   YNP   2008          canyon       20            6       0        0
## 77   YNP   2009          canyon       20            4       0        0
## 78   YNP   2010          canyon       20            6       0        0
## 79   YNP   2011          canyon       20            8       0        0
## 80   YNP   2012          canyon       20            9       0        0
## 81   YNP   2014          canyon       20            5       0        0
## 82   YNP   2015          canyon       20            6       0        0
## 83   YNP   2016          canyon       20            6       0        0
## 84   YNP   2021       carnelian       21            0       0        0
## 85   YNP   2016        cinnabar       22            3       0        0
## 86   YNP   2008      cottonwood       23            4       0        0
## 87   YNP   2001          cougar       24            6       0        0
## 88   YNP   2002          cougar       24           11       0        0
## 89   YNP   2003          cougar       24           14       0        0
## 90   YNP   2004          cougar       24           15       0        0
## 91   YNP   2005          cougar       24           14       0        0
## 92   YNP   2006          cougar       24            4       0        0
## 93   YNP   2007          cougar       24            7       0        0
## 94   YNP   2008          cougar       24            5       0        0
## 95   YNP   2009          cougar       24            6       0        0
## 96   YNP   2010          cougar       24            4       0        0
## 97   YNP   2011          cougar       24            7       0        0
## 98   YNP   2012          cougar       24           11       0        0
## 99   YNP   2013          cougar       24           14       0        0
## 100  YNP   2016          cougar       24            8       0        0
## 101  YNP   2018          cougar       24           10       0        0
## 102  YNP   2019          cougar       24            6       0        0
## 103  YNP   2020          cougar       24            6       0        0
## 104  YNP   2017         crevice       25           NA       0        0
## 105  YNP   1996           druid       26            5       0        0
## 106  YNP   1998           druid       26            8       0        0
## 107  YNP   1999           druid       26            9       0        0
## 108  YNP   2001           druid       26           37       0        0
## 109  YNP   2002           druid       26           16       0        0
## 110  YNP   2003           druid       26           18       0        0
## 111  YNP   2004           druid       26           13       0        0
## 112  YNP   2005           druid       26            5       0        0
## 113  YNP   2006           druid       26           15       0        0
## 114  YNP   2007           druid       26           18       0        0
## 115  YNP   2008           druid       26           21       0        0
## 116  YNP   2009           druid       26           12       0        0
## 117  YNP   2010           druid       26            0       0        0
## 118  YNP   2008          everts       27            9       0        0
## 119  YNP   2009          everts       27           12       0        0
## 120  YNP   2002      geode/hell       28            9       0        0
## 121  YNP   2003      geode/hell       28            9       0        0
## 122  YNP   2004      geode/hell       28           12       0        0
## 123  YNP   2005      geode/hell       28            7       0        0
## 124  YNP   2003     gibbon/mary       29           NA       0        0
## 125  YNP   2005     gibbon/mary       29           10       0        0
## 126  YNP   2006     gibbon/mary       29           12       0        0
## 127  YNP   2007     gibbon/mary       29           18       0        0
## 128  YNP   2008     gibbon/mary       29           25       0        0
## 129  YNP   2010     gibbon/mary       29            8       0        0
## 130  YNP   2011     gibbon/mary       29           11       0        0
## 131  YNP   2012     gibbon/mary       29            0       0        0
## 132  YNP   2009        grayling       30            6       0        0
## 133  YNP   2004          hayden       31            4       0        0
## 134  YNP   2005          hayden       31            5       0        0
## 135  YNP   2006          hayden       31            7       0        0
## 136  YNP   2007          hayden       31            9       0        0
## 137  YNP   2019           heart       32            2       0        0
## 138  YNP   2020           heart       32            7       0        0
## 139  YNP   2015        junction       33           19       0        0
## 140  YNP   2017        junction       33            8       0        0
## 141  YNP   2018        junction       33           11       0        0
## 142  YNP   2020        junction       33           35       0        0
## 143  YNP   2010           lamar       34            7       0        0
## 144  YNP   2011           lamar       34           11       0        0
## 145  YNP   2014           lamar       34            8       0        0
## 146  YNP   2015           lamar       34           12       0        0
## 147  YNP   2016           lamar       34            4       0        0
## 148  YNP   2017           lamar       34            3       0        0
## 149  YNP   2008            lava       35            5       0        0
## 150  YNP   2009            lava       35            3       0        0
## 151  YNP   2010            lava       35            1       0        0
## 152  YNP   1996         leopold       36            5       0        0
## 153  YNP   1997         leopold       36           10       0        0
## 154  YNP   1998         leopold       36           12       0        0
## 155  YNP   1999         leopold       36           11       0        0
## 156  YNP   2000         leopold       36           15       0        0
## 157  YNP   2001         leopold       36           14       0        0
## 158  YNP   2002         leopold       36           16       0        0
## 159  YNP   2003         leopold       36           21       0        0
## 160  YNP   2004         leopold       36           28       0        0
## 161  YNP   2005         leopold       36           26       0        0
## 162  YNP   2006         leopold       36           20       0        0
## 163  YNP   2007         leopold       36           19       0        0
## 164  YNP   2008         leopold       36            7       0        0
## 165  YNP   1996        lonestar       37            0       0        0
## 166  YNP   1995         mollies       38            5       0        0
## 167  YNP   1996         mollies       38            2       0        0
## 168  YNP   1997         mollies       38            8       0        0
## 169  YNP   1998         mollies       38           16       0        0
## 170  YNP   1999         mollies       38           15       0        0
## 171  YNP   2000         mollies       38            5       0        0
## 172  YNP   2001         mollies       38           10       0        0
## 173  YNP   2002         mollies       38           13       0        0
## 174  YNP   2003         mollies       38            8       0        0
## 175  YNP   2004         mollies       38            9       0        0
## 176  YNP   2005         mollies       38            7       0        0
## 177  YNP   2006         mollies       38           11       0        0
## 178  YNP   2008         mollies       38           15       0        0
## 179  YNP   2009         mollies       38           17       0        0
## 180  YNP   2010         mollies       38           17       0        0
## 181  YNP   2014         mollies       38           12       0        0
## 182  YNP   2015         mollies       38           17       0        0
## 183  YNP   2016         mollies       38           18       0        0
## 184  YNP   2017         mollies       38           15       0        0
## 185  YNP   2018         mollies       38           10       0        0
## 186  YNP   2020         mollies       38            8       0        0
## 187  YNP   2006        nezperce       39            0       0        0
## 188  YNP   1998        nezperce       39            7       0        0
## 189  YNP   1999        nezperce       39           12       0        0
## 190  YNP   2001        nezperce       39           19       0        0
## 191  YNP   2002        nezperce       39           18       0        0
## 192  YNP   2003        nezperce       39           18       0        0
## 193  YNP   2004        nezperce       39           14       0        0
## 194  YNP   2005        nezperce       39           11       0        0
## 195  YNP   2006           oxbow       40           12       0        0
## 196  YNP   2007           oxbow       40           24       0        0
## 197  YNP   2008           oxbow       40           20       0        0
## 198  YNP   2015        prospect       42           13       0        0
## 199  YNP   2016        prospect       42           12       0        0
## 200  YNP   2007        quadrant       43           NA       0        0
## 201  YNP   2008        quadrant       43            4       0        0
## 202  YNP   2009        quadrant       43            7       0        0
## 203  YNP   2010        quadrant       43            7       0        0
## 204  YNP   1996            rose       44           11       0        0
## 205  YNP   1997            rose       44           16       0        0
## 206  YNP   1999            rose       44           22       0        0
## 207  YNP   2000            rose       44           21       0        0
## 208  YNP   2001            rose       44           10       0        0
## 209  YNP   2002            rose       44           11       0        0
## 210  YNP   2003          slough       45           10       0        0
## 211  YNP   2004          slough       45           21       0        0
## 212  YNP   2005          slough       45           15       0        0
## 213  YNP   2006          slough       45            9       0        0
## 214  YNP   2004 specimen/silver       47            5       0        0
## 215  YNP   2010 specimen/silver       47            8       0        0
## 216  YNP   2000            swan       48            7       0        0
## 217  YNP   2001            swan       48            9       0        0
## 218  YNP   2002            swan       48           18       0        0
## 219  YNP   2003            swan       48           20       0        0
## 220  YNP   2004            swan       48           10       0        0
## 221  YNP   2005            swan       48            3       0        0
## 222  YNP   1996       thorofare       49            2       0        0
## 223  YNP   1997       thorofare       49            8       0        0
## 224  YNP   1998       thorofare       49            0       0        0
## 225  YNP   2001           tower       50            4       0        0
## 226  YNP   2002           tower       50            2       0        0
## 227  YNP   2014          wapiti       51            2       0        0
## 228  YNP   2016          wapiti       51            9       0        0
## 229  YNP   2017          wapiti       51           21       0        0
## 230  YNP   2018          wapiti       51           18       0        0
## 231  YNP   2019          wapiti       51           19       0        0
## 232  YNP   1995       yelldelta       52            6       0        0
## 233  YNP   1996       yelldelta       52            5       0        0
## 234  YNP   1997       yelldelta       52            8       0        0
## 235  YNP   1998       yelldelta       52            8       0        0
## 236  YNP   1999       yelldelta       52            6       0        0
## 237  YNP   2000       yelldelta       52           15       0        0
## 238  YNP   2001       yelldelta       52           18       0        0
## 239  YNP   2002       yelldelta       52           17       0        0
## 240  YNP   2003       yelldelta       52           17       0        0
## 241  YNP   2004       yelldelta       52           19       0        0
## 242  YNP   2005       yelldelta       52           19       0        0
## 243  YNP   2006       yelldelta       52           15       0        0
## 244  YNP   2009       yelldelta       52            4       0        0
## 245  YNP   2010       yelldelta       52            9       0        0
## 246  YNP   2011       yelldelta       52           13       0        0
## 247  YNP   2012       yelldelta       52           11       0        0
## 248  YNP   2013       yelldelta       52           17       0        0
##     mort_lead mort_nonlead reprody1 persisty1
## 1           1            3        0         0
## 2           0            3        1         1
## 3           3            0        0         0
## 4           0            3        1         1
## 5           0            3        1         1
## 6           1            1        0         0
## 7           0            2        1         1
## 8           0            2        1         1
## 9           1            1        1         1
## 10          0            2        1         1
## 11          0            2        1         1
## 12          1            1        0         0
## 13          0            2        1         1
## 14          0            1        0         0
## 15          1            0        0         0
## 16          0            1        1         1
## 17          1            0        0         0
## 18          0            1        0         1
## 19          1            0        1         1
## 20          0            1        1         1
## 21          0            1        1         1
## 22          0            1        1         1
## 23          0            1        1         1
## 24          0            1        1         1
## 25          1            0        1         1
## 26          0            1        1         1
## 27          1            0        1         1
## 28         NA           NA        0         1
## 29          0            3        1         1
## 30          1            1        1         1
## 31          0            2        1         1
## 32          0            1        1         1
## 33          0            1        1         1
## 34          0            1        1         1
## 35          0            1        0         1
## 36          1            0        1         1
## 37          0            1        1         1
## 38          0            1        1         1
## 39          0            1        1         1
## 40          1            0       NA         0
## 41          0            1        1         1
## 42          0            1        0         1
## 43          0            1        1         1
## 44          0            1        1         1
## 45          0            1        1         1
## 46          0            1        1         1
## 47          0            1        1         1
## 48          0            1        1         1
## 49          1            0        1         1
## 50          0            0       NA         1
## 51          0            0       NA         0
## 52          0            0        0         0
## 53          0            0        1         0
## 54          0            0        1         1
## 55          0            0        1         1
## 56          0            0        1         1
## 57          0            0        1         1
## 58          0            0        1         1
## 59          0            0        1         1
## 60          0            0        1         1
## 61          0            0        1         1
## 62          0            0        1         1
## 63          0            0        1         1
## 64          0            0        1         1
## 65          0            0        1         1
## 66          0            0        1         1
## 67          0            0        1         1
## 68          0            0        1         1
## 69          0            0        0         0
## 70          0            0        1         1
## 71          0            0        0         1
## 72          0            0        1         1
## 73          0            0        1         1
## 74          0            0        1         1
## 75          0            0        0         0
## 76          0            0        1         1
## 77          0            0        1         1
## 78          0            0        1         1
## 79          0            0        1         1
## 80          0            0        1         1
## 81          0            0        1         1
## 82          0            0        1         1
## 83          0            0        1         1
## 84          0            0        0         0
## 85          0            0        1         1
## 86          0            0        1         1
## 87          0            0        1         1
## 88          0            0        1         1
## 89          0            0        1         1
## 90          0            0        1         1
## 91          0            0        1         1
## 92          0            0        1         1
## 93          0            0        0         1
## 94          0            0        1         1
## 95          0            0        1         1
## 96          0            0        1         1
## 97          0            0        1         1
## 98          0            0        1         1
## 99          0            0        1         1
## 100         0            0        1         1
## 101         0            0        1         1
## 102         0            0        1         1
## 103         0            0        1         1
## 104         0            0        1         1
## 105         0            0        1         1
## 106         0            0        1         1
## 107         0            0        1         1
## 108         0            0        1         1
## 109         0            0        1         1
## 110         0            0        1         1
## 111         0            0        1         1
## 112         0            0        1         1
## 113         0            0        1         1
## 114         0            0        1         1
## 115         0            0        1         1
## 116         0            0        0         0
## 117         0            0        0        NA
## 118         0            0        1         1
## 119         0            0        0         0
## 120         0            0        1         1
## 121         0            0        1         1
## 122         0            0        1         1
## 123         0            0        1         1
## 124         0            0        1         1
## 125         0            0        1         1
## 126         0            0        1         1
## 127         0            0        1         1
## 128         0            0        1         1
## 129         0            0        1         1
## 130         0            0        1         1
## 131         0            0        0         0
## 132         0            0       NA         1
## 133         0            0        1         1
## 134         0            0        1         1
## 135         0            0        1         1
## 136         0            0       NA         0
## 137         0            0        1         1
## 138         0            0        1         1
## 139         0            0        1         1
## 140         0            0        1         1
## 141         0            0        1         1
## 142         0            0        1         1
## 143         0            0        1         1
## 144         0            0        1         1
## 145         0            0        1         1
## 146         0            0        1         1
## 147         0            0        1         1
## 148         0            0        1         1
## 149         0            0        1         1
## 150         0            0        1         1
## 151         0            0        0         0
## 152         0            0        1         1
## 153         0            0        1         1
## 154         0            0        1         1
## 155         0            0        1         1
## 156         0            0        1         1
## 157         0            0        1         1
## 158         0            0        1         1
## 159         0            0        1         1
## 160         0            0        1         1
## 161         0            0        1         1
## 162         0            0        1         1
## 163         0            0        1         1
## 164         0            0        0         0
## 165         0            0        0         0
## 166         0            0        1         1
## 167         0            0        1         1
## 168         0            0        1         1
## 169         0            0        1         1
## 170         0            0        0         1
## 171         0            0        1         1
## 172         0            0        1         1
## 173         0            0        1         1
## 174         0            0        1         1
## 175         0            0        0         1
## 176         0            0        1         1
## 177         0            0        1         1
## 178         0            0        1         1
## 179         0            0        1         1
## 180         0            0        1         1
## 181         0            0        1         1
## 182         0            0        1         1
## 183         0            0        1         1
## 184         0            0        1         1
## 185         0            0        1         1
## 186         0            0        1         1
## 187         0            0        0        NA
## 188         0            0        1         1
## 189         0            0        1         1
## 190         0            0        1         1
## 191         0            0        1         1
## 192         0            0        1         1
## 193         0            0        1         1
## 194         0            0        0         0
## 195         0            0        1         1
## 196         0            0        1         1
## 197         0            0        0         0
## 198         0            0        1         1
## 199         0            0        1         1
## 200         0            0        1         1
## 201         0            0        1         1
## 202         0            0        1         1
## 203         0            0        0         1
## 204         0            0        1         1
## 205         0            0        1         1
## 206         0            0        1         1
## 207         0            0        1         1
## 208         0            0        1         1
## 209         0            0        1         1
## 210         0            0        1         1
## 211         0            0        1         1
## 212         0            0        1         1
## 213         0            0        1         1
## 214         0            0       NA         1
## 215         0            0        0         0
## 216         0            0        1         1
## 217         0            0        1         1
## 218         0            0        1         1
## 219         0            0        1         1
## 220         0            0        1         1
## 221         0            0        1         1
## 222         0            0        1         1
## 223         0            0        0         0
## 224         0            0       NA        NA
## 225         0            0        1         1
## 226         0            0        0         0
## 227         0            0        1         1
## 228         0            0        1         1
## 229         0            0        1         1
## 230         0            0        1         1
## 231         0            0        1         1
## 232         0            0        1         1
## 233         0            0        1         1
## 234         0            0        0         1
## 235         0            0        0         1
## 236         0            0        1         1
## 237         0            0        1         1
## 238         0            0        1         1
## 239         0            0        1         1
## 240         0            0        1         1
## 241         0            0        1         1
## 242         0            0        1         1
## 243         0            0        1         1
## 244         0            0        1         1
## 245         0            0        1         1
## 246         0            0        0         1
## 247         0            0        1         1
## 248         0            0        1         1
```


Problem 7. (3 points) Among the Yellowstone wolf packs, the [Druid Peak Pack](https://www.pbs.org/wnet/nature/in-the-valley-of-the-wolves-the-druid-wolf-pack-story/209/) is one of most famous. What was the average pack size of this pack for the years represented in the data?

```r
ynp %>% 
  filter(pack=="druid") %>% 
  summarise(mean_pack_size=mean(packsize_aug))
```

```
##   mean_pack_size
## 1       13.93333
```


Problem 8. (4 points) Pack dynamics can be hard to predict- even for strong packs like the Druid Peak pack. At which year did the Druid Peak pack have the largest pack size? What do you think happened in 2010?

```r
#ynp %>% 
  #filter(pack=="druid") %>% 
  #summarise(largest_druid_pack=max(packsize_aug)) %>% 
  #group_by(largest_druid_pack)
```
In 2010, I think the Druid Peak pack went extinct.


Problem 9. (5 points) Among the YNP wolf packs, which one has had the highest overall persistence `persisty1` for the years represented in the data? Look this pack up online and tell me what is unique about its behavior- specifically, what prey animals does this pack specialize on? 

```r
#ynp %>% 
  #group_by(pack) %>% 
  #summarise(highest_persistence=max(persisty1)) 
```


Problem 10. (3 points) Perform one analysis or exploration of your choice on the `wolves` data. Your answer needs to include at least two lines of code and not be a summary function. 

Which park has the smallest number of wolf packs?

```r
wolves %>% 
  group_by(park) %>% 
  summarise(total=n())  %>% 
  arrange((total))
```

```
## # A tibble: 5 × 2
##   park  total
##   <fct> <int>
## 1 VNP      48
## 2 GNTP     77
## 3 YUCH    151
## 4 YNP     248
## 5 DENA    340
```
Voyageurs National Park appears to have the smallest number of wolf packs.

