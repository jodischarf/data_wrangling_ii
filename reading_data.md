Reading data from the web
================

## Scrape a table

I want the first table from \[this page\]
(<http://samhda.s3-us-gov-west-1.amazonaws.com/s3fs-public/field-uploads/2k15StateFiles/NSDUHsaeShortTermCHG2015.htm>)

read in the html

``` r
url = "http://samhda.s3-us-gov-west-1.amazonaws.com/s3fs-public/field-uploads/2k15StateFiles/NSDUHsaeShortTermCHG2015.htm"

drug_use_html = read_html(url)

drug_use_html
```

    ## {html_document}
    ## <html lang="en">
    ## [1] <head>\n<link rel="P3Pv1" href="http://www.samhsa.gov/w3c/p3p.xml">\n<tit ...
    ## [2] <body>\r\n\r\n<noscript>\r\n<p>Your browser's Javascript is off. Hyperlin ...

extract the table(s)

``` r
drug_use_html %>%
  html_nodes(css = "table")
```

    ## {xml_nodeset (15)}
    ##  [1] <table class="rti" border="1" cellspacing="0" cellpadding="1" width="100 ...
    ##  [2] <table class="rti" border="1" cellspacing="0" cellpadding="1" width="100 ...
    ##  [3] <table class="rti" border="1" cellspacing="0" cellpadding="1" width="100 ...
    ##  [4] <table class="rti" border="1" cellspacing="0" cellpadding="1" width="100 ...
    ##  [5] <table class="rti" border="1" cellspacing="0" cellpadding="1" width="100 ...
    ##  [6] <table class="rti" border="1" cellspacing="0" cellpadding="1" width="100 ...
    ##  [7] <table class="rti" border="1" cellspacing="0" cellpadding="1" width="100 ...
    ##  [8] <table class="rti" border="1" cellspacing="0" cellpadding="1" width="100 ...
    ##  [9] <table class="rti" border="1" cellspacing="0" cellpadding="1" width="100 ...
    ## [10] <table class="rti" border="1" cellspacing="0" cellpadding="1" width="100 ...
    ## [11] <table class="rti" border="1" cellspacing="0" cellpadding="1" width="100 ...
    ## [12] <table class="rti" border="1" cellspacing="0" cellpadding="1" width="100 ...
    ## [13] <table class="rti" border="1" cellspacing="0" cellpadding="1" width="100 ...
    ## [14] <table class="rti" border="1" cellspacing="0" cellpadding="1" width="100 ...
    ## [15] <table class="rti" border="1" cellspacing="0" cellpadding="1" width="100 ...

extract the table(s); focus on the first one

``` r
table_marj = 
  drug_use_html %>% 
  html_nodes(css = "table") %>% 
  first() %>%
  html_table() 
```

``` r
table_marj = 
  drug_use_html %>% 
  html_nodes(css = "table") %>% 
  first() %>% 
  html_table() %>%
  slice(-1) %>% 
  as_tibble()

table_marj
```

    ## # A tibble: 56 × 16
    ##    State      `12+(2013-2014)` `12+(2014-2015)` `12+(P Value)` `12-17(2013-2014…
    ##    <chr>      <chr>            <chr>            <chr>          <chr>            
    ##  1 Total U.S. 12.90a           13.36            0.002          13.28b           
    ##  2 Northeast  13.88a           14.66            0.005          13.98            
    ##  3 Midwest    12.40b           12.76            0.082          12.45            
    ##  4 South      11.24a           11.64            0.029          12.02            
    ##  5 West       15.27            15.62            0.262          15.53a           
    ##  6 Alabama    9.98             9.60             0.426          9.90             
    ##  7 Alaska     19.60a           21.92            0.010          17.30            
    ##  8 Arizona    13.69            13.12            0.364          15.12            
    ##  9 Arkansas   11.37            11.59            0.678          12.79            
    ## 10 California 14.49            15.25            0.103          15.03            
    ## # … with 46 more rows, and 11 more variables: 12-17(2014-2015) <chr>,
    ## #   12-17(P Value) <chr>, 18-25(2013-2014) <chr>, 18-25(2014-2015) <chr>,
    ## #   18-25(P Value) <chr>, 26+(2013-2014) <chr>, 26+(2014-2015) <chr>,
    ## #   26+(P Value) <chr>, 18+(2013-2014) <chr>, 18+(2014-2015) <chr>,
    ## #   18+(P Value) <chr>
