Daily Activities
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

input_file <- "D:/Projects/Bellabeat/original_data/dailyActivity_merged.csv"

cleaned_data_path <- "D:/Projects/Bellabeat/cleaned_data"

output_file <- "daily_activities.csv"
```

## Read file and formatting

``` r
daily_activity <- read_csv(input_file)
```

    ## Rows: 940 Columns: 15

    ## -- Column specification --------------------------------------------------------
    ## Delimiter: ","
    ## chr  (1): ActivityDate
    ## dbl (14): Id, TotalSteps, TotalDistance, TrackerDistance, LoggedActivitiesDi...

    ## 
    ## i Use `spec()` to retrieve the full column specification for this data.
    ## i Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
daily_activity <- mutate(daily_activity, 
               Id = as.character(Id),
               ActivityDate = as.Date(ActivityDate, "%m/%d/%Y"),
               TotalSteps = as.integer(TotalSteps),
               TotalDistance = as.double(TotalDistance),
               TrackerDistance = as.double(TrackerDistance),
               LoggedActivitiesDistance = as.double(LoggedActivitiesDistance),
               VeryActiveDistance = as.double(VeryActiveDistance),
               ModeratelyActiveDistance = as.double(ModeratelyActiveDistance),
               LightActiveDistance = as.double(LightActiveDistance),
               SedentaryActiveDistance = as.double(SedentaryActiveDistance),
               VeryActiveMinutes = as.double(VeryActiveMinutes),
               FairlyActiveMinutes = as.double(FairlyActiveMinutes),
               LightlyActiveMinutes = as.double(LightlyActiveMinutes),
               SedentaryMinutes = as.double(SedentaryMinutes),
               Calories = as.double(Calories)
               )

daily_activity <- rename(
  daily_activity,
  user_id = Id,
  activity_date = ActivityDate,
  total_steps = TotalSteps,
  total_distance = TotalDistance,
  tracker_distance = TrackerDistance,
  logged_activity_distance = LoggedActivitiesDistance,
  very_active_distance = VeryActiveDistance,
  moderately_active_distance = ModeratelyActiveDistance,
  lightly_active_distance = LightActiveDistance,
  sedentary_active_distance = SedentaryActiveDistance,
  very_active_minutes = VeryActiveMinutes,
  moderately_active_minutes = FairlyActiveMinutes,
  lightly_active_minutes = LightlyActiveMinutes,
  sedentary_active_minutes = SedentaryMinutes,
  calories_burn = Calories
)

glimpse(daily_activity)
```

    ## Rows: 940
    ## Columns: 15
    ## $ user_id                    <chr> "1503960366", "1503960366", "1503960366", "~
    ## $ activity_date              <date> 2016-04-12, 2016-04-13, 2016-04-14, 2016-0~
    ## $ total_steps                <int> 13162, 10735, 10460, 9762, 12669, 9705, 130~
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
daily_activity <- daily_activity %>% distinct()

glimpse(daily_activity)
```

    ## Rows: 940
    ## Columns: 15
    ## $ user_id                    <chr> "1503960366", "1503960366", "1503960366", "~
    ## $ activity_date              <date> 2016-04-12, 2016-04-13, 2016-04-14, 2016-0~
    ## $ total_steps                <int> 13162, 10735, 10460, 9762, 12669, 9705, 130~
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
daily_activity %>% filter(is.na(user_id))
```

    ## # A tibble: 0 x 15
    ## # ... with 15 variables: user_id <chr>, activity_date <date>,
    ## #   total_steps <int>, total_distance <dbl>, tracker_distance <dbl>,
    ## #   logged_activity_distance <dbl>, very_active_distance <dbl>,
    ## #   moderately_active_distance <dbl>, lightly_active_distance <dbl>,
    ## #   sedentary_active_distance <dbl>, very_active_minutes <dbl>,
    ## #   moderately_active_minutes <dbl>, lightly_active_minutes <dbl>,
    ## #   sedentary_active_minutes <dbl>, calories_burn <dbl>

``` r
daily_activity %>% filter(is.na(activity_date))
```

    ## # A tibble: 0 x 15
    ## # ... with 15 variables: user_id <chr>, activity_date <date>,
    ## #   total_steps <int>, total_distance <dbl>, tracker_distance <dbl>,
    ## #   logged_activity_distance <dbl>, very_active_distance <dbl>,
    ## #   moderately_active_distance <dbl>, lightly_active_distance <dbl>,
    ## #   sedentary_active_distance <dbl>, very_active_minutes <dbl>,
    ## #   moderately_active_minutes <dbl>, lightly_active_minutes <dbl>,
    ## #   sedentary_active_minutes <dbl>, calories_burn <dbl>

