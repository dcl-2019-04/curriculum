---
title: Google sheets
---

<!-- Generated automatically from googlesheets.yml. Do not edit by hand -->

# Google sheets <small class='wrangle'>[wrangle]</small>
<small>(Builds on: [Parsing basics](parse-basics.md))</small>


``` r
# Libraries
library(tidyverse)
library(googlesheets)

# Parameters
  # URL for Gapminder example
url_gapminder <- "https://docs.google.com/spreadsheets/d/1BzfL0kZUz1TsI5zxJF1WNF01IxvC67FbOJUiiGMZ_mQ/"
```

In this reading, we’ll show you how to use the googlesheets package by
Jenny Bryan to (surprise\!) extract data from Google sheets. Google
sheets are a surprisingly useful way to collect and collaboratively work
with data, and the googlesheets package makes it easy to pull that data
into R. One useful workflow involves using Google forms to collect form
data into a Google sheet, and then using the googlesheets package to
extract and analyze that data in R.

## Public sheets

Some Google sheets are public, which means that anyone can read them.
Take a look at this
[example](https://docs.google.com/spreadsheets/d/1BzfL0kZUz1TsI5zxJF1WNF01IxvC67FbOJUiiGMZ_mQ/)
of data from [Gapminder](https://www.gapminder.org/).

Each Google sheet has a sheet key. You’ll need this key to load a sheet
with googlesheets. Here’s how to get the sheet key from a sheet’s URL.

``` r
sheet_key <- extract_key_from_url(url_gapminder)

sheet_key
```

    ## [1] "1BzfL0kZUz1TsI5zxJF1WNF01IxvC67FbOJUiiGMZ_mQ"

Once you have the sheet key, you can use it to create a googlesheets
object.

``` r
gs <- gs_key(sheet_key)

class(gs)
```

    ## [1] "googlesheet" "list"

You can also list all the worksheets in a single Google sheet with
`gs_ws_ls()`.

``` r
gs_ws_ls(gs)
```

    ## [1] "Africa"   "Americas" "Asia"     "Europe"   "Oceania"

To read in a worksheet, use `gs_read()`. `gs_read()` takes a
googlesheets object and, optionally, the name of a specific worksheet.

``` r
asia <- 
  gs %>% 
  gs_read(ws = "Asia")
```

    ## Accessing worksheet titled 'Asia'.

    ## Parsed with column specification:
    ## cols(
    ##   country = col_character(),
    ##   continent = col_character(),
    ##   year = col_double(),
    ##   lifeExp = col_double(),
    ##   pop = col_double(),
    ##   gdpPercap = col_double()
    ## )

``` r
asia
```

    ## # A tibble: 396 x 6
    ##    country     continent  year lifeExp      pop gdpPercap
    ##    <chr>       <chr>     <dbl>   <dbl>    <dbl>     <dbl>
    ##  1 Afghanistan Asia       1952    28.8  8425333      779.
    ##  2 Afghanistan Asia       1957    30.3  9240934      821.
    ##  3 Afghanistan Asia       1962    32.0 10267083      853.
    ##  4 Afghanistan Asia       1967    34.0 11537966      836.
    ##  5 Afghanistan Asia       1972    36.1 13079460      740.
    ##  6 Afghanistan Asia       1977    38.4 14880372      786.
    ##  7 Afghanistan Asia       1982    39.9 12881816      978.
    ##  8 Afghanistan Asia       1987    40.8 13867957      852.
    ##  9 Afghanistan Asia       1992    41.7 16317921      649.
    ## 10 Afghanistan Asia       1997    41.8 22227415      635.
    ## # … with 386 more rows

## Private sheets

Accessing private sheets requires you to authenticate to Google.
Authentication is done with this command.

``` r
# Give googlesheets permission to access spreadsheet
gs_auth()
```

A page will open in your browser and you’ll be prompted to log into
Google. Once you’ve logged in, googlesheets will create a file called
`.httr-oauth` in your current directory. **NEVER CHECK THIS INTO GIT OR
UPLOAD IT TO GITHUB**. To ensure that you don’t actually push your
`.httr-oauth`, `gs_auth()` will add `.httr-oauth` to the `.gitignore`
file in your current directory.

The `.httr-oauth` file allows you to avoid logging into Google in the
future. You don’t upload this file to GitHub because if someone else had
it, they could use it to access your Google files.

A common error you’ll encounter involves a googlesheets function being
unable find the `.httr-oauth` file. If you’re using an RStudio project,
your working directory is usually the top level of the project, and so
`gs_auth()` will put `.httr-oauth` in the top-level of the directory.
However, you’ll often be working with an R Markdown document or R script
located in a subfolder, and so the googlesheets function will look for
`.httr-oauth` in that subfolder. To avoid this problem, you can copy
`.httr-oauth` over to your subfolder, so that you have one `.httr-oauth`
the top level of the project and one in your subfolder.

Once authenticated into Google, you now need to find the sheet key for
the relevant Google sheet. Like we did earlier, you can find the sheet’s
URl and then call `extract_key_from_url()` like we did earlier.

With private sheets, another option is to use `gs_ls()` to see all the
sheets to which you have access. `gs_ls()` orders by modification time,
with the most recently modified sheets first.

``` r
gs_ls() %>% view()
```

Locate the relevant sheet and copy its sheet key. Create a variable for
the key in your parameters (e.g., `sheet_key <- "XdkfjOkdjk"`), and then
read in the sheet using

``` r
df <- 
  gs_key(sheet_key) %>%
  gs_read()
```

