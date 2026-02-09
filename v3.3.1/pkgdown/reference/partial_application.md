# Convenience Functions for Partial Application

Convenience functions for using partial application with BioCro

## Usage

``` r
partial_run_biocro(
    initial_values = list(),
    parameters = list(),
    drivers,
    direct_module_names = list(),
    differential_module_names = list(),
    ode_solver = BioCro::default_ode_solvers$homemade_euler,
    arg_names,
    verbose = FALSE
)

partial_evaluate_module(
  module_name,
  input_quantities,
  arg_names,
  stop_on_exception = FALSE
)
```

## Arguments

- arg_names:

  A vector of strings specifying input quantities whose values should
  not be fixed when using partial application.

- initial_values:

  Identical to the corresponding argument from
  [`run_biocro`](run_biocro.md).

- parameters:

  Identical to the corresponding argument from
  [`run_biocro`](run_biocro.md).

- drivers:

  Identical to the corresponding argument from
  [`run_biocro`](run_biocro.md).

- direct_module_names:

  Identical to the corresponding argument from
  [`run_biocro`](run_biocro.md).

- differential_module_names:

  Identical to the corresponding argument from
  [`run_biocro`](run_biocro.md).

- ode_solver:

  Identical to the corresponding argument from
  [`run_biocro`](run_biocro.md).

- verbose:

  Identical to the corresponding argument from
  [`run_biocro`](run_biocro.md).

- module_name:

  Identical to the corresponding argument from
  [`evaluate_module`](modules.md).

- input_quantities:

  A list of named numeric elements representing any input quantities
  required by the module that are not included in `arg_names`; any
  extraneous quantities will be ignored by the module.

- stop_on_exception:

  Identical to the corresponding argument from
  [`evaluate_module`](modules.md).

## Details

