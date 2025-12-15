# Run module test cases for an entire BioCro module library

Modules can be tested using test cases, which are sets of known outputs
that correspond to particular inputs. The `test_module_library` function
provides a way to run all test cases for all modules in a BioCro module
library.

Note that *module tests* are distinct from the *model tests* described
in [`model_testing`](model_testing.md).

## Usage

``` r
test_module_library(
    library_name,
    directory,
    modules_to_skip = c(),
    verbose = TRUE
  )
```

## Arguments

- library_name:

  The name of a BioCro module library.

- directory:

  The directory where module test case files are stored, e.g.
  `file.path('tests', 'module_test_cases')`

- modules_to_skip:

  A vector of local module name strings indicating any modules from the
  library that should not be tested. This feature should be used
  sparingly, since there are very few legitimate reasons to skip a
  module test.

- verbose:

  To be passed to [`test_module`](test_module.md).

## Details

For each CSV file in the specified directory, `test_module_library`
determines the corresponding module name and checks to make sure it is
part of the specified library. If there are test cases for modules not
in the library, `test_module_library` throws an error with a message
containing the "unexpected" module test cases.

For each module in the specified library, `test_module_library` loads
stored test cases from the specified directory and runs each test case,
storing information about any test failures or other issues that may
occur. If any problems are detected, `test_module_library` throws an
error with a message describing the issues.

