# Soil properties

A collection of soil property data.

## Usage

``` r
soil_parameters
```

## Format

A list of named elements, where each element represents the hydraulic
properties of one type of soil. The soil types are defined following the
[USDA soil texture
classification](https://en.wikipedia.org/wiki/Soil_texture) scheme, and
11 of the 12 possible types are included ("silt" is not available). The
following names are used to indicate the various soil types:

- `sand`

- `loamy_sand`

- `sandy_loam`

- `loam`

- `silt_loam`

- `sandy_clay_loam`

- `clay_loam`

- `silty_clay_loam`

- `sandy_clay`

- `silty_clay`

- `clay`

For each soil type, the following parameter values are provided:

- `soil_silt_content` (dimensionless)

- `soil_clay_content` (dimensionless)

- `soil_sand_content` (dimensionless)

- `soil_air_entry` (J / kg)

- `soil_b_coefficient` (dimensionless)

- `soil_saturated_conductivity` (J \* s / m^3)

- `soil_saturation_capacity` (dimensionless)

- `soil_field_capacity` (dimensionless)

- `soil_wilting_point` (dimensionless)

- `soil_bulk_density` (Mg / m^3)

## Source

These soil property values are based on Table 9.1 from Campbell and
Norman's textbook *An Introduction to Environmental Biophysics* (1998).
Bulk density values are taken from function `getsoilprop.c` from Melanie
(Colorado). The bulk density of sand in `getsoilprop.c` is 0, which
isn't sensible, and here a value of `1.60 Mg / m^3` is used instead.

The wilting point value of 0.21 (corrected from 0.32) for silty clay
loam is based on the list of book corrections available from [Brian
Hornbuckle's teaching
website](https://web.archive.org/web/20150806180927/http://www.public.iastate.edu/~bkh/teaching/505/norman_book_corrections.pdf)
using the Wayback Machine, since it does not seem to be available on his
[current site](https://faculty.sites.iastate.edu/bkh/teaching).
