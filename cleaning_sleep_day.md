Sleep Day
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

input_file <- "D:/Projects/Bellabeat/original_data/sleepDay_merged.csv"

cleaned_data_path <- "D:/Projects/Bellabeat/cleaned_data"

output_file <- "sleep_day.csv"
```

## Read file and formatting

``` r
sleep_days <- read_csv(input_file)
```

    ## Rows: 413 Columns: 5

    ## -- Column specification --------------------------------------------------------
    ## Delimiter: ","
    ## chr (1): SleepDay
    ## dbl (4): Id, TotalSleepRecords, TotalMinutesAsleep, TotalTimeInBed

    ## 
    ## i Use `spec()` to retrieve the full column specification for this data.
    ## i Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
glimpse(sleep_days)
```

    ## Rows: 413
    ## Columns: 5
    ## $ Id                 <dbl> 1503960366, 1503960366, 1503960366, 1503960366, 150~
    ## $ SleepDay           <chr> "4/12/2016 12:00:00 AM", "4/13/2016 12:00:00 AM", "~
    ## $ TotalSleepRecords  <dbl> 1, 2, 1, 2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, ~
    ## $ TotalMinutesAsleep <dbl> 327, 384, 412, 340, 700, 304, 360, 325, 361, 430, 2~
    ## $ TotalTimeInBed     <dbl> 346, 407, 442, 367, 712, 320, 377, 364, 384, 449, 3~

``` r
sleep_days <- mutate(sleep_days, 
               Id = as.character(Id),
               SleepDay = as.Date(SleepDay, format="%m/%d/%Y %I:%M:%S %p", tz = "UTC"),
               TotalSleepRecords = as.double(TotalSleepRecords),
               TotalMinutesAsleep = as.double(TotalMinutesAsleep),
               TotalTimeInBed = as.double(TotalTimeInBed)
               )

sleep_days <- rename(
  sleep_days,
  user_id = Id,
  activity_date = SleepDay,
  total_sleep_records = TotalSleepRecords,
  total_sleep_minutes = TotalMinutesAsleep,
  total_bedtime_minutes = TotalTimeInBed
)

sleep_days <- sleep_days %>% distinct()

glimpse(sleep_days)
```

    ## Rows: 410
    ## Columns: 5
    ## $ user_id               <chr> "1503960366", "1503960366", "1503960366", "15039~
    ## $ activity_date         <date> 2016-04-12, 2016-04-13, 2016-04-15, 2016-04-16,~
    ## $ total_sleep_records   <dbl> 1, 2, 1, 2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, ~
    ## $ total_sleep_minutes   <dbl> 327, 384, 412, 340, 700, 304, 360, 325, 361, 430~
    ## $ total_bedtime_minutes <dbl> 346, 407, 442, 367, 712, 320, 377, 364, 384, 449~

``` r
sleep_days %>% select(user_id) %>% distinct()
```

    ## # A tibble: 24 x 1
    ##    user_id   
    ##    <chr>     
    ##  1 1503960366
    ##  2 1644430081
    ##  3 1844505072
    ##  4 1927972279
    ##  5 2026352035
    ##  6 2320127002
    ##  7 2347167796
    ##  8 3977333714
    ##  9 4020332650
    ## 10 4319703577
    ## # ... with 14 more rows

``` r
sleep_days %>% filter(is.na(user_id))
```

    ## # A tibble: 0 x 5
    ## # ... with 5 variables: user_id <chr>, activity_date <date>,
    ## #   total_sleep_records <dbl>, total_sleep_minutes <dbl>,
    ## #   total_bedtime_minutes <dbl>

``` r
sleep_days %>% filter(is.na(activity_date))
```

    ## # A tibble: 0 x 5
    ## # ... with 5 variables: user_id <chr>, activity_date <date>,
    ## #   total_sleep_records <dbl>, total_sleep_minutes <dbl>,
    ## #   total_bedtime_minutes <dbl>

``` r
sleep_days %>% filter(is.na(total_sleep_records))
```

    ## # A tibble: 0 x 5
    ## # ... with 5 variables: user_id <chr>, activity_date <date>,
    ## #   total_sleep_records <dbl>, total_sleep_minutes <dbl>,
    ## #   total_bedtime_minutes <dbl>

``` r
sleep_days %>% filter(is.na(total_sleep_minutes))
```

    ## # A tibble: 0 x 5
    ## # ... with 5 variables: user_id <chr>, activity_date <date>,
    ## #   total_sleep_records <dbl>, total_sleep_minutes <dbl>,
    ## #   total_bedtime_minutes <dbl>

``` r
sleep_days %>% filter(is.na(total_bedtime_minutes))
```

    ## # A tibble: 0 x 5
    ## # ... with 5 variables: user_id <chr>, activity_date <date>,
    ## #   total_sleep_records <dbl>, total_sleep_minutes <dbl>,
    ## #   total_bedtime_minutes <dbl>

-   24 unique user ids
-   All users don’t have the same number of daily records
-   No null variables

## Save files

``` r
write_csv(sleep_days, output_file)
file.move(output_file, cleaned_data_path, overwrite = TRUE)
```

    ## 1 file moved. 0 failed.
