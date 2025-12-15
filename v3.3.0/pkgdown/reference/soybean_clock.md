# Soybean-BioCro circadian clock model definition

Initial values, parameters, direct modules, differential modules, and a
differential equation solver that can be used to run soybean circadian
clock simulations in Champaign, Illinois and other locations. Along with
the soybean growth specifications ([`soybean`](soybean.md)), these
values define the soybean growth model of Matthews *et al.* (2022)
\[[doi:10.1093/insilicoplants/diab032](https://doi.org/10.1093/insilicoplants/diab032)
\], which is commonly referred to as *Soybean-BioCro*.

To represent a soybean circadian clock in Champaign, Illinois, these
values must be paired with the weather data from
[`cmi_weather_data`](cmi_weather_data.md).

## Usage

``` r
soybean_clock
```

## Format

A list of 5 named elements that are suitable for passing to
[`run_biocro`](run_biocro.md), as described in the help page for
[`crop_model_definitions`](crop_model_definitions.md).

## Source

This model is described in detail in Matthews *et al.* (2022)
\[[doi:10.1093/insilicoplants/diab032](https://doi.org/10.1093/insilicoplants/diab032)
\] and Lochocki & McGrath (2021)
\[[doi:10.1093/insilicoplants/diab016](https://doi.org/10.1093/insilicoplants/diab016)
\].

Here, we use initial phases for the dawn and dusk oscillators of `200.0`
and `80.0` radians, respectively. These values are optimized for
simulations beginning at midnight on January 1, and should require
minimal time for transient signals to die down. These values were
determined by running a simulation for one year starting on January 1,
and recording the oscillator states at the end of December 31.

## See also

- [`run_biocro`](run_biocro.md)

- [`modules`](modules.md)

- [`crop_model_definitions`](crop_model_definitions.md)

- [`soybean`](soybean.md)
