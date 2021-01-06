
<!-- README.md is generated from README.Rmd. Please edit that file -->

# rvest <img src="man/figures/logo.png" align="right" height="139"/>

<!-- badges: start -->

[![CRAN
status](https://www.r-pkg.org/badges/version/rvest)](https://cran.r-project.org/package=rvest)
[![R-CMD-check](https://github.com/tidyverse/rvest/workflows/R-CMD-check/badge.svg)](https://github.com/tidyverse/rvest/actions)
[![Codecov test
coverage](https://codecov.io/gh/tidyverse/rvest/branch/master/graph/badge.svg)](https://codecov.io/gh/tidyverse/rvest?branch=master)

<!-- badges: end -->

rvest helps you scrape information from web pages. It is designed to
work with [magrittr](https://github.com/smbache/magrittr) to make it
easy to express common web scraping tasks, inspired by libraries like
[beautiful soup](https://www.crummy.com/software/BeautifulSoup/).

If you’re scraping multiple pages, I highly recommend using rvest in
concert with [polite](https://dmi3kno.github.io/polite/). The polite
package ensures that you’re respecting the
[robots.txt](https://en.wikipedia.org/wiki/Robots_exclusion_standard)
and not hammering the site with too many requests.

## Installation

``` r
# The easiest way to get rvest is to install the whole tidyverse:
install.packages("tidyverse")

# Alternatively, install just rvest:
install.packages("dplyr")
```

## Usage

``` r
library(rvest)
starwars <- read_html("https://rvest.tidyverse.org/articles/starwars.html")

films <- starwars %>% html_elements("section")
films
#> {xml_nodeset (7)}
#> [1] <section><h2 data-id="1">\nThe Phantom Menace\n</h2>\n<p>\nReleased: 1999 ...
#> [2] <section><h2 data-id="2">\nAttack of the Clones\n</h2>\n<p>\nReleased: 20 ...
#> [3] <section><h2 data-id="3">\nRevenge of the Sith\n</h2>\n<p>\nReleased: 200 ...
#> [4] <section><h2 data-id="4">\nA New Hope\n</h2>\n<p>\nReleased: 1977-05-25\n ...
#> [5] <section><h2 data-id="5">\nThe Empire Strikes Back\n</h2>\n<p>\nReleased: ...
#> [6] <section><h2 data-id="6">\nReturn of the Jedi\n</h2>\n<p>\nReleased: 1983 ...
#> [7] <section><h2 data-id="7">\nThe Force Awakens\n</h2>\n<p>\nReleased: 2015- ...

title <- films %>% 
  html_element("h2") %>% 
  html_text(trim = TRUE)
title
#> [1] "The Phantom Menace"      "Attack of the Clones"   
#> [3] "Revenge of the Sith"     "A New Hope"             
#> [5] "The Empire Strikes Back" "Return of the Jedi"     
#> [7] "The Force Awakens"

episode <- films %>% 
  html_element("h2") %>% 
  html_attr("data-id") %>% 
  readr::parse_integer()
episode
#> [1] 1 2 3 4 5 6 7

released <- films %>% 
  html_element("p:nth-child(2)") %>% 
  html_text(trim = TRUE) %>% 
  gsub("Released: ", "", .) %>% 
  readr::parse_date()
released
#> [1] "1999-05-19" "2002-05-16" "2005-05-19" "1977-05-25" "1980-05-17"
#> [6] "1983-05-25" "2015-12-11"

crawl <- films %>% 
  html_element("div") %>%
  html_text2()
cat(crawl[[1]])
#> Turmoil has engulfed the Galactic Republic. The taxation of trade routes to outlying star systems is in dispute.
#> 
#> Hoping to resolve the matter with a blockade of deadly battleships, the greedy Trade Federation has stopped all shipping to the small planet of Naboo.
#> 
#> While the Congress of the Republic endlessly debates this alarming chain of events, the Supreme Chancellor has secretly dispatched two Jedi Knights, the guardians of peace and justice in the galaxy, to settle the conflict….
```

## Key functions

Once you have read a HTML document with `read_html()`, you can:

-   Select parts of a document using CSS selectors,
    `html_elements(doc, "table td")`, or XPath expressions,
    `html_elements(doc, xpath = "//table//td")`). If you haven’t heard
    of [selectorgadget](http://selectorgadget.com/), make sure to read
    `vignette("selectorgadget")` to learn about it.

-   Extract data from text with `html_text2()` and from attributes with
    `html_attr()`.

-   Parse tables into data frames with `html_table()`.

-   Navigate around a website as if you’re in a browser with
    `html_session()`, `jump_to()`, `follow_link()`, `back()`, and
    `forward()`. Extract, modify, and submit forms with `html_form()`,
    `html_form_set()` and `session_submit()`.

## Inspirations

-   Python:
    [RoboBrowser](http://robobrowser.readthedocs.org/en/latest/readme.html),
    [Beautiful Soup](https://www.crummy.com/software/BeautifulSoup/).

## Code of Conduct

Please note that the rvest project is released with a [Contributor Code
of Conduct](https://rvest.tidyverse.org/CODE_OF_CONDUCT.html). By
contributing to this project, you agree to abide by its terms.
