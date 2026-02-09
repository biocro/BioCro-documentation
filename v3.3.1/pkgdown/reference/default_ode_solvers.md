# Default ODE solver settings

A collection of reasonable settings to use with each ODE solver type.
Users may need or wish to modify them for particular applications.

## Usage

``` r
default_ode_solvers
```

## Format

A list of 6 named elements, where each name is one of the possible ODE
solver types. Each element is itself a list of 5 named elements that can
be passed to [`run_biocro`](run_biocro.md) as its `ode_solver` input
argument.

## Details

A full list of solver types can be obtained with the
[`get_all_ode_solvers`](get_all.md) function.
