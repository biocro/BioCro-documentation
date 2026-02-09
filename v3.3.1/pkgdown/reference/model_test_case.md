# Define BioCro model test cases

BioCro models can be tested using test cases, which are sets of known
outputs that correspond to particular inputs. The `model_test_case`
function defines such a test case.

Note that *model tests* are distinct from the *module tests* described
in [`module_testing`](module_testing.md).

## Usage

``` r
model_test_case(
    test_case_name,
    model_definition,
    drivers,
    check_outputs,
    directory = '.',
    quantities_to_ignore = character(),
    row_interval = 24,
    digits = 5,
    relative_tolerance = 1e-3
  )
```

## Arguments

- test_case_name:

  A string describing the test case.

- model_definition:

  A list defining a model, as described in the documentation for
  [`crop_model_definitions`](crop_model_definitions.md).

- drivers:

  A set of drivers to be passed to [`run_biocro`](run_biocro.md) along
  with the `model_definition`.

- check_outputs:

  A logical value indicating whether to compare the simulation output
  against a stored result.

- directory:

  A relative or absolute path to a directory containing a stored
  simulation result. Only used when `check_outputs` is `TRUE`.

- quantities_to_ignore:

  A character vector of any quantities that should not be compared
  against the stored results. Only used when `check_outputs` is `TRUE`.

- row_interval:

  Determines which rows are saved and compared when using
  [`update_stored_model_results`](update_stored_model_results.md),
  [`compare_model_output`](compare_model_output.md), or
  [`run_model_test_cases`](run_model_test_cases.md). Only used when
  `check_outputs` is `TRUE`.

