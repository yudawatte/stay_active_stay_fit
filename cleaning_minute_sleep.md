Minute Sleep
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

input_file <- "D:/Projects/Bellabeat/original_data/minuteSleep_merged.csv"

cleaned_data_path <- "D:/Projects/Bellabeat/cleaned_data"

output_file <- "minute_sleep.csv"
```

## Read file and formatting

``` r
minitue_sleep <- read_csv(input_file)
```

    ## Rows: 188521 Columns: 4

    ## -- Column specification --------------------------------------------------------
    ## Delimiter: ","
    ## chr (1): date
    ## dbl (3): Id, value, logId

    ## 
    ## i Use `spec()` to retrieve the full column specification for this data.
    ## i Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
glimpse(minitue_sleep)
```

    ## Rows: 188,521
    ## Columns: 4
    ## $ Id    <dbl> 1503960366, 1503960366, 1503960366, 1503960366, 1503960366, 1503~
    ## $ date  <chr> "4/12/2016 2:47:30 AM", "4/12/2016 2:48:30 AM", "4/12/2016 2:49:~
    ## $ value <dbl> 3, 2, 1, 1, 1, 1, 1, 2, 2, 2, 3, 3, 3, 3, 3, 2, 1, 1, 1, 1, 1, 1~
    ## $ logId <dbl> 11380564589, 11380564589, 11380564589, 11380564589, 11380564589,~

``` r
minitue_sleep <- mutate(minitue_sleep, 
               Id = as.character(Id),
               date = ymd_hms(as.POSIXct(date, format="%m/%d/%Y %I:%M:%S %p", tz = "UTC")),
               value = as.double(value),
               logId = as.character(logId)
               )

minitue_sleep <- rename(
  minitue_sleep,
  user_id = Id,
  activity_minute = date,
  sleep_value = value,
  log_id = logId
)

minitue_sleep <- minitue_sleep %>% distinct()

glimpse(minitue_sleep)
```

    ## Rows: 187,978
    ## Columns: 4
    ## $ user_id         <chr> "1503960366", "1503960366", "1503960366", "1503960366"~
    ## $ activity_minute <dttm> 2016-04-12 02:47:30, 2016-04-12 02:48:30, 2016-04-12 ~
    ## $ sleep_value     <dbl> 3, 2, 1, 1, 1, 1, 1, 2, 2, 2, 3, 3, 3, 3, 3, 2, 1, 1, ~
    ## $ log_id          <chr> "11380564589", "11380564589", "11380564589", "11380564~

``` r
minitue_sleep %>% select(user_id) %>% distinct()
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
minitue_sleep %>% filter(is.na(user_id))
```

    ## # A tibble: 0 x 4
    ## # ... with 4 variables: user_id <chr>, activity_minute <dttm>,
    ## #   sleep_value <dbl>, log_id <chr>

``` r
minitue_sleep %>% filter(is.na(activity_minute))
```

    ## # A tibble: 0 x 4
    ## # ... with 4 variables: user_id <chr>, activity_minute <dttm>,
    ## #   sleep_value <dbl>, log_id <chr>

``` r
minitue_sleep %>% filter(is.na(sleep_value))
```

    ## # A tibble: 0 x 4
    ## # ... with 4 variables: user_id <chr>, activity_minute <dttm>,
    ## #   sleep_value <dbl>, log_id <chr>

``` r
minitue_sleep %>% filter(is.na(log_id))
```

    ## # A tibble: 0 x 4
    ## # ... with 4 variables: user_id <chr>, activity_minute <dttm>,
    ## #   sleep_value <dbl>, log_id <chr>

-   33 unique user ids
-   All users don’t have the same number of daily records
-   No null variables

## Save files

``` r
write_csv(minitue_sleep, output_file)
file.move(output_file, cleaned_data_path, overwrite = TRUE)
```

    ## 1 file moved. 0 failed.
