Daily Calories
================

## Load libraries and set current working directory

You can include R code in the document as follows:

``` r
library(tidyverse)
```

    ## -- Attaching packages --------------------------------------- tidyverse 1.3.1 --

    ## v ggplot2 3.3.5     v purrr   0.3.4
    ## v tibble  3.1.4     v dplyr   1.0.7
    ## v tidyr   1.1.3     v stringr 1.4.0
    ## v readr   2.0.2     v forcats 0.5.1

    ## -- Conflicts ------------------------------------------ tidyverse_conflicts() --
    ## x dplyr::filter() masks stats::filter()
    ## x dplyr::lag()    masks stats::lag()

``` r
library(readr)
library(filesstrings)
```

    ## 
    ## Attaching package: 'filesstrings'

    ## The following object is masked from 'package:dplyr':
    ## 
    ##     all_equal

``` r
library(lubridate)
```

    ## 
    ## Attaching package: 'lubridate'

    ## The following objects are masked from 'package:base':
    ## 
    ##     date, intersect, setdiff, union

``` r
library(gapminder)
library(dplyr)

setwd("D:/Projects/Bellabeat/data_cleaning")

input_file <- "D:/Projects/Bellabeat/original_data/dailyCalories_merged.csv"

cleaned_data_path <- "D:/Projects/Bellabeat/cleaned_data"

output_file <- "daily_calories.csv"
```

## Read file and formatting

``` r
daily_calories <- read_csv(input_file)
```

    ## Rows: 940 Columns: 3

    ## -- Column specification --------------------------------------------------------
    ## Delimiter: ","
    ## chr (1): ActivityDay
    ## dbl (2): Id, Calories

    ## 
    ## i Use `spec()` to retrieve the full column specification for this data.
    ## i Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
glimpse(daily_calories)
```

    ## Rows: 940
    ## Columns: 3
    ## $ Id          <dbl> 1503960366, 1503960366, 1503960366, 1503960366, 1503960366~
    ## $ ActivityDay <chr> "4/12/2016", "4/13/2016", "4/14/2016", "4/15/2016", "4/16/~
    ## $ Calories    <dbl> 1985, 1797, 1776, 1745, 1863, 1728, 1921, 2035, 1786, 1775~

``` r
daily_calories <- mutate(daily_calories, 
               Id = as.character(Id),
               ActivityDay = as.Date(ActivityDay, "%m/%d/%Y"),
               Calories = as.double(Calories)
               )

daily_calories <- rename(
  daily_calories,
  user_id = Id,
  activity_date = ActivityDay,
  calories_burn = Calories
)

glimpse(daily_calories)
```

    ## Rows: 940
    ## Columns: 3
    ## $ user_id       <chr> "1503960366", "1503960366", "1503960366", "1503960366", ~
    ## $ activity_date <date> 2016-04-12, 2016-04-13, 2016-04-14, 2016-04-15, 2016-04~
    ## $ calories_burn <dbl> 1985, 1797, 1776, 1745, 1863, 1728, 1921, 2035, 1786, 17~

``` r
daily_calories <- daily_calories %>% distinct()

glimpse(daily_calories)
```

    ## Rows: 940
    ## Columns: 3
    ## $ user_id       <chr> "1503960366", "1503960366", "1503960366", "1503960366", ~
    ## $ activity_date <date> 2016-04-12, 2016-04-13, 2016-04-14, 2016-04-15, 2016-04~
    ## $ calories_burn <dbl> 1985, 1797, 1776, 1745, 1863, 1728, 1921, 2035, 1786, 17~

## Read file and formatting

``` r
daily_calories %>% select(user_id) %>% distinct()
```

    ## # A tibble: 33 x 1
    ##    user_id   
    ##    <chr>     
    ##  1 1503960366
    ##  2 1624580081
    ##  3 1644430081
    ##  4 1844505072
    ##  5 1927972279
    ##  6 2022484408
    ##  7 2026352035
    ##  8 2320127002
    ##  9 2347167796
    ## 10 2873212765
    ## # ... with 23 more rows

``` r
daily_calories %>% select(user_id) %>% group_by(user_id) %>% count() %>% arrange(user_id)
```

    ## # A tibble: 33 x 2
    ## # Groups:   user_id [33]
    ##    user_id        n
    ##    <chr>      <int>
    ##  1 1503960366    31
    ##  2 1624580081    31
    ##  3 1644430081    30
    ##  4 1844505072    31
    ##  5 1927972279    31
    ##  6 2022484408    31
    ##  7 2026352035    31
    ##  8 2320127002    31
    ##  9 2347167796    18
    ## 10 2873212765    31
    ## # ... with 23 more rows

``` r
daily_calories %>% filter(is.na(user_id))
```

    ## # A tibble: 0 x 3
    ## # ... with 3 variables: user_id <chr>, activity_date <date>,
    ## #   calories_burn <dbl>

``` r
daily_calories %>% filter(is.na(activity_date))
```

    ## # A tibble: 0 x 3
    ## # ... with 3 variables: user_id <chr>, activity_date <date>,
    ## #   calories_burn <dbl>

``` r
daily_calories %>% filter(is.na(calories_burn))
```

    ## # A tibble: 0 x 3
    ## # ... with 3 variables: user_id <chr>, activity_date <date>,
    ## #   calories_burn <dbl>

-   33 unique user ids
-   All users don’t have the same number of daily records
-   No null variables

## Save files

``` r
write_csv(daily_calories, output_file)
file.move(output_file, cleaned_data_path, overwrite = TRUE)
```

    ## 1 file moved. 0 failed.
