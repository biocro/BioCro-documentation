# Add a time component to input

Ensure, if possible, that input data that varies over time has a "time"
component. See the documentation for [`time`](time_variable.md) for more
information about this quantity.

It is rare for users to call this function directly because it is called
internally by [`run_biocro`](run_biocro.md).

## Usage

``` r
add_time_to_weather_data(drivers)
```

## Arguments

- drivers:

  A list or dataframe representing known system parameters that vary
  over time, such as weather data.

## Value

If `drivers` has `doy` and `hour` columns, then it is assumed to
represent weather data, and will be modified as follows:

- A new `time` column will be computed from `doy` and `hour`.

- The original `doy` and `hour` columns will be removed.

In this case, it is expected that the `BioCro:format_time` direct module
will be used to re-compute `doy` and `hour` from `time`.

If `drivers` does not have `doy` and `hour` columns, then `drivers` will
be returned as-is.

## Note

**Preconditions:**

- If `drivers` is a list, the values should be vectors of equal length.

- If `drivers` already contains a `time` component, then it shouldn't
  contain either a `doy` or an `hour` component unless it contains both
  of them and the values are mutually consistent.

**Why is the 'BioCro:format_time' module necessary?**

If values of `doy` and `hour` are supplied to `run_biocro` in the
drivers, undesired results may happen during interpolation. For example,
if two sequential rows have `(time = 3599, doy = 150, hour = 23)` and
`(time = 3600, doy = 151, hour = 0)`, and the results are to be returned
at half-hour time intervals, then linear interpolation between these
rows would produce `(time = 3599.5, doy = 150.5, hour = 11.5)`.
Typically it is expected that `doy` takes only integer values, so this
may cause issues. Using the `BioCro:format_time` module to calculate
`doy` and `hour` from `time` will ensure that the result includes
`(time = 3599.5, doy = 150, hour = 23.5)` instead.

## Examples

``` r
  # Add a time column to the buit-in 2002 weather data
  new_weather <- add_time_to_weather_data(weather[['2002']])

  # Compare column names
  colnames(weather[['2002']])
#> [1] "year"             "doy"              "hour"             "time_zone_offset"
#> [5] "precip"           "rh"               "solar"            "temp"            
#> [9] "windspeed"       
  colnames(new_weather)
#> [1] "year"             "time_zone_offset" "precip"           "rh"              
#> [5] "solar"            "temp"             "windspeed"        "time"            
```
