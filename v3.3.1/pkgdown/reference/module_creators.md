# Create instances of modules

Creates pointers to module wrapper objects

## Usage

``` r
module_creators(module_names)
```

## Arguments

- module_names:

  A vector of module names

## Details

This function is used internally by several other BioCro functions,
where its purpose is to create instances of module wrapper pointers
using BioCro's module library and return pointers to those wrappers. In
turn, module wrappers can be used to obtain information about a module's
inputs, outputs, and other properties, and can also be used to create a
module instance. The `See Also` section contains a list of functions
that directly rely on `module_creators`.

Although the description of `externalptr` objects is sparse, they are
briefly mentioned in the R documentation: `externalptr-class`.

This function should not be used directly, and each module library
package must have its own version. For these reasons, this function is
not exported to the package namespace and can only be accessed using the
package name via the [`:::`](https://rdrr.io/r/base/ns-dblcolon.html)
operator.

## Value

A vector of R `externalptr` objects that each point to a
`module_creator` C++ object

## See also

- [`run_biocro`](run_biocro.md)

- [`module_info`](modules.md)

- [`evaluate_module`](modules.md)
