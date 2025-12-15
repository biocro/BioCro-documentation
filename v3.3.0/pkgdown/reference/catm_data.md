# Global annual mean atmopspheric CO2 levels

Multiple years of globally averaged annual mean atmospheric CO2 levels
and their uncertainties.

This data is included in the BioCro package so users can reproduce
calculations in Lochocki *et al.* (2022)
\[[doi:10.1093/insilicoplants/diac003](https://doi.org/10.1093/insilicoplants/diac003)
\] and for exploratory purposes; it is likely that most BioCro studies
will require different data sets, and no attempt is made here to be
exhaustive.

## Usage

``` r
catm_data
```

## Format

Data frame with 3 columns and 45 rows:

- `year`: the year

- `Catm`: CO2 concentration (micromol / mol)

- `unc`: the uncertainty associated with the CO2 concentration (micromol
  / mol)

## Source

Data were obtained from the National Oceanic and Atmospheric
Administration's Global Monitoring Laboratory
(https://gml.noaa.gov/ccgg/trends/data.html) on 2025-10-21.

The exact link used was
https://gml.noaa.gov/webdata/ccgg/trends/co2/co2_annmean_gl.txt.

Alternatively, the data can be accessed from
https://gml.noaa.gov/ccgg/trends/gl_data.html by clicking the link to
`Globally averaged marine surface annual mean data (CSV)`.

These data are provided here as a convenience to BioCro users; please
visit the NOAA GML webpage for guidelines regarding the use of this data
if you are intending to include it in a publication.
