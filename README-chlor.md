GOM-series chlorophyll
================

``` r
source("setup.R")
```

### CMEMS Copernicus multi-year chlorophyll

We downloaded monthly mean chlorophyll data from
[Copernicus](https://data.marine.copernicus.eu/product/OCEANCOLOUR_GLO_BGC_L4_MY_009_108/description)
to local storage in GeoTIFF format. From these we extracted mean
chlorophyll for each study region. Missing data were ignored
(`mean(x, na.rm = TRUE)`) before computing the mean.

``` r
x <- read_chlor_cmems(logscale = FALSE) |> 
  dplyr::mutate(month = factor(format(date, "%b"), levels = month.abb)) |>
  dplyr::group_by(region)
```

``` r
ggplot(data = x, aes(x = date, y = chlor)) +
  scale_y_log10() + 
  geom_line() + 
  geom_smooth(method = "lm", se = FALSE) +
  facet_wrap(~ region)
```

    ## `geom_smooth()` using formula = 'y ~ x'

    ## Warning: Removed 4 rows containing non-finite values (`stat_smooth()`).

![](README-chlor_files/figure-gfm/unnamed-chunk-3-1.png)<!-- -->

``` r
georges_basin = dplyr::filter(x, region == 'Georges Basin')
ggplot(data = georges_basin, aes(x = date, y = chlor)) +
  scale_y_log10() + 
  geom_line() + 
  geom_smooth(method = "lm", se = FALSE) +
  facet_wrap(~ month)
```

    ## `geom_smooth()` using formula = 'y ~ x'

    ## Warning: Removed 4 rows containing non-finite values (`stat_smooth()`).

![](README-chlor_files/figure-gfm/unnamed-chunk-4-1.png)<!-- -->