*Partial application* is the technique of fixing some of the input
arguments to a function, producing a new function with fewer inputs. In
the context of BioCro, partial application can often be useful while
varying some parameters, initial values, or drivers while performing
optimization or sensitivity analysis. Optimizers (such as
[`optim`](https://rdrr.io/r/stats/optim.html)) typically require a
function with a single input argument, so the partial application tools
provided here help to create such functions.

Both `partial_run_biocro` and `partial_evaluate_module` accept the same
arguments as their "regular" counterparts ([`run_biocro`](run_biocro.md)
and [`evaluate_module`](modules.md)) with the addition of `arg_names`,
which specifies the input quantities that should not be fixed.

For `partial_run_biocro`, each element of `arg_names` must be the name
of a quantity that is one of the `initial_values`, `parameters`, or
`drivers`. For `partial_evaluate_module`, each element of `arg_names`
must be the name of one of the module's input quantities.

When using one of the pre-defined crop growth models, it may be helpful
to use the `with` command to pass arguments to `partial_run_biocro`; see
the documentation for
[`crop_model_definitions`](crop_model_definitions.md) for more
information.

## Value

- partial_run_biocro:

  A function that calls [`run_biocro`](run_biocro.md) with all of the
  inputs (except those specified in `arg_names`) set to the values
  specified by the original call to `partial_run_biocro`. The new
  function has one input (`x`), which can be a vector or list specifying
  the values of the quantities in `arg_names`. If `x` has no names, its
  elements must be supplied in the same order as in the original
  `arg_names`. If `x` has names, they must be identical to the elements
  of `arg_names` but can be in any order. Elements of `x` corresponding
  to drivers must be vectors having the same length as the other
  drivers; they can be specified as a named element of a list or as
  sequential elements of a vector without names. The return value of the
  new function is a data frame as would be produced by
  [`run_biocro`](run_biocro.md).

- partial_evaluate_module:

  A function that calls [`evaluate_module`](modules.md) with the input
  quantities (except those specified in `arg_names`) set to the values
  specified by the original call to `partial_evaluate_module`. The new
  function has one input (`x`), which can be a vector or list specifying
  the values of the quantities in `arg_names`. If `x` has no names, its
  elements must be supplied in the same order as in the original
  `arg_names`. If `x` has names, they must be identical to the elements
  of `arg_names` but can be in any order. The return value of the new
  function is a list with two elements (`inputs` and `outputs`), each of
  which is a list of named numeric elements representing the module's
  input and output values. (Note that this differs from the output of
  `evaluate_module`, which only returns the outputs.)

## See also

- [`run_biocro`](run_biocro.md)

- [`evaluate_module`](modules.md)

## Examples

``` r
# Specify weather data to use in these examples
ex_weather <- get_growing_season_climate(weather$'2005')

# Example 1: varying the thermal time values at which senescence starts for
# different organs in a simulation; here we set them to the following values
# instead of the defaults:
#  - seneLeaf: 2000 degrees C * day
#  - seneStem: 2100 degrees C * day
#  - seneRoot: 2200 degrees C * day
#  - seneRhizome: 2300 degrees C * day
senescence_simulation <- partial_run_biocro(
  miscanthus_x_giganteus$initial_values,
  miscanthus_x_giganteus$parameters,
  ex_weather,
  miscanthus_x_giganteus$direct_modules,
  miscanthus_x_giganteus$differential_modules,
  miscanthus_x_giganteus$ode_solver,
  c('seneLeaf', 'seneStem', 'seneRoot', 'seneRhizome')
)
senescence_result <- senescence_simulation(c(2000, 2100, 2200, 2300))

# Example 2: a crude method for simulating the effects of climate change; here
# we increase the atmospheric CO2 concentration to 500 ppm and the temperature
# by 2 degrees C relative to 2005 temperatures. The commands below that call
# `temperature_simulation` all produce the same result.
temperature_simulation <- partial_run_biocro(
  miscanthus_x_giganteus$initial_values,
  miscanthus_x_giganteus$parameters,
  ex_weather,
  miscanthus_x_giganteus$direct_modules,
  miscanthus_x_giganteus$differential_modules,
  miscanthus_x_giganteus$ode_solver,
  c("Catm", "temp")
)
hot_result_1 <- temperature_simulation(c(500, ex_weather$temp + 2.0))
hot_result_2 <- temperature_simulation(list(Catm = 500, temp = ex_weather$temp + 2.0))
hot_result_3 <- temperature_simulation(list(temp = ex_weather$temp + 2.0, Catm = 500))

# Note that these commands will both produce errors:
# hot_result_4 <- temperature_simulation(c(Catm = 500, temp = ex_weather$temp + 2.0))
# hot_result_5 <- temperature_simulation(stats::setNames(
#   c(500, ex_weather$temp + 2.0),
#   c("Catm", rep("temp", length(ex_weather$temp)))
# ))

# Note that this command will produce a strange result where the first
# temperature value will be incorrectly interpreted as a `Catm` value, and the
# `Catm` value will be interpreted as the final temperature value.
# hot_result_6 <- temperature_simulation(c(ex_weather$temp + 2.0, 500))

# Example 3: varying the base and air temperature inputs to the
# 'thermal_time_linear' module from the 'BioCro' module library. The commands
# below that call `thermal_time_rate` all produce the same result.
thermal_time_rate <- partial_evaluate_module(
  'BioCro:thermal_time_linear',
  within(miscanthus_x_giganteus$parameters, {fractional_doy = 1}),
  c("temp", "tbase")
)
rate_result_1 <- thermal_time_rate(c(25, 10))
rate_result_2 <- thermal_time_rate(c(temp = 25, tbase = 10))
rate_result_3 <- thermal_time_rate(c(tbase = 10, temp = 25))
rate_result_4 <- thermal_time_rate(list(temp = 25, tbase = 10))
rate_result_5 <- thermal_time_rate(list(tbase = 10, temp = 25))
```
