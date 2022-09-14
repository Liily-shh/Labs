Lab04
================
Lily
2022-09-14

``` r
library(lubridate)
```

    ## 
    ## Attaching package: 'lubridate'

    ## The following objects are masked from 'package:base':
    ## 
    ##     date, intersect, setdiff, union

``` r
library(tidyverse)
```

    ## ── Attaching packages
    ## ───────────────────────────────────────
    ## tidyverse 1.3.2 ──

    ## ✔ ggplot2 3.3.6     ✔ purrr   0.3.4
    ## ✔ tibble  3.1.8     ✔ dplyr   1.0.9
    ## ✔ tidyr   1.2.0     ✔ stringr 1.4.1
    ## ✔ readr   2.1.2     ✔ forcats 0.5.2
    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ lubridate::as.difftime() masks base::as.difftime()
    ## ✖ lubridate::date()        masks base::date()
    ## ✖ dplyr::filter()          masks stats::filter()
    ## ✖ lubridate::intersect()   masks base::intersect()
    ## ✖ dplyr::lag()             masks stats::lag()
    ## ✖ lubridate::setdiff()     masks base::setdiff()
    ## ✖ lubridate::union()       masks base::union()

``` r
library(data.table)
```

    ## 
    ## Attaching package: 'data.table'
    ## 
    ## The following objects are masked from 'package:dplyr':
    ## 
    ##     between, first, last
    ## 
    ## The following object is masked from 'package:purrr':
    ## 
    ##     transpose
    ## 
    ## The following objects are masked from 'package:lubridate':
    ## 
    ##     hour, isoweek, mday, minute, month, quarter, second, wday, week,
    ##     yday, year

``` r
if (!file.exists("met_all.gz"))
  download.file(
    url = "https://raw.githubusercontent.com/USCbiostats/data-science-data/master/02_met/met_all.gz",
    destfile = "met_all.gz",
    method   = "libcurl",
    timeout  = 60
    )
met <- data.table::fread("met_all.gz")
```

``` r
met <- met[temp> -17][elev == 9999.0, elev:=NA]
```

``` r
met <- met [ , ymd:= as.Date(paste(year, month, day, sep = "-"))]
```

``` r
met[, table(week(ymd))]
```

    ## 
    ##     31     32     33     34     35 
    ## 297259 521600 527922 523847 446576

``` r
met <- met[week(ymd) == 31]
```

``` r
met[, . (
    temp      = mean(temp, na.rm=T),
    rh        = mean(rh, na.rm=T),
    wind.sp   = mean(wind.sp, na.rm=T),
    vis.dist  = mean(vis.dist, na.rm=T),
    dew.point = mean(dew.point, na.rm=T),
    lat       = mean(lat, na.rm=T),
    lon       = mean(lon, na.rm=T),
    elev      = mean(elev, na.rm=T)
)]
```

    ##        temp       rh  wind.sp vis.dist dew.point      lat      lon     elev
    ## 1: 24.01861 71.48636 1.973041 14901.39  17.49984 37.91779 -92.2633 415.6731

``` r
met_avg <- met[, . (
    temp      = mean(temp, na.rm=T),
    rh        = mean(rh, na.rm=T),
    wind.sp   = mean(wind.sp, na.rm=T),
    vis.dist  = mean(vis.dist, na.rm=T),
    dew.point = mean(dew.point, na.rm=T),
    lat       = mean(lat, na.rm=T),
    lon       = mean(lon, na.rm=T),
    elev      = mean(elev, na.rm=T)
), by = "USAFID"]
```
