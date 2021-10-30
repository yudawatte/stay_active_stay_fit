Hourly Intensities
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

input_file <- "D:/Projects/Bellabeat/original_data/hourlyIntensities_merged.csv"

cleaned_data_path <- "D:/Projects/Bellabeat/cleaned_data"

output_file <- "hourly_intensities.csv"
```

## Read file and formatting

``` r
hourly_intensities <- read_csv(input_file)
```

    ## Rows: 22099 Columns: 4

    ## -- Column specification --------------------------------------------------------
    ## Delimiter: ","
    ## chr (1): ActivityHour
    ## dbl (3): Id, TotalIntensity, AverageIntensity

    ## 
    ## i Use `spec()` to retrieve the full column specification for this data.
    ## i Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
glimpse(hourly_intensities)
```

    ## Rows: 22,099
    ## Columns: 4
    ## $ Id               <dbl> 1503960366, 1503960366, 1503960366, 1503960366, 15039~
    ## $ ActivityHour     <chr> "4/12/2016 12:00:00 AM", "4/12/2016 1:00:00 AM", "4/1~
    ## $ TotalIntensity   <dbl> 20, 8, 7, 0, 0, 0, 0, 0, 13, 30, 29, 12, 11, 6, 36, 5~
    ## $ AverageIntensity <dbl> 0.333333, 0.133333, 0.116667, 0.000000, 0.000000, 0.0~

``` r
hourly_intensities <- mutate(hourly_intensities, 
               Id = as.character(Id),
               ActivityHour = ymd_hms(as.POSIXct(ActivityHour, format="%m/%d/%Y %I:%M:%S %p", tz = "UTC")),
               TotalIntensity = as.double(TotalIntensity),
               AverageIntensity = as.double(AverageIntensity)
               )

hourly_intensities <- rename(
  hourly_intensities,
  user_id = Id,
  activity_hour = ActivityHour,
  total_intensity = TotalIntensity,
  average_intensity = AverageIntensity
)

hourly_intensities <- hourly_intensities %>% distinct()

glimpse(hourly_intensities)
```

    ## Rows: 22,099
    ## Columns: 4
    ## $ user_id           <chr> "1503960366", "1503960366", "1503960366", "150396036~
    ## $ activity_hour     <dttm> 2016-04-12 00:00:00, 2016-04-12 01:00:00, 2016-04-1~
    ## $ total_intensity   <dbl> 20, 8, 7, 0, 0, 0, 0, 0, 13, 30, 29, 12, 11, 6, 36, ~
    ## $ average_intensity <dbl> 0.333333, 0.133333, 0.116667, 0.000000, 0.000000, 0.~

``` r
hourly_intensities %>% select(user_id) %>% distinct()
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
hourly_intensities %>% filter(is.na(user_id))
```

    ## # A tibble: 0 x 4
    ## # ... with 4 variables: user_id <chr>, activity_hour <dttm>,
    ## #   total_intensity <dbl>, average_intensity <dbl>

``` r
hourly_intensities %>% filter(is.na(activity_hour))
```

    ## # A tibble: 0 x 4
    ## # ... with 4 variables: user_id <chr>, activity_hour <dttm>,
    ## #   total_intensity <dbl>, average_intensity <dbl>

``` r
hourly_intensities %>% filter(is.na(total_intensity))
```

    ## # A tibble: 0 x 4
    ## # ... with 4 variables: user_id <chr>, activity_hour <dttm>,
    ## #   total_intensity <dbl>, average_intensity <dbl>

``` r
hourly_intensities %>% filter(is.na(average_intensity))
```

    ## # A tibble: 0 x 4
    ## # ... with 4 variables: user_id <chr>, activity_hour <dttm>,
    ## #   total_intensity <dbl>, average_intensity <dbl>

-   33 unique user ids
-   All users don’t have the same number of daily records
-   No null variables

## Save files

``` r
write_csv(hourly_intensities, output_file)
file.move(output_file, cleaned_data_path, overwrite = TRUE)
```

    ## 1 file moved. 0 failed.
