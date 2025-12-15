# Miscanthus model definition

Initial values, parameters, direct modules, differential modules, and a
differential equation solver that can be used to run *Miscanthus x
giganteus* growth simulations in Champaign, Illinois and other
locations.

To represent *Miscanthus* growth in Champaign, IL, these values must be
paired with the Champaign weather data
([`cmi_weather_data`](cmi_weather_data.md)). The parameters already
include the `clay_loam` values from the
[`soil_parameters`](soil_parameters.md) dataset, which is the
appropriate soil type for Champaign.

Some specifications, such as the values of photosynthetic parameters,
would remain the same in any location; others, such as the latitude or
longitude, would need to change when simulating crop growth in different
locations. Care must be taken to understand each input quantity before
attempting to run simulations in other places or for other cultivars.

## Usage

``` r
miscanthus_x_giganteus
```

## Format

A list of 5 named elements that are suitable for passing to
[`run_biocro`](run_biocro.md), as described in the help page for
[`crop_model_definitions`](crop_model_definitions.md).

## Source

This model was originally described in Miguez *et al.* (2009)
\[[doi:10.1111/j.1757-1707.2009.01019.x](https://doi.org/10.1111/j.1757-1707.2009.01019.x)
\] and Miguez *et al.* (2012)
\[[doi:10.1111/j.1757-1707.2011.01150.x](https://doi.org/10.1111/j.1757-1707.2011.01150.x)
\]. Since its original parameterization, the behavior of several of its
core modules has changed as bugs have been identified and fixed, so this
model likely needs to be reparameterized before it can be used for
realistic simulations.

## See also

- [`run_biocro`](run_biocro.md)

- [`modules`](modules.md)

- [`crop_model_definitions`](crop_model_definitions.md)
