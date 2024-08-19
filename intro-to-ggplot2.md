# Introduction to ggplot2
Reiko Okamoto
2024-08-19

## 🎨Introduction to ggplot2

ggplot2 helps you create a wide range of static, informative, and
visually appealing graphics. Its name comes from the Grammar of
Graphics, which is a framework for building plots in a structured way.
You build a plot incrementally by adding layers like data points, axes,
colours, and labels.

💻Load the necessary packages:

``` r
library(tidyverse)
```

    ── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
    ✔ dplyr     1.1.4     ✔ readr     2.1.5
    ✔ forcats   1.0.0     ✔ stringr   1.5.1
    ✔ ggplot2   3.5.1     ✔ tibble    3.2.1
    ✔ lubridate 1.9.3     ✔ tidyr     1.3.1
    ✔ purrr     1.0.2     
    ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ✖ dplyr::filter() masks stats::filter()
    ✖ dplyr::lag()    masks stats::lag()
    ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors

``` r
library(RColorBrewer)
```

## 1️⃣Explore the sample data

<img src="imgs/antoine-schibler-H84rHbx7IP0-unsplash.jpg"
data-fig-align="center" width="500" />

We’ll use a data set sourced from a [GitHub
repository](https://github.com/rfordatascience/tidytuesday/tree/master/data/2019/2019-02-26),
which originally retrieved the data from the SNCF open data portal.
SCNF, founded in 1938, is France’s national railway company. It operates
systems like the TGV high-speed trains and regional TER services.

💻Read in the data using the
[`read_csv()`](https://readr.tidyverse.org/reference/read_delim.html)
function from the [readr](https://readr.tidyverse.org/) package:

``` r
trains_df <- read_csv("french_trains.csv")
```

    Rows: 5462 Columns: 11
    ── Column specification ────────────────────────────────────────────────────────
    Delimiter: ","
    chr (3): service, departure_station, arrival_station
    dbl (8): year, month, journey_time_avg, total_num_trips, avg_delay_all_depar...

    ℹ Use `spec()` to retrieve the full column specification for this data.
    ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
trains_df
```

    # A tibble: 5,462 × 11
        year month service  departure_station       arrival_station journey_time_avg
       <dbl> <dbl> <chr>    <chr>                   <chr>                      <dbl>
     1  2015     1 National AIX EN PROVENCE TGV     PARIS LYON                 182. 
     2  2015     1 National ANGERS SAINT LAUD       PARIS MONTPARN…             98.9
     3  2015     1 National ANGOULEME               PARIS MONTPARN…            156. 
     4  2015     1 National ANNECY                  PARIS LYON                 224. 
     5  2015     1 National ARRAS                   PARIS NORD                  55.6
     6  2015     1 National AVIGNON TGV             PARIS LYON                 161. 
     7  2015     1 National BELLEGARDE (AIN)        PARIS LYON                 164. 
     8  2015     1 National BESANCON FRANCHE COMTE… PARIS LYON                 131. 
     9  2015     1 National BORDEAUX ST JEAN        PARIS MONTPARN…            211. 
    10  2015     1 National BREST                   PARIS MONTPARN…            274. 
    # ℹ 5,452 more rows
    # ℹ 5 more variables: total_num_trips <dbl>, avg_delay_all_departing <dbl>,
    #   avg_delay_all_arriving <dbl>, num_late_at_departure <dbl>,
    #   num_arriving_late <dbl>

🧠Explore the type and description of each variable:

| Variable                  | Type      | Description                          |
|---------------------------|-----------|--------------------------------------|
| `year`                    | double    | Year of observation                  |
| `month`                   | double    | Month of obsrevation                 |
| `service`                 | character | Type of service                      |
| `departure_station`       | character | Departure station                    |
| `arrival_station`         | character | Arrival station                      |
| `journey_time_avg`        | double    | Average journey time (in minutes)    |
| `total_num_trips`         | double    | Total number of trips recorded       |
| `avg_delay_all_departing` | double    | Average departure delay (in minutes) |
| `avg_delay_all_arriving`  | double    | Average arrival delay (in minutes)   |
| `num_late_at_departure`   | double    | Number of trains that departed late  |
| `num_arriving_late`       | double    | Number of trains that arrived late   |

The data set contains **monthly summaries** train journeys from 2015 to
2018. Each row represents aggregated data for a specific route and
month, including average journey times, delays, and the total number of
trips. This structure will allow us to explore trends over time and
visualize overall patterns in travel travel.

💻Inspect the first and last parts of the data set:

``` r
head(trains_df)
```

    # A tibble: 6 × 11
       year month service  departure_station   arrival_station    journey_time_avg
      <dbl> <dbl> <chr>    <chr>               <chr>                         <dbl>
    1  2015     1 National AIX EN PROVENCE TGV PARIS LYON                    182. 
    2  2015     1 National ANGERS SAINT LAUD   PARIS MONTPARNASSE             98.9
    3  2015     1 National ANGOULEME           PARIS MONTPARNASSE            156. 
    4  2015     1 National ANNECY              PARIS LYON                    224. 
    5  2015     1 National ARRAS               PARIS NORD                     55.6
    6  2015     1 National AVIGNON TGV         PARIS LYON                    161. 
    # ℹ 5 more variables: total_num_trips <dbl>, avg_delay_all_departing <dbl>,
    #   avg_delay_all_arriving <dbl>, num_late_at_departure <dbl>,
    #   num_arriving_late <dbl>

``` r
tail(trains_df)
```

    # A tibble: 6 × 11
       year month service departure_station  arrival_station      journey_time_avg
      <dbl> <dbl> <chr>   <chr>              <chr>                           <dbl>
    1  2018    11 <NA>    TOURCOING          BORDEAUX ST JEAN                285. 
    2  2018    11 <NA>    TOURCOING          MARSEILLE ST CHARLES            305. 
    3  2018    11 <NA>    TOURS              PARIS MONTPARNASSE               76.9
    4  2018    11 <NA>    VALENCE ALIXAN TGV PARIS LYON                      133. 
    5  2018    11 <NA>    VANNES             PARIS MONTPARNASSE              157. 
    6  2018    11 <NA>    ZURICH             PARIS LYON                      242. 
    # ℹ 5 more variables: total_num_trips <dbl>, avg_delay_all_departing <dbl>,
    #   avg_delay_all_arriving <dbl>, num_late_at_departure <dbl>,
    #   num_arriving_late <dbl>

- The first row captures information on trips from **AIX EN PROVENCE
  TGV** to **PARIS LYON** for the month of **JANUARY 2015**

- The last row captures information on trips from **ZURICH** to **PARIS
  LYON** for the month of **NOVEMBER 2018**

## 2️⃣Histograms

💻Create a histogram to visualize the distribution of average journey
time:

``` r
ggplot(trains_df, aes(x = journey_time_avg)) +
  geom_histogram(bins = 20)
```

![](intro-to-ggplot2_files/figure-commonmark/unnamed-chunk-4-1.png)

We can see that the distribution of average journey time is
right-skewed.

Breaking down the code:

- The first argument in ggplot() indicates which data frame we want to
  use.

- The second argument, called the aesthetic mapping, specifies how the
  columns of the data frame should be mapped to different parts of the
  plot (e.g., x-axis, y-axis, colour, etc.).

- We use the `+` operator to add each layer to the plot.

- `geom_histogram()` is a geometric object (or “geom”) that decides how
  the mapped data is displayed—in this case, as a histogram.

## 3️⃣Scatter plots

Scatter plots are a powerful way to visualize the relationship between
two continuous variables.

💻Create a scatter plot to visualize the relationship between the
average journey time and the number of trains that departed late:

``` r
trains_df |> 
  ggplot(aes(x = journey_time_avg, y = num_late_at_departure)) +
  geom_point()
```

![](intro-to-ggplot2_files/figure-commonmark/unnamed-chunk-5-1.png)

Sometimes, when data points overlap too much, it can be challenging to
see the individual points. This is called overplotting.

💻To address this, adjust the transparency of the points using the
`alpha` argument:

``` r
trains_df |> 
  ggplot(aes(x = journey_time_avg, y = num_late_at_departure)) +
  geom_point(alpha = 0.2)
```

![](intro-to-ggplot2_files/figure-commonmark/unnamed-chunk-6-1.png)

💻Make the plot more visually appealing by changing the colour of the
points and adding a title and axis labels:

``` r
trains_df |> 
  ggplot(aes(x = journey_time_avg, y = num_late_at_departure)) +
  geom_point(alpha = 0.2, colour = "#3182bd") +
  labs(
    title = "French train punctuality",
    x = "Average journey time (in minutes)",
    y = "Number of trains that departed late"
  )
```

![](intro-to-ggplot2_files/figure-commonmark/unnamed-chunk-7-1.png)

#### 📝**Exercise 1**

Create a scatter plot to visualize the relationship between two other
continuous variables in the data. This time, change the default size of
the points by using the `size` argument.
