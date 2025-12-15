# Generate a BioCro module header file.

To facilitate the creation of new BioCro modules, `module_write`
generates a BioCro module header file. Given a set of input and output
variables, `module_write` will create a C++ header file ('.h' file) by
filling in a template with the input and output variables, ensuring the
correct C++ syntax for a BioCro module.

## Usage

``` r
module_write(
    module_name,
    module_library,
    module_type,
    inputs,
    outputs,
    output_equations = NULL,
    input_units = NULL,
    output_units = NULL
  )
```

## Arguments

- module_name:

  A string for the module's name.

- module_library:

  A string for the module's library namespace. E.g., `'biocro'`.

- module_type:

  A string setting the module type: `'direct'` or `'differential'`.

- inputs:

  A character vector of the module's input variables.

- outputs:

  A character vector of the module's output variables.

- output_equations:

  A character vector. The module's output variables will be updated with
  these variables. If `NULL`, a zero is inserted instead.

- input_units:

  A character vector of the inputs' units. If `NULL`, no units are
  embedded.

- output_units:

  A character vector of the outputs' units. If `NULL`, no units are
  embedded.

## Details

`type` should be either `'direct'` or `'differential'`; however,
`module_write` does not enforce this in case new module types are
created in the future.

## Value

A string containing a new BioCro module header file.

## Note

