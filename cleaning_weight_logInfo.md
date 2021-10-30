Weight Log Info
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

input_file <- "D:/Projects/Bellabeat/original_data/weightLogInfo_merged.csv"

cleaned_data_path <- "D:/Projects/Bellabeat/cleaned_data"

output_file <- "weight_log_info.csv"
```

## Read file and formatting

``` r
weight_log_info <- read_csv(input_file)
```

    ## Rows: 67 Columns: 8

    ## -- Column specification --------------------------------------------------------
    ## Delimiter: ","
    ## chr (1): Date
    ## dbl (6): Id, WeightKg, WeightPounds, Fat, BMI, LogId
    ## lgl (1): IsManualReport

    ## 
    ## i Use `spec()` to retrieve the full column specification for this data.
    ## i Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
glimpse(weight_log_info)
```

    ## Rows: 67
    ## Columns: 8
    ## $ Id             <dbl> 1503960366, 1503960366, 1927972279, 2873212765, 2873212~
    ## $ Date           <chr> "5/2/2016 11:59:59 PM", "5/3/2016 11:59:59 PM", "4/13/2~
    ## $ WeightKg       <dbl> 52.6, 52.6, 133.5, 56.7, 57.3, 72.4, 72.3, 69.7, 70.3, ~
    ## $ WeightPounds   <dbl> 115.9631, 115.9631, 294.3171, 125.0021, 126.3249, 159.6~
    ## $ Fat            <dbl> 22, NA, NA, NA, NA, 25, NA, NA, NA, NA, NA, NA, NA, NA,~
    ## $ BMI            <dbl> 22.65, 22.65, 47.54, 21.45, 21.69, 27.45, 27.38, 27.25,~
    ## $ IsManualReport <lgl> TRUE, TRUE, FALSE, TRUE, TRUE, TRUE, TRUE, TRUE, TRUE, ~
    ## $ LogId          <dbl> 1.462234e+12, 1.462320e+12, 1.460510e+12, 1.461283e+12,~

``` r
weight_log_info <- mutate(weight_log_info, 
               Id = as.character(Id),
               Date = ymd_hms(as.POSIXct(Date, format="%m/%d/%Y %I:%M:%S %p", tz = "UTC")),
               WeightKg = as.double(WeightKg),
               WeightPounds = as.double(WeightPounds),
               Fat = as.double(Fat),
               BMI = as.double(BMI),
               IsManualReport = as.logical(IsManualReport),
               LogId = as.double(LogId)
               )

weight_log_info <- rename(
  weight_log_info,
  user_id = Id,
  activity_date_time = Date,
  weight_kg = WeightKg,
  weight_pounds = WeightPounds,
  fat_measure = Fat,
  bmi_measure = BMI,
  is_manual_report = IsManualReport,
  log_id = LogId
)

weight_log_info <- weight_log_info %>% distinct()

glimpse(weight_log_info)
```

    ## Rows: 67
    ## Columns: 8
    ## $ user_id            <chr> "1503960366", "1503960366", "1927972279", "28732127~
    ## $ activity_date_time <dttm> 2016-05-02 23:59:59, 2016-05-03 23:59:59, 2016-04-~
    ## $ weight_kg          <dbl> 52.6, 52.6, 133.5, 56.7, 57.3, 72.4, 72.3, 69.7, 70~
    ## $ weight_pounds      <dbl> 115.9631, 115.9631, 294.3171, 125.0021, 126.3249, 1~
    ## $ fat_measure        <dbl> 22, NA, NA, NA, NA, 25, NA, NA, NA, NA, NA, NA, NA,~
    ## $ bmi_measure        <dbl> 22.65, 22.65, 47.54, 21.45, 21.69, 27.45, 27.38, 27~
    ## $ is_manual_report   <lgl> TRUE, TRUE, FALSE, TRUE, TRUE, TRUE, TRUE, TRUE, TR~
    ## $ log_id             <dbl> 1.462234e+12, 1.462320e+12, 1.460510e+12, 1.461283e~

``` r
weight_log_info %>% select(user_id) %>% distinct()
```

    ## # A tibble: 8 x 1
    ##   user_id   
    ##   <chr>     
    ## 1 1503960366
    ## 2 1927972279
    ## 3 2873212765
    ## 4 4319703577
    ## 5 4558609924
    ## 6 5577150313
    ## 7 6962181067
    ## 8 8877689391

``` r
weight_log_info %>% filter(is.na(user_id))
```

    ## # A tibble: 0 x 8
    ## # ... with 8 variables: user_id <chr>, activity_date_time <dttm>,
    ## #   weight_kg <dbl>, weight_pounds <dbl>, fat_measure <dbl>, bmi_measure <dbl>,
    ## #   is_manual_report <lgl>, log_id <dbl>

