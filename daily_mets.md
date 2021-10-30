Daly MET
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

input_file <- "D:/Projects/Bellabeat/cleaned_data/minute_met_narrow.csv"

cleaned_data_path <- "D:/Projects/Bellabeat/cleaned_data"

output_file <- "daily_met.csv"
```

## Read file and formatting

``` r
daily_met <- read_csv(input_file)
```

    ## Rows: 1325580 Columns: 3

    ## -- Column specification --------------------------------------------------------
    ## Delimiter: ","
    ## dbl  (2): user_id, mets
    ## dttm (1): activity_minute

    ## 
    ## i Use `spec()` to retrieve the full column specification for this data.
    ## i Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
glimpse(daily_met)
```

    ## Rows: 1,325,580
    ## Columns: 3
    ## $ user_id         <dbl> 1503960366, 1503960366, 1503960366, 1503960366, 150396~
    ## $ activity_minute <dttm> 2016-04-12 00:00:00, 2016-04-12 00:01:00, 2016-04-12 ~
    ## $ mets            <dbl> 10, 10, 10, 10, 10, 12, 12, 12, 12, 12, 12, 12, 10, 10~

``` r
daily_met <- mutate(daily_met,
                    activity_date = as.Date(activity_minute))

glimpse(daily_met)
```

    ## Rows: 1,325,580
    ## Columns: 4
    ## $ user_id         <dbl> 1503960366, 1503960366, 1503960366, 1503960366, 150396~
    ## $ activity_minute <dttm> 2016-04-12 00:00:00, 2016-04-12 00:01:00, 2016-04-12 ~
    ## $ mets            <dbl> 10, 10, 10, 10, 10, 12, 12, 12, 12, 12, 12, 12, 10, 10~
    ## $ activity_date   <date> 2016-04-12, 2016-04-12, 2016-04-12, 2016-04-12, 2016-~

``` r
daily_met <- daily_met %>% 
  group_by(user_id, activity_date) %>% 
  summarise(avg_mets = mean(mets))
```

    ## `summarise()` has grouped output by 'user_id'. You can override using the `.groups` argument.

``` r
daily_met <- daily_met %>% distinct()

glimpse(daily_met)
```

    ## Rows: 934
    ## Columns: 3
    ## Groups: user_id [33]
    ## $ user_id       <dbl> 1503960366, 1503960366, 1503960366, 1503960366, 15039603~
    ## $ activity_date <date> 2016-04-12, 2016-04-13, 2016-04-14, 2016-04-15, 2016-04~
    ## $ avg_mets      <dbl> 17.52847, 15.87431, 15.68681, 15.40972, 16.45417, 15.258~

-   33 unique user ids
-   All users don’t have the same number of daily records
-   No null variables

## Save files

``` r
write_csv(daily_met, output_file)
file.move(output_file, cleaned_data_path, overwrite = TRUE)
```

    ## 1 file moved. 0 failed.
