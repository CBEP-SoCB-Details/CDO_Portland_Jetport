REvised Graphics for Length of Growing Season
================
Curtis C. Bohlen, Casco Bay Estuary Partnership.
02/01/2021

-   [Load Libraries](#load-libraries)
-   [Read Data](#read-data)
-   [Find First and Last Frost Dates](#find-first-and-last-frost-dates)
-   [Frost Free Graphics](#frost-free-graphics)
    -   [Basic Graphic](#basic-graphic)
    -   [Graphic With No Labels](#graphic-with-no-labels)

<img
    src="https://www.cascobayestuary.org/wp-content/uploads/2014/04/logo_sm.jpg"
    style="position:absolute;top:10px;right:50px;" />

# Load Libraries

``` r
library(tidyverse)
#> -- Attaching packages --------------------------------------- tidyverse 1.3.0 --
#> v ggplot2 3.3.3     v purrr   0.3.4
#> v tibble  3.0.5     v dplyr   1.0.3
#> v tidyr   1.1.2     v stringr 1.4.0
#> v readr   1.4.0     v forcats 0.5.0
#> -- Conflicts ------------------------------------------ tidyverse_conflicts() --
#> x dplyr::filter() masks stats::filter()
#> x dplyr::lag()    masks stats::lag()
library(readr)

library(CBEPgraphics)
load_cbep_fonts()
theme_set(theme_cbep())
```

# Read Data

The frost-free period needs to be derived directly from the daily
temperature data. Note that temperature in the source data is in tenths
of a degree C.

``` r
sibfldnm <- 'Original_Data'
parent <- dirname(getwd())
sibling <- paste(parent,sibfldnm, sep = '/')
fn <- 'longdailydata.csv'

longdailydata <- read_csv(paste(sibling,fn, sep = '/')) %>%
  select(-station) %>%
  filter(datatype %in% c('TMAX', 'TMIN')) %>%
  mutate(doy = as.numeric(format(date, format = '%j'))) %>%
  mutate(month = as.numeric(format(date, format = '%m'))) %>%
  mutate(year = as.numeric(format(date, format = '%Y'))) %>%
  mutate(cyear = year - 1980)
#> 
#> -- Column specification --------------------------------------------------------
#> cols(
#>   date = col_datetime(format = ""),
#>   datatype = col_character(),
#>   value = col_double(),
#>   attributes = col_character(),
#>   station = col_character()
#> )
```

# Find First and Last Frost Dates

The following strategy was suggested by a Stack Overflow answer here:  
<https://stackoverflow.com/questions/41328946/find-last-values-by-condition>

In brief, we filter a dataframe down to data that meets our criteria –
here, days with temperatures below freezing – and then group\_by() to
isolate data from each year, and finally slice() to pick the first or
last item in the list. Be aware that this strategy depends on the data
being in chronological order.

``` r
cutoffinF <- 32
cutoffinC <- (cutoffinF - 32) * 5/9

firstfrosts <- longdailydata %>%
  spread(key = datatype, value = value) %>%
  filter(year > 1940) %>%               # 1940 is an incomplete year in the daily data
  mutate(TMIN = TMIN/10) %>%            # raw data is in tenths of degree C
  filter(TMIN <= cutoffinC) %>%          # Only days below freezing
  filter(month >= 7) %>%                  # only the last half of the year
  group_by(year) %>%
  slice(1)  %>%                         # Choose the first record
  select(date, year, doy)

lastfrosts <- longdailydata %>%
  spread(key = datatype, value = value) %>%
  filter(year > 1940) %>%                 # 1940 is an incomplete year in the daily data
  mutate(TMIN = TMIN/10) %>%            # raw data is in tenths of degree C
  filter(TMIN <= cutoffinC) %>%          # Only days below freezing
  filter(month < 7) %>%                   # only the first half of the year
  group_by(year) %>%
  slice(n())  %>%                       # Choose the last record
  select(date, year, doy)

frosts <- inner_join(firstfrosts,lastfrosts, by = "year", 
                     suffix = c('.first', '.last')) %>%
  mutate(frostfree = doy.first - doy.last - 1) # calculate frost-free period

rm(firstfrosts,lastfrosts)
```

# Frost Free Graphics

## Basic Graphic

Note that as all we fit were simple linear models, we can create
graphics using geom\_smooth() with method =‘lm’. We really only need the
model to extract net change data from model predictions.

``` r
plt <- ggplot(frosts, aes(year, frostfree)) + 
  geom_point(color = 'gray85', size = 1) +
  geom_smooth(method = 'lm', se = FALSE, lty=1, lwd = 0.5, color = cbep_colors()[4]) +
  geom_hline(yintercept = 134, lty = 2, color = 'gray50') +
  #geom_segment(x = 2010, xend = 2023, y = 168, yend = 168,
  #             lty = 2, color = 'gray50') +
  ylab('Frost Free Days') +
  xlab('Year') +
  theme_cbep(base_size = 9)
  
plt
#> `geom_smooth()` using formula 'y ~ x'
```

<img src="REVISED_growing_season_files/figure-gfm/frost_free_plot-1.png" style="display: block; margin: auto;" />

``` r
ggsave('small_frostfree.pdf', device=cairo_pdf, width = 2.5, height = 1.75)
#> `geom_smooth()` using formula 'y ~ x'
```

## Graphic With No Labels

``` r
plt <- ggplot(frosts, aes(year, frostfree)) + 
  geom_point(color = 'gray85', size = 1) +
  geom_smooth(method = 'lm', se = FALSE, lty=1, lwd = 0.5, color = cbep_colors()[4]) +
  geom_hline(yintercept = 134, lty = 2, color = 'gray50') +
  
  theme_cbep(base_size = 9) +
  theme(legend.position= 'none',
        axis.text = element_blank(),
        axis.title = element_blank()) +
  xlab('') +
  ylab('')
  
plt
#> `geom_smooth()` using formula 'y ~ x'
```

<img src="REVISED_growing_season_files/figure-gfm/frost_free_plot_bare-1.png" style="display: block; margin: auto;" />

``` r
ggsave('small_frostfree_bare.pdf', device=cairo_pdf, width = 2.25, height = 1.5)
#> `geom_smooth()` using formula 'y ~ x'
```
