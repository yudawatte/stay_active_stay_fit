Minute Steps Wide
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

input_file <- "D:/Projects/Bellabeat/original_data/minuteStepsWide_merged.csv"

cleaned_data_path <- "D:/Projects/Bellabeat/cleaned_data"

output_file <- "minute_steps_wide.csv"
```

## Read file and formatting

``` r
minitue_steps <- read_csv(input_file)
```

    ## Rows: 21645 Columns: 62

    ## -- Column specification --------------------------------------------------------
    ## Delimiter: ","
    ## chr  (1): ActivityHour
    ## dbl (61): Id, Steps00, Steps01, Steps02, Steps03, Steps04, Steps05, Steps06,...

    ## 
    ## i Use `spec()` to retrieve the full column specification for this data.
    ## i Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
glimpse(minitue_steps)
```

    ## Rows: 21,645
    ## Columns: 62
    ## $ Id           <dbl> 1503960366, 1503960366, 1503960366, 1503960366, 150396036~
    ## $ ActivityHour <chr> "4/13/2016 12:00:00 AM", "4/13/2016 1:00:00 AM", "4/13/20~
    ## $ Steps00      <dbl> 4, 0, 0, 0, 0, 0, 0, 0, 0, 0, 37, 0, 9, 0, 0, 0, 0, 0, 91~
    ## $ Steps01      <dbl> 16, 0, 0, 0, 0, 0, 0, 0, 0, 14, 11, 0, 0, 0, 0, 0, 0, 0, ~
    ## $ Steps02      <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 10, 30, 0, 0, 0, 64, 0, 0, 0, ~
    ## $ Steps03      <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 31, 51, 0, 0, 0, 22, 0, 0, 0, ~
    ## $ Steps04      <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 37, 0, 0, 24, 0, 0, 0, 0, 0, 0~
    ## $ Steps05      <dbl> 9, 0, 0, 0, 0, 0, 0, 0, 0, 17, 0, 0, 0, 0, 0, 0, 0, 0, 32~
    ## $ Steps06      <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 25, 0, 0, 0, 5, 0, 0, 30, 0, 0~
    ## $ Steps07      <dbl> 17, 0, 0, 0, 0, 0, 0, 0, 0, 12, 8, 0, 0, 0, 0, 0, 35, 0, ~
    ## $ Steps08      <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 6, 81, 0, 0, 4, 0, 0, 0, 0, 0,~
    ## $ Steps09      <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 30, 0, 0, 14, 0, 0, 0, 0, 0, 0~
    ## $ Steps10      <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 7, 0, 0, 0, 0, 0, 0, 0, 0, 81,~
    ## $ Steps11      <dbl> 0, 0, 0, 10, 0, 0, 0, 0, 6, 109, 0, 0, 0, 0, 14, 0, 0, 0,~
    ## $ Steps12      <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 140, 0, 29, 9, 0, 31, 0, 23, 0~
    ## $ Steps13      <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 19, 145, 0, 0, 0, 0, 39, 0, 51, 0~
    ## $ Steps14      <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 152, 0, 15, 0, 0, 51, 0, 0, 0,~
    ## $ Steps15      <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 117, 0, 62, 6, 0, 0, 0, 0, 0, ~
    ## $ Steps16      <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 20, 0, 31, 0, 8, 24, 0, 0, 0, ~
    ## $ Steps17      <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 17, 0, 58, 65, 43, 0, 0,~
    ## $ Steps18      <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 110, 15, 92, 0, 0,~
    ## $ Steps19      <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 97, 51, 80, 0, 0, ~
    ## $ Steps20      <dbl> 6, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 127, 10, 0, 0, ~
    ## $ Steps21      <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 7, 27, 0, 5, 126, 9, 0, 0, ~
    ## $ Steps22      <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 32, 0, 0, 17, 0, 4, 126, 0, 0, 0,~
    ## $ Steps23      <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 45, 0, 0, 109, 0, 0, 0, ~
    ## $ Steps24      <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 10, 29, 0, 0, 112, 0, 0, 0,~
    ## $ Steps25      <dbl> 11, 0, 0, 0, 0, 0, 0, 26, 0, 0, 9, 13, 65, 0, 93, 0, 0, 0~
    ## $ Steps26      <dbl> 21, 0, 0, 0, 0, 0, 0, 11, 0, 0, 6, 0, 0, 4, 121, 0, 0, 0,~
    ## $ Steps27      <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 12, 0, 0, 123, 0, 0, 0, ~
    ## $ Steps28      <dbl> 8, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 120, 0, 0, 0, 0~
    ## $ Steps29      <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 70, 0, 0, 0, 0,~
    ## $ Steps30      <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 54, 0, 66, 0, 0~
    ## $ Steps31      <dbl> 0, 0, 0, 0, 0, 0, 0, 9, 0, 0, 0, 0, 0, 0, 20, 0, 66, 0, 0~
    ## $ Steps32      <dbl> 8, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 46, 0, 0, 0, 0, 0, 0,~
    ## $ Steps33      <dbl> 6, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 101, 0, 0, 0, 0, 0, 0~
    ## $ Steps34      <dbl> 0, 0, 0, 11, 0, 0, 0, 0, 0, 0, 0, 0, 82, 0, 0, 0, 0, 0, 0~
    ## $ Steps35      <dbl> 0, 0, 0, 9, 0, 0, 0, 0, 0, 0, 4, 0, 83, 0, 0, 0, 0, 4, 0,~
    ## $ Steps36      <dbl> 0, 0, 0, 6, 0, 0, 0, 0, 45, 21, 0, 8, 8, 0, 0, 0, 0, 0, 0~
    ## $ Steps37      <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 39, 0, 0, 0, 0, 0, 0, 0, 0, 0,~
    ## $ Steps38      <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 7, 84, 0, 65, 0, 0, 0, 0, 0, 0, 0~
    ## $ Steps39      <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 117, 0, 65, 0, 0, 0, 0, 0, 0, ~
    ## $ Steps40      <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 22, 8, 0, 0, 0, 0, 0, 0, 0, 0,~
    ## $ Steps41      <dbl> 0, 0, 0, 0, 0, 0, 0, 28, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0, 27~
    ## $ Steps42      <dbl> 0, 0, 0, 0, 0, 0, 0, 7, 31, 0, 0, 0, 0, 0, 0, 0, 0, 0, 12~
    ## $ Steps43      <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 20, 0, 0, 0, 0, 0, 0, 0, 0, 0, 12~
    ## $ Steps44      <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 16, 0, 0, 0, 0, 0, 0, 0, 11~
    ## $ Steps45      <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 122, 0, 0, 0, 0, 0, 0, 4, 0, 4~
    ## $ Steps46      <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 125, 0, 0, 0, 0, 0, 64, 0, 0, ~
    ## $ Steps47      <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 91, 0, 0, 0, 0, 0, 29, 7, 35, ~
    ## $ Steps48      <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 73, 16, 0, 7, 0, 0, 23, 7, 31,~
    ## $ Steps49      <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 19, 0, 0, 0, 0, 0, 0, 12, 0, 8, 6~
    ## $ Steps50      <dbl> 0, 0, 0, 0, 0, 0, 0, 16, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 11~
    ## $ Steps51      <dbl> 9, 0, 0, 0, 0, 0, 0, 13, 21, 8, 0, 0, 0, 0, 0, 12, 10, 39~
    ## $ Steps52      <dbl> 8, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 27, 0, 13, 1~
    ## $ Steps53      <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 9, 0, 0, 35, 0, 63, 1~
    ## $ Steps54      <dbl> 20, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 57, 0, 36, ~
    ## $ Steps55      <dbl> 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 84, 0, 7, 0, 0, 0, 81, 4~
    ## $ Steps56      <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 31, 0, 0, 5, 0, 0, 0, 0, 0, 47, 2~
    ## $ Steps57      <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 11, 0, 0, 0, 0, 115, ~
    ## $ Steps58      <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 42, 0, 0, 0, 0, 0, 0, 0, 0, 105, ~
    ## $ Steps59      <dbl> 0, 0, 0, 0, 0, 0, 0, 16, 2, 105, 0, 0, 12, 0, 0, 0, 0, 11~

