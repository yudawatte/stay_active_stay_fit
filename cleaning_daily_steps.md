Daily Steps
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

input_file <- "D:/Projects/Bellabeat/original_data/dailySteps_merged.csv"

cleaned_data_path <- "D:/Projects/Bellabeat/cleaned_data"

output_file <- "daily_steps.csv"
```

## Read file and formatting

``` r
daily_steps <- read_csv(input_file)
```

    ## Rows: 940 Columns: 3

    ## -- Column specification --------------------------------------------------------
    ## Delimiter: ","
    ## chr (1): ActivityDay
    ## dbl (2): Id, StepTotal

    ## 
    ## i Use `spec()` to retrieve the full column specification for this data.
    ## i Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
glimpse(daily_steps)
```

    ## Rows: 940
    ## Columns: 3
    ## $ Id          <dbl> 1503960366, 1503960366, 1503960366, 1503960366, 1503960366~
    ## $ ActivityDay <chr> "4/12/2016", "4/13/2016", "4/14/2016", "4/15/2016", "4/16/~
    ## $ StepTotal   <dbl> 13162, 10735, 10460, 9762, 12669, 9705, 13019, 15506, 1054~

``` r
daily_steps <- mutate(daily_steps, 
               Id = as.character(Id),
               ActivityDay = as.Date(ActivityDay, "%m/%d/%Y"),
               StepTotal = as.integer(StepTotal)
               )

daily_steps <- rename(
  daily_steps,
  user_id = Id,
  activity_date = ActivityDay,
  total_steps = StepTotal
)

glimpse(daily_steps)
```

    ## Rows: 940
    ## Columns: 3
    ## $ user_id       <chr> "1503960366", "1503960366", "1503960366", "1503960366", ~
    ## $ activity_date <date> 2016-04-12, 2016-04-13, 2016-04-14, 2016-04-15, 2016-04~
    ## $ total_steps   <int> 13162, 10735, 10460, 9762, 12669, 9705, 13019, 15506, 10~

``` r
daily_steps <- daily_steps %>% distinct()

glimpse(daily_steps)
```

    ## Rows: 940
    ## Columns: 3
    ## $ user_id       <chr> "1503960366", "1503960366", "1503960366", "1503960366", ~
    ## $ activity_date <date> 2016-04-12, 2016-04-13, 2016-04-14, 2016-04-15, 2016-04~
    ## $ total_steps   <int> 13162, 10735, 10460, 9762, 12669, 9705, 13019, 15506, 10~

``` r
daily_steps %>% select(user_id) %>% distinct()
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
daily_steps %>% filter(is.na(user_id))
```

    ## # A tibble: 0 x 3
    ## # ... with 3 variables: user_id <chr>, activity_date <date>, total_steps <int>

``` r
daily_steps %>% filter(is.na(activity_date))
```

    ## # A tibble: 0 x 3
    ## # ... with 3 variables: user_id <chr>, activity_date <date>, total_steps <int>

``` r
daily_steps %>% filter(is.na(total_steps))
```

    ## # A tibble: 0 x 3
    ## # ... with 3 variables: user_id <chr>, activity_date <date>, total_steps <int>

-   33 unique user ids
-   All users don’t have the same number of daily records
-   No null variables

## Save files

``` r
write_csv(daily_steps, output_file)
file.move(output_file, cleaned_data_path, overwrite = TRUE)
```

    ## 1 file moved. 0 failed.
