Introduction to dplyr
================
Reiko Okamoto
2024-08-14

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