``` r
minitue_steps <- mutate(minitue_steps, 
               Id = as.character(Id),
               ActivityHour = ymd_hms(as.POSIXct(ActivityHour, format="%m/%d/%Y %I:%M:%S %p", tz = "UTC")),
               )

minitue_steps <- rename(
  minitue_steps,
  user_id = Id,
  activity_hour = ActivityHour
)

minitue_steps <- minitue_steps %>% distinct()

glimpse(minitue_steps)
```

    ## Rows: 21,645
    ## Columns: 62
    ## $ user_id       <chr> "1503960366", "1503960366", "1503960366", "1503960366", ~
    ## $ activity_hour <dttm> 2016-04-13 00:00:00, 2016-04-13 01:00:00, 2016-04-13 02~
    ## $ Steps00       <dbl> 4, 0, 0, 0, 0, 0, 0, 0, 0, 0, 37, 0, 9, 0, 0, 0, 0, 0, 9~
    ## $ Steps01       <dbl> 16, 0, 0, 0, 0, 0, 0, 0, 0, 14, 11, 0, 0, 0, 0, 0, 0, 0,~
    ## $ Steps02       <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 10, 30, 0, 0, 0, 64, 0, 0, 0,~
    ## $ Steps03       <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 31, 51, 0, 0, 0, 22, 0, 0, 0,~
    ## $ Steps04       <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 37, 0, 0, 24, 0, 0, 0, 0, 0, ~
    ## $ Steps05       <dbl> 9, 0, 0, 0, 0, 0, 0, 0, 0, 17, 0, 0, 0, 0, 0, 0, 0, 0, 3~
    ## $ Steps06       <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 25, 0, 0, 0, 5, 0, 0, 30, 0, ~
    ## $ Steps07       <dbl> 17, 0, 0, 0, 0, 0, 0, 0, 0, 12, 8, 0, 0, 0, 0, 0, 35, 0,~
    ## $ Steps08       <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 6, 81, 0, 0, 4, 0, 0, 0, 0, 0~
    ## $ Steps09       <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 30, 0, 0, 14, 0, 0, 0, 0, 0, ~
    ## $ Steps10       <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 7, 0, 0, 0, 0, 0, 0, 0, 0, 81~
    ## $ Steps11       <dbl> 0, 0, 0, 10, 0, 0, 0, 0, 6, 109, 0, 0, 0, 0, 14, 0, 0, 0~
    ## $ Steps12       <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 140, 0, 29, 9, 0, 31, 0, 23, ~
    ## $ Steps13       <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 19, 145, 0, 0, 0, 0, 39, 0, 51, ~
    ## $ Steps14       <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 152, 0, 15, 0, 0, 51, 0, 0, 0~
    ## $ Steps15       <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 117, 0, 62, 6, 0, 0, 0, 0, 0,~
    ## $ Steps16       <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 20, 0, 31, 0, 8, 24, 0, 0, 0,~
    ## $ Steps17       <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 17, 0, 58, 65, 43, 0, 0~
    ## $ Steps18       <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 110, 15, 92, 0, 0~
    ## $ Steps19       <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 97, 51, 80, 0, 0,~
    ## $ Steps20       <dbl> 6, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 127, 10, 0, 0,~
    ## $ Steps21       <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 7, 27, 0, 5, 126, 9, 0, 0,~
    ## $ Steps22       <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 32, 0, 0, 17, 0, 4, 126, 0, 0, 0~
    ## $ Steps23       <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 45, 0, 0, 109, 0, 0, 0,~
    ## $ Steps24       <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 10, 29, 0, 0, 112, 0, 0, 0~
    ## $ Steps25       <dbl> 11, 0, 0, 0, 0, 0, 0, 26, 0, 0, 9, 13, 65, 0, 93, 0, 0, ~
    ## $ Steps26       <dbl> 21, 0, 0, 0, 0, 0, 0, 11, 0, 0, 6, 0, 0, 4, 121, 0, 0, 0~
    ## $ Steps27       <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 12, 0, 0, 123, 0, 0, 0,~
    ## $ Steps28       <dbl> 8, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 120, 0, 0, 0, ~
    ## $ Steps29       <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 70, 0, 0, 0, 0~
    ## $ Steps30       <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 54, 0, 66, 0, ~
    ## $ Steps31       <dbl> 0, 0, 0, 0, 0, 0, 0, 9, 0, 0, 0, 0, 0, 0, 20, 0, 66, 0, ~
    ## $ Steps32       <dbl> 8, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 46, 0, 0, 0, 0, 0, 0~
    ## $ Steps33       <dbl> 6, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 101, 0, 0, 0, 0, 0, ~
    ## $ Steps34       <dbl> 0, 0, 0, 11, 0, 0, 0, 0, 0, 0, 0, 0, 82, 0, 0, 0, 0, 0, ~
    ## $ Steps35       <dbl> 0, 0, 0, 9, 0, 0, 0, 0, 0, 0, 4, 0, 83, 0, 0, 0, 0, 4, 0~
    ## $ Steps36       <dbl> 0, 0, 0, 6, 0, 0, 0, 0, 45, 21, 0, 8, 8, 0, 0, 0, 0, 0, ~
    ## $ Steps37       <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 39, 0, 0, 0, 0, 0, 0, 0, 0, 0~
    ## $ Steps38       <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 7, 84, 0, 65, 0, 0, 0, 0, 0, 0, ~
    ## $ Steps39       <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 117, 0, 65, 0, 0, 0, 0, 0, 0,~
    ## $ Steps40       <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 22, 8, 0, 0, 0, 0, 0, 0, 0, 0~
    ## $ Steps41       <dbl> 0, 0, 0, 0, 0, 0, 0, 28, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0, 2~
    ## $ Steps42       <dbl> 0, 0, 0, 0, 0, 0, 0, 7, 31, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1~
    ## $ Steps43       <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 20, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1~
    ## $ Steps44       <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 16, 0, 0, 0, 0, 0, 0, 0, 1~
    ## $ Steps45       <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 122, 0, 0, 0, 0, 0, 0, 4, 0, ~
    ## $ Steps46       <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 125, 0, 0, 0, 0, 0, 64, 0, 0,~
    ## $ Steps47       <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 91, 0, 0, 0, 0, 0, 29, 7, 35,~
    ## $ Steps48       <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 73, 16, 0, 7, 0, 0, 23, 7, 31~
    ## $ Steps49       <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 19, 0, 0, 0, 0, 0, 0, 12, 0, 8, ~
    ## $ Steps50       <dbl> 0, 0, 0, 0, 0, 0, 0, 16, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1~
    ## $ Steps51       <dbl> 9, 0, 0, 0, 0, 0, 0, 13, 21, 8, 0, 0, 0, 0, 0, 12, 10, 3~
    ## $ Steps52       <dbl> 8, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 27, 0, 13, ~
    ## $ Steps53       <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 9, 0, 0, 35, 0, 63, ~
    ## $ Steps54       <dbl> 20, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 57, 0, 36,~
    ## $ Steps55       <dbl> 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 84, 0, 7, 0, 0, 0, 81, ~
    ## $ Steps56       <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 31, 0, 0, 5, 0, 0, 0, 0, 0, 47, ~
    ## $ Steps57       <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 11, 0, 0, 0, 0, 115,~
    ## $ Steps58       <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 42, 0, 0, 0, 0, 0, 0, 0, 0, 105,~
    ## $ Steps59       <dbl> 0, 0, 0, 0, 0, 0, 0, 16, 2, 105, 0, 0, 12, 0, 0, 0, 0, 1~

