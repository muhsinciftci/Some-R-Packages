---
title: "Package lubridate"
author:
  name: Muhsin Ciftci
  affiliation: Goethe University Frankfurt
date: "25 March 2020"
output:
  html_document:
    highlight: kate # “tango”, “pygments”, “kate”, “haddock”
    number_sections: yes
    theme: flatly
    toc: yes
    toc_depth: 4
    toc_float: yes
    keep_md: true
editor_options: 
  chunk_output_type: console
---



```r
knitr::opts_chunk$set(message = FALSE, warning=FALSE, echo = TRUE, cache = F, dpi=300)
```

# Getting Time Components
This package makes it easier  to work  with dates etc in   `R`. Here how it goes:


```r
library(tidyverse)
library(lubridate)
```


```r
rm(list = ls())
graphics.off()

dt1 <- ymd("2020-03-25") # or today()
# Pick the components (years, months, days) from it
y1 <- year(dt1)
y1
```

```
## [1] 2020
```

```r
m1 <- month(dt1)
m1
```

```
## [1] 3
```

```r
d1 <- day(dt1)
d1
```

```
## [1] 25
```

```r
q1 <- quarter(dt1)
q1
```

```
## [1] 1
```

```r
y_day <- yday(dt1)
y_day # Day of the year
```

```
## [1] 85
```


```r
# Get the  day of the week
wday(dt1)
```

```
## [1] 4
```

```r
# OR
wday(dt1, label = T, week_start = 1) # week_start = 1 for Monday
```

```
## [1] Wed
## Levels: Mon < Tue < Wed < Thu < Fri < Sat < Sun
```

The same operations can be done for the function `now()` as well.


```r
d_this_moment <- now()
d_h <- hour(d_this_moment)
d_h
```

```
## [1] 23
```

```r
d_m <- minute(d_this_moment)
d_m
```

```
## [1] 33
```

```r
d_s <- second(d_this_moment)
d_s
```

```
## [1] 37.3785
```

## Time intervals


```r
d2 <-  interval(ymd("2010-03-20"), ymd("2020-01-01"))

# Find the start and end of the date
int_start(d2)
```

```
## [1] "2010-03-20 UTC"
```

```r
int_end(d2)
```

```
## [1] "2020-01-01 UTC"
```

```r
# Shift the interval
d3 <- int_shift(d2, duration(days = 10))
d3 # The whole date has shifted by 10 days
```

```
## [1] 2010-03-30 UTC--2020-01-11 UTC
```

## Operations on Dates
We can do arithmetic operations on date functions


```r
j1 <- ymd("2000-01-01")
j2 <- j1 + months(1:60) # Create a date series of length of 60 months
j2
```

```
##  [1] "2000-02-01" "2000-03-01" "2000-04-01" "2000-05-01" "2000-06-01"
##  [6] "2000-07-01" "2000-08-01" "2000-09-01" "2000-10-01" "2000-11-01"
## [11] "2000-12-01" "2001-01-01" "2001-02-01" "2001-03-01" "2001-04-01"
## [16] "2001-05-01" "2001-06-01" "2001-07-01" "2001-08-01" "2001-09-01"
## [21] "2001-10-01" "2001-11-01" "2001-12-01" "2002-01-01" "2002-02-01"
## [26] "2002-03-01" "2002-04-01" "2002-05-01" "2002-06-01" "2002-07-01"
## [31] "2002-08-01" "2002-09-01" "2002-10-01" "2002-11-01" "2002-12-01"
## [36] "2003-01-01" "2003-02-01" "2003-03-01" "2003-04-01" "2003-05-01"
## [41] "2003-06-01" "2003-07-01" "2003-08-01" "2003-09-01" "2003-10-01"
## [46] "2003-11-01" "2003-12-01" "2004-01-01" "2004-02-01" "2004-03-01"
## [51] "2004-04-01" "2004-05-01" "2004-06-01" "2004-07-01" "2004-08-01"
## [56] "2004-09-01" "2004-10-01" "2004-11-01" "2004-12-01" "2005-01-01"
```

```r
length(j2)
```

```
## [1] 60
```

> Oberve that here the current date (j1) is NOT INCLUDED in j2. To do so, we start months from `0` instead of `1`. Thus we use the function `months(0:60)` and in this case the length of the date function will be 61. 


OR

```r
k1 <- today()
k2 <- k1 - years(0:10) # Go back 10 years
k2
```

```
##  [1] "2020-03-25" "2019-03-25" "2018-03-25" "2017-03-25" "2016-03-25"
##  [6] "2015-03-25" "2014-03-25" "2013-03-25" "2012-03-25" "2011-03-25"
## [11] "2010-03-25"
```

> As we see, the time is quite opposite of what we wanted. To avoid that: Interchange 10 and 0 (It is like flipping the dates. Otherwise when we plot the time series data, the most recent dates will come first!!!)


```r
k3 <- today() - years(100:0) # 
length(k3)
```

```
## [1] 101
```


## Create some Time series Data

```r
create_data <- data.frame(Date = k3)
class(create_data)
```

```
## [1] "data.frame"
```

```r
create_data <- create_data %>%
  mutate(Values = rnorm(length(k3)))

head(create_data, 10)
```

```
##          Date     Values
## 1  1920-03-25  0.8014295
## 2  1921-03-25 -0.1683764
## 3  1922-03-25 -0.7378748
## 4  1923-03-25  1.1767771
## 5  1924-03-25 -0.4405444
## 6  1925-03-25 -0.8563558
## 7  1926-03-25 -0.7114580
## 8  1927-03-25 -1.1926271
## 9  1928-03-25 -2.2307855
## 10 1929-03-25  0.7345971
```

```r
ggplot(create_data, aes(x = Date, y = Values))+
  geom_line()+
  labs(title = "Some random numbers", caption = "lubridate package")+
  theme_light()
```

![](Lubridate_files/figure-html/unnamed-chunk-10-1.png)<!-- -->
















# References 
- [Tidyverse](https://lubridate.tidyverse.org/reference/interval.html)