For an example of how this function can be used along with the
[`testthat`](https://testthat.r-lib.org/reference/testthat-package.html)
package, see `tests/testthat/test.Modules.R`.

## Value

None

## See also

- [`modules`](modules.md)

- [`module_case_files`](module_case_files.md)

- [`module_testing`](module_testing.md)

- [`test_module`](test_module.md)

## Examples

``` r
# Here we will initialize a module test case file in a temporary directory, and
# then use `test_module_library` to test it. We will need to skip most of the
# modules in the library, since we only have a test case for one of them.

td <- file.path(tempdir(), 'module_test_cases')
dir.create(td, showWarnings = FALSE)

initialize_csv(
  'BioCro:thermal_time_linear',
  td,
  nonstandard_inputs = list(temp = -1),
  overwrite = TRUE
)
#> [1] "Case file `/tmp/RtmpjEx6OI/module_test_cases/BioCro_thermal_time_linear.csv` was initialized; any pre-existing file was overwritten"

# Get a list of local module names, excluding the module that has a test case
all_modules <- get_all_modules('BioCro')
skip <- all_modules[all_modules != 'BioCro:thermal_time_linear']
skip <- gsub('BioCro:', '', skip)

test_module_library('BioCro', td, skip)

# If we attempt to test the entire library, we will get errors since only one
# module actually has an associated case file
tryCatch(
  {
    test_module_library('BioCro', td)
  },
  error = function(e) {print(e)}
)
#> <simpleError in test_module_library("BioCro", td): Problems occurred while testing modules from the `BioCro` library:
#>   Module `BioCro:aba_decay`: could not load test cases: Error in cases_from_csv(module, directory): Module test case file `/tmp/RtmpjEx6OI/module_test_cases/BioCro_aba_decay.csv` does not exist
#>   Module `BioCro:ball_berry`: could not load test cases: Error in cases_from_csv(module, directory): Module test case file `/tmp/RtmpjEx6OI/module_test_cases/BioCro_ball_berry.csv` does not exist
#>   Module `BioCro:biomass_leaf_n_limitation`: could not load test cases: Error in cases_from_csv(module, directory): Module test case file `/tmp/RtmpjEx6OI/module_test_cases/BioCro_biomass_leaf_n_limitation.csv` does not exist
#>   Module `BioCro:broyden_test`: could not load test cases: Error in cases_from_csv(module, directory): Module test case file `/tmp/RtmpjEx6OI/module_test_cases/BioCro_broyden_test.csv` does not exist
#>   Module `BioCro:buck_swvp`: could not load test cases: Error in cases_from_csv(module, directory): Module test case file `/tmp/RtmpjEx6OI/module_test_cases/BioCro_buck_swvp.csv` does not exist
#>   Module `BioCro:bucket_soil_drainage`: could not load test cases: Error in cases_from_csv(module, directory): Module test case file `/tmp/RtmpjEx6OI/module_test_cases/BioCro_bucket_soil_drainage.csv` does not exist
#>   Module `BioCro:c3_assimilation`: could not load test cases: Error in cases_from_csv(module, directory): Module test case file `/tmp/RtmpjEx6OI/module_test_cases/BioCro_c3_assimilation.csv` does not exist
#>   Module `BioCro:c3_canopy`: could not load test cases: Error in cases_from_csv(module, directory): Module test case file `/tmp/RtmpjEx6OI/module_test_cases/BioCro_c3_canopy.csv` does not exist
#>   Module `BioCro:c3_leaf_photosynthesis`: could not load test cases: Error in cases_from_csv(module, directory): Module test case file `/tmp/RtmpjEx6OI/module_test_cases/BioCro_c3_leaf_photosynthesis.csv` does not exist
#>   Module `BioCro:c3_parameters`: could not load test cases: Error in cases_from_csv(module, directory): Module test case file `/tmp/RtmpjEx6OI/module_test_cases/BioCro_c3_parameters.csv` does not exist
#>   Module `BioCro:c4_assimilation`: could not load test cases: Error in cases_from_csv(module, directory): Module test case file `/tmp/RtmpjEx6OI/module_test_cases/BioCro_c4_assimilation.csv` does not exist
#>   Module `BioCro:c4_canopy`: could not load test cases: Error in cases_from_csv(module, directory): Module test case file `/tmp/RtmpjEx6OI/module_test_cases/BioCro_c4_canopy.csv` does not exist
#>   Module `BioCro:c4_leaf_photosynthesis`: could not load test cases: Error in cases_from_csv(module, directory): Module test case file `/tmp/RtmpjEx6OI/module_test_cases/BioCro_c4_leaf_photosynthesis.csv` does not exist
#>   Module `BioCro:canopy_gbw_thornley`: could not load test cases: Error in cases_from_csv(module, directory): Module test case file `/tmp/RtmpjEx6OI/module_test_cases/BioCro_canopy_gbw_thornley.csv` does not exist
#>   Module `BioCro:carbon_assimilation_to_biomass`: could not load test cases: Error in cases_from_csv(module, directory): Module test case file `/tmp/RtmpjEx6OI/module_test_cases/BioCro_carbon_assimilation_to_biomass.csv` does not exist
#>   Module `BioCro:cumulative_carbon_dynamics`: could not load test cases: Error in cases_from_csv(module, directory): Module test case file `/tmp/RtmpjEx6OI/module_test_cases/BioCro_cumulative_carbon_dynamics.csv` does not exist
#>   Module `BioCro:cumulative_water_dynamics`: could not load test cases: Error in cases_from_csv(module, directory): Module test case file `/tmp/RtmpjEx6OI/module_test_cases/BioCro_cumulative_water_dynamics.csv` does not exist
#>   Module `BioCro:daylength_calculator`: could not load test cases: Error in cases_from_csv(module, directory): Module test case file `/tmp/RtmpjEx6OI/module_test_cases/BioCro_daylength_calculator.csv` does not exist
#>   Module `BioCro:development_index`: could not load test cases: Error in cases_from_csv(module, directory): Module test case file `/tmp/RtmpjEx6OI/module_test_cases/BioCro_development_index.csv` does not exist
#>   Module `BioCro:development_index_from_thermal_time`: could not load test cases: Error in cases_from_csv(module, directory): Module test case file `/tmp/RtmpjEx6OI/module_test_cases/BioCro_development_index_from_thermal_time.csv` does not exist
#>   Module `BioCro:example_model_mass_gain`: could not load test cases: Error in cases_from_csv(module, directory): Module test case file `/tmp/RtmpjEx6OI/module_test_cases/BioCro_example_model_mass_gain.csv` does not exist
#>   Module `BioCro:example_model_partitioning`: could not load test cases: Error in cases_from_csv(module, directory): Module test case file `/tmp/RtmpjEx6OI/module_test_cases/BioCro_example_model_partitioning.csv` does not exist
#>   Module `BioCro:format_time`: could not load test cases: Error in cases_from_csv(module, directory): Module test case file `/tmp/RtmpjEx6OI/module_test_cases/BioCro_format_time.csv` does not exist
#>   Module `BioCro:FvCB`: could not load test cases: Error in cases_from_csv(module, directory): Module test case file `/tmp/RtmpjEx6OI/module_test_cases/BioCro_FvCB.csv` does not exist
#>   Module `BioCro:golden_ratio_hyperbola`: could not load test cases: Error in cases_from_csv(module, directory): Module test case file `/tmp/RtmpjEx6OI/module_test_cases/BioCro_golden_ratio_hyperbola.csv` does not exist
#>   Module `BioCro:grimm_soybean_flowering`: could not load test cases: Error in cases_from_csv(module, directory): Module test case file `/tmp/RtmpjEx6OI/module_test_cases/BioCro_grimm_soybean_flowering.csv` does not exist
#>   Module `BioCro:grimm_soybean_flowering_calculator`: could not load test cases: Error in cases_from_csv(module, directory): Module test case file `/tmp/RtmpjEx6OI/module_test_cases/BioCro_grimm_soybean_flowering_calculator.csv` does not exist
#>   Module `BioCro:harmonic_energy`: could not load test cases: Error in cases_from_csv(module, directory): Module test case file `/tmp/RtmpjEx6OI/module_test_cases/BioCro_harmonic_energy.csv` does not exist
#>   Module `BioCro:harmonic_oscillator`: could not load test cases: Error in cases_from_csv(module, directory): Module test case file `/tmp/RtmpjEx6OI/module_test_cases/BioCro_harmonic_oscillator.csv` does not exist
#>   Module `BioCro:height_from_lai`: could not load test cases: Error in cases_from_csv(module, directory): Module test case file `/tmp/RtmpjEx6OI/module_test_cases/BioCro_height_from_lai.csv` does not exist
#>   Module `BioCro:hyperbola_2d`: could not load test cases: Error in cases_from_csv(module, directory): Module test case file `/tmp/RtmpjEx6OI/module_test_cases/BioCro_hyperbola_2d.csv` does not exist
#>   Module `BioCro:incident_shortwave_from_ground_par`: could not load test cases: Error in cases_from_csv(module, directory): Module test case file `/tmp/RtmpjEx6OI/module_test_cases/BioCro_incident_shortwave_from_ground_par.csv` does not exist
#>   Module `BioCro:leaf_evapotranspiration`: could not load test cases: Error in cases_from_csv(module, directory): Module test case file `/tmp/RtmpjEx6OI/module_test_cases/BioCro_leaf_evapotranspiration.csv` does not exist
#>   Module `BioCro:leaf_evapotranspiration_check`: could not load test cases: Error in cases_from_csv(module, directory): Module test case file `/tmp/RtmpjEx6OI/module_test_cases/BioCro_leaf_evapotranspiration_check.csv` does not exist
#>   Module `BioCro:leaf_gbw_campbell`: could not load test cases: Error in cases_from_csv(module, directory): Module test case file `/tmp/RtmpjEx6OI/module_test_cases/BioCro_leaf_gbw_campbell.csv` does not exist
#>   Module `BioCro:leaf_gbw_nikolov`: could not load test cases: Error in cases_from_csv(module, directory): Module test case file `/tmp/RtmpjEx6OI/module_test_cases/BioCro_leaf_gbw_nikolov.csv` does not exist
#>   Module `BioCro:leaf_shape_factor`: could not load test cases: Error in cases_from_csv(module, directory): Module test case file `/tmp/RtmpjEx6OI/module_test_cases/BioCro_leaf_shape_factor.csv` does not exist
#>   Module `BioCro:leaf_water_stress_exponential`: could not load test cases: Error in cases_from_csv(module, directory): Module test case file `/tmp/RtmpjEx6OI/module_test_cases/BioCro_leaf_water_stress_expon>
```