``` r
minitue_steps %>% select(user_id) %>% distinct()
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
minitue_steps %>% filter(is.na(user_id))
```

    ## # A tibble: 0 x 62
    ## # ... with 62 variables: user_id <chr>, activity_hour <dttm>, Steps00 <dbl>,
    ## #   Steps01 <dbl>, Steps02 <dbl>, Steps03 <dbl>, Steps04 <dbl>, Steps05 <dbl>,
    ## #   Steps06 <dbl>, Steps07 <dbl>, Steps08 <dbl>, Steps09 <dbl>, Steps10 <dbl>,
    ## #   Steps11 <dbl>, Steps12 <dbl>, Steps13 <dbl>, Steps14 <dbl>, Steps15 <dbl>,
    ## #   Steps16 <dbl>, Steps17 <dbl>, Steps18 <dbl>, Steps19 <dbl>, Steps20 <dbl>,
    ## #   Steps21 <dbl>, Steps22 <dbl>, Steps23 <dbl>, Steps24 <dbl>, Steps25 <dbl>,
    ## #   Steps26 <dbl>, Steps27 <dbl>, Steps28 <dbl>, Steps29 <dbl>, ...

``` r
minitue_steps %>% filter(is.na(activity_hour))
```

    ## # A tibble: 0 x 62
    ## # ... with 62 variables: user_id <chr>, activity_hour <dttm>, Steps00 <dbl>,
    ## #   Steps01 <dbl>, Steps02 <dbl>, Steps03 <dbl>, Steps04 <dbl>, Steps05 <dbl>,
    ## #   Steps06 <dbl>, Steps07 <dbl>, Steps08 <dbl>, Steps09 <dbl>, Steps10 <dbl>,
    ## #   Steps11 <dbl>, Steps12 <dbl>, Steps13 <dbl>, Steps14 <dbl>, Steps15 <dbl>,
    ## #   Steps16 <dbl>, Steps17 <dbl>, Steps18 <dbl>, Steps19 <dbl>, Steps20 <dbl>,
    ## #   Steps21 <dbl>, Steps22 <dbl>, Steps23 <dbl>, Steps24 <dbl>, Steps25 <dbl>,
    ## #   Steps26 <dbl>, Steps27 <dbl>, Steps28 <dbl>, Steps29 <dbl>, ...

-   33 unique user ids

## Save files

``` r
write_csv(minitue_steps, output_file)
file.move(output_file, cleaned_data_path, overwrite = TRUE)
```

    ## 1 file moved. 0 failed.