This function returns a string and has no file I/O. Use
[`writeLines`](https://rdrr.io/r/base/writeLines.html) to print the
output to console, or to save the output. See examples below. Note that
it is customary to name the header file with the same name as the
module.

`module_write` checks for duplicate input or output variables, and if
detected, it will raise an error. In theory, this check should ensure
that the generated module will compile correctly. However, it is still
possible to define an module that is circular and will not pass the
checks in [`validate_dynamical_system_inputs`](dynamical_system.md). See
`Example 4`.

## Examples

``` r
# Example 1
# Inputs as character vector
xs = c('x1','x2','x3')

# Units
xs_units <- c('Mg / ha', 'Mg / ha / hr', 'dimensionless')

# Outputs
ys = c('y1','y2')

out <- module_write('testmod', 'testlib', 'direct',
    inputs=xs, input_units= xs_units, outputs=ys)

# Use writeLines to print to console
writeLines(out)
#> #ifndef TESTLIB_TESTMOD_H
#> #define TESTLIB_TESTMOD_H
#> 
#> #include "../framework/module.h"
#> #include "../framework/state_map.h"
#> 
#> namespace testlib
#> {
#> /**
#>  * @class testmod
#>  *
#>  * @brief Put documentation here.
#>  *
#>  */
#> class testmod : public direct_module
#> {
#>    public:
#>     testmod(
#>         state_map const& input_quantities,
#>         state_map* output_quantities)
#>         : direct_module{},
#> 
#>           // Get references to input quantities
#>           x1{get_input(input_quantities, "x1")},
#>           x2{get_input(input_quantities, "x2")},
#>           x3{get_input(input_quantities, "x3")},
#> 
#>           // Get pointers to output quantities
#>           y1_op{get_op(output_quantities, "y1")},
#>           y2_op{get_op(output_quantities, "y2")}
#>     {
#>     }
#>     static string_vector get_inputs();
#>     static string_vector get_outputs();
#>     static std::string get_name() { return "testmod"; }
#> 
#>    private:
#>     // References to input quantities
#>     double const& x1;
#>     double const& x2;
#>     double const& x3;
#> 
#>     // Pointers to output quantities
#>     double* y1_op;
#>     double* y2_op;
#> 
#>     // Main operation
#>     void do_operation() const;
#> };
#> 
#> string_vector testmod::get_inputs()
#> {
#>     return {
#>         "x1",          // Mg / ha
#>         "x2",          // Mg / ha / hr
#>         "x3"           // dimensionless
#>     };
#> }
#> 
#> string_vector testmod::get_outputs()
#> {
#>     return {
#>         "y1",          // Put y1 units here
#>         "y2"           // Put y2 units here
#>     };
#> }
#> 
#> void testmod::do_operation() const
#> {
#>     // Make calculations here
#> 
#>     // Use `update` to set outputs
#>     update(y1_op, 0);
#>     update(y2_op, 0);
#> }
#> 
#> }  // namespace testlib
#> #endif
#> 

if (FALSE) { # \dontrun{
  # Use writeLines to save as a `.h` file
  writeLines(out, "./testmod.h")
} # }

# Example 2: A differential module
xs <- c('var_1','var_2')
out <- module_write('testmod', 'testlib', 'differential', xs, xs)
writeLines(out)
#> #ifndef TESTLIB_TESTMOD_H
#> #define TESTLIB_TESTMOD_H
#> 
#> #include "../framework/module.h"
#> #include "../framework/state_map.h"
#> 
#> namespace testlib
#> {
#> /**
#>  * @class testmod
#>  *
#>  * @brief Put documentation here.
#>  *
#>  */
#> class testmod : public differential_module
#> {
#>    public:
#>     testmod(
#>         state_map const& input_quantities,
#>         state_map* output_quantities)
#>         : differential_module{},
#> 
#>           // Get references to input quantities
#>           var_1{get_input(input_quantities, "var_1")},
#>           var_2{get_input(input_quantities, "var_2")},
#> 
#>           // Get pointers to output quantities
#>           var_1_op{get_op(output_quantities, "var_1")},
#>           var_2_op{get_op(output_quantities, "var_2")}
#>     {
#>     }
#>     static string_vector get_inputs();
#>     static string_vector get_outputs();
#>     static std::string get_name() { return "testmod"; }
#> 
#>    private:
#>     // References to input quantities
#>     double const& var_1;
#>     double const& var_2;
#> 
#>     // Pointers to output quantities
#>     double* var_1_op;
#>     double* var_2_op;
#> 
#>     // Main operation
#>     void do_operation() const;
#> };
#> 
#> string_vector testmod::get_inputs()
#> {
#>     return {
#>         "var_1",          // Put var_1 units here
#>         "var_2"           // Put var_2 units here
#>     };
#> }
#> 
#> string_vector testmod::get_outputs()
#> {
#>     return {
#>         "var_1",          // Put var_1 units here
#>         "var_2"           // Put var_2 units here
#>     };
#> }
#> 
#> void testmod::do_operation() const
#> {
#>     // Make calculations here
#> 
#>     // Use `update` to set outputs
#>     update(var_1_op, 0);
#>     update(var_2_op, 0);
#> }
#> 
#> }  // namespace testlib
#> #endif
#> 

# Example 3: A module with pairwise names
# Here we use an outer product to generate pairwise combinations of
# tissues and pool types
tissues <- c('leaf', 'stem', 'root')
pools <- c('carbon', 'nitrogen')
xs <- as.vector(outer(tissues, pools, paste, sep = '_'))
out <- module_write('testmod', 'testlib', 'differential', xs, xs)
writeLines(out)
#> #ifndef TESTLIB_TESTMOD_H
#> #define TESTLIB_TESTMOD_H
#> 
#> #include "../framework/module.h"
#> #include "../framework/state_map.h"
#> 
#> namespace testlib
#> {
#> /**
#>  * @class testmod
#>  *
#>  * @brief Put documentation here.
#>  *
#>  */
#> class testmod : public differential_module
#> {
#>    public:
#>     testmod(
#>         state_map const& input_quantities,
#>         state_map* output_quantities)
#>         : differential_module{},
#> 
#>           // Get references to input quantities
#>           leaf_carbon{get_input(input_quantities, "leaf_carbon")},
#>           stem_carbon{get_input(input_quantities, "stem_carbon")},
#>           root_carbon{get_input(input_quantities, "root_carbon")},
#>           leaf_nitrogen{get_input(input_quantities, "leaf_nitrogen")},
#>           stem_nitrogen{get_input(input_quantities, "stem_nitrogen")},
#>           root_nitrogen{get_input(input_quantities, "root_nitrogen")},
#> 
#>           // Get pointers to output quantities
#>           leaf_carbon_op{get_op(output_quantities, "leaf_carbon")},
#>           stem_carbon_op{get_op(output_quantities, "stem_carbon")},
#>           root_carbon_op{get_op(output_quantities, "root_carbon")},
#>           leaf_nitrogen_op{get_op(output_quantities, "leaf_nitrogen")},
#>           stem_nitrogen_op{get_op(output_quantities, "stem_nitrogen")},
#>           root_nitrogen_op{get_op(output_quantities, "root_nitrogen")}
#>     {
#>     }
#>     static string_vector get_inputs();
#>     static string_vector get_outputs();
#>     static std::string get_name() { return "testmod"; }
#> 
#>    private:
#>     // References to input quantities
#>     double const& leaf_carbon;
#>     double const& stem_carbon;
#>     double const& root_carbon;
#>     double const& leaf_nitrogen;
#>     double const& stem_nitrogen;
#>     double const& root_nitrogen;
#> 
#>     // Pointers to output quantities
#>     double* leaf_carbon_op;
#>     double* stem_carbon_op;
#>     double* root_carbon_op;
#>     double* leaf_nitrogen_op;
#>     double* stem_nitrogen_op;
#>     double* root_nitrogen_op;
#> 
#>     // Main operation
#>     void do_operation() const;
#> };
#> 
#> string_vector testmod::get_inputs()
#> {
#>     return {
#>         "leaf_carbon",          // Put leaf_carbon units here
#>         "stem_carbon",          // Put stem_carbon units here
#>         "root_carbon",          // Put root_carbon units here
#>         "leaf_nitrogen",          // Put leaf_nitrogen units here
#>         "stem_nitrogen",          // Put stem_nitrogen units here
#>         "root_nitrogen"           // Put root_nitrogen units here
#>     };
#> }
#> 
#> string_vector testmod::get_outputs()
#> {
#>     return {
#>         "leaf_carbon",          // Put leaf_carbon units here
#>         "stem_carbon",          // Put stem_carbon units here
#>         "root_carbon",          // Put root_carbon units here
#>         "leaf_nitrogen",          // Put leaf_nitrogen units here
#>         "stem_nitrogen",          // Put stem_nitrogen units here
#>         "root_nitrogen"           // Put root_nitrogen units here
#>     };
#> }
#> 
#> void testmod::do_operation() const
#> {
#>     // Make calculations here
#> 
#>     // Use `update` to set outputs
#>     update(leaf_carbon_op, 0);
#>     update(stem_carbon_op, 0);
#>     update(root_carbon_op, 0);
#>     update(leaf_nitrogen_op, 0);
#>     update(stem_nitrogen_op, 0);
#>     update(root_nitrogen_op, 0);
#> }
#> 
#> }  // namespace testlib
#> #endif
#> 

# Example 4: Circular modules

if (FALSE) { # \dontrun{
  out <- module_write(inputs = c('x' ,'x'))

  # Will compile, but will cause a "circular quantities" error if it is used
  # in a BioCro simulation:
  out <- module_write('inconsistent', 'examplelib', type='direct',
          inputs = 'x', outputs = 'x')
} # }
```