``` r
daily_activity %>% filter(is.na(sedentary_active_minutes))
```

    ## # A tibble: 0 x 15
    ## # ... with 15 variables: user_id <chr>, activity_date <date>,
    ## #   total_steps <int>, total_distance <dbl>, tracker_distance <dbl>,
    ## #   logged_activity_distance <dbl>, very_active_distance <dbl>,
    ## #   moderately_active_distance <dbl>, lightly_active_distance <dbl>,
    ## #   sedentary_active_distance <dbl>, very_active_minutes <dbl>,
    ## #   moderately_active_minutes <dbl>, lightly_active_minutes <dbl>,
    ## #   sedentary_active_minutes <dbl>, calories_burn <dbl>

``` r
daily_activity %>% filter(is.na(lightly_active_minutes))
```

    ## # A tibble: 0 x 15
    ## # ... with 15 variables: user_id <chr>, activity_date <date>,
    ## #   total_steps <int>, total_distance <dbl>, tracker_distance <dbl>,
    ## #   logged_activity_distance <dbl>, very_active_distance <dbl>,
    ## #   moderately_active_distance <dbl>, lightly_active_distance <dbl>,
    ## #   sedentary_active_distance <dbl>, very_active_minutes <dbl>,
    ## #   moderately_active_minutes <dbl>, lightly_active_minutes <dbl>,
    ## #   sedentary_active_minutes <dbl>, calories_burn <dbl>

``` r
daily_activity %>% filter(is.na(moderately_active_minutes))
```

    ## # A tibble: 0 x 15
    ## # ... with 15 variables: user_id <chr>, activity_date <date>,
    ## #   total_steps <int>, total_distance <dbl>, tracker_distance <dbl>,
    ## #   logged_activity_distance <dbl>, very_active_distance <dbl>,
    ## #   moderately_active_distance <dbl>, lightly_active_distance <dbl>,
    ## #   sedentary_active_distance <dbl>, very_active_minutes <dbl>,
    ## #   moderately_active_minutes <dbl>, lightly_active_minutes <dbl>,
    ## #   sedentary_active_minutes <dbl>, calories_burn <dbl>

``` r
daily_activity %>% filter(is.na(very_active_minutes))
```

    ## # A tibble: 0 x 15
    ## # ... with 15 variables: user_id <chr>, activity_date <date>,
    ## #   total_steps <int>, total_distance <dbl>, tracker_distance <dbl>,
    ## #   logged_activity_distance <dbl>, very_active_distance <dbl>,
    ## #   moderately_active_distance <dbl>, lightly_active_distance <dbl>,
    ## #   sedentary_active_distance <dbl>, very_active_minutes <dbl>,
    ## #   moderately_active_minutes <dbl>, lightly_active_minutes <dbl>,
    ## #   sedentary_active_minutes <dbl>, calories_burn <dbl>

``` r
daily_activity %>% filter(is.na(sedentary_active_distance))
```

    ## # A tibble: 0 x 15
    ## # ... with 15 variables: user_id <chr>, activity_date <date>,
    ## #   total_steps <int>, total_distance <dbl>, tracker_distance <dbl>,
    ## #   logged_activity_distance <dbl>, very_active_distance <dbl>,
    ## #   moderately_active_distance <dbl>, lightly_active_distance <dbl>,
    ## #   sedentary_active_distance <dbl>, very_active_minutes <dbl>,
    ## #   moderately_active_minutes <dbl>, lightly_active_minutes <dbl>,
    ## #   sedentary_active_minutes <dbl>, calories_burn <dbl>

``` r
daily_activity %>% filter(is.na(lightly_active_distance))
```

    ## # A tibble: 0 x 15
    ## # ... with 15 variables: user_id <chr>, activity_date <date>,
    ## #   total_steps <int>, total_distance <dbl>, tracker_distance <dbl>,
    ## #   logged_activity_distance <dbl>, very_active_distance <dbl>,
    ## #   moderately_active_distance <dbl>, lightly_active_distance <dbl>,
    ## #   sedentary_active_distance <dbl>, very_active_minutes <dbl>,
    ## #   moderately_active_minutes <dbl>, lightly_active_minutes <dbl>,
    ## #   sedentary_active_minutes <dbl>, calories_burn <dbl>

``` r
daily_activity %>% filter(is.na(moderately_active_distance))
```

    ## # A tibble: 0 x 15
    ## # ... with 15 variables: user_id <chr>, activity_date <date>,
    ## #   total_steps <int>, total_distance <dbl>, tracker_distance <dbl>,
    ## #   logged_activity_distance <dbl>, very_active_distance <dbl>,
    ## #   moderately_active_distance <dbl>, lightly_active_distance <dbl>,
    ## #   sedentary_active_distance <dbl>, very_active_minutes <dbl>,
    ## #   moderately_active_minutes <dbl>, lightly_active_minutes <dbl>,
    ## #   sedentary_active_minutes <dbl>, calories_burn <dbl>

