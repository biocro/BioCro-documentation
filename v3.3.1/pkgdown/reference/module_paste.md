# Prepend library name to module names

Prepends a library name to a set of module names to create a
suitably-formatted set of fully-qualified module names that can be
passed to [`run_biocro`](run_biocro.md) or other BioCro functions.

## Usage

``` r
module_paste(lib_name, local_module_names)
```

## Arguments

- lib_name:

  A string specifying a module library name.

- local_module_names:

  A vector or list of module name strings.

## Details

`module_paste` is a convenience function for specifying multiple modules
from the same library; it prepends the library name to each module name,
preserving the `names` and `class` of `local_module_names`.

Note that a simple call to `paste0(lib_name, ':', local_module_names)`
will produce a similar output with two important differences: (1)
[`paste0`](https://rdrr.io/r/base/paste.html) will not preserve names if
`local_module_names` has any named elements and (2)
[`paste0`](https://rdrr.io/r/base/paste.html) will always return a
character vector, even if `local_module_names` is a list.

## Value

A vector or list of fully-qualified module name strings formatted like
`lib_name:local_module_name`.

## See also

- [`modules`](modules.md)

- [`run_biocro`](run_biocro.md)

## Examples

``` r
# Example: Specifying several modules from the `BioCro` module library.
modules <- module_paste(
  'BioCro',
  list('total_biomass', canopy_photosynthesis = 'c3_canopy')
)

# Compare to the output from `paste0`
modules2 <- paste0(
  'BioCro',
  ':',
  list('total_biomass', canopy_photosynthesis = 'c3_canopy')
)

str(modules)
#> List of 2
#>  $                      : chr "BioCro:total_biomass"
#>  $ canopy_photosynthesis: chr "BioCro:c3_canopy"
str(modules2)
#>  chr [1:2] "BioCro:total_biomass" "BioCro:c3_canopy"
```
