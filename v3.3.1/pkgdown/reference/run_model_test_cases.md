# Run BioCro model test cases

BioCro models can be tested using test cases, which are sets of known
outputs that correspond to particular inputs. The `run_model_test_cases`
function runs one or more of these tests.

Note that *model tests* are distinct from the *module tests* described
in [`module_testing`](module_testing.md).

## Usage

``` r
run_model_test_cases(model_test_cases)
```

## Arguments

- model_test_cases:

  A list of module test cases, each of which should be created using
  [`model_test_case`](model_test_case.md).

## Details

The `run_model_test_cases` function is a key part of the BioCro model
testing system. See [`model_testing`](model_testing.md) for more
information.

For each test case, the following checks will be performed:

- The model definition must be valid according to
  [`validate_dynamical_system_inputs`](dynamical_system.md).

- The model will be run, which should not cause any errors or warnings.

For each test case where `check_outputs` was set to `TRUE`, the
following additional checks comparing the new result to a saved result
will be performed:

- The new result should have the same number of rows as the old result.

- With the exception of any columns in `quantities_to_ignore`, all
  columns in the stored result should be included in the new result.

- With the exception of any columns in `quantities_to_ignore`, all
  columns in the stored result should have the same values in the new
  result (to within the specified tolerance). This check will be made
  using [`all.equal`](https://rdrr.io/r/base/all.equal.html) with
  `tolerance` set to `relative_tolerance`.

When comparing the values of each column, values will only be checked
for every Nth row of the new result, where N is the value of
`row_interval` specified when defining the test case.

For each test case where `check_outputs` is `TRUE`, the stored result
should be created using the
[`update_stored_model_results`](update_stored_model_results.md)
function.

If any of the above checks fail for any of the supplied test cases, an
error will be thrown with a descriptive message.

Besides the checks above, a warning message will also be sent to the
user if there are columns in the new result that are not included in the
saved result.

## Value

If no issues are found, the function will return `TRUE`.

## See also

- [`model_testing`](model_testing.md)

- [`model_test_case`](model_test_case.md)

- [`update_stored_model_results`](update_stored_model_results.md)

- [`compare_model_output`](compare_model_output.md)

## Examples

``` r
# Define and run a test case for the miscanthus model
miscanthus_test_case <- model_test_case(
    'miscanthus_x_giganteus',
    miscanthus_x_giganteus,
    get_growing_season_climate(weather$'2005'),
    FALSE
)

run_model_test_cases(
  list(
    miscanthus_test_case
  )
)
#> [1] TRUE
```
