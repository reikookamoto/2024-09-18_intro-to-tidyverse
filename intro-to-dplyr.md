Introduction to dplyr
================
Reiko Okamoto
2024-08-15

## 👋Welcome to the tidyverse

#### ***What is the tidyverse?***

The [tidyverse](https://tidyverse.tidyverse.org/) is a collection of R
packages designed for data science. Arguably, two of the most popular
packages in the tidyverse are [dplyr](https://dplyr.tidyverse.org/) for
data manipulation and [ggplot2](https://ggplot2.tidyverse.org/) for data
visualization. These are also the packages that we are covering today!

***Why learn it?***

The skills you gain are not just limited to R. The concepts, like
filtering data and creating plots, are applicable to other languages
like SQL and Python. This makes it easier to pick up other tools and
languages later. Additionally, the tidyverse is in tune with open
science practices, helping you create analyses that are more accessible,
transparent, and reproducible.

***Keep in mind…***

There’s no expectation for you to memorize everything. Even experienced
programmers don’t have every function memorized - they’re constantly
googling things! My goal today is to help you get comfortable with, and
hopefully interested in, using the tidyverse for data analysis.

## 🐧Meet the Palmer penguins

<img src="imgs/penguins.png" width="500" />

We’ll use the **penguins** data set from the
[palmerpenguins](https://allisonhorst.github.io/palmerpenguins/)
package. It contains measurements for adult penguins observed in the
islands in the Palmer Archipelago off the Antarctic Peninsula. The data
were collected and made available by Dr. Kristen Gorman and the Palmer
Station Long Term Ecological Research (LTER) Program. This data set is
great for learning because it’s small enough to be manageable for
beginners but contains a range of different types of data and features.

💻Load the necessary packages:

``` r
library(tidyverse)
```

    ## ── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
    ## ✔ dplyr     1.1.4     ✔ readr     2.1.5
    ## ✔ forcats   1.0.0     ✔ stringr   1.5.1
    ## ✔ ggplot2   3.5.1     ✔ tibble    3.2.1
    ## ✔ lubridate 1.9.3     ✔ tidyr     1.3.1
    ## ✔ purrr     1.0.2     
    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()
    ## ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors

``` r
library(palmerpenguins)
```

💻Open the data set documentation:

``` r
?penguins
```

    ## starting httpd help server ... done

<img src="imgs/culmen_depth.png" width="500" />

## 1️⃣Get a glimpse of your data: glimpse()

💻Inspect the data using the
[glimpse()](https://dplyr.tidyverse.org/reference/glimpse.html)
function.

``` r
glimpse(penguins)
```

    ## Rows: 344
    ## Columns: 8
    ## $ species           <fct> Adelie, Adelie, Adelie, Adelie, Adelie, Adelie, Adel…
    ## $ island            <fct> Torgersen, Torgersen, Torgersen, Torgersen, Torgerse…
    ## $ bill_length_mm    <dbl> 39.1, 39.5, 40.3, NA, 36.7, 39.3, 38.9, 39.2, 34.1, …
    ## $ bill_depth_mm     <dbl> 18.7, 17.4, 18.0, NA, 19.3, 20.6, 17.8, 19.6, 18.1, …
    ## $ flipper_length_mm <int> 181, 186, 195, NA, 193, 190, 181, 195, 193, 190, 186…
    ## $ body_mass_g       <int> 3750, 3800, 3250, NA, 3450, 3650, 3625, 4675, 3475, …
    ## $ sex               <fct> male, female, female, NA, female, male, female, male…
    ## $ year              <int> 2007, 2007, 2007, 2007, 2007, 2007, 2007, 2007, 2007…

- *How many rows?*

- *How many columns?*

- *How many data types?*

- *Are there any missing values?*

## 2️⃣Keep or drop columns: select()

Sometimes, you only need to work with a few specific columns instead of
the entire data. The
[select()](https://dplyr.tidyverse.org/reference/select.html) function
makes it easy to do just that. It allows you to focus on the columns you
need and ignore the rest, making your code cleaner.

💻Select one column by name:

``` r
select(penguins, species)
```

    ## # A tibble: 344 × 1
    ##    species
    ##    <fct>  
    ##  1 Adelie 
    ##  2 Adelie 
    ##  3 Adelie 
    ##  4 Adelie 
    ##  5 Adelie 
    ##  6 Adelie 
    ##  7 Adelie 
    ##  8 Adelie 
    ##  9 Adelie 
    ## 10 Adelie 
    ## # ℹ 334 more rows

💻Select two columns by name:

``` r
select(penguins, species, island)
```

    ## # A tibble: 344 × 2
    ##    species island   
    ##    <fct>   <fct>    
    ##  1 Adelie  Torgersen
    ##  2 Adelie  Torgersen
    ##  3 Adelie  Torgersen
    ##  4 Adelie  Torgersen
    ##  5 Adelie  Torgersen
    ##  6 Adelie  Torgersen
    ##  7 Adelie  Torgersen
    ##  8 Adelie  Torgersen
    ##  9 Adelie  Torgersen
    ## 10 Adelie  Torgersen
    ## # ℹ 334 more rows

``` r
select(penguins, c(species, island))
```

    ## # A tibble: 344 × 2
    ##    species island   
    ##    <fct>   <fct>    
    ##  1 Adelie  Torgersen
    ##  2 Adelie  Torgersen
    ##  3 Adelie  Torgersen
    ##  4 Adelie  Torgersen
    ##  5 Adelie  Torgersen
    ##  6 Adelie  Torgersen
    ##  7 Adelie  Torgersen
    ##  8 Adelie  Torgersen
    ##  9 Adelie  Torgersen
    ## 10 Adelie  Torgersen
    ## # ℹ 334 more rows

💻Select all columns except certain ones:

``` r
select(penguins, -species, -island)
```

    ## # A tibble: 344 × 6
    ##    bill_length_mm bill_depth_mm flipper_length_mm body_mass_g sex     year
    ##             <dbl>         <dbl>             <int>       <int> <fct>  <int>
    ##  1           39.1          18.7               181        3750 male    2007
    ##  2           39.5          17.4               186        3800 female  2007
    ##  3           40.3          18                 195        3250 female  2007
    ##  4           NA            NA                  NA          NA <NA>    2007
    ##  5           36.7          19.3               193        3450 female  2007
    ##  6           39.3          20.6               190        3650 male    2007
    ##  7           38.9          17.8               181        3625 female  2007
    ##  8           39.2          19.6               195        4675 male    2007
    ##  9           34.1          18.1               193        3475 <NA>    2007
    ## 10           42            20.2               190        4250 <NA>    2007
    ## # ℹ 334 more rows

💻Select columns that start with “bill”:

``` r
select(penguins, starts_with("bill"))
```

    ## # A tibble: 344 × 2
    ##    bill_length_mm bill_depth_mm
    ##             <dbl>         <dbl>
    ##  1           39.1          18.7
    ##  2           39.5          17.4
    ##  3           40.3          18  
    ##  4           NA            NA  
    ##  5           36.7          19.3
    ##  6           39.3          20.6
    ##  7           38.9          17.8
    ##  8           39.2          19.6
    ##  9           34.1          18.1
    ## 10           42            20.2
    ## # ℹ 334 more rows

Similar functions like ends_with() and contains() are also available to
select columns based on the ending or presence of specific characters.

💻Select numeric variables:

``` r
select(penguins, where(is.numeric))
```

    ## # A tibble: 344 × 5
    ##    bill_length_mm bill_depth_mm flipper_length_mm body_mass_g  year
    ##             <dbl>         <dbl>             <int>       <int> <int>
    ##  1           39.1          18.7               181        3750  2007
    ##  2           39.5          17.4               186        3800  2007
    ##  3           40.3          18                 195        3250  2007
    ##  4           NA            NA                  NA          NA  2007
    ##  5           36.7          19.3               193        3450  2007
    ##  6           39.3          20.6               190        3650  2007
    ##  7           38.9          17.8               181        3625  2007
    ##  8           39.2          19.6               195        4675  2007
    ##  9           34.1          18.1               193        3475  2007
    ## 10           42            20.2               190        4250  2007
    ## # ℹ 334 more rows

## 3️⃣Keep rows that match a condition: filter()

Filtering data is a common task in data analysis. Use the
[filter()](https://dplyr.tidyverse.org/reference/filter.html) function
to focus on specific observations.

💻Show only the penguins of the Gentoo species:

``` r
filter(penguins, species == "Gentoo")
```

    ## # A tibble: 124 × 8
    ##    species island bill_length_mm bill_depth_mm flipper_length_mm body_mass_g
    ##    <fct>   <fct>           <dbl>         <dbl>             <int>       <int>
    ##  1 Gentoo  Biscoe           46.1          13.2               211        4500
    ##  2 Gentoo  Biscoe           50            16.3               230        5700
    ##  3 Gentoo  Biscoe           48.7          14.1               210        4450
    ##  4 Gentoo  Biscoe           50            15.2               218        5700
    ##  5 Gentoo  Biscoe           47.6          14.5               215        5400
    ##  6 Gentoo  Biscoe           46.5          13.5               210        4550
    ##  7 Gentoo  Biscoe           45.4          14.6               211        4800
    ##  8 Gentoo  Biscoe           46.7          15.3               219        5200
    ##  9 Gentoo  Biscoe           43.3          13.4               209        4400
    ## 10 Gentoo  Biscoe           46.8          15.4               215        5150
    ## # ℹ 114 more rows
    ## # ℹ 2 more variables: sex <fct>, year <int>

💻Find all Adelie penguins that are also on Torgersen Island:

``` r
filter(penguins, species == "Adelie", island == "Torgersen")
```

    ## # A tibble: 52 × 8
    ##    species island    bill_length_mm bill_depth_mm flipper_length_mm body_mass_g
    ##    <fct>   <fct>              <dbl>         <dbl>             <int>       <int>
    ##  1 Adelie  Torgersen           39.1          18.7               181        3750
    ##  2 Adelie  Torgersen           39.5          17.4               186        3800
    ##  3 Adelie  Torgersen           40.3          18                 195        3250
    ##  4 Adelie  Torgersen           NA            NA                  NA          NA
    ##  5 Adelie  Torgersen           36.7          19.3               193        3450
    ##  6 Adelie  Torgersen           39.3          20.6               190        3650
    ##  7 Adelie  Torgersen           38.9          17.8               181        3625
    ##  8 Adelie  Torgersen           39.2          19.6               195        4675
    ##  9 Adelie  Torgersen           34.1          18.1               193        3475
    ## 10 Adelie  Torgersen           42            20.2               190        4250
    ## # ℹ 42 more rows
    ## # ℹ 2 more variables: sex <fct>, year <int>

The comma acts as an AND operator, meaning both conditions must be true
for a row to be included.

💻Find all penguins that either Adelie or Gentoo:

``` r
filter(penguins, species == "Adelie" | species == "Gentoo")
```

    ## # A tibble: 276 × 8
    ##    species island    bill_length_mm bill_depth_mm flipper_length_mm body_mass_g
    ##    <fct>   <fct>              <dbl>         <dbl>             <int>       <int>
    ##  1 Adelie  Torgersen           39.1          18.7               181        3750
    ##  2 Adelie  Torgersen           39.5          17.4               186        3800
    ##  3 Adelie  Torgersen           40.3          18                 195        3250
    ##  4 Adelie  Torgersen           NA            NA                  NA          NA
    ##  5 Adelie  Torgersen           36.7          19.3               193        3450
    ##  6 Adelie  Torgersen           39.3          20.6               190        3650
    ##  7 Adelie  Torgersen           38.9          17.8               181        3625
    ##  8 Adelie  Torgersen           39.2          19.6               195        4675
    ##  9 Adelie  Torgersen           34.1          18.1               193        3475
    ## 10 Adelie  Torgersen           42            20.2               190        4250
    ## # ℹ 266 more rows
    ## # ℹ 2 more variables: sex <fct>, year <int>

The vertical bar acts as an OR operator, meaning a row is returned if
any of the conditions are true.

#### 📝Exercise 1

1.  Select the “sex” and “year” columns from the data.

2.  Filter the data to show only the rows where “flipper_length_mm” is
    greater than 200.

3.  Filter the data to find all penguins that are either on Biscoe
    Island or Torgersen Island.

## 4️⃣Pipes

💻Filter the data to only include Adelie penguins, and then keep only
the columns that start with “bill”:

``` r
select(filter(penguins, species == "Adelie"), starts_with("bill"))
```

    ## # A tibble: 152 × 2
    ##    bill_length_mm bill_depth_mm
    ##             <dbl>         <dbl>
    ##  1           39.1          18.7
    ##  2           39.5          17.4
    ##  3           40.3          18  
    ##  4           NA            NA  
    ##  5           36.7          19.3
    ##  6           39.3          20.6
    ##  7           38.9          17.8
    ##  8           39.2          19.6
    ##  9           34.1          18.1
    ## 10           42            20.2
    ## # ℹ 142 more rows

This works, but it can get difficult to read, especially as your code
grows more complex. The nested functions force you to read the code
inside-out, making it less intuitive

💻Use the pipe to do the same thing in a more streamlined way:

``` r
penguins |> 
  filter(species == "Adelie") |> 
  select(starts_with("bill"))
```

    ## # A tibble: 152 × 2
    ##    bill_length_mm bill_depth_mm
    ##             <dbl>         <dbl>
    ##  1           39.1          18.7
    ##  2           39.5          17.4
    ##  3           40.3          18  
    ##  4           NA            NA  
    ##  5           36.7          19.3
    ##  6           39.3          20.6
    ##  7           38.9          17.8
    ##  8           39.2          19.6
    ##  9           34.1          18.1
    ## 10           42            20.2
    ## # ℹ 142 more rows

The pipe allows us to pass the output of one function directly to the
next. This approach makes your code easier to read because the
operations flow left to right, top to bottom.

Keyboard shortcuts for the pipe:

- Windows: Ctrl + Shift + M

- Mac: Cmd + Shift + M

#### 📝Exercise 2

Filter the data include only female penguins with “bill_length_mm” less
than or equal to 40, and then select the “species” and “bill_length_mm”
columns.

``` r
penguins |> 
  filter(sex == "female", bill_length_mm <= 40) |> 
  select(species, bill_length_mm)
```

    ## # A tibble: 66 × 2
    ##    species bill_length_mm
    ##    <fct>            <dbl>
    ##  1 Adelie            39.5
    ##  2 Adelie            36.7
    ##  3 Adelie            38.9
    ##  4 Adelie            36.6
    ##  5 Adelie            38.7
    ##  6 Adelie            34.4
    ##  7 Adelie            37.8
    ##  8 Adelie            35.9
    ##  9 Adelie            35.3
    ## 10 Adelie            37.9
    ## # ℹ 56 more rows

## 