``` r
daily_activity %>% filter(is.na(very_active_distance))
```

    ## # A tibble: 0 x 15
    ## # ... with 15 variables: user_id <chr>, activity_date <date>,
    ## #   total_steps <int>, total_distance <dbl>, tracker_distance <dbl>,
    ## #   logged_activity_distance <dbl>, very_active_distance <dbl>,
    ## #   moderately_active_distance <dbl>, lightly_active_distance <dbl>,
    ## #   sedentary_active_distance <dbl>, very_active_minutes <dbl>,
    ## #   moderately_active_minutes <dbl>, lightly_active_minutes <dbl>,
    ## #   sedentary_active_minutes <dbl>, calories_burn <dbl>

``` r
daily_activity %>% filter(is.na(total_steps))
```

    ## # A tibble: 0 x 15
    ## # ... with 15 variables: user_id <chr>, activity_date <date>,
    ## #   total_steps <int>, total_distance <dbl>, tracker_distance <dbl>,
    ## #   logged_activity_distance <dbl>, very_active_distance <dbl>,
    ## #   moderately_active_distance <dbl>, lightly_active_distance <dbl>,
    ## #   sedentary_active_distance <dbl>, very_active_minutes <dbl>,
    ## #   moderately_active_minutes <dbl>, lightly_active_minutes <dbl>,
    ## #   sedentary_active_minutes <dbl>, calories_burn <dbl>

``` r
daily_activity %>% filter(is.na(total_distance))
```

    ## # A tibble: 0 x 15
    ## # ... with 15 variables: user_id <chr>, activity_date <date>,
    ## #   total_steps <int>, total_distance <dbl>, tracker_distance <dbl>,
    ## #   logged_activity_distance <dbl>, very_active_distance <dbl>,
    ## #   moderately_active_distance <dbl>, lightly_active_distance <dbl>,
    ## #   sedentary_active_distance <dbl>, very_active_minutes <dbl>,
    ## #   moderately_active_minutes <dbl>, lightly_active_minutes <dbl>,
    ## #   sedentary_active_minutes <dbl>, calories_burn <dbl>

``` r
daily_activity %>% filter(is.na(tracker_distance))
```

    ## # A tibble: 0 x 15
    ## # ... with 15 variables: user_id <chr>, activity_date <date>,
    ## #   total_steps <int>, total_distance <dbl>, tracker_distance <dbl>,
    ## #   logged_activity_distance <dbl>, very_active_distance <dbl>,
    ## #   moderately_active_distance <dbl>, lightly_active_distance <dbl>,
    ## #   sedentary_active_distance <dbl>, very_active_minutes <dbl>,
    ## #   moderately_active_minutes <dbl>, lightly_active_minutes <dbl>,
    ## #   sedentary_active_minutes <dbl>, calories_burn <dbl>

``` r
daily_activity %>% filter(is.na(logged_activity_distance))
```

    ## # A tibble: 0 x 15
    ## # ... with 15 variables: user_id <chr>, activity_date <date>,
    ## #   total_steps <int>, total_distance <dbl>, tracker_distance <dbl>,
    ## #   logged_activity_distance <dbl>, very_active_distance <dbl>,
    ## #   moderately_active_distance <dbl>, lightly_active_distance <dbl>,
    ## #   sedentary_active_distance <dbl>, very_active_minutes <dbl>,
    ## #   moderately_active_minutes <dbl>, lightly_active_minutes <dbl>,
    ## #   sedentary_active_minutes <dbl>, calories_burn <dbl>

``` r
daily_activity %>% filter(is.na(calories_burn))
```

    ## # A tibble: 0 x 15
    ## # ... with 15 variables: user_id <chr>, activity_date <date>,
    ## #   total_steps <int>, total_distance <dbl>, tracker_distance <dbl>,
    ## #   logged_activity_distance <dbl>, very_active_distance <dbl>,
    ## #   moderately_active_distance <dbl>, lightly_active_distance <dbl>,
    ## #   sedentary_active_distance <dbl>, very_active_minutes <dbl>,
    ## #   moderately_active_minutes <dbl>, lightly_active_minutes <dbl>,
    ## #   sedentary_active_minutes <dbl>, calories_burn <dbl>

-   31 unique user ids
-   All users don’t the have same number of daily records
-   No null variables

## Save files

``` r
write_csv(daily_activity, output_file)
file.move(output_file, cleaned_data_path, overwrite = TRUE)
```

    ## 1 file moved. 0 failed.
