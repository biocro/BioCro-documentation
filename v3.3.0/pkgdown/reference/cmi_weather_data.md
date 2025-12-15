# Champaign, IL weather data

Champaign, IL weather data specified at hourly intervals in the CST time
zone for the years 1995–2023. The data includes typical inputs required
for BioCro imulations. Note: some values are missing near the start of
1995 since those time points are not available from SURFRAD.

This weather data is included in the BioCro package so users can
reproduce the calculations of Lochocki *et al.* (2022)
\[[doi:10.1093/insilicoplants/diac003](https://doi.org/10.1093/insilicoplants/diac003)
\] and for exploratory purposes; it is likely that most BioCro studies
will require different data sets, and no attempt is made here to be
exhaustive.

## Usage

``` r
weather
```

## Format

A list of 29 named elements, where each element is a data frame
corresponding to one year of weather data and the name of each element
is a year, for example `'2004'`. Each data frame has 8760 or 8784
observations (representing hourly time points) of 9 variables:

- `year`: the year

- `doy`: the day of year

- `hour`: the hour

- `time_zone_offset`: the time zone offset relative to UTC (hr)

- `precip`: preciptation rate (mm / hr)

- `rh`: the ambient relative humidity (dimensionless)

- `solar`: the incoming photosynthetically active photon flux density
  (PPFD) measured on a ground area basis including direct and diffuse
  sunlight light just outside the crop canopy (micromol / m^2 / s)

- `temp`: the ambient air temperature (degrees Celsius)

- `windspeed`: the wind speed in the ambient air just outside the canopy
  (m / s)

## Source

Weather data were obtained from the public SURFRAD and WARM databases
and processed according to the method described in Lochocki *et al.*
(2022)
\[[doi:10.1093/insilicoplants/diac003](https://doi.org/10.1093/insilicoplants/diac003)
\]. See version 1.2.0 of the
`eloch216/oscillator-based-circadian-clock-analysis` [GitHub
repository](https://github.com/eloch216/oscillator-based-circadian-clock-analysis)
for a full description of the data processing.

In brief, the columns in the data frames were determined from SURFRAD
and WARM variables as follows:

- `precip`: from the `precip` variable in the WARM data set

- `rh`: from the `rh` variable in the SURFRAD data set

- `solar`: from the `par` variable in the SURFRAD data set; when these
  values are not available, the `direct_n`, `diffuse`, and `zen`
  variables are used to make an estimate

- `temp`: from the `temp` variable in the SURFRAD data set

- `windspeed`: from the `windspd` variable in the SURFRAD data set

The WARM data set includes daily values. Hourly values for precipitation
are derived from daily totals by assuming a constant rate of
precipitation throughout the day.

The SURFRAD data set includes values at 1 or 3 minute intervals. Hourly
values are determined by averaging over hourly intervals, where the
value at hour `h` is the average over the hour-long interval centered at
`h`. Some values are missing; any missing entries are filled via an
interpolation procedure based on the assumption that values at the same
hour of sequential days should be similar.

The `time_zone_offset` is set to a constant value of -6 since this data
is specified in the CST time zone (i.e., UTC-6). Since the value of this
quantity does not change, it could in principle be considered a
parameter rather than a driver; however, it is included with the weather
data for convenience.

To reduce size the in the BioCro repository, the raw data values are
rounded. This was done using the commands in a script that is included
with the BioCro package. This script can be located by typing
`system.file('extdata', 'rounding_weather_values.R', package = 'BioCro')`.
