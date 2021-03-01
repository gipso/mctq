
<!-- README.md is generated from README.Rmd. Please edit that file -->

# mctq <a href='https://gipsousp.github.io/mctq'><img src='man/figures/logo.png' align="right" height="139" /></a>

<!-- badges: start -->

[![Project Status: Active – The project has reached a stable, usable
state and is being actively
developed.](https://www.repostatus.org/badges/latest/active.svg)](https://www.repostatus.org/#active)
[![Lifecycle:
maturing](https://img.shields.io/badge/lifecycle-maturing-blue.svg)](https://www.tidyverse.org/lifecycle/#maturing)
[![R-CMD-check](https://github.com/gipsousp/mctq/workflows/R-CMD-check/badge.svg)](https://github.com/gipsousp/mctq/actions)
[![Travis build
status](https://travis-ci.com/gipsousp/mctq.svg?branch=master)](https://travis-ci.com/gipsousp/mctq)
[![Codecov test
coverage](https://codecov.io/gh/gipsousp/mctq/branch/master/graph/badge.svg)](https://codecov.io/gh/gipsousp/mctq?branch=master)
[![Contributor
Covenant](https://img.shields.io/badge/Contributor%20Covenant-v2.0%20adopted-ff69b4.svg)](https://gipsousp.github.io/mctq/CODE_OF_CONDUCT.html)
<!-- badges: end -->

## Overview

`mctq` provides a consistent and fast way to process Munich Chronotype
Questionnaire (MCTQ) data in R for all of its three versions (standard,
micro, and shift).

Please note that this package is currently on the development stage and
have not been [peer
reviewed](https://devguide.ropensci.org/softwarereviewintro.html) yet.
That means that people can try it out and provide feedback, but it comes
with no promises for long term stability.

<!-- ### About MCTQ -->
<!-- __UNDER DEVELOPMENT__ -->
<!-- To learn more about the standard Munich Chronotype Questionnaire (MCTQ), _cf._ Roenneberg, Wirz-Justice, & Merrow (2003), Roenneberg, Allebrandt, Merrow, & Vetter (2012), Roenneberg _et al._ (2015), and Roenneberg, Pilz, Zerbini, & Winnebeck (2019). -->
<!-- To know about different MCTQ versions, _cf._ Juda, Vetter, & Roenneberg (2013) and Ghotbi _et.al_ (2020). -->
<!-- If you curious about the variable computations and want to have access to the full questionnaire, _cf._ The Worldwide Experimental Platform (n.d.). -->

### Wait, a R package for a questionnaire?

If you’re familiar with MCTQ, you know that it’s not a simple
questionnaire, as it requires a lot of date/time manipulation. That can
be real challenging, especially if you’re dealing with a large set of
data.

The main advantage to use the `mctq` package in your research is that
you will have reliable tools, thoroughly tested, at your disposition,
made and supported by a sleep science research group (GIPSO) from a well
know university ([USP](https://www5.usp.br/)). `mctq` also helps with
research reproducibility, since it’s a free and open source package that
anyone can find and use.

This package is also equipped with several utility functions that allows
you to easily convert and visualize your MCTQ data. It also provides
fictional datasets for testing and learning purposes.

## Installation

The first stable `mctq` version is in its final development stage, we
hope that it will be available on [CRAN](https://cran.r-project.org/)
soon. Until that moment comes, you can install the development version
from GitHub with:

``` r
# install.packages("devtools")
devtools::install_github("gipsousp/mctq", dependencies = TRUE)
```

## Usage

`mctq` works with a set of object classes specially created to hold time
values. This classes can be found in the
[hms](https://hms.tidyverse.org/) and
[lubridate](https://lubridate.tidyverse.org/) package from
[tidyverse](https://www.tidyverse.org/packages/). If your data do not
conform to the object classes required, don’t worry, just use
`convert()` to convert it. You can always convert it back if you want.

Here are some examples of how to convert your data using `convert()`:

``` r
# From decimal hours to `hms`
convert(6.5, "hms", input_unit = "H")
#> 06:30:00
# From radians to `Duration`
convert(1.308997, "Duration", input_unit = "rad")
#> [1] "18000s (~5 hours)"
# From radians to decimal minutes
convert(0.2617994, "numeric", input_unit = "rad", output_unit = "M")
#> [1] 60
# From `character` `HMS` to `Duration`
convert("19:55:17", "Duration", orders = "HMS")
#> [1] "71717s (~19.92 hours)"
# From `character` `HM AM/PM ` to `hms`
convert("10:00 PM", "hms", orders = "IMp")
#> 22:00:00
```

### Basic MCTQ computation

After your data is all set, just use the `mctq` functions below to
process it.

Note that `mctq` uses a similar naming scheme as those used in the MCTQ
articles, that make it easy to find and apply any computation necessary.

-   `fd()` compute MCTQ work-free days
-   `so()` compute MCTQ sleep onset
-   `gu()` compute MCTQ local time of getting out of bed
-   `sd()` compute MCTQ sleep duration
-   `tbt()` compute MCTQ total time in bed
-   `ms()` compute MCTQ mid-sleep
-   `napd()` compute MCTQ nap duration (only for MCTQ Shift)
-   `sd24()` compute MCTQ 24h sleep duration (only for MCTQ Shift)

Example:

``` r
# Local time of preparing to sleep on workdays
sprep_w <- c(hms::parse_hms("23:45:00"), hms::parse_hms("02:15:00"))
# Sleep latency on workdays
slat_w <- c(lubridate::dminutes(30), lubridate::dminutes(90))
# Sleep onset
so(sprep_w, slat_w)
#> 00:15:00
#> 03:45:00
```

### MCTQ analysis

For computations combining workdays and work-free days, use:

-   `sd_week()` compute MCTQ average weekly sleep duration
-   `sd_overall()` compute MCTQ overall sleep duration (only for MCTQ
    Shift)
-   `sloss_week()` compute MCTQ weekly sleep loss
-   `le_week()` compute MCTQ average weekly light exposure
-   `msf_sc()` compute MCTQ chronotype or corrected mid-sleep on
    work-free days
-   `sjl()` and `sjl_rel()` compute MCTQ social jet lag
-   `sjl_weighted()` compute MCTQ absolute social jetlag across all
    shifts (only for MCTQ Shift)

Example:

``` r
# Mid-sleep on workdays
msw <- c(hms::parse_hms("02:05:00"), hms::parse_hms("04:05:00"))
# Mid-sleep on work-free days
msf <- c(hms::parse_hms("23:05:00"), hms::parse_hms("08:30:00"))
# Relative social jetlag
sjl_rel(msw, msf)
#> [1] "-10800s (~-3 hours)"  "15900s (~4.42 hours)"
```

### Utilities

In addition to `convert()`, `mctq` is also equipped with many other
utility functions.

All functions are well documented, showing all the guidelines behind the
computations. Click
[here](https://gipsousp.github.io/mctq/reference/index.html) to see a
list of them.

## Citation

If you use `mctq` in your research, please consider citing it. We put a
lot of work to build and maintain a free and open source R package. You
can find `mctq` citation below.

``` r
citation("mctq")
#> 
#> To cite mctq in publications use:
#> 
#>   Vartanian D., Benedito-Silva, A. A., Pedrazzoli, M. (2021). mctq: a R
#>   package for the Munich ChronoType Questionnaire (MCTQ). Retrieved
#>   from https://gipsousp.github.io/mctq/ .
#> 
#> A BibTeX entry for LaTeX users is
#> 
#>   @Unpublished{,
#>     title = {mctq: a R package for the Munich ChronoType Questionnaire (MCTQ)},
#>     author = {Daniel Vartanian and Ana Amelia Benedito-Silva and Mario Pedrazzoli},
#>     year = {2021},
#>     url = {https://gipsousp.github.io/mctq/},
#>     note = {Lifecycle: maturing},
#>   }
```

## Contributing

`mctq` is a community project, anyone and everyone is welcome to
contribute. Take a moment to review our [Guidelines for
Contributing](https://gipsousp.github.io/mctq/CONTRIBUTING.html).

Please note that `mctq` is released with a [Contributor Code of
Conduct](https://gipsousp.github.io/mctq/CODE_OF_CONDUCT.html). By
contributing to this project, you agree to abide by its terms.
