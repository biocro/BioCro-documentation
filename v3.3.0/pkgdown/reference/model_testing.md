# The BioCro model testing system

BioCro provides several functions for defining, modifying, and running
model test cases. These functions together allow model developers to
easily create regression tests that ensure the models continue to
function correctly.

Note that *model tests* are distinct from the *module tests* described
in [`module_testing`](module_testing.md).

## Details

Together, [`model_test_case`](model_test_case.md),
[`run_model_test_cases`](run_model_test_cases.md),
[`update_stored_model_results`](update_stored_model_results.md), and
[`compare_model_output`](compare_model_output.md) form a simple and
convenient system for defining and running model test cases. Such tests
form a critical component of BioCro's regression testing system, and
test cases should be defined for all BioCro models in all BioCro-related
repositories. These functions are not required in order to use the
BioCro package, but they are critical to understand when creating or
modifying models, or the modules they use.

A model test case consists of a model definition, a set of drivers, a
short name, and a few additional settings that specify some of the
testing behavior. To run a test, the model definition and drivers are
passed to [`run_biocro`](run_biocro.md) to ensure the model is
well-defined, and then the results are (optionally) compared against
saved results to ensure the model behavior has not changed. Multiple
test cases can be defined in a single list and passed to
[`run_model_test_cases`](run_model_test_cases.md), which will run all of
them.

In this system, stored data for a test case with name `'test_name'` must
be stored in a CSV file called `'test_name_simulation.csv'`. The
[`update_stored_model_results`](update_stored_model_results.md) function
can be used to generate a suitable file.

Typically, a BioCro-related repository will include a model testing file
that defines test cases and runs them to check for issues. An example
can be found in the `tests/testthat/test.CropModels.R` file. The
associated stored test results can be found in the
`tests/testthat/test_data` directory.

If any of the initial values, parameters, modules or weather data
change, or if the behavior of any of these modules changes, the stored
data for one or more model test cases will likely need to be updated.
This can be done using the
[`update_stored_model_results`](update_stored_model_results.md)
function.

Sometimes these changes are not expected to alter key outputs like the
biomass values. In this case, it is helpful to visually compare the new
and old biomass values. This can be done using the
[`compare_model_output`](compare_model_output.md) function before
updating the results.

## See also

- [`crop_model_definitions`](crop_model_definitions.md)

- [`model_test_case`](model_test_case.md)

- [`update_stored_model_results`](update_stored_model_results.md)

- [`compare_model_output`](compare_model_output.md)

- [`run_model_test_cases`](run_model_test_cases.md)
