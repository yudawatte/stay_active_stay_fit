Minute MET Narrow
================

A MET is a ratio of your working metabolic rate relative to your resting
metabolic rate. Metabolic - the breakdown of food and its transformation
into energy

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

input_file <- "D:/Projects/Bellabeat/original_data/minuteMETsNarrow_merged.csv"

cleaned_data_path <- "D:/Projects/Bellabeat/cleaned_data"

output_file <- "minute_met_narrow.csv"
```

## Read file and formatting

``` r
minitue_met_rates <- read_csv(input_file)
```

    ## Rows: 1325580 Columns: 3

    ## -- Column specification --------------------------------------------------------
    ## Delimiter: ","
    ## chr (1): ActivityMinute
    ## dbl (2): Id, METs

    ## 
    ## i Use `spec()` to retrieve the full column specification for this data.
    ## i Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
glimpse(minitue_met_rates)
```

    ## Rows: 1,325,580
    ## Columns: 3
    ## $ Id             <dbl> 1503960366, 1503960366, 1503960366, 1503960366, 1503960~
    ## $ ActivityMinute <chr> "4/12/2016 12:00:00 AM", "4/12/2016 12:01:00 AM", "4/12~
    ## $ METs           <dbl> 10, 10, 10, 10, 10, 12, 12, 12, 12, 12, 12, 12, 10, 10,~

``` r
minitue_met_rates <- mutate(minitue_met_rates, 
               Id = as.character(Id),
               ActivityMinute = ymd_hms(as.POSIXct(ActivityMinute, format="%m/%d/%Y %I:%M:%S %p", tz = "UTC")),
               METs = as.double(METs)
               )

minitue_met_rates <- rename(
  minitue_met_rates,
  user_id = Id,
  activity_minute = ActivityMinute,
  mets = METs
)

minitue_met_rates <- minitue_met_rates %>% distinct()

glimpse(minitue_met_rates)
```

    ## Rows: 1,325,580
    ## Columns: 3
    ## $ user_id         <chr> "1503960366", "1503960366", "1503960366", "1503960366"~
    ## $ activity_minute <dttm> 2016-04-12 00:00:00, 2016-04-12 00:01:00, 2016-04-12 ~
    ## $ mets            <dbl> 10, 10, 10, 10, 10, 12, 12, 12, 12, 12, 12, 12, 10, 10~

``` r
minitue_met_rates %>% select(user_id) %>% distinct()
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
minitue_met_rates %>% filter(is.na(user_id))
```

    ## # A tibble: 0 x 3
    ## # ... with 3 variables: user_id <chr>, activity_minute <dttm>, mets <dbl>

``` r
minitue_met_rates %>% filter(is.na(activity_minute))
```

    ## # A tibble: 0 x 3
    ## # ... with 3 variables: user_id <chr>, activity_minute <dttm>, mets <dbl>

``` r
minitue_met_rates %>% filter(is.na(mets))
```

    ## # A tibble: 0 x 3
    ## # ... with 3 variables: user_id <chr>, activity_minute <dttm>, mets <dbl>

-   33 unique user ids
-   All users don’t have the same number of daily records
-   No null variables

## Save files

``` r
write_csv(minitue_met_rates, output_file)
file.move(output_file, cleaned_data_path, overwrite = TRUE)
```

    ## 1 file moved. 0 failed.