- digits:

  Passed to [`signif`](https://rdrr.io/r/base/Round.html) to round
  values when storing saved results. Only used when `check_outputs` is
  `TRUE`.

- relative_tolerance:

  A relative tolerance to be used when comparing new values against
  stored ones. This value will be passed to
  [`all.equal`](https://rdrr.io/r/base/all.equal.html) as its
  `tolerance` input argument. Only used when `check_outputs` is `TRUE`.

## Details

The `model_test_case` function forms the basis for the BioCro model
testing system. See [`model_testing`](model_testing.md) for more
information.

With the default settings:

- Every 24 rows of the simulation output will be stored and compared.
  When using drivers with an hourly time step, this corresponds to one
  row for each day.

- Values in the stored simulation results will be rounded to five
  significant digits. This reduces the size of the stored result file.

- The value of the relative tolerance was chosen to be the smallest
  value that enabled the tests to pass on all operating systems.

These default settings have proven useful for the BioCro
[`miscanthus_x_giganteus`](miscanthus_x_giganteus.md),
[`willow`](willow.md), and [`soybean`](soybean.md) models.

## Value

A list that defines a model test case, which can be passed to
[`update_stored_model_results`](update_stored_model_results.md),
[`compare_model_output`](compare_model_output.md), or
[`run_model_test_cases`](run_model_test_cases.md).

## See also

- [`model_testing`](model_testing.md)

- [`crop_model_definitions`](crop_model_definitions.md)

- [`update_stored_model_results`](update_stored_model_results.md)

- [`compare_model_output`](compare_model_output.md)

- [`run_model_test_cases`](run_model_test_cases.md)

## Examples

``` r
# Define a test case for the miscanthus model
miscanthus_test_case <- model_test_case(
    'miscanthus_x_giganteus',
    miscanthus_x_giganteus,
    get_growing_season_climate(weather$'2005'),
    TRUE,
    tempdir(),
    'soil_evaporation_rate'
)

# The result is a specially formatted list
str(miscanthus_test_case)
#> List of 13
#>  $ test_case_name      : chr "miscanthus_x_giganteus"
#>  $ initial_values      :List of 18
#>   ..$ cws1                    : num 0.32
#>   ..$ cws2                    : num 0.32
#>   ..$ Grain                   : num 0
#>   ..$ Shell                   : num 0
#>   ..$ Leaf                    : num 7e-04
#>   ..$ LeafLitter              : num 0
#>   ..$ leaf_senescence_index   : num 0
#>   ..$ Rhizome                 : num 7
#>   ..$ RhizomeLitter           : num 0
#>   ..$ rhizome_senescence_index: num 0
#>   ..$ Root                    : num 0.007
#>   ..$ RootLitter              : num 0
#>   ..$ root_senescence_index   : num 0
#>   ..$ soil_water_content      : num 0.32
#>   ..$ Stem                    : num 0.007
#>   ..$ StemLitter              : num 0
#>   ..$ stem_senescence_index   : num 0
#>   ..$ TTc                     : num 0
#>  $ parameters          :List of 127
#>   ..$ alpha1                     : num 0.04
#>   ..$ alphab1                    : num 0
#>   ..$ atmospheric_pressure       : num 101325
#>   ..$ atmospheric_scattering     : num 0.3
#>   ..$ atmospheric_transmittance  : num 0.6
#>   ..$ b0                         : num 0.08
#>   ..$ b1                         : num 3
#>   ..$ beta                       : num 0.93
#>   ..$ Catm                       : num 400
#>   ..$ chil                       : num 1
#>   ..$ dry_biomass_per_carbon     : num 30
#>   ..$ emissivity_sky             : num 1
#>   ..$ grc_grain                  : num 0
#>   ..$ grc_leaf                   : num 0.02
#>   ..$ grc_rhizome                : num 0.03
#>   ..$ grc_root                   : num 0.03
#>   ..$ grc_shell                  : num 0
#>   ..$ grc_stem                   : num 0.02
#>   ..$ growth_respiration_fraction: num 0
#>   ..$ Gs_min                     : num 0.001
#>   ..$ heightf                    : num 1.33
#>   ..$ hydrDist                   : num 0
#>   ..$ iSp                        : num 1.7
#>   ..$ k_diffuse                  : num 0.1
#>   ..$ kGrain1                    : num 0
#>   ..$ kGrain2                    : num 0
#>   ..$ kGrain3                    : num 0
#>   ..$ kGrain4                    : num 0
#>   ..$ kGrain5                    : num 0
#>   ..$ kGrain6                    : num 0
#>   ..$ kLeaf1                     : num 0.33
#>   ..$ kLeaf2                     : num 0.14
#>   ..$ kLeaf3                     : num 0.01
#>   ..$ kLeaf4                     : num 0.01
#>   ..$ kLeaf5                     : num 0.01
#>   ..$ kLeaf6                     : num 0.01
#>   ..$ kparm                      : num 0.7
#>   ..$ kpLN                       : num 0.2
#>   ..$ kRhizome1                  : num -8e-04
#>   ..$ kRhizome2                  : num -5e-04
#>   ..$ kRhizome3                  : num 0.35
#>   ..$ kRhizome4                  : num 0.35
#>   ..$ kRhizome5                  : num 0.35
#>   ..$ kRhizome6                  : num 0.35
#>   ..$ kRoot1                     : num 0.3
#>   ..$ kRoot2                     : num 0.01
#>   ..$ kRoot3                     : num 0.01
#>   ..$ kRoot4                     : num 0.01
#>   ..$ kRoot5                     : num 0.01
#>   ..$ kRoot6                     : num 0.01
#>   ..$ kShell                     : num 0
#>   ..$ kStem1                     : num 0.37
#>   ..$ kStem2                     : num 0.85
#>   ..$ kStem3                     : num 0.63
#>   ..$ kStem4                     : num 0.63
#>   ..$ kStem5                     : num 0.63
#>   ..$ kStem6                     : num 0.63
#>   ..$ lat                        : num 40
#>   ..$ leaf_reflectance_nir       : num 0.38
#>   ..$ leaf_reflectance_par       : num 0.09
#>   ..$ leaf_transmittance_nir     : num 0.45
#>   ..$ leaf_transmittance_par     : num 0.04
#>   ..$ LeafN                      : num 2
#>   ..$ LeafN_0                    : num 2
#>   ..$ leafwidth                  : num 0.04
#>   ..$ lnfun                      : num 0
#>   ..$ longitude                  : num -88
#>   ..$ lowerT                     : num 3
#>   ..$ min_gbw_canopy             : num 0.005
#>   ..$ nalphab0                   : num 0.0237
#>   ..$ nalphab1                   : num 0.000488
#>   ..$ nileafn                    : num 85
#>   ..$ nkln                       : num 0.5
#>   ..$ nkpLN                      : num 0.17
#>   ..$ nlayers                    : num 10
#>   ..$ nlnb0                      : num -5
#>   ..$ nlnb1                      : num 18
#>   ..$ nRdb0                      : num -4.59
#>   ..$ nRdb1                      : num 0.125
#>   ..$ nvmaxb0                    : num -16.2
#>   ..$ nvmaxb1                    : num 0.694
#>   ..$ par_energy_content         : num 0.219
#>   ..$ par_energy_fraction        : num 0.5
#>   ..$ phi1                       : num 0.01
#>   ..$ phi2                       : num 10
#>   ..$ remobilization_fraction    : num 0.6
#>   ..$ retrans                    : num 0.9
#>   ..$ retrans_rhizome            : num 1
#>   ..$ rfl                        : num 0.2
#>   ..$ RL_at_25                   : num 0.8
#>   ..$ rsdf                       : num 0.44
#>   ..$ rsec                       : num 0.2
#>   ..$ seneLeaf                   : num 3000
#>   ..$ seneRhizome                : num 4000
#>   ..$ seneRoot                   : num 4000
#>   ..$ seneStem                   : num 3500
#>   ..$ soil_air_entry             : num -2.6
#>   ..$ soil_b_coefficient         : num 5.2
#>   ..$ soil_bulk_density          : num 1.35
#>   .. [list output truncated]
#>  $ drivers             :'data.frame':    4296 obs. of  9 variables:
#>   ..$ year            : num [1:4296] 2005 2005 2005 2005 2005 ...
#>   ..$ doy             : num [1:4296] 123 123 123 123 123 123 123 123 123 123 ...
#>   ..$ hour            : num [1:4296] 0 1 2 3 4 5 6 7 8 9 ...
#>   ..$ time_zone_offset: num [1:4296] -6 -6 -6 -6 -6 -6 -6 -6 -6 -6 ...
#>   ..$ precip          : num [1:4296] 0 0 0 0 0 0 0 0 0 0 ...
#>   ..$ rh              : num [1:4296] 0.68 0.7 0.71 0.72 0.75 0.79 0.78 0.63 0.49 0.47 ...
#>   ..$ solar           : num [1:4296] 0 0 0 0 0 36 277 693 1090 1440 ...
#>   ..$ temp            : num [1:4296] 2.12 1.72 1.61 1.13 0.47 -0.435 0.45 3.55 5.71 6.97 ...
#>   ..$ windspeed       : num [1:4296] 3.22 2.86 2.7 2.99 2.3 1.46 1.32 1.41 1.53 2.89 ...
#>  $ direct_modules      :List of 14
#>   ..$                               : chr "BioCro:format_time"
#>   ..$ stomata_water_stress          : chr "BioCro:stomata_water_stress_linear"
#>   ..$                               : chr "BioCro:leaf_water_stress_exponential"
#>   ..$ specific_leaf_area            : chr "BioCro:sla_linear"
#>   ..$                               : chr "BioCro:parameter_calculator"
#>   ..$                               : chr "BioCro:soil_evaporation"
#>   ..$ solar_coordinates             : chr "BioCro:solar_position_michalsky"
#>   ..$                               : chr "BioCro:height_from_lai"
#>   ..$                               : chr "BioCro:canopy_gbw_thornley"
#>   ..$                               : chr "BioCro:stefan_boltzmann_longwave"
#>   ..$ canopy_photosynthesis         : chr "BioCro:c4_canopy"
#>   ..$ partitioning_coefficients     : chr "BioCro:partitioning_coefficient_selector"
#>   ..$ partitioning_growth_calculator: chr "BioCro:partitioning_growth_calculator"
#>   ..$                               : chr "BioCro:carbon_assimilation_to_biomass"
#>  $ differential_modules:List of 4
#>   ..$ senescence  : chr "BioCro:thermal_time_senescence"
#>   ..$             : chr "BioCro:partitioning_growth"
#>   ..$ thermal_time: chr "BioCro:thermal_time_linear"
#>   ..$ soil_profile: chr "BioCro:two_layer_soil_profile"
#>  $ ode_solver          :List of 5
#>   ..$ type                  : chr "homemade_euler"
#>   ..$ output_step_size      : num 1
#>   ..$ adaptive_rel_error_tol: num 1e-05
#>   ..$ adaptive_abs_error_tol: num 1e-05
#>   ..$ adaptive_max_steps    : num 200
#>  $ check_outputs       : logi TRUE
#>  $ stored_result_file  : chr "/tmp/Rtmp6yrElc/miscanthus_x_giganteus_simulation.csv"
#>  $ quantities_to_ignore: chr "soil_evaporation_rate"
#>  $ row_interval        : num 24
#>  $ digits              : num 5
#>  $ relative_tolerance  : num 0.001
```
