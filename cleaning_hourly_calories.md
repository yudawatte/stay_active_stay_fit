Hourly Calories
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

input_file <- "D:/Projects/Bellabeat/original_data/hourlyCalories_merged.csv"

cleaned_data_path <- "D:/Projects/Bellabeat/cleaned_data"

output_file <- "hourly_Calories.csv"
```

## Read file and formatting

``` r
hourly_calories <- read_csv(input_file)
```

    ## Rows: 22099 Columns: 3

    ## -- Column specification --------------------------------------------------------
    ## Delimiter: ","
    ## chr (1): ActivityHour
    ## dbl (2): Id, Calories

    ## 
    ## i Use `spec()` to retrieve the full column specification for this data.
    ## i Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
glimpse(hourly_calories)
```

    ## Rows: 22,099
    ## Columns: 3
    ## $ Id           <dbl> 1503960366, 1503960366, 1503960366, 1503960366, 150396036~
    ## $ ActivityHour <chr> "4/12/2016 12:00:00 AM", "4/12/2016 1:00:00 AM", "4/12/20~
    ## $ Calories     <dbl> 81, 61, 59, 47, 48, 48, 48, 47, 68, 141, 99, 76, 73, 66, ~

``` r
hourly_calories <- mutate(hourly_calories, 
               Id = as.character(Id),
               ActivityHour = ymd_hms(as.POSIXct(ActivityHour, format="%m/%d/%Y %I:%M:%S %p", tz = "UTC")),
               Calories = as.double(Calories)
               )

hourly_calories <- rename(
  hourly_calories,
  user_id = Id,
  activity_hour = ActivityHour,
  calories_burn = Calories
)

hourly_calories <- hourly_calories %>% distinct()

glimpse(hourly_calories)
```

    ## Rows: 22,099
    ## Columns: 3
    ## $ user_id       <chr> "1503960366", "1503960366", "1503960366", "1503960366", ~
    ## $ activity_hour <dttm> 2016-04-12 00:00:00, 2016-04-12 01:00:00, 2016-04-12 02~
    ## $ calories_burn <dbl> 81, 61, 59, 47, 48, 48, 48, 47, 68, 141, 99, 76, 73, 66,~

``` r
hourly_calories %>% select(user_id) %>% distinct()
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
hourly_calories %>% filter(is.na(user_id))
```

    ## # A tibble: 0 x 3
    ## # ... with 3 variables: user_id <chr>, activity_hour <dttm>,
    ## #   calories_burn <dbl>

``` r
hourly_calories %>% filter(is.na(activity_hour))
```

    ## # A tibble: 0 x 3
    ## # ... with 3 variables: user_id <chr>, activity_hour <dttm>,
    ## #   calories_burn <dbl>

``` r
hourly_calories %>% filter(is.na(calories_burn))
```

    ## # A tibble: 0 x 3
    ## # ... with 3 variables: user_id <chr>, activity_hour <dttm>,
    ## #   calories_burn <dbl>

-   33 unique user ids
-   All users don’t have the same number of daily records
-   No null variables

## Save files

``` r
write_csv(hourly_calories, output_file)
file.move(output_file, cleaned_data_path, overwrite = TRUE)
```

    ## 1 file moved. 0 failed.
