# Run BioCro module test cases

Modules can be tested using test cases, which are sets of known outputs
that correspond to particular inputs. The functions here provide ways to
create test cases and test modules.

`test_module` runs one test case for a module, returning an error
message if its output does not match the expected value.

`case` helps define test cases for module testing by combining the
required elements into a list with the correct names as required by
`test_module`.

`cases_from_csv` helps define test cases for module testing by creating
a list of test cases from a `csv` file.

Note that *module tests* are distinct from the *model tests* described
in [`model_testing`](model_testing.md).

## Usage

``` r
test_module(module_name, case_to_test, verbose = TRUE)

  case(inputs, expected_outputs, description)

  cases_from_csv(module_name, directory)
```

## Arguments

- module_name:

  A string specifying one BioCro module, formatted like
  `library_name:local_module_name`, where `library_name` is the name of
  a library that contains a module with local name `local_module_name`;
  such fully-qualified module names can be formed manually or with
  [`module_paste`](module_paste.md).

- case_to_test:

  A list with three named elements that describe a module test case:

  - `inputs`: A list of module inputs, i.e., a list of named numeric
    elements corresponding to the module's input quantities.

  - `expected_outputs`: A list of expected module outputs, i.e., a list
    of named numeric elements corresponding to the expected values of
    the module's output quantities.

  - `description`: A string describing the test case, e.g.
    `"temp below tbase"`. The description should be succinct and not
    contain any newline characters.

- verbose:

  A logical value indicating whether to print information about failed
  tests to the R terminal. When `verbose` is `TRUE`, any module outputs
  that do not match the expected value will be printed.

- inputs:

  See the corresponding entry in `test_case` above.

- expected_outputs:

  See the corresponding entry in `test_case` above.

- description:

  See the corresponding entry in `test_case` above.

- directory:

  The directory where module test case files are stored, e.g.
  `file.path('tests', 'module_test_cases')`

## Details

The `test_module` function forms the basis for the BioCro module testing
system. (See [`module_testing`](module_testing.md) for more
information.) The functions `case` and `cases_from_csv` are
complementary to `test_module` because they help to pass
suitably-formatted test cases to `test_module`.

## Value

- test_module:

  If the test passes, an empty string; otherwise, an informative message
  about what went wrong.

- case:

  A list with three named elements (`inputs`, `expected_outputs`, and
  `description`) formed from the input arguments.

- cases_from_csv:

  A list of test cases created by the `case` function that are each
  suitable for passing to the `test_module` function.

## See also

- [`modules`](modules.md)

- [`module_case_files`](module_case_files.md)

- [`module_paste`](module_paste.md)

- [`module_testing`](module_testing.md)

- [`test_module_library`](test_module_library.md)

## Examples

``` r
# Example 1: Defining an individual test case for the 'BioCro' module library's
# 'thermal_time_linear' module and running the test. This test will pass, so the
# return value will be an empty string: `character(0)`
test_module(
  'BioCro:thermal_time_linear',
  case(
    list(fractional_doy = 101, sowing_fractional_doy = 100, tbase = 20, temp = 44),
    list(TTc = 1.0),
    'temp above tbase'
  )
)
#> character(0)

# Example 2: Defining an individual test case for the 'BioCro' module library's
# 'thermal_time_linear' module and running the test. This test will fail, so the
# return value will be a message indicating the failure. Because verbose is TRUE
# by default, information about the failure will also be printed.
test_module(
  'BioCro:thermal_time_linear',
  case(
    list(fractional_doy = 101, sowing_fractional_doy = 100, tbase = 20, temp = 44),
    list(TTc = 2.0),
    'temp above tbase'
  )
)
#> 
#> Differences found for Module `BioCro:thermal_time_linear` test case `temp above tbase`:
#> 
#>   Quantity Expected Actual
#> 1      TTc        2      1
#> 
#> [1] "Module `BioCro:thermal_time_linear` test case `temp above tbase`: calculated outputs do not match expected outputs"

# Example 3: Loading a set of test cases from a file and running one of them.
# Note: here we use the `initialize_csv` function first to ensure that there is
# a properly defined test file in a temporary directory.

td <- tempdir()

module_name <- 'BioCro:thermal_time_linear'
initialize_csv(module_name, td)
#> [1] "Did not initialize case file because `/tmp/RtmpjEx6OI/BioCro_thermal_time_linear.csv` already exists"
cases <- cases_from_csv(module_name, td)
test_module(module_name, cases[[1]])
#> character(0)
```
