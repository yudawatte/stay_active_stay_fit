Measure Types
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

cleaned_data_path <- "D:/Projects/Bellabeat/cleaned_data"

output_file <- "measurement_types.csv"
```

## Read file and formatting

``` r
distance_measures <- read_csv("D:/Projects/Bellabeat/cleaned_data/daily_activities.csv")
```

    ## Rows: 940 Columns: 15

    ## -- Column specification --------------------------------------------------------
    ## Delimiter: ","
    ## dbl  (14): user_id, total_steps, total_distance, tracker_distance, logged_ac...
    ## date  (1): activity_date

    ## 
    ## i Use `spec()` to retrieve the full column specification for this data.
    ## i Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
glimpse(distance_measures)
```

    ## Rows: 940
    ## Columns: 15
    ## $ user_id                    <dbl> 1503960366, 1503960366, 1503960366, 1503960~
    ## $ activity_date              <date> 2016-04-12, 2016-04-13, 2016-04-14, 2016-0~
    ## $ total_steps                <dbl> 13162, 10735, 10460, 9762, 12669, 9705, 130~
    ## $ total_distance             <dbl> 8.50, 6.97, 6.74, 6.28, 8.16, 6.48, 8.59, 9~
    ## $ tracker_distance           <dbl> 8.50, 6.97, 6.74, 6.28, 8.16, 6.48, 8.59, 9~
    ## $ logged_activity_distance   <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0~
    ## $ very_active_distance       <dbl> 1.88, 1.57, 2.44, 2.14, 2.71, 3.19, 3.25, 3~
    ## $ moderately_active_distance <dbl> 0.55, 0.69, 0.40, 1.26, 0.41, 0.78, 0.64, 1~
    ## $ lightly_active_distance    <dbl> 6.06, 4.71, 3.91, 2.83, 5.04, 2.51, 4.71, 5~
    ## $ sedentary_active_distance  <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0~
    ## $ very_active_minutes        <dbl> 25, 21, 30, 29, 36, 38, 42, 50, 28, 19, 66,~
    ## $ moderately_active_minutes  <dbl> 13, 19, 11, 34, 10, 20, 16, 31, 12, 8, 27, ~
    ## $ lightly_active_minutes     <dbl> 328, 217, 181, 209, 221, 164, 233, 264, 205~
    ## $ sedentary_active_minutes   <dbl> 728, 776, 1218, 726, 773, 539, 1149, 775, 8~
    ## $ calories_burn              <dbl> 1985, 1797, 1776, 1745, 1863, 1728, 1921, 2~

``` r
n <- distance_measures %>% select(user_id) %>% distinct() %>% count()
measurement_type <- data.frame(measurement = "Distance", n)


calory_measures <- read_csv("D:/Projects/Bellabeat/cleaned_data/daily_calories.csv")
```

    ## Rows: 940 Columns: 3

    ## -- Column specification --------------------------------------------------------
    ## Delimiter: ","
    ## dbl  (2): user_id, calories_burn
    ## date (1): activity_date

    ## 
    ## i Use `spec()` to retrieve the full column specification for this data.
    ## i Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
n <- calory_measures %>% select(user_id) %>% distinct() %>% count()
measurement_type_temp <- data.frame(measurement = "Calories", n)
measurement_type <- rbind(measurement_type, measurement_type_temp)


intensity_measures <- read_csv("D:/Projects/Bellabeat/cleaned_data/daily_intensities.csv")
```

    ## Rows: 940 Columns: 10

    ## -- Column specification --------------------------------------------------------
    ## Delimiter: ","
    ## dbl  (9): user_id, very_active_distance, moderately_active_distance, lightly...
    ## date (1): activity_date

    ## 
    ## i Use `spec()` to retrieve the full column specification for this data.
    ## i Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
glimpse(intensity_measures)
```

    ## Rows: 940
    ## Columns: 10
    ## $ user_id                    <dbl> 1503960366, 1503960366, 1503960366, 1503960~
    ## $ activity_date              <date> 2016-04-12, 2016-04-13, 2016-04-14, 2016-0~
    ## $ very_active_distance       <dbl> 1.88, 1.57, 2.44, 2.14, 2.71, 3.19, 3.25, 3~
    ## $ moderately_active_distance <dbl> 0.55, 0.69, 0.40, 1.26, 0.41, 0.78, 0.64, 1~
    ## $ lightly_active_distance    <dbl> 6.06, 4.71, 3.91, 2.83, 5.04, 2.51, 4.71, 5~
    ## $ sedentary_active_distance  <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0~
    ## $ very_active_minutes        <dbl> 25, 21, 30, 29, 36, 38, 42, 50, 28, 19, 66,~
    ## $ moderately_active_minutes  <dbl> 13, 19, 11, 34, 10, 20, 16, 31, 12, 8, 27, ~
    ## $ lightly_active_minutes     <dbl> 328, 217, 181, 209, 221, 164, 233, 264, 205~
    ## $ sedentary_active_minutes   <dbl> 728, 776, 1218, 726, 773, 539, 1149, 775, 8~

