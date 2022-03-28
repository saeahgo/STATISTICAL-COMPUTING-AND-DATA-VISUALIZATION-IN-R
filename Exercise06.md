Exercise06
================
Saeah Go
3/28/2022

# Tuesday Lecture: Flight problems with `dplyr`

<span style="color:cornflowerblue">How many flights to Los Angeles (LAX)
did each of the legacy carriers (AA, UA, DL or US) have in May from JFK,
and what was their average duration?</span>

``` r
# your code here
flights %>% 
  filter(carrier%in%c("AA", "UA", "DL", "US"),
         month == 5, 
         origin == "JFK",
         dest == "LAX") %>% 
  group_by(carrier) %>%
  summarise(num_flights = n(),
            mean_duration = mean(air_time, na.rm=T))
```

    ## # A tibble: 3 x 3
    ##   carrier num_flights mean_duration
    ##   <chr>         <int>         <dbl>
    ## 1 AA              273          319.
    ## 2 DL              235          321.
    ## 3 UA              177          320.

<span style="color:cornflowerblue">What was the shortest flight out of
each airport in terms of distance? </span>

``` r
# your code here
flights %>% 
  group_by(origin) %>% 
  summarise(min_dist = min(distance, na.rm = T))
```

    ## # A tibble: 3 x 2
    ##   origin min_dist
    ##   <chr>     <dbl>
    ## 1 EWR          17
    ## 2 JFK          94
    ## 3 LGA          96

<span style="color:cornflowerblue">In terms of duration?</span>

``` r
# your code here
flights %>% 
  group_by(origin) %>% 
  summarise(min_time = min(air_time, na.rm = T))
```

    ## # A tibble: 3 x 2
    ##   origin min_time
    ##   <chr>     <dbl>
    ## 1 EWR          20
    ## 2 JFK          21
    ## 3 LGA          21

<span style="color:cornflowerblue">Which plane (check the tail number)
flew out of each New York airport the most?</span>

``` r
# your code here
flights %>% 
  filter(!is.na(tailnum)) %>% # check NAs
  group_by(origin, tailnum) %>% # grouping by origin and tailnum
  tally() %>% # count
  filter(n==max(n))
```

    ## # A tibble: 3 x 3
    ## # Groups:   origin [3]
    ##   origin tailnum     n
    ##   <chr>  <chr>   <int>
    ## 1 EWR    N15980    314
    ## 2 JFK    N328AA    393
    ## 3 LGA    N725MQ    567

<span style="color:cornflowerblue">Which date should you fly on if you
want to have the lowest possible average departure delay? </span>

``` r
# your code here
flights %>% 
  mutate(date = paste(day, month, year, sep = "/")) %>% 
  group_by(date) %>% # grouping by date (so total 365 groups)
  summarise(mean.dep.delay = mean(dep_delay, na.rm = T)) %>% # calculate means each day
  ungroup() %>% # removes grouping
  filter(mean.dep.delay == min(mean.dep.delay)) # get minimum of mean.dep.delay
```

    ## # A tibble: 1 x 2
    ##   date      mean.dep.delay
    ##   <chr>              <dbl>
    ## 1 24/9/2013          -1.33

<span style="color:cornflowerblue">What about arrival delay? </span>

``` r
# your code here
flights %>% mutate(date = paste(day, month, year, sep= "/")) %>% 
  group_by(date) %>% 
  summarise(mean.dep.delay = mean(dep_delay, na.rm = TRUE)) %>% 
  ungroup() %>% 
  filter(mean.dep.delay == min(mean.dep.delay))
```

    ## # A tibble: 1 x 2
    ##   date      mean.dep.delay
    ##   <chr>              <dbl>
    ## 1 24/9/2013          -1.33

# Thursday Lecture

## Problem 1

Compute the rate for `table2`, and `table4a + table4b`. You will need to
perform four operations:

1.  Extract the number of TB cases per country per year
2.  Extract the matching population per country per year
3.  Divide cases by population, and multiply by 10000
4.  Store back in the appropriate place.

``` r
# your code here
table2
```

    ## # A tibble: 12 x 4
    ##    country      year type            count
    ##    <chr>       <int> <chr>           <int>
    ##  1 Afghanistan  1999 cases             745
    ##  2 Afghanistan  1999 population   19987071
    ##  3 Afghanistan  2000 cases            2666
    ##  4 Afghanistan  2000 population   20595360
    ##  5 Brazil       1999 cases           37737
    ##  6 Brazil       1999 population  172006362
    ##  7 Brazil       2000 cases           80488
    ##  8 Brazil       2000 population  174504898
    ##  9 China        1999 cases          212258
    ## 10 China        1999 population 1272915272
    ## 11 China        2000 cases          213766
    ## 12 China        2000 population 1280428583

``` r
cases <- table2 %>% filter(type=="cases") %>% pull(count)
table2 %>% 
  filter(type == "population") %>% 
  mutate(rate = (cases/count)*10000) %>% 
  select(-type, -count)
```

    ## # A tibble: 6 x 3
    ##   country      year  rate
    ##   <chr>       <int> <dbl>
    ## 1 Afghanistan  1999 0.373
    ## 2 Afghanistan  2000 1.29 
    ## 3 Brazil       1999 2.19 
    ## 4 Brazil       2000 4.61 
    ## 5 China        1999 1.67 
    ## 6 China        2000 1.67