``` r
weight_log_info %>% filter(is.na(activity_date_time))
```

    ## # A tibble: 0 x 8
    ## # ... with 8 variables: user_id <chr>, activity_date_time <dttm>,
    ## #   weight_kg <dbl>, weight_pounds <dbl>, fat_measure <dbl>, bmi_measure <dbl>,
    ## #   is_manual_report <lgl>, log_id <dbl>

``` r
weight_log_info %>% filter(is.na(weight_kg))
```

    ## # A tibble: 0 x 8
    ## # ... with 8 variables: user_id <chr>, activity_date_time <dttm>,
    ## #   weight_kg <dbl>, weight_pounds <dbl>, fat_measure <dbl>, bmi_measure <dbl>,
    ## #   is_manual_report <lgl>, log_id <dbl>

``` r
weight_log_info %>% filter(is.na(weight_pounds))
```

    ## # A tibble: 0 x 8
    ## # ... with 8 variables: user_id <chr>, activity_date_time <dttm>,
    ## #   weight_kg <dbl>, weight_pounds <dbl>, fat_measure <dbl>, bmi_measure <dbl>,
    ## #   is_manual_report <lgl>, log_id <dbl>

``` r
weight_log_info %>% filter(is.na(fat_measure))
```

    ## # A tibble: 65 x 8
    ##    user_id    activity_date_time  weight_kg weight_pounds fat_measure bmi_measure
    ##    <chr>      <dttm>                  <dbl>         <dbl>       <dbl>       <dbl>
    ##  1 1503960366 2016-05-03 23:59:59      52.6          116.          NA        22.6
    ##  2 1927972279 2016-04-13 01:08:52     134.           294.          NA        47.5
    ##  3 2873212765 2016-04-21 23:59:59      56.7          125.          NA        21.5
    ##  4 2873212765 2016-05-12 23:59:59      57.3          126.          NA        21.7
    ##  5 4319703577 2016-05-04 23:59:59      72.3          159.          NA        27.4
    ##  6 4558609924 2016-04-18 23:59:59      69.7          154.          NA        27.2
    ##  7 4558609924 2016-04-25 23:59:59      70.3          155.          NA        27.5
    ##  8 4558609924 2016-05-01 23:59:59      69.9          154.          NA        27.3
    ##  9 4558609924 2016-05-02 23:59:59      69.2          153.          NA        27.0
    ## 10 4558609924 2016-05-09 23:59:59      69.1          152.          NA        27  
    ## # ... with 55 more rows, and 2 more variables: is_manual_report <lgl>,
    ## #   log_id <dbl>

``` r
weight_log_info %>% filter(is.na(bmi_measure))
```

    ## # A tibble: 0 x 8
    ## # ... with 8 variables: user_id <chr>, activity_date_time <dttm>,
    ## #   weight_kg <dbl>, weight_pounds <dbl>, fat_measure <dbl>, bmi_measure <dbl>,
    ## #   is_manual_report <lgl>, log_id <dbl>

``` r
weight_log_info %>% filter(is.na(is_manual_report))
```

    ## # A tibble: 0 x 8
    ## # ... with 8 variables: user_id <chr>, activity_date_time <dttm>,
    ## #   weight_kg <dbl>, weight_pounds <dbl>, fat_measure <dbl>, bmi_measure <dbl>,
    ## #   is_manual_report <lgl>, log_id <dbl>

``` r
weight_log_info %>% filter(is.na(log_id))
```

    ## # A tibble: 0 x 8
    ## # ... with 8 variables: user_id <chr>, activity_date_time <dttm>,
    ## #   weight_kg <dbl>, weight_pounds <dbl>, fat_measure <dbl>, bmi_measure <dbl>,
    ## #   is_manual_report <lgl>, log_id <dbl>

``` r
weight_log_info <- weight_log_info %>% select(-fat_measure)
glimpse(weight_log_info)
```

    ## Rows: 67
    ## Columns: 7
    ## $ user_id            <chr> "1503960366", "1503960366", "1927972279", "28732127~
    ## $ activity_date_time <dttm> 2016-05-02 23:59:59, 2016-05-03 23:59:59, 2016-04-~
    ## $ weight_kg          <dbl> 52.6, 52.6, 133.5, 56.7, 57.3, 72.4, 72.3, 69.7, 70~
    ## $ weight_pounds      <dbl> 115.9631, 115.9631, 294.3171, 125.0021, 126.3249, 1~
    ## $ bmi_measure        <dbl> 22.65, 22.65, 47.54, 21.45, 21.69, 27.45, 27.38, 27~
    ## $ is_manual_report   <lgl> TRUE, TRUE, FALSE, TRUE, TRUE, TRUE, TRUE, TRUE, TR~
    ## $ log_id             <dbl> 1.462234e+12, 1.462320e+12, 1.460510e+12, 1.461283e~

-   8 unique user ids
-   Fat content measure is null in most cases
-   No null variables

## Save files

``` r
write_csv(weight_log_info, output_file)
file.move(output_file, cleaned_data_path, overwrite = TRUE)
```

    ## 1 file moved. 0 failed.
