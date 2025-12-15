# The BioCro module testing system

BioCro provides several functions for defining, modifying, and running
module test cases. These functions together allow module developers to
easily create regression tests that ensure the modules continue to
function correctly.

Note that *module tests* are distinct from the *model tests* described
in [`model_testing`](model_testing.md).

## Details

Together, [`test_module_library`](test_module_library.md),
[`test_module`](test_module.md), [`case`](test_module.md),
[`cases_from_csv`](test_module.md),
[`initialize_csv`](module_case_files.md),
[`add_csv_row`](module_case_files.md), and
[`update_csv_cases`](module_case_files.md) form a simple and convenient
system for defining and running module test cases. Such tests form a
critical component of BioCro's regression testing system, and test cases
should be defined for all BioCro modules in all BioCro module libraries.
These functions are not required in order to use the BioCro package, but
they are critical to understand when creating or modifying modules.

A module test case consists of a set of module inputs, a set of module
outputs, and a short description of the case. To run the test, the
inputs are passed to the module, and then the calculated outputs are
compared to the expected ones. If the outputs match, the test is passed;
otherwise, it fails. This operation is handled by the
[`test_module`](test_module.md) function.

For simple on-the-fly testing, it is possible to define a test case
using the [`case`](test_module.md) function and run it using
[`test_module`](test_module.md). However, a more robust method is
available to facilitate regression testing, where module test cases are
stored in suitably-formatted `csv` files, allowing multiple test cases
to be defined for each module and easily checked afterwards. If test
case files for each module in a module library are stored in a single
directory, all the test cases can be checked with one call to
[`test_module_library`](test_module_library.md).

In this system, test cases for a module with fully-qualified name
`module_name` must be stored in `module_name.csv`, where the colon in
the module name has been replaced by an underscore; for example, the
module named `BioCro:total_biomass` would be associated with
`BioCro_total_biomass.csv`. The first row of a test case file must be
the quantity types (`input` or `output`), the second row must be the
quantity names, and the remaining rows must each specify input quantity
values along with the expected output values they should produce. There
must also be a `description` column (with `description` in the first
row) containing short descriptions of the test cases. These formatting
requirements will automatically be satisfied for any test case file
produced by [`initialize_csv`](module_case_files.md) or modified by
[`add_csv_row`](module_case_files.md) or
[`update_csv_cases`](module_case_files.md). Such files can be read from
R using [`cases_from_csv`](test_module.md), and the resulting case
objects can be passed to [`test_module`](test_module.md).

Although it is possible, directly editing the case files is not
recommended since [`initialize_csv`](module_case_files.md),
[`add_csv_row`](module_case_files.md), and
[`update_csv_cases`](module_case_files.md) are easier to use. There are
several exceptions to this suggestion: (1) when a case must be deleted,
(2) when a module input must be added or removed, and (3) during the
initialization of a test file, where a user may wish to batch-initialize
using [`update_csv_cases`](module_case_files.md) (see its documentation
for an explanation of batch-initialization).

Case files can easily be viewed using Excel or other spreadsheet
viewers, and are also nicely formatted when viewed on the GitHub website
for the repository.

Examples of module test case files can be found in the
`tests/module_test_cases` directory, while code that uses the
[`testthat`](https://testthat.r-lib.org/reference/testthat-package.html)
package to automatically run all the defined test cases for the standard
BioCro module library via
[`test_module_library`](test_module_library.md) can be found in the
`tests/testthat/test.Modules.R` file.

## See also

- [`modules`](modules.md)

- [`module_case_files`](module_case_files.md)

- [`test_module_library`](test_module_library.md)

- [`test_module`](test_module.md)
