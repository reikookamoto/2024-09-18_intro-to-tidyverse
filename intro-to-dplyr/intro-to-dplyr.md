# Introduction to dplyr
Reiko Okamoto
2024-09-16

## 👋Welcome to the tidyverse

#### *What is the tidyverse?*

The [tidyverse](https://tidyverse.tidyverse.org/) is a collection of R
packages designed for data science. Arguably, two of the most popular
packages in the tidyverse are [dplyr](https://dplyr.tidyverse.org/) for
data manipulation and [ggplot2](https://ggplot2.tidyverse.org/) for data
visualization. These are also two of the packages we are covering today!

#### *Why learn it?*

The skills you gain are not just limited to R. The concepts, like
filtering data and creating plots, are applicable to other languages
like SQL and Python. This makes it easier to pick up other tools and
languages later. Additionally, the tidyverse is in tune with open
science practices, helping you create analyses that are more accessible,
transparent, and reproducible.

#### *Keep in mind…*

There’s no expectation for you to memorize everything. Even experienced
programmers don’t have every function memorized - they’re constantly
googling things! My goal today is to help you get comfortable with, and
hopefully interested in, using the tidyverse for data analysis.

## 🐧Meet the Palmer penguins

<img src="../imgs/penguins.png" data-fig-align="center"
alt="Artwork by @allison_horst" />

We’ll use the `penguins` data set from the
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
library(palmerpenguins)
```

We only need to install a package once, but we’ll need to reload it
every time we start a new session.

💻Open the data set documentation:

``` r
?penguins
```

    starting httpd help server ... done

<img src="../imgs/culmen_depth.png" data-fig-align="center" width="500"
alt="Artwork by @allison_horst" />

## 1️⃣Get a glimpse of our data: glimpse()

💻Inspect the data using the
[`glimpse()`](https://dplyr.tidyverse.org/reference/glimpse.html)
function.

``` r
glimpse(penguins)
```

    Rows: 344
    Columns: 8
    $ species           <fct> Adelie, Adelie, Adelie, Adelie, Adelie, Adelie, Adel…
    $ island            <fct> Torgersen, Torgersen, Torgersen, Torgersen, Torgerse…
    $ bill_length_mm    <dbl> 39.1, 39.5, 40.3, NA, 36.7, 39.3, 38.9, 39.2, 34.1, …
    $ bill_depth_mm     <dbl> 18.7, 17.4, 18.0, NA, 19.3, 20.6, 17.8, 19.6, 18.1, …
    $ flipper_length_mm <int> 181, 186, 195, NA, 193, 190, 181, 195, 193, 190, 186…
    $ body_mass_g       <int> 3750, 3800, 3250, NA, 3450, 3650, 3625, 4675, 3475, …
    $ sex               <fct> male, female, female, NA, female, male, female, male…
    $ year              <int> 2007, 2007, 2007, 2007, 2007, 2007, 2007, 2007, 2007…

- *How many rows?*

- *How many columns?*

- *How many data types?*

- *Are there any missing values?*

## 2️⃣Keep or drop columns: select()

Sometimes, we only need to work with a few specific columns instead of
the entire data. The
[`select()`](https://dplyr.tidyverse.org/reference/select.html) function
makes it easy to do just that. It allows us to focus on the columns we
need and ignore the rest, making our code cleaner.

💻Select one column by name:

``` r
select(penguins, species)
```

    # A tibble: 344 × 1
       species
       <fct>  
     1 Adelie 
     2 Adelie 
     3 Adelie 
     4 Adelie 
     5 Adelie 
     6 Adelie 
     7 Adelie 
     8 Adelie 
     9 Adelie 
    10 Adelie 
    # ℹ 334 more rows

💻Select two columns by name:

``` r
select(penguins, species, island)
```

    # A tibble: 344 × 2
       species island   
       <fct>   <fct>    
     1 Adelie  Torgersen
     2 Adelie  Torgersen
     3 Adelie  Torgersen
     4 Adelie  Torgersen
     5 Adelie  Torgersen
     6 Adelie  Torgersen
     7 Adelie  Torgersen
     8 Adelie  Torgersen
     9 Adelie  Torgersen
    10 Adelie  Torgersen
    # ℹ 334 more rows

``` r
select(penguins, c(species, island))
```

    # A tibble: 344 × 2
       species island   
       <fct>   <fct>    
     1 Adelie  Torgersen
     2 Adelie  Torgersen
     3 Adelie  Torgersen
     4 Adelie  Torgersen
     5 Adelie  Torgersen
     6 Adelie  Torgersen
     7 Adelie  Torgersen
     8 Adelie  Torgersen
     9 Adelie  Torgersen
    10 Adelie  Torgersen
    # ℹ 334 more rows

💻Select all columns except certain ones:

``` r
select(penguins, -species, -island)
```

    # A tibble: 344 × 6
       bill_length_mm bill_depth_mm flipper_length_mm body_mass_g sex     year
                <dbl>         <dbl>             <int>       <int> <fct>  <int>
     1           39.1          18.7               181        3750 male    2007
     2           39.5          17.4               186        3800 female  2007
     3           40.3          18                 195        3250 female  2007
     4           NA            NA                  NA          NA <NA>    2007
     5           36.7          19.3               193        3450 female  2007
     6           39.3          20.6               190        3650 male    2007
     7           38.9          17.8               181        3625 female  2007
     8           39.2          19.6               195        4675 male    2007
     9           34.1          18.1               193        3475 <NA>    2007
    10           42            20.2               190        4250 <NA>    2007
    # ℹ 334 more rows

💻Select columns that start with “bill”:

``` r
select(penguins, starts_with("bill"))
```

    # A tibble: 344 × 2
       bill_length_mm bill_depth_mm
                <dbl>         <dbl>
     1           39.1          18.7
     2           39.5          17.4
     3           40.3          18  
     4           NA            NA  
     5           36.7          19.3
     6           39.3          20.6
     7           38.9          17.8
     8           39.2          19.6
     9           34.1          18.1
    10           42            20.2
    # ℹ 334 more rows

Similar functions like `ends_with()` and `contains()` are also available
to select columns based on the ending or presence of specific
characters.

💻Select numeric variables:

``` r
select(penguins, where(is.numeric))
```

    # A tibble: 344 × 5
       bill_length_mm bill_depth_mm flipper_length_mm body_mass_g  year
                <dbl>         <dbl>             <int>       <int> <int>
     1           39.1          18.7               181        3750  2007
     2           39.5          17.4               186        3800  2007
     3           40.3          18                 195        3250  2007
     4           NA            NA                  NA          NA  2007
     5           36.7          19.3               193        3450  2007
     6           39.3          20.6               190        3650  2007
     7           38.9          17.8               181        3625  2007
     8           39.2          19.6               195        4675  2007
     9           34.1          18.1               193        3475  2007
    10           42            20.2               190        4250  2007
    # ℹ 334 more rows

## 3️⃣Keep rows that match a condition: filter()

Filtering data is a common task in data analysis. Use the
[`filter()`](https://dplyr.tidyverse.org/reference/filter.html) function
to focus on specific observations.

💻Show only the penguins of the Gentoo species:

``` r
filter(penguins, species == "Gentoo")
```

    # A tibble: 124 × 8
       species island bill_length_mm bill_depth_mm flipper_length_mm body_mass_g
       <fct>   <fct>           <dbl>         <dbl>             <int>       <int>
     1 Gentoo  Biscoe           46.1          13.2               211        4500
     2 Gentoo  Biscoe           50            16.3               230        5700
     3 Gentoo  Biscoe           48.7          14.1               210        4450
     4 Gentoo  Biscoe           50            15.2               218        5700
     5 Gentoo  Biscoe           47.6          14.5               215        5400
     6 Gentoo  Biscoe           46.5          13.5               210        4550
     7 Gentoo  Biscoe           45.4          14.6               211        4800
     8 Gentoo  Biscoe           46.7          15.3               219        5200
     9 Gentoo  Biscoe           43.3          13.4               209        4400
    10 Gentoo  Biscoe           46.8          15.4               215        5150
    # ℹ 114 more rows
    # ℹ 2 more variables: sex <fct>, year <int>

💻Find all Adelie penguins that are also on Torgersen Island:

``` r
filter(penguins, species == "Adelie", island == "Torgersen")
```

    # A tibble: 52 × 8
       species island    bill_length_mm bill_depth_mm flipper_length_mm body_mass_g
       <fct>   <fct>              <dbl>         <dbl>             <int>       <int>
     1 Adelie  Torgersen           39.1          18.7               181        3750
     2 Adelie  Torgersen           39.5          17.4               186        3800
     3 Adelie  Torgersen           40.3          18                 195        3250
     4 Adelie  Torgersen           NA            NA                  NA          NA
     5 Adelie  Torgersen           36.7          19.3               193        3450
     6 Adelie  Torgersen           39.3          20.6               190        3650
     7 Adelie  Torgersen           38.9          17.8               181        3625
     8 Adelie  Torgersen           39.2          19.6               195        4675
     9 Adelie  Torgersen           34.1          18.1               193        3475
    10 Adelie  Torgersen           42            20.2               190        4250
    # ℹ 42 more rows
    # ℹ 2 more variables: sex <fct>, year <int>

The comma acts as an AND operator, meaning both conditions must be true
for a row to be included.

💻Find all penguins that either Adelie or Gentoo:

``` r
filter(penguins, species == "Adelie" | species == "Gentoo")
```

    # A tibble: 276 × 8
       species island    bill_length_mm bill_depth_mm flipper_length_mm body_mass_g
       <fct>   <fct>              <dbl>         <dbl>             <int>       <int>
     1 Adelie  Torgersen           39.1          18.7               181        3750
     2 Adelie  Torgersen           39.5          17.4               186        3800
     3 Adelie  Torgersen           40.3          18                 195        3250
     4 Adelie  Torgersen           NA            NA                  NA          NA
     5 Adelie  Torgersen           36.7          19.3               193        3450
     6 Adelie  Torgersen           39.3          20.6               190        3650
     7 Adelie  Torgersen           38.9          17.8               181        3625
     8 Adelie  Torgersen           39.2          19.6               195        4675
     9 Adelie  Torgersen           34.1          18.1               193        3475
    10 Adelie  Torgersen           42            20.2               190        4250
    # ℹ 266 more rows
    # ℹ 2 more variables: sex <fct>, year <int>

``` r
filter(penguins, species != "Chinstrap")
```

    # A tibble: 276 × 8
       species island    bill_length_mm bill_depth_mm flipper_length_mm body_mass_g
       <fct>   <fct>              <dbl>         <dbl>             <int>       <int>
     1 Adelie  Torgersen           39.1          18.7               181        3750
     2 Adelie  Torgersen           39.5          17.4               186        3800
     3 Adelie  Torgersen           40.3          18                 195        3250
     4 Adelie  Torgersen           NA            NA                  NA          NA
     5 Adelie  Torgersen           36.7          19.3               193        3450
     6 Adelie  Torgersen           39.3          20.6               190        3650
     7 Adelie  Torgersen           38.9          17.8               181        3625
     8 Adelie  Torgersen           39.2          19.6               195        4675
     9 Adelie  Torgersen           34.1          18.1               193        3475
    10 Adelie  Torgersen           42            20.2               190        4250
    # ℹ 266 more rows
    # ℹ 2 more variables: sex <fct>, year <int>

The vertical bar acts as an OR operator, meaning a row is returned if
any of the conditions are true.

#### 📝Exercise 1

1.  Select the `sex` and `year` columns from the data.

2.  Filter the data to show only the rows where `flipper_length_mm` is
    greater than 200.

3.  Filter the data to find all penguins that are either on Biscoe
    Island or Torgersen Island.

    ``` r
    select(penguins, sex, year)
    ```

        # A tibble: 344 × 2
           sex     year
           <fct>  <int>
         1 male    2007
         2 female  2007
         3 female  2007
         4 <NA>    2007
         5 female  2007
         6 male    2007
         7 female  2007
         8 male    2007
         9 <NA>    2007
        10 <NA>    2007
        # ℹ 334 more rows

    ``` r
    filter(penguins, flipper_length_mm > 200)
    ```

        # A tibble: 148 × 8
           species island    bill_length_mm bill_depth_mm flipper_length_mm body_mass_g
           <fct>   <fct>              <dbl>         <dbl>             <int>       <int>
         1 Adelie  Dream               35.7          18                 202        3550
         2 Adelie  Dream               41.1          18.1               205        4300
         3 Adelie  Dream               40.8          18.9               208        4300
         4 Adelie  Biscoe              41            20                 203        4725
         5 Adelie  Torgersen           41.4          18.5               202        3875
         6 Adelie  Torgersen           44.1          18                 210        4000
         7 Adelie  Dream               41.5          18.5               201        4000
         8 Gentoo  Biscoe              46.1          13.2               211        4500
         9 Gentoo  Biscoe              50            16.3               230        5700
        10 Gentoo  Biscoe              48.7          14.1               210        4450
        # ℹ 138 more rows
        # ℹ 2 more variables: sex <fct>, year <int>

    ``` r
    filter(penguins, island == "Biscoe" | island == "Torgersen")
    ```

        # A tibble: 220 × 8
           species island    bill_length_mm bill_depth_mm flipper_length_mm body_mass_g
           <fct>   <fct>              <dbl>         <dbl>             <int>       <int>
         1 Adelie  Torgersen           39.1          18.7               181        3750
         2 Adelie  Torgersen           39.5          17.4               186        3800
         3 Adelie  Torgersen           40.3          18                 195        3250
         4 Adelie  Torgersen           NA            NA                  NA          NA
         5 Adelie  Torgersen           36.7          19.3               193        3450
         6 Adelie  Torgersen           39.3          20.6               190        3650
         7 Adelie  Torgersen           38.9          17.8               181        3625
         8 Adelie  Torgersen           39.2          19.6               195        4675
         9 Adelie  Torgersen           34.1          18.1               193        3475
        10 Adelie  Torgersen           42            20.2               190        4250
        # ℹ 210 more rows
        # ℹ 2 more variables: sex <fct>, year <int>

## 4️⃣Pipes

💻Filter the data to only include Adelie penguins, and then keep only
the columns that start with “bill”:

``` r
select(filter(penguins, species == "Adelie"), starts_with("bill"))
```

    # A tibble: 152 × 2
       bill_length_mm bill_depth_mm
                <dbl>         <dbl>
     1           39.1          18.7
     2           39.5          17.4
     3           40.3          18  
     4           NA            NA  
     5           36.7          19.3
     6           39.3          20.6
     7           38.9          17.8
     8           39.2          19.6
     9           34.1          18.1
    10           42            20.2
    # ℹ 142 more rows

This works, but it can get difficult to read, especially as our code
grows more complex. The nested functions force us to read the code
inside-out, making it less intuitive

💻Use the pipe to do the same thing in a more streamlined way:

Keyboard shortcuts for the pipe:

- Windows: Ctrl + Shift + M

- Mac: Cmd + Shift + M

``` r
penguins |> 
  filter(species == "Adelie") |> 
  select(starts_with("bill"))
```

    # A tibble: 152 × 2
       bill_length_mm bill_depth_mm
                <dbl>         <dbl>
     1           39.1          18.7
     2           39.5          17.4
     3           40.3          18  
     4           NA            NA  
     5           36.7          19.3
     6           39.3          20.6
     7           38.9          17.8
     8           39.2          19.6
     9           34.1          18.1
    10           42            20.2
    # ℹ 142 more rows

The pipe allows us to pass the output of one function directly to the
next. This approach makes our code easier to read because the operations
flow left to right, top to bottom.

#### 📝Exercise 2

Using the pipe, filter the data to include only female penguins with
`bill_length_mm` less than or equal to 40, and then select the `species`
and `bill_length_mm` columns.

``` r
penguins |> 
  filter(sex == "female", bill_length_mm <= 40) |> 
  select(species, bill_length_mm)
```

    # A tibble: 66 × 2
       species bill_length_mm
       <fct>            <dbl>
     1 Adelie            39.5
     2 Adelie            36.7
     3 Adelie            38.9
     4 Adelie            36.6
     5 Adelie            38.7
     6 Adelie            34.4
     7 Adelie            37.8
     8 Adelie            35.9
     9 Adelie            35.3
    10 Adelie            37.9
    # ℹ 56 more rows

## 5️⃣Create and modify columns: mutate()

In data analysis, it’s common to derive new variables from existing
ones. The
[`mutate()`](https://dplyr.tidyverse.org/reference/mutate.html) function
is essential for these tasks.

💻Create a new column called `body_mass_kg` (i.e., the `body_mass_g`
column converted from grams to kilograms):

``` r
penguins |> 
  mutate(body_mass_kg = body_mass_g / 1000)
```

    # A tibble: 344 × 9
       species island    bill_length_mm bill_depth_mm flipper_length_mm body_mass_g
       <fct>   <fct>              <dbl>         <dbl>             <int>       <int>
     1 Adelie  Torgersen           39.1          18.7               181        3750
     2 Adelie  Torgersen           39.5          17.4               186        3800
     3 Adelie  Torgersen           40.3          18                 195        3250
     4 Adelie  Torgersen           NA            NA                  NA          NA
     5 Adelie  Torgersen           36.7          19.3               193        3450
     6 Adelie  Torgersen           39.3          20.6               190        3650
     7 Adelie  Torgersen           38.9          17.8               181        3625
     8 Adelie  Torgersen           39.2          19.6               195        4675
     9 Adelie  Torgersen           34.1          18.1               193        3475
    10 Adelie  Torgersen           42            20.2               190        4250
    # ℹ 334 more rows
    # ℹ 3 more variables: sex <fct>, year <int>, body_mass_kg <dbl>

By separating the new columns with a comma, we can create multiple new
variables in one function call.

💻Use [`if_else()`](https://dplyr.tidyverse.org/reference/if_else.html)
to create a column called `large_penguin`, which indicates whether the
penguin’s body mass is greater than 4,000 grams:

``` r
penguins |> 
  mutate(large_penguin = if_else(body_mass_g > 4000, TRUE, FALSE))
```

    # A tibble: 344 × 9
       species island    bill_length_mm bill_depth_mm flipper_length_mm body_mass_g
       <fct>   <fct>              <dbl>         <dbl>             <int>       <int>
     1 Adelie  Torgersen           39.1          18.7               181        3750
     2 Adelie  Torgersen           39.5          17.4               186        3800
     3 Adelie  Torgersen           40.3          18                 195        3250
     4 Adelie  Torgersen           NA            NA                  NA          NA
     5 Adelie  Torgersen           36.7          19.3               193        3450
     6 Adelie  Torgersen           39.3          20.6               190        3650
     7 Adelie  Torgersen           38.9          17.8               181        3625
     8 Adelie  Torgersen           39.2          19.6               195        4675
     9 Adelie  Torgersen           34.1          18.1               193        3475
    10 Adelie  Torgersen           42            20.2               190        4250
    # ℹ 334 more rows
    # ℹ 3 more variables: sex <fct>, year <int>, large_penguin <lgl>

💻Use
[`case_when()`](https://dplyr.tidyverse.org/reference/case_when.html) to
categorize the penguins based on their body mass:

``` r
penguins |> 
  mutate(size_category = case_when(
    body_mass_g < 3000 ~ "small",
    body_mass_g < 4000 ~ "medium",
    body_mass_g >= 4000 ~ "large",
    .default = "unknown"
  ))
```

    # A tibble: 344 × 9
       species island    bill_length_mm bill_depth_mm flipper_length_mm body_mass_g
       <fct>   <fct>              <dbl>         <dbl>             <int>       <int>
     1 Adelie  Torgersen           39.1          18.7               181        3750
     2 Adelie  Torgersen           39.5          17.4               186        3800
     3 Adelie  Torgersen           40.3          18                 195        3250
     4 Adelie  Torgersen           NA            NA                  NA          NA
     5 Adelie  Torgersen           36.7          19.3               193        3450
     6 Adelie  Torgersen           39.3          20.6               190        3650
     7 Adelie  Torgersen           38.9          17.8               181        3625
     8 Adelie  Torgersen           39.2          19.6               195        4675
     9 Adelie  Torgersen           34.1          18.1               193        3475
    10 Adelie  Torgersen           42            20.2               190        4250
    # ℹ 334 more rows
    # ℹ 3 more variables: sex <fct>, year <int>, size_category <chr>

`if_else()` is best for binary conditions where we need to choose
between two outcomes. In contrast, `case_when()` is ideal for handling
multiple conditions, allowing us to return different values based on
various criteria.

💻Use [`across()`](https://dplyr.tidyverse.org/reference/across.html) to
efficiently round values in multiple columns simultaneously:

``` r
penguins |> 
  mutate(across(.cols = c(bill_length_mm, bill_depth_mm), .fns = round))
```

    # A tibble: 344 × 8
       species island    bill_length_mm bill_depth_mm flipper_length_mm body_mass_g
       <fct>   <fct>              <dbl>         <dbl>             <int>       <int>
     1 Adelie  Torgersen             39            19               181        3750
     2 Adelie  Torgersen             40            17               186        3800
     3 Adelie  Torgersen             40            18               195        3250
     4 Adelie  Torgersen             NA            NA                NA          NA
     5 Adelie  Torgersen             37            19               193        3450
     6 Adelie  Torgersen             39            21               190        3650
     7 Adelie  Torgersen             39            18               181        3625
     8 Adelie  Torgersen             39            20               195        4675
     9 Adelie  Torgersen             34            18               193        3475
    10 Adelie  Torgersen             42            20               190        4250
    # ℹ 334 more rows
    # ℹ 2 more variables: sex <fct>, year <int>

#### 📝Exercise 3

1.  Create a new column called `bill_depth_cm` that converts the
    `bill_depth_mm` columns from millimeters to centimeters.

2.  Create a new column called `flipper_size` that categorizes penguins
    as short, average, or long based on their `flipper_length_mm`. Hint:
    Define short as less than 190 mm, average as between 190 and 210 mm,
    and long as greater than 210 mm.

    ``` r
    penguins |> 
      mutate(bill_depth_cm = bill_depth_mm / 0.1)
    ```

        # A tibble: 344 × 9
           species island    bill_length_mm bill_depth_mm flipper_length_mm body_mass_g
           <fct>   <fct>              <dbl>         <dbl>             <int>       <int>
         1 Adelie  Torgersen           39.1          18.7               181        3750
         2 Adelie  Torgersen           39.5          17.4               186        3800
         3 Adelie  Torgersen           40.3          18                 195        3250
         4 Adelie  Torgersen           NA            NA                  NA          NA
         5 Adelie  Torgersen           36.7          19.3               193        3450
         6 Adelie  Torgersen           39.3          20.6               190        3650
         7 Adelie  Torgersen           38.9          17.8               181        3625
         8 Adelie  Torgersen           39.2          19.6               195        4675
         9 Adelie  Torgersen           34.1          18.1               193        3475
        10 Adelie  Torgersen           42            20.2               190        4250
        # ℹ 334 more rows
        # ℹ 3 more variables: sex <fct>, year <int>, bill_depth_cm <dbl>

    ``` r
    penguins |> 
      mutate(flipper_size = case_when(
        flipper_length_mm < 190 ~ "short",
        190 <= flipper_length_mm & flipper_length_mm <= 210 ~ "average",
        flipper_length_mm > 210 ~ "long",
        .default = "unknown"
      ))
    ```

        # A tibble: 344 × 9
           species island    bill_length_mm bill_depth_mm flipper_length_mm body_mass_g
           <fct>   <fct>              <dbl>         <dbl>             <int>       <int>
         1 Adelie  Torgersen           39.1          18.7               181        3750
         2 Adelie  Torgersen           39.5          17.4               186        3800
         3 Adelie  Torgersen           40.3          18                 195        3250
         4 Adelie  Torgersen           NA            NA                  NA          NA
         5 Adelie  Torgersen           36.7          19.3               193        3450
         6 Adelie  Torgersen           39.3          20.6               190        3650
         7 Adelie  Torgersen           38.9          17.8               181        3625
         8 Adelie  Torgersen           39.2          19.6               195        4675
         9 Adelie  Torgersen           34.1          18.1               193        3475
        10 Adelie  Torgersen           42            20.2               190        4250
        # ℹ 334 more rows
        # ℹ 3 more variables: sex <fct>, year <int>, flipper_size <chr>

## 6️⃣Compute summary statistics: summarise()

We often need to summarize our data to understand key characteristics
(e.g., measures of central tendency, measures of variability,
frequency). The
[`summarise()`](https://dplyr.tidyverse.org/reference/summarise.html)
function lets us calculate these summary statistics efficiently.

💻Calculate the mean body mass for all the penguins in the data.

``` r
penguins |> 
  summarise(mean_body_mass = mean(body_mass_g, na.rm = TRUE))
```

    # A tibble: 1 × 1
      mean_body_mass
               <dbl>
    1          4202.

The function creates a new data frame with a single row containing the
summary statistic.

## 7️⃣Group by one or more variables: group_by()

In data analysis, a common task is to split our data into groups, apply
a function to each group, and then combine the results. This approach is
known as the split-apply-combine paradigm. The
[`group_by()`](https://dplyr.tidyverse.org/reference/group_by.html)
function helps us achieve this by allowing us to specify how we want to
split our data into groups.

💻Calculate mean body mass for each species:

``` r
penguins |> 
  group_by(species) |> 
  summarise(mean_body_mass = mean(body_mass_g, na.rm = TRUE))
```

    # A tibble: 3 × 2
      species   mean_body_mass
      <fct>              <dbl>
    1 Adelie             3701.
    2 Chinstrap          3733.
    3 Gentoo             5076.

A grouped data frame has all the properties of a regular data frame but
has an additional property that describes the grouping structure. R will
treat each group as it were a separate data frame, so operations like
`summarise()` are applied to each group separately, and then brought
back together.

💻Count the number of penguins on each island:

``` r
penguins |> 
  group_by(island) |> 
  tally()
```

    # A tibble: 3 × 2
      island        n
      <fct>     <int>
    1 Biscoe      168
    2 Dream       124
    3 Torgersen    52

Alternatively, use the
[`count()`](https://dplyr.tidyverse.org/reference/count.html) function,
which combines `group_by()` and `tally()` in one step.

💻Calculate the mean and standard deviation of body mass for each
combination of species and sex:

``` r
penguins |> 
  group_by(species, sex) |> 
  summarise(mean_body_mass = mean(body_mass_g, na.rm = TRUE),
            sd_body_mass = sd(body_mass_g, na.rm = TRUE))
```

    `summarise()` has grouped output by 'species'. You can override using the
    `.groups` argument.

    # A tibble: 8 × 4
    # Groups:   species [3]
      species   sex    mean_body_mass sd_body_mass
      <fct>     <fct>           <dbl>        <dbl>
    1 Adelie    female          3369.         269.
    2 Adelie    male            4043.         347.
    3 Adelie    <NA>            3540          477.
    4 Chinstrap female          3527.         285.
    5 Chinstrap male            3939.         362.
    6 Gentoo    female          4680.         282.
    7 Gentoo    male            5485.         313.
    8 Gentoo    <NA>            4588.         338.

By default, when we apply a grouping with multiple factors, dplyr will
keep the last level of grouping after the summary. Here, the output is
still grouped by `species`. To remove grouping from a data frame, use
the `ungroup()` function or the `.groups = "drop"` argument in the
`summarise()` function. Both methods will allow us to continue working
with the data as a regular data frame.

``` r
# option 1
penguins |> 
  group_by(species, sex) |> 
  summarise(mean_body_mass = mean(body_mass_g, na.rm = TRUE),
            sd_body_mass = sd(body_mass_g, na.rm = TRUE)) |> 
  ungroup()
```

    `summarise()` has grouped output by 'species'. You can override using the
    `.groups` argument.

    # A tibble: 8 × 4
      species   sex    mean_body_mass sd_body_mass
      <fct>     <fct>           <dbl>        <dbl>
    1 Adelie    female          3369.         269.
    2 Adelie    male            4043.         347.
    3 Adelie    <NA>            3540          477.
    4 Chinstrap female          3527.         285.
    5 Chinstrap male            3939.         362.
    6 Gentoo    female          4680.         282.
    7 Gentoo    male            5485.         313.
    8 Gentoo    <NA>            4588.         338.

``` r
# option 2
df <- penguins |> 
  group_by(species, sex) |> 
  summarise(mean_body_mass = mean(body_mass_g, na.rm = TRUE),
            sd_body_mass = sd(body_mass_g, na.rm = TRUE),
            .groups = "drop")
```

Similar to what we’ve seen in other functions, we can create multiple
summaries in a single step by separating them with commas.

## 8️⃣Sort rows and extract specific values: arrange(), slice(), pull()

Sometimes, we’re interested in extracting a particular value from a data
frame, like finding the largest or smallest value in a column.

1.  💻Use the
    [`arrange()`](https://dplyr.tidyverse.org/reference/arrange.html)
    function to reorder our data by `mean_body_mass` in descending order

2.  💻Once our data is sorted, use
    [`slice()`](https://dplyr.tidyverse.org/reference/slice.html) to
    retrieve the row with the largest mean body mass

3.  💻Use [`pull()`](https://dplyr.tidyverse.org/reference/pull.html) to
    extract the mean body mass value from the data as a single numeric
    vector

``` r
df |> 
  arrange(desc(mean_body_mass)) |> 
  slice(1) |> 
  pull(mean_body_mass)
```

    [1] 5484.836

#### 📝Exercise 4

Use the dplyr verbs we have learned so far to analyze penguin flipper
length:

- Select the relevant columns: `island`, `species`, and
  `flipper_length_mm`

- Group the data by `island` and `species`, then calculate the
  **average** and **maximum** flipper length for each group

- Arrange the results by the **average** flipper length in **ascending**
  order

Hint: Remember to handle missing values using `na.rm = TRUE` when
calculating the summary statistics.

``` r
penguins |> 
  select(island, species, flipper_length_mm) |> 
  group_by(island, species) |> 
  summarise(mean_flipper_len = mean(flipper_length_mm, na.rm = TRUE),
            max_flipped_len = max(flipper_length_mm, na.rm = TRUE),
            .groups = "drop") |> 
  arrange(mean_flipper_len)
```

    # A tibble: 5 × 4
      island    species   mean_flipper_len max_flipped_len
      <fct>     <fct>                <dbl>           <int>
    1 Biscoe    Adelie                189.             203
    2 Dream     Adelie                190.             208
    3 Torgersen Adelie                191.             210
    4 Dream     Chinstrap             196.             212
    5 Biscoe    Gentoo                217.             231

## 🌟Bonus: lengthen and widen data: pivot_longer(), pivot_wider()

The [tidyr](https://tidyr.tidyverse.org/) package is another key
component of the tidyverse. It focuses on reshaping data to ensure it’s
in the right format for analysis.

Sometimes we need data in a “long” format (more rows, fewer columns) for
certain analyses, or in a “wide” format (more columns, fewer rows) for
others. For example, a wide data set might have separate columns for
different measurements (e.g., bill length, flipper length), but for some
analyses or visualizations, we might need to convert this into a long
format where all measurements are in a single column, with another
column specifying the measurement type.

Imagine our observation of interest is the measurement itself, rather
than the penguin…

💻Use the
[`pivot_longer()`](https://tidyr.tidyverse.org/reference/pivot_longer.html)
function to reshape the `penguins` data so that all the measurements
related to the penguins are in a single column, and another column
indicates what measurement type it is:

``` r
penguins_long <- penguins |> 
  mutate(id = row_number()) |> 
  pivot_longer(
    cols = ends_with("_mm") | ends_with("_g"),
    names_to = "measurement_type",
    values_to = "value"
  )
```

What if we want to go back to the original wide format?

💻Use the
[`pivot_wider()`](https://tidyr.tidyverse.org/reference/pivot_wider.html)
function to reverse the process:

``` r
penguins_wide <- penguins_long |> 
  pivot_wider(
    names_from = measurement_type,
    values_from = value
)
```

## 📚Resources

| Function                | Description                                      |
|-------------------------|--------------------------------------------------|
| `dplyr::glimpse()`      | Get a glimpse of your data                       |
| `dplyr::select()`       | Keep or drop columns using their names and types |
| `dplyr::filter()`       | Keep rows that match a condition                 |
| `dplyr::mutate()`       | Create, modify, and delete columns               |
| `dplyr::summarise()`    | Summarise each group down to one row             |
| `dplyr::group_by()`     | Group by one or more variables                   |
| `dplyr::count()`        | Count the observations in each group             |
| `dplyr::arrange()`      | Order rows using column values                   |
| `dplyr::slice()`        | Subset rows using their positions                |
| `dplyr::pull()`         | Extract a single column                          |
| `tidyr::pivot_longer()` | Pivot data from wide to long                     |
| `tidyr::pivot_wider()`  | Pivot data from long to wide                     |

- [Data transformation with dplyr
  cheatsheet](https://rstudio.github.io/cheatsheets/html/data-transformation.html?_gl=1*1foigmt*_ga*Mjg1MTU3NDY3LjE3MDU2OTExOTY.*_ga_2C0WZ1JHG0*MTcyNDA3ODA5Ni4yMy4xLjE3MjQwNzgxNDkuMC4wLjA.)
