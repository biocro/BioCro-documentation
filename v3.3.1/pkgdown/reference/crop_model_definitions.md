# Crop model definitions

In BioCro, a crop model is defined by sets of direct modules,
differential modules, initial values, and parameters, along with an
ordinary differential equation (ODE) solver. To run a model, these
values, along with a set of weather data, are passed to the
[`run_biocro`](run_biocro.md) function. For convenience, several crop
model definitions are included in the BioCro R package. A full list can
be obtained by typing `??crop_models` into the R terminal.

## Details

Each crop model definition is stored as a list with the following named
elements:

- `direct_modules`: A list of direct module names; can be passed to
  [`run_biocro`](run_biocro.md) as its `direct_module_names` argument.

- `differential_modules`: A list of differential module names; can be
  passed to [`run_biocro`](run_biocro.md) as its
  `differential_module_names` argument.

- `ode_solver`: A list specifying details of a numerical ODE solver; can
  be passed to [`run_biocro`](run_biocro.md) as its `ode_solver`
  argument.

- `initial_values`: A list of named quantity values; can be passed to
  [`run_biocro`](run_biocro.md) as its `initial_values` argument.

- `parameters`: A list of named quantity values; can be passed to
  [`run_biocro`](run_biocro.md) as its `parameters` argument, and also
  can be passed to [`evaluate_module`](modules.md) and
  [`module_response_curve`](modules.md) when investigating the behavior
  of one of the crop's modules.

These model definitions are not sufficient for running a simulation
because [`run_biocro`](run_biocro.md) also requires drivers; for these
crop growth models, the drivers should be sets of weather data. The
[`soybean`](soybean.md) model is intended to be used along with the
specialized soybean weather data (see
[`cmi_soybean_weather_data`](cmi_soybean_weather_data.md)). The other
crops should be used with the other weather data (see
[`cmi_weather_data`](cmi_weather_data.md)).

Some quantities in the crop model definitions, such as the values of
photosynthetic parameters, would remain the same in any location;
others, such as the latitude or longitude, would need to change when
simulating crop growth in different locations. Care must be taken to
understand each input quantity before attempting to run simulations in
other places or for other cultivars.

Typically, the modules in a crop model definition are defined as lists
with some named elements; the names facilitate on-the-fly module
swapping via the [`within`](https://rdrr.io/r/base/with.html) function.
For example, to change the soybean canopy photosynthesis module to the
`BioCro:ten_layer_rue_canopy` module, one could pass
`within(soybean$direct_modules, {canopy_photosynthesis = "BioCro:ten_layer_rue_canopy"})`
as the `direct_module_names` argument when calling
[`run_biocro`](run_biocro.md) instead of `soybean$direct_modules`.

Because each crop model definition is stored as a list with named
elements, it is possible to use the
[`with`](https://rdrr.io/r/base/with.html) function to save some typing
when calling [`run_biocro`](run_biocro.md) or related functions such as
[`partial_run_biocro`](partial_application.md) or
[`validate_dynamical_system_inputs`](dynamical_system.md). For an
example, compare `Example 1` and `Example 2` below. Besides shortening
the code, using `with` also makes it easy to modify a command to
simulate the growth of a different crop; if the two models can use the
same drivers, this switch can be accomplished with one small change
(`Example 3`).

## See also

- [`run_biocro`](run_biocro.md)

- [`modules`](modules.md)

## Examples

``` r
# Example 1: Simulating Miscanthus growth using its model definition list
result1 <- run_biocro(
  miscanthus_x_giganteus$initial_values,
  miscanthus_x_giganteus$parameters,
  get_growing_season_climate(weather$'2002'),
  miscanthus_x_giganteus$direct_modules,
  miscanthus_x_giganteus$differential_modules,
  miscanthus_x_giganteus$ode_solver
)

# Example 2: Performing the same simulation as in Example 1, but making use of
# the `with` command to reduce repeated references to the model definition list
result2 <- with(miscanthus_x_giganteus, {run_biocro(
  initial_values,
  parameters,
  get_growing_season_climate(weather$'2002'),
  direct_modules,
  differential_modules,
  ode_solver
)})

# Example 3: Simulating willow growth using the same weather data as Examples 1
# and 2, which just requires one change relative to Example 2
result3 <- with(willow, {run_biocro(
  initial_values,
  parameters,
  get_growing_season_climate(weather$'2002'),
  direct_modules,
  differential_modules,
  ode_solver
)})
```
