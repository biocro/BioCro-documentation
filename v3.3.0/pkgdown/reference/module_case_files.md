# Define and modify BioCro module test case files

Test cases for testing modules can be stored in files. The functions
here provide ways to create and update those files.

`initialize_csv` helps define test cases for module testing by
initializing the `csv` file for one module based on either a set of
default input values or user-supplied ones.

`add_csv_row` helps define test cases for module testing by adding one
test case to a module's `csv` file based on the user-supplied inputs and
description.

`update_csv_cases` helps define cases for module testing by updating the
expected output values for each case stored in a module's csv file.

Note that *module tests* are distinct from the *model tests* described
in [`model_testing`](model_testing.md).

## Usage

``` r
initialize_csv(
    module_name,
    directory,
    nonstandard_inputs = list(),
    description = "automatically-generated test case",
    overwrite = FALSE
  )

  add_csv_row(module_name, directory, inputs, description)

  update_csv_cases(module_name, directory)
```

## Arguments

- module_name:

  A string specifying one BioCro module, formatted like
  `library_name:local_module_name`, where `library_name` is the name of
  a library that contains a module with local name `local_module_name`;
  such fully-qualified module names can be formed manually or with
  [`module_paste`](module_paste.md).

- directory:

  The directory where module test case files are stored, e.g.
  `file.path('tests', 'module_test_cases')`.

- inputs:

  A list of module inputs, i.e., a list of named numeric elements
  corresponding to the module's input quantities.

- description:

  A string describing the test case, e.g. `"temp above tbase"`. The
  description should be succinct and not contain any newline characters.

- nonstandard_inputs:

  An optional list of input quantities whose values will override the
  default value of 1.0; see the `inputs` entry above.

- overwrite:

  A logical value indicating whether an existing file should be
  overwritten.

## Details

Module test case files form a critical component of BioCro's regression
testing system. For more details, see the help page for
[`module_testing`](module_testing.md).

The `initialize_csv` function will evaluate the module for a set of
input quantities and store the results as a test case `csv` file.
Typically, both of its optional arguments can be omitted. However, some
modules produce errors when all inputs are set to 1.0. In this case, it
would be necessary to supply some nonstandard inputs and (possibly) an
alternate case description.

The `add_csv_row` function will evaluate the module for a set of input
quantities, define a test case from the resulting outputs and the
description, and add it to the module's corresponding `csv` file. If no
`csv` file exists, one will be initialized with the new case.

The `update_csv_cases` function will evaluate the module for all input
values specified in its `csv` case file and update the stored values of
the corresponding outputs. Any output columns not present in the file
will be added automatically and filled in with the correct values.
Although the output columns are optional, the description column must
exist in the `csv` file.

If a module test fails and `update_csv_cases` is used to update the
test, care should be taken to ensure that the new outputs are sensible.
This function should not be used to blindly ensure that tests pass,
since a test failure may indicate a real problem with a module.

Note that `update_csv_cases` can be used to batch-initialize test cases.
To do this, manually create a test case `csv` file with the proper name
that only includes columns for the inputs and the description; now,
calling `update_csv_cases` will automatically fill in the outputs for
each case. With this method, care must be taken when manually specifying
the values of the description column; the descriptions must be double
quoted, and if they contain internal double quotes, those quotes must be
doubled. Generally it is safest to simply avoid double quotes in the
descriptions. (See `qmethod` in the help file for
[`write.csv`](https://rdrr.io/r/utils/write.table.html) for more details
about quoting.)

## Value

A message indicating whether a file was created, overwritten, or not
written.

## See also

- [`modules`](modules.md)

- [`module_paste`](module_paste.md)

- [`module_testing`](module_testing.md)

- [`test_module_library`](test_module_library.md)

- [`test_module`](test_module.md)

## Examples

``` r
# First, we will initialize a test case file for the 'BioCro' library's
# 'thermal_time_linear' module, which will be saved in a temporary directory as
# 'BioCro_thermal_time_linear.csv'. Then, we will add a new case to the file.
# Finally, we will update the file. Note that the call to `update_csv_cases`
# will not actually modify the file unless it is manually edited beforehand to
# change an input or output value.

td <- tempdir()

initialize_csv(
  'BioCro:thermal_time_linear',
  td,
  nonstandard_inputs = list(temp = -1),
  overwrite = TRUE
)
#> [1] "Case file `/tmp/RtmpjEx6OI/BioCro_thermal_time_linear.csv` was initialized; any pre-existing file was overwritten"

writeLines(readLines(file.path(td, 'BioCro_thermal_time_linear.csv')))
#> input,input,input,input,output,"description"
#> fractional_doy,sowing_fractional_doy,tbase,temp,TTc,NA
#> 1,1,1,-1,0,"automatically-generated test case"

add_csv_row(
  'BioCro:thermal_time_linear',
  td,
  list(fractional_doy = 101, sowing_fractional_doy = 100, tbase = 20, temp = 44),
  'temp above tbase'
)
#> [1] "Added new case to file `/tmp/RtmpjEx6OI/BioCro_thermal_time_linear.csv`"

writeLines(readLines(file.path(td, 'BioCro_thermal_time_linear.csv')))
#> input,input,input,input,output,"description"
#> fractional_doy,sowing_fractional_doy,tbase,temp,TTc,NA
#> 1,1,1,-1,0,"automatically-generated test case"
#> 101,100,20,44,1,"temp above tbase"

update_csv_cases('BioCro:thermal_time_linear', td)
#> [1] "Updated case file `/tmp/RtmpjEx6OI/BioCro_thermal_time_linear.csv`"
```