``` r
n <- intensity_measures %>% select(user_id) %>% distinct() %>% count()
measurement_type_temp <- data.frame(measurement = "Intensities", n)
measurement_type <- rbind(measurement_type, measurement_type_temp)


met_measures <- read_csv("D:/Projects/Bellabeat/cleaned_data/daily_met.csv")
```

    ## Rows: 934 Columns: 3

    ## -- Column specification --------------------------------------------------------
    ## Delimiter: ","
    ## dbl  (2): user_id, avg_mets
    ## date (1): activity_date

    ## 
    ## i Use `spec()` to retrieve the full column specification for this data.
    ## i Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
glimpse(met_measures)
```

    ## Rows: 934
    ## Columns: 3
    ## $ user_id       <dbl> 1503960366, 1503960366, 1503960366, 1503960366, 15039603~
    ## $ activity_date <date> 2016-04-12, 2016-04-13, 2016-04-14, 2016-04-15, 2016-04~
    ## $ avg_mets      <dbl> 17.52847, 15.87431, 15.68681, 15.40972, 16.45417, 15.258~

``` r
n <- met_measures %>% select(user_id) %>% distinct() %>% count()
measurement_type_temp <- data.frame(measurement = "METs", n)
measurement_type <- rbind(measurement_type, measurement_type_temp)


steps_measures <- read_csv("D:/Projects/Bellabeat/cleaned_data/daily_steps.csv")
```

    ## Rows: 940 Columns: 3

    ## -- Column specification --------------------------------------------------------
    ## Delimiter: ","
    ## dbl  (2): user_id, total_steps
    ## date (1): activity_date

    ## 
    ## i Use `spec()` to retrieve the full column specification for this data.
    ## i Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
glimpse(steps_measures)
```

    ## Rows: 940
    ## Columns: 3
    ## $ user_id       <dbl> 1503960366, 1503960366, 1503960366, 1503960366, 15039603~
    ## $ activity_date <date> 2016-04-12, 2016-04-13, 2016-04-14, 2016-04-15, 2016-04~
    ## $ total_steps   <dbl> 13162, 10735, 10460, 9762, 12669, 9705, 13019, 15506, 10~

``` r
n <- steps_measures %>% select(user_id) %>% distinct() %>% count()
measurement_type_temp <- data.frame(measurement = "Steps", n)
measurement_type <- rbind(measurement_type, measurement_type_temp)


heart_rate_measures <- read_csv("D:/Projects/Bellabeat/cleaned_data/heart_rate_in_seconds.csv")
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
glimpse(heart_rate_measures)
```

    ## Rows: 2,483,658
    ## Columns: 3
    ## $ user_id            <dbl> 2022484408, 2022484408, 2022484408, 2022484408, 202~
    ## $ activity_date_time <dttm> 2016-04-12 07:21:00, 2016-04-12 07:21:05, 2016-04-~
    ## $ heart_rate         <dbl> 97, 102, 105, 103, 101, 95, 91, 93, 94, 93, 92, 89,~

``` r
n <- heart_rate_measures %>% select(user_id) %>% distinct() %>% count()
measurement_type_temp <- data.frame(measurement = "Heart rate", n)
measurement_type <- rbind(measurement_type, measurement_type_temp)


weight_measures <- read_csv("D:/Projects/Bellabeat/cleaned_data/weight_log_info.csv")
```

    ## Rows: 67 Columns: 7

    ## -- Column specification --------------------------------------------------------
    ## Delimiter: ","
    ## dbl  (5): user_id, weight_kg, weight_pounds, bmi_measure, log_id
    ## lgl  (1): is_manual_report
    ## dttm (1): activity_date_time

    ## 
    ## i Use `spec()` to retrieve the full column specification for this data.
    ## i Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
glimpse(weight_measures)
```

    ## Rows: 67
    ## Columns: 7
    ## $ user_id            <dbl> 1503960366, 1503960366, 1927972279, 2873212765, 287~
    ## $ activity_date_time <dttm> 2016-05-02 23:59:59, 2016-05-03 23:59:59, 2016-04-~
    ## $ weight_kg          <dbl> 52.6, 52.6, 133.5, 56.7, 57.3, 72.4, 72.3, 69.7, 70~
    ## $ weight_pounds      <dbl> 115.9631, 115.9631, 294.3171, 125.0021, 126.3249, 1~
    ## $ bmi_measure        <dbl> 22.65, 22.65, 47.54, 21.45, 21.69, 27.45, 27.38, 27~
    ## $ is_manual_report   <lgl> TRUE, TRUE, FALSE, TRUE, TRUE, TRUE, TRUE, TRUE, TR~
    ## $ log_id             <dbl> 1.462234e+12, 1.462320e+12, 1.460510e+12, 1.461283e~

``` r
n <- weight_measures %>% filter(is_manual_report == FALSE) %>% select(user_id) %>% distinct() %>% count()
measurement_type_temp <- data.frame(measurement = "Weight/BMI", n)
measurement_type <- rbind(measurement_type, measurement_type_temp)

measurement_type <- rename(measurement_type, no_of_users = n)

glimpse(measurement_type)
```

    ## Rows: 7
    ## Columns: 2
    ## $ measurement <chr> "Distance", "Calories", "Intensities", "METs", "Steps", "H~
    ## $ no_of_users <int> 33, 33, 33, 33, 33, 14, 3

## Save files

``` r
write_csv(measurement_type, output_file)
file.move(output_file, cleaned_data_path, overwrite = TRUE)
```

    ## 1 file moved. 0 failed.