``` r
# your code here
table4a
```

    ## # A tibble: 3 x 3
    ##   country     `1999` `2000`
    ## * <chr>        <int>  <int>
    ## 1 Afghanistan    745   2666
    ## 2 Brazil       37737  80488
    ## 3 China       212258 213766

``` r
table4b
```

    ## # A tibble: 3 x 3
    ##   country         `1999`     `2000`
    ## * <chr>            <int>      <int>
    ## 1 Afghanistan   19987071   20595360
    ## 2 Brazil       172006362  174504898
    ## 3 China       1272915272 1280428583

``` r
table4a %>% 
  mutate(rate1999=(`1999`/table4b$`1999`)*10000, # it shoud be `1999`, not 1999
         rate2000=(`2000`/table4b$`2000`)*10000) %>% 
  select(country, rate1999, rate2000)
```

    ## # A tibble: 3 x 3
    ##   country     rate1999 rate2000
    ##   <chr>          <dbl>    <dbl>
    ## 1 Afghanistan    0.373     1.29
    ## 2 Brazil         2.19      4.61
    ## 3 China          1.67      1.67

``` r
table1 %>% mutate(rate=(cases/population)*10000)
```

    ## # A tibble: 6 x 5
    ##   country      year  cases population  rate
    ##   <chr>       <int>  <int>      <int> <dbl>
    ## 1 Afghanistan  1999    745   19987071 0.373
    ## 2 Afghanistan  2000   2666   20595360 1.29 
    ## 3 Brazil       1999  37737  172006362 2.19 
    ## 4 Brazil       2000  80488  174504898 4.61 
    ## 5 China        1999 212258 1272915272 1.67 
    ## 6 China        2000 213766 1280428583 1.67

## Problem 2

Use tb to generate a tibble having as columns iso2, gender and age, as
well as the counts for each year from 1980 through 2008.

``` r
tb = readr::read_csv(paste0("https://raw.githubusercontent.com/tidyverse/tidyr/main/vignettes/tb.csv")) 
```

    ## Rows: 5769 Columns: 22

    ## -- Column specification --------------------------------------------------------
    ## Delimiter: ","
    ## chr  (1): iso2
    ## dbl (21): year, m04, m514, m014, m1524, m2534, m3544, m4554, m5564, m65, mu,...

    ## 
    ## i Use `spec()` to retrieve the full column specification for this data.
    ## i Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
                     # paste0("https://github.com/tidyverse/",
                     #        "tidyr/raw/master/vignettes/tb.csv"))


# your code here
tb %>%
  gather(key = group, value = counts,
         -iso2, -year,
         na.rm = TRUE) %>% 
  separate(col = group, into = c("gender", "age"), sep = 1) %>%
  mutate(
    age = case_when(
      age == "04" ~ "0-4",
      age == "514" ~ "5-14",
      age == "014" ~ "0-14",
      age == "1524" ~ "15-24",
      age == "2534" ~ "25-34",
      age == "3544" ~ "35-44",
      age == "4554" ~ "45-54",
      age == "5564" ~ "55-64",
      age == "65" ~ "65",
      age == "u" ~ NA_character_
      )
  ) %>% 
  spread(key = year, value = counts) 
```

    ## # A tibble: 3,919 x 32
    ##    iso2  gender age   `1980` `1981` `1982` `1983` `1984` `1985` `1986` `1987`
    ##    <chr> <chr>  <chr>  <dbl>  <dbl>  <dbl>  <dbl>  <dbl>  <dbl>  <dbl>  <dbl>
    ##  1 AD    f      0-14      NA     NA     NA     NA     NA     NA     NA     NA
    ##  2 AD    f      0-4       NA     NA     NA     NA     NA     NA     NA     NA
    ##  3 AD    f      15-24     NA     NA     NA     NA     NA     NA     NA     NA
    ##  4 AD    f      25-34     NA     NA     NA     NA     NA     NA     NA     NA
    ##  5 AD    f      35-44     NA     NA     NA     NA     NA     NA     NA     NA
    ##  6 AD    f      45-54     NA     NA     NA     NA     NA     NA     NA     NA
    ##  7 AD    f      5-14      NA     NA     NA     NA     NA     NA     NA     NA
    ##  8 AD    f      55-64     NA     NA     NA     NA     NA     NA     NA     NA
    ##  9 AD    f      65        NA     NA     NA     NA     NA     NA     NA     NA
    ## 10 AD    f      <NA>      NA     NA     NA     NA     NA     NA     NA     NA
    ## # ... with 3,909 more rows, and 21 more variables: 1988 <dbl>, 1989 <dbl>,
    ## #   1990 <dbl>, 1991 <dbl>, 1992 <dbl>, 1993 <dbl>, 1994 <dbl>, 1995 <dbl>,
    ## #   1996 <dbl>, 1997 <dbl>, 1998 <dbl>, 1999 <dbl>, 2000 <dbl>, 2001 <dbl>,
    ## #   2002 <dbl>, 2003 <dbl>, 2004 <dbl>, 2005 <dbl>, 2006 <dbl>, 2007 <dbl>,
    ## #   2008 <dbl>
