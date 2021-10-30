Heart Rate by Minutes
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

input_file <- "D:/Projects/Bellabeat/cleaned_data/heart_rate_in_seconds.csv"

cleaned_data_path <- "D:/Projects/Bellabeat/cleaned_data"

output_file <- "minute_heart_rate.csv"
```

## Read file and formatting

``` r
minute_heart_rate <- read_csv(input_file)
```

    ## Rows: 2483658 Columns: 3

    ## -- Column specification --------------------------------------------------------
    ## Delimiter: ","
    ## dbl  (2): user_id, heart_rate
    ## dttm (1): activity_date_time

    ## 
    ## i Use `spec()` to retrieve the full column specification for this data.
    ## i Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
glimpse(minute_heart_rate)
```

    ## Rows: 2,483,658
    ## Columns: 3
    ## $ user_id            <dbl> 2022484408, 2022484408, 2022484408, 2022484408, 202~
    ## $ activity_date_time <dttm> 2016-04-12 07:21:00, 2016-04-12 07:21:05, 2016-04-~
    ## $ heart_rate         <dbl> 97, 102, 105, 103, 101, 95, 91, 93, 94, 93, 92, 89,~

``` r
glimpse(minute_heart_rate)
```

    ## Rows: 2,483,658
    ## Columns: 3
    ## $ user_id            <dbl> 2022484408, 2022484408, 2022484408, 2022484408, 202~
    ## $ activity_date_time <dttm> 2016-04-12 07:21:00, 2016-04-12 07:21:05, 2016-04-~
    ## $ heart_rate         <dbl> 97, 102, 105, 103, 101, 95, 91, 93, 94, 93, 92, 89,~

``` r
minute_heart_rate <- mutate(minute_heart_rate,
                    activity_minute = round_date(activity_date_time, "minute"))

glimpse(minute_heart_rate)
```

    ## Rows: 2,483,658
    ## Columns: 4
    ## $ user_id            <dbl> 2022484408, 2022484408, 2022484408, 2022484408, 202~
    ## $ activity_date_time <dttm> 2016-04-12 07:21:00, 2016-04-12 07:21:05, 2016-04-~
    ## $ heart_rate         <dbl> 97, 102, 105, 103, 101, 95, 91, 93, 94, 93, 92, 89,~
    ## $ activity_minute    <dttm> 2016-04-12 07:21:00, 2016-04-12 07:21:00, 2016-04-~

``` r
minute_heart_rate <- minute_heart_rate %>% 
  group_by(user_id, activity_minute) %>% 
  summarise(avg_hear_rate = mean(heart_rate))
```

    ## `summarise()` has grouped output by 'user_id'. You can override using the `.groups` argument.

``` r
minute_heart_rate <- minute_heart_rate %>% distinct()

glimpse(minute_heart_rate)
```

    ## Rows: 333,790
    ## Columns: 3
    ## Groups: user_id [14]
    ## $ user_id         <dbl> 2022484408, 2022484408, 2022484408, 2022484408, 202248~
    ## $ activity_minute <dttm> 2016-04-12 07:21:00, 2016-04-12 07:22:00, 2016-04-12 ~
    ## $ avg_hear_rate   <dbl> 101.60000, 93.20000, 72.42857, 57.16667, 58.00000, 54.~

## Save files

``` r
write_csv(minute_heart_rate, output_file)
file.move(output_file, cleaned_data_path, overwrite = TRUE)
```

    ## 1 file moved. 0 failed.
