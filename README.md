433 HW 3
================
Alyssa Gjervold
10/5/2021

#### What time of day should you fly if you want to avoid delays as much as possible? Does this choice depend on anything? Season? Weather? Airport? Airline? Find three patterns (“null results” are ok\!). Write your results into Rmarkdown. Include a short introduction that summarizes the three results. Then, have a section for each finding. Support each finding with data summaries and visualizations. Include your code when necessary. This shouldn’t be long, but it might take some time to find the things you want to talk about and lay them out in an orderly way.

``` r
# group by hour
flights %>%
  group_by(hour) %>%
  summarise(arr_delay = mean(arr_delay, na.rm = TRUE)) %>%
  arrange(arr_delay)
```

    ## `summarise()` ungrouping output (override with `.groups` argument)

    ## # A tibble: 20 x 2
    ##     hour arr_delay
    ##    <dbl>     <dbl>
    ##  1     7    -5.30 
    ##  2     5    -4.80 
    ##  3     6    -3.38 
    ##  4     9    -1.45 
    ##  5     8    -1.11 
    ##  6    10     0.954
    ##  7    11     1.48 
    ##  8    12     3.49 
    ##  9    13     6.54 
    ## 10    14     9.20 
    ## 11    23    11.8  
    ## 12    15    12.3  
    ## 13    16    12.6  
    ## 14    18    14.8  
    ## 15    22    16.0  
    ## 16    17    16.0  
    ## 17    19    16.7  
    ## 18    20    16.7  
    ## 19    21    18.4  
    ## 20     1   NaN

Looking at the arrival delay by hour of departure shows that 10 am
flights tend to have the most on-time flights. However, if you want to
arrive early, then 7 am flights are your best option as they tend to
arrive the earliest. For the most part, the later the flight is, the
greater the delay is

## Introduction

The airline carrier, the weather, and the season all impact which
flights you should choose in addition to the time of day that you fly.
The three carriers with the lowest average arrival delay are, in order
starting from least delay, AS, HA, and AA. If you want the most on-time
arrival, then AA is your best option. If you want the earliest possible
arrival, then AS is your best option. As for weather, there does not
appear to be a relation between the amount of precipitation and arrival
delay. If anything, there may be a slight positive correlation, meaning
the higher the precipitation, the higher the arrival delay. Lastly, for
the airport, JFK airport as a point of origin has the lowest average
arrival delay.

## Section 1: Airline–`carrier`

``` r
flights2 <- flights %>%
  select(arr_delay, carrier, hour)
airlines2 <- airlines %>%
  select(carrier)
flights_airlines <- left_join(flights2, airlines2)
```

    ## Joining, by = "carrier"

``` r
# arr_delay and carrier
flights_airlines %>%
  group_by(carrier) %>%
  summarise(arr_delay = mean(arr_delay, na.rm = TRUE)) %>%
  arrange(arr_delay)
```

    ## `summarise()` ungrouping output (override with `.groups` argument)

    ## # A tibble: 16 x 2
    ##    carrier arr_delay
    ##    <chr>       <dbl>
    ##  1 AS         -9.93 
    ##  2 HA         -6.92 
    ##  3 AA          0.364
    ##  4 DL          1.64 
    ##  5 VX          1.76 
    ##  6 US          2.13 
    ##  7 UA          3.56 
    ##  8 9E          7.38 
    ##  9 B6          9.46 
    ## 10 WN          9.65 
    ## 11 MQ         10.8  
    ## 12 OO         11.9  
    ## 13 YV         15.6  
    ## 14 EV         15.8  
    ## 15 FL         20.1  
    ## 16 F9         21.9

The three carriers with the lowest average arrival delay are, in order
starting from least delay, AS, HA, and AA. If you want the most on-time
arrival, then AA is your best bet. If you want the earliest possible
arrival, then AS is your best bet.

## Section 2: Weather–`precip`

``` r
flights2 <- flights %>%
  select(arr_delay, hour)
weather2 <- weather %>%
  select(hour, precip)
#(flights_weather <- left_join(flights2, weather2))

# arr_delay and precip
# flights_weather %>%
#  group_by(precip) %>%
#  summarise(arr_delay = mean(arr_delay, na.rm = TRUE)) %>%
#  arrange(arr_delay) %>%
#  ggplot(flights_weather, aes(precip, arr_delay)) +
#  geom_point()
```

There does not appear to be a relation between the amount of
precipitation and arrival delay. If anything, there may be a slight
positive correlation, meaning the higher the precipitation, the higher
the arrival delay. NOTE: I was able to execute the code the first time
but each subsequent time it threw me an error; hence, I have tagged out
the code but I still saw the data and the figure when I initially ran
the code.

## Section 3: Airport–`faa`/`origin`

``` r
flights2 <- flights %>%
  select(arr_delay, hour, origin)
airports2 <- airports %>%
  rename(origin = faa)
flights_airports <- left_join(flights2, airports2)
```

    ## Joining, by = "origin"

``` r
# arr_delay and origin/faa
flights_airports %>%
  group_by(origin) %>%
  summarise(arr_delay = mean(arr_delay, na.rm = TRUE)) %>%
  arrange(arr_delay)
```

    ## `summarise()` ungrouping output (override with `.groups` argument)

    ## # A tibble: 3 x 2
    ##   origin arr_delay
    ##   <chr>      <dbl>
    ## 1 JFK         5.55
    ## 2 LGA         5.78
    ## 3 EWR         9.11

JFK airport as a point of origin has the lowest average arrival delay.
