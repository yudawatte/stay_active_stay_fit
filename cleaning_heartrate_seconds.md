Heart Rate
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

input_file <- "D:/Projects/Bellabeat/original_data/heartrate_seconds_merged.csv"

cleaned_data_path <- "D:/Projects/Bellabeat/cleaned_data"

output_file <- "heart_rate_in_seconds.csv"
```

## Read file and formatting

``` r
heart_rates <- read_csv(input_file)
```

    ## Rows: 2483658 Columns: 3

    ## -- Column specification --------------------------------------------------------
    ## Delimiter: ","
    ## chr (1): Time
    ## dbl (2): Id, Value

    ## 
    ## i Use `spec()` to retrieve the full column specification for this data.
    ## i Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
glimpse(heart_rates)
```

    ## Rows: 2,483,658
    ## Columns: 3
    ## $ Id    <dbl> 2022484408, 2022484408, 2022484408, 2022484408, 2022484408, 2022~
    ## $ Time  <chr> "4/12/2016 7:21:00 AM", "4/12/2016 7:21:05 AM", "4/12/2016 7:21:~
    ## $ Value <dbl> 97, 102, 105, 103, 101, 95, 91, 93, 94, 93, 92, 89, 83, 61, 60, ~

``` r
heart_rates <- mutate(heart_rates, 
               Id = as.character(Id),
               Time = ymd_hms(as.POSIXct(Time, format="%m/%d/%Y %I:%M:%S %p", tz = "UTC")),
               Value = as.double(Value)
               )

heart_rates <- rename(
  heart_rates,
  user_id = Id,
  activity_date_time = Time,
  heart_rate = Value
)

heart_rates <- heart_rates %>% distinct()

glimpse(heart_rates)
```

    ## Rows: 2,483,658
    ## Columns: 3
    ## $ user_id            <chr> "2022484408", "2022484408", "2022484408", "20224844~
    ## $ activity_date_time <dttm> 2016-04-12 07:21:00, 2016-04-12 07:21:05, 2016-04-~
    ## $ heart_rate         <dbl> 97, 102, 105, 103, 101, 95, 91, 93, 94, 93, 92, 89,~

``` r
heart_rates %>% select(user_id) %>% distinct()
```

    ## # A tibble: 14 x 1
    ##    user_id   
    ##    <chr>     
    ##  1 2022484408
    ##  2 2026352035
    ##  3 2347167796
    ##  4 4020332650
    ##  5 4388161847
    ##  6 4558609924
    ##  7 5553957443
    ##  8 5577150313
    ##  9 6117666160
    ## 10 6775888955
    ## 11 6962181067
    ## 12 7007744171
    ## 13 8792009665
    ## 14 8877689391

``` r
heart_rates %>% select(user_id) %>% group_by(user_id) %>% count()
```

    ## # A tibble: 14 x 2
    ## # Groups:   user_id [14]
    ##    user_id         n
    ##    <chr>       <int>
    ##  1 2022484408 154104
    ##  2 2026352035   2490
    ##  3 2347167796 152683
    ##  4 4020332650 285461
    ##  5 4388161847 249748
    ##  6 4558609924 192168
    ##  7 5553957443 255174
    ##  8 5577150313 248560
    ##  9 6117666160 158899
    ## 10 6775888955  32771
    ## 11 6962181067 266326
    ## 12 7007744171 133592
    ## 13 8792009665 122841
    ## 14 8877689391 228841

``` r
heart_rates %>% filter(is.na(user_id))
```

    ## # A tibble: 0 x 3
    ## # ... with 3 variables: user_id <chr>, activity_date_time <dttm>,
    ## #   heart_rate <dbl>

``` r
heart_rates %>% filter(is.na(activity_date_time))
```

    ## # A tibble: 0 x 3
    ## # ... with 3 variables: user_id <chr>, activity_date_time <dttm>,
    ## #   heart_rate <dbl>

``` r
heart_rates %>% filter(is.na(heart_rate))
```

    ## # A tibble: 0 x 3
    ## # ... with 3 variables: user_id <chr>, activity_date_time <dttm>,
    ## #   heart_rate <dbl>

-   14 unique user ids
-   All users don’t have the same number of daily records
-   No null variables

## Save files

``` r
write_csv(heart_rates, output_file)
file.move(output_file, cleaned_data_path, overwrite = TRUE)
```

    ## 1 file moved. 0 failed.
