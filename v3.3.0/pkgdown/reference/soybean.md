# Soybean-BioCro model definition

Initial values, parameters, direct modules, differential modules, and a
differential equation solver that can be used to run soybean growth
simulations in Champaign, Illinois and other locations. Along with the
soybean circadian clock specifications
([`soybean_clock`](soybean_clock.md)), these values define the soybean
growth model of Matthews *et al.* (2022)
\[[doi:10.1093/insilicoplants/diab032](https://doi.org/10.1093/insilicoplants/diab032)
\], which is commonly referred to as *Soybean-BioCro*.

To represent soybean growth in Champaign, IL, these values must be
paired with the Champaign weather data
([`cmi_soybean_weather_data`](cmi_soybean_weather_data.md)). This
weather data includes the output from the soybean circadian clock model
([`soybean_clock`](soybean_clock.md)), so the clock components do not
need to be included when running a soybean growth simulation using this
weather data. The parameters already include the `clay_loam` values from
the [`soil_parameters`](soil_parameters.md) dataset, which is the
appropriate soil type for Champaign.

Some specifications, such as the values of photosynthetic parameters,
would remain the same in any location; others, such as the latitude or
longitude, would need to change when simulating crop growth in different
locations. Care must be taken to understand each input quantity before
attempting to run simulations in other places or for other cultivars.

## Usage

``` r
soybean
```

## Format

A list of 5 named elements that are suitable for passing to
[`run_biocro`](run_biocro.md), as described in the help page for
[`crop_model_definitions`](crop_model_definitions.md).

## Details

As improvements are made to the BioCro modules, their behavior changes,
and the soybean model parameters must be updated. Following significant
module updates, reparameterization is performed using a script that is
included with the BioCro package; its location can be found by typing
`system.file('extdata', 'parameterize_soybean.R', package = 'BioCro')`
in an R session. The parameterization script generally uses the same
method and data as used in Matthews *et al.* (2022), with a few
differences, such as the separation of pod mass into separate seed and
shell components.

The following is a summary of reparameterizations that have occurred
since the original publication of the Soybean-BioCro model:

- *2023-06-18*: Several modules have been updated, and the value of the
  atmospheric transmittance has been changed from 0.85 to 0.6 based on
  Campbell and Norman, An Introduction to Environmental Biophysics, 2nd
  Edition, Pg 173. Due to these changes, reparameterization of the
  following was required: `alphaLeaf`, `alphaRoot`, `alphaStem`,
  `alphaShell`, `betaLeaf`, `betaRoot`, `betaStem`, `betaShell`,
  `rateSeneLeaf`, `rateSeneStem`, `alphaSeneLeaf`, `betaSeneLeaf`,
  `alphaSeneStem`, and `betaSeneStem`.

- *2023-03-15*: Several modules have been updated. The most significant
  changes are that (1) the
  `BioCro:no_leaf_resp_neg_assim_partitioning_growth_calculator` now
  reduces the leaf growth rate in response to water stress and (2) the
  partitioning modules now include a new tissue type (`shell`). The new
  component allows us to distinguish between components of the soybean
  pod, where `shell` represents the pericarp and `grain` represents the
  seed. This distinction has been found to be important for accurately
  predicting seed biomass, which is more important in agricultural
  settings than the entire pod mass, since the pericarp is not included
  in typical yield measurements. Due to these changes,
  reparameterization of the following was required: `alphaLeaf`,
  `alphaRoot`, `alphaStem`, `alphaShell`, `betaLeaf`, `betaRoot`,
  `betaStem`, `betaShell`, `rateSeneLeaf`, `rateSeneStem`,
  `alphaSeneLeaf`, `betaSeneLeaf`, `alphaSeneStem`, and `betaSeneStem`.
  It was also necessary to add a new direct module to the model
  definition: `BioCro:leaf_water_stress_exponential`. This module
  calculates the fractional reduction in leaf growth rate due to water
  stress.

- *2024-09-12*: Several changes have been made: (1) The `mrc1` and
  `mrc2` were renamed to `grc_stem` and `grc_root`, respectively. These
  two parameters are used to scale the assimilate rate, which is
  commonly called growth respiration coefficient (grc). (2) A new
  module, `BioCro:maintenance_respiration`, has been added to account
  for maintenance respiration during the biomass partitioning. This
  module removes a fraction from each organ by a constant parameter
  called `mrc_*` (e.g., `mrc_leaf`) and also by a temperature-dependent
  Q10 scaling factor. Among these `mrc_*` parameters, `mrc_leaf` and
  `mrc_stem` are set equal to represent maintenance respiration for the
  shoot, while `mrc_grain` is assigned a negligible value to prevent
  grain biomass reduction at the season end. No decreasing trends have
  been seen in the observed data. (3) Parameter optimizations against
  the 2002-2006 biomass datasets were performed to accommodate these
  changes.

- *2025-04-23*: Several changes have been made since the previous
  update. Regarding modules, several new modules have been added:
  `BioCro:format_time`, `BioCro:sla_linear`,
  `BioCro:carbon_assimilation_to_biomass`, and
  `BioCro:maintenance_respiration_calculator`. These modules do not
  change the model's overall behavior; rather, they make some new
  quantities available in the outputs, such as maintenance respiration
  rates. The `BioCro:format_time` module is related to a change in how
  BioCro defines time; previously it was defined in units of days, but
  now it is given in hours. The partitioning growth calculator module
  was also changed to `BioCro:partitioning_growth_calculator`, since the
  previous module was removed from the library; this module has
  identical behavior to the old one
  (`BioCro:no_leaf_resp_neg_assim_partitioning_growth_calculator`).
  Regarding parameters, several modules now require new input parameters
  whose values were previously hard-coded: `dry_biomass_per_carbon`,
  `grc_leaf`, `grc_rhizome`, `grc_grain`, `grc_shell`, `mrc_rhizome`,
  `mrc_shell`, `alphaRhizome`, `betaRhizome`, `kRhizome_emr_DVI`, and
  several parameters related to the temperature dependence of FvCB model
  parameters (e.g. `Gstar_c` and `Gstar_Ea`). A few parameters have also
  been renamed to better indicate their meaning, such as `Rd` becoming
  `RL_at_25`. Finally, due to changes in the calculation of growth
  respiration rates, the model needed to be re-parameterized against the
  2002-2006 biomass data sets. This resulted in changes to the values of
  `alphaLeaf`, `alphaStem`, `betaLeaf`, `betaStem`, `alphaShell`,
  `betaShell`, `grc_root`, `grc_stem`, `mrc_leaf`, `mrc_root`,
  `mrc_stem`, `rateSeneLeaf`, `rateSeneStem`, `alphaSeneLeaf`,
  `alphaSeneStem`, `betaSeneLeaf`, and `betaSeneStem`.

- *2025-08-25*: The leaf boundary layer conductance model has been
  changed to a model described in Campbell & Norman (1998), causing
  small differences in the simulated soybean biomass and requiring a
  reparameterization. This update alters the values of the same
  parameters that were changed in the previous update on 2025-04-23.

Whenever a reparameterization is made, this list should be updated, and
any vignettes using the soybean model should be checked to see if any
axis limits, etc., need to change.

## Source

This model is described in detail in Matthews *et al.* (2022)
\[[doi:10.1093/insilicoplants/diab032](https://doi.org/10.1093/insilicoplants/diab032)
\]. Here we make a few notes about some of its components.

**General Comments**

- For historical reasons, the seed tissue in this model is called
  `Grain`. The entire pod biomass can be calculated by adding the
  `Grain` and `Shell` biomass.

- For historical reasons, this model includes a `Rhizome` tissue.
  Soybean does not have a rhizome, so the rhizome-related values in the
  model are chosen such that the rhizome begins with zero mass and does
  not grow or senesce. These values include the initial `Rhizome` and
  `RhizomeLitter`, and the following parameters: `alphaRhizome`,
  `betaRhizome`, `kRhizome_emr`, `kRhizome_emr_DVI`, `grc_rhizome`,
  `mrc_rhizome`, `rateSeneRhizome`, `alphaSeneRhizome`,
  `betaSeneRhizome`, and `retrans_rhizome`.

- Some parameter values are specific to Champaign, Illinois (2002-2006)
  and may need to change for other years and locations: the soil type,
  `Catm`, `maturity_group`, `windspeed_height`, `lat`, and `longitude`.

**ode_solver**

- `type`: For this model, the ODE solver type should not be
  `boost_rosenbrock` or `auto` (which defaults to `boost_rosenbrock`
  when a fixed step size Euler ODE solver is not required, as in this
  case) since the integration will fail unless the tolerances are
  stringent (e.g., `output_step_size = 0.01`,
  `adaptive_rel_error_tol = 1e-9`, `adaptive_abs_error_tol = 1e-9`).

**initial_values**

- `Leaf`: The initial leaf biomass is set to 80% of the mass per land
  area of seeds sown at the start of the season. For the initial total
  seed mass per land area, we use the following equation:
  `Number of seeds per meter * weight per seed / row spacing`. The
  number of seeds per meter is 20 and the row spacing is 0.38 m, as
  reported in Morgan *et al.* (2004)
  \[[doi:10.1104/pp.104.043968](https://doi.org/10.1104/pp.104.043968)
  \]. The weight per seed is based on the average of .12 to .18 grams,
  as reported by [Feedipedia](https://www.feedipedia.org/node/42). Thus,
  we have an initial biomass of
  `(20 seeds / m) * (0.15 g / seed) / (0.38 m) = 7.89 g / m^2`,
  equivalent to `0.0789 Mg / ha` in the typical BioCro units.

- `Stem`: The initial stem biomass is set to 10% of the initial seed
  biomass; see `Leaf` for more info.

- `Root`: The initial root biomass is set to 10% of the initial seed
  biomass; see `Leaf` for more info.

- `Grain`: In principle, the initial seed mass should be zero, but here
  it is instead set to a very small value (1e-5) for compatibility with
  some models where key tissue masses cannot be zero.

- `Seed`: Initialized to a small value for the same reasons as the
  `Grain`.

**parameters**

- Optimized parameters: The following parameter values are determined
  using an optimization procedure: `alphaLeaf`, `alphaStem`, `betaLeaf`,
  `betaStem`, `alphaShell`, `betaShell`, `grc_root`, `grc_stem`,
  `mrc_leaf`, `mrc_root`, `mrc_stem`, `rateSeneLeaf`, `rateSeneStem`,
  `alphaSeneLeaf`, `alphaSeneStem`, `betaSeneLeaf`, and `betaSeneStem`.

- Basic soil info: Soil parameter values corresponding to `clay_loam`
  from the [`soil_parameters`](soil_parameters.md) dataset are used
  here, which is the appropriate soil type for Champaign, Illinois.
  These parameters are: `soil_air_entry`, `soil_b_coefficient`,
  `soil_bulk_density`, `soil_clay_content`, `soil_field_capacity`,
  `soil_sand_content`, `soil_saturated_conductivity`,
  `soil_saturation_capacity`, `soil_silt_content`, and
  `soil_wilting_point`.

- `iSp`: Specific leaf area; determined using 2002 values of
  `average_LAI / average_leaf_biomass` as reported in Dermody et al.
  2006
  \[[doi:10.1111/j.1469-8137.2005.01565.x](https://doi.org/10.1111/j.1469-8137.2005.01565.x)
  \] and Morgan et al. 2005
  \[[doi:10.1111/j.1365-2486.2005.001017.x](https://doi.org/10.1111/j.1365-2486.2005.001017.x)
  \].

- `Sp_thermal_time_decay`: Set to 0 to ensure specific leaf area is
  constant with time.

- `maturity_group`: Typical maturity group grown in Champaign, IL.

- Soybean development (R0 - R1): The "emr - V0" values from Table 2 of
  Setiyono et al., 2007
  \[[doi:10.1016/j.fcr.2006.07.011](https://doi.org/10.1016/j.fcr.2006.07.011)
  \] were used for `Tmin_R0R1`, `Topt_R0R1`, and `Tmax_R0R1`.

- Soybean development (emr - V0, R1 - R7): Values from Table 2 of
  Setiyono et al., 2007
  \[[doi:10.1016/j.fcr.2006.07.011](https://doi.org/10.1016/j.fcr.2006.07.011)
  \] were used for `Rmax_emrV0`, `Tmin_emrV0`, `Topt_emrV0`,
  `Tmax_emrV0`, `Tmin_R1R7`, `Topt_R1R7`, and `Tmax_R1R7`.

- `sowing_fractional_doy`: Set to 0 because Soybean-BioCro uses the
  weather data to set the sowing time. In other words, the weather data
  is truncated so it begins at the beginning of the simulation.

- `atmospheric_transmittance`: Campbell and Norman, An Introduction to
  Environmental Biophysics, 2nd Edition, Pg 173.

- `par_energy_content`: Set to 1 / 4.57, as in Plant Growth Chamber
  Handbook. CHAPTER 1 - RADIATION - John C. Sager and J. Craig
  McFarlane. Table 2, Pg 3 [online
  PDF](https://www.controlledenvironments.org/wp-content/uploads/sites/6/2017/06/Ch01.pdf)

- `heightf`: Estimated using a typical soybean LAI of 6 when canopy is 1
  m tall.

- `chil`: Campbell and Norman, An Introduction to Environmental
  Biophysics, 2nd Edition, Table 15.1, pg 253.

- `k_diffuse`: Estimated from Campbell and Norman, An Introduction to
  Environmental Biophysics, 2nd Edition, Figure 15.4, pg 254

- `lnfun`: Set to 0 to use the same value of `Vcmax_at_25` for all
  canopy layers.

- Leaf optical properties: Leaf reflectance and transmittance in the PAR
  band (`leaf_reflectance_par` and `leaf_transmittance_par`) are
  estimated from
  \[[doi:10.2134/agronmonogr31.c7](https://doi.org/10.2134/agronmonogr31.c7)
  \],
  \[[doi:10.2134/agronj1971.00021962006300010038x](https://doi.org/10.2134/agronj1971.00021962006300010038x)
  \], and
  \[[doi:10.2134/agronj1991.00021962008300030026x](https://doi.org/10.2134/agronj1991.00021962008300030026x)
  \]. Reflectance and transmittance in the NIR band
  (`leaf_reflectance_nir` and `leaf_transmittance_nir`) are from
  \[[doi:10.2134/agronmonogr31.c7](https://doi.org/10.2134/agronmonogr31.c7)
  \].

- FvCB model (temperature response of Rubisco kinetic properties): The
  following values are from Table 1 of Bernacchi et al. 2001
  \[[doi:10.1111/j.1365-3040.2001.00668.x](https://doi.org/10.1111/j.1365-3040.2001.00668.x)
  \]: `Gstar_c`, `Gstar_Ea`, `Kc_c`, `Kc_Ea`, `Ko_c`, `Ko_Ea`,
  `Vcmax_c`, `Vcmax_Ea`.

- FvCB model (temperature response of non-photorespiratory CO2 release):
  `RL_c` and `RL_Ea` are from Table 1 of Bernacchi et al. 2001
  \[[doi:10.1111/j.1365-3040.2001.00668.x](https://doi.org/10.1111/j.1365-3040.2001.00668.x)
  \].

- FvCB model (temperature response of electron transport): The following
  values are from Table 1 of Bernacchi et al. 2003
  \[[doi:10.1046/j.0016-8025.2003.01050.x](https://doi.org/10.1046/j.0016-8025.2003.01050.x)
  \]: `Jmax_c` and `Jmax_Ea`. The following are from Table 2:
  `phi_PSII_0`, `phi_PSII_1`, `phi_PSII_2`, `theta_0`, `theta_1`,
  `theta_2`.

- FvCB model (temperature response of triose phosphate utilization): The
  following values are from the caption of Figure 7 of Yang et al. 2016
  \[[doi:10.1007/s00425-015-2436-8](https://doi.org/10.1007/s00425-015-2436-8)
  \]: `Tp_Ha`, `Tp_Hd`, and `Tp_S`. The value of `Tp_c` was chosen to
  ensure that Tp_norm = 1 at 25 degrees C.

- FvCB model (light response): `beta_PSII`,
  `electrons_per_carboxylation`, and `electrons_per_oxygenation` are
  from Bernacchi et al. 2003
  \[[doi:10.1046/j.0016-8025.2003.01050.x](https://doi.org/10.1046/j.0016-8025.2003.01050.x)
  \].

- FvCB model (rates at 25 degrees C): `Vcmax_at_25` and `Jmax_at_25` are
  seasonal averages across 2002 as reported in Bernacchi et. al. 2005
  \[[doi:10.1007/s00425-004-1320-8](https://doi.org/10.1007/s00425-004-1320-8)
  \]. `RL_at_25` is from Davey et al. 2004
  \[[doi:10.1104/pp.103.030569](https://doi.org/10.1104/pp.103.030569)
  \] (Table 3, cv Pana, co2 368 ppm). `Tp_at_25` was estimated from
  unpublished data provided by A. Digrado (UIUC, August 2019).

- Ball-Berry model: `b0` and `b1` are from Leakey et al. 2006
  \[[doi:10.1111/j.1365-3040.2006.01556.x](https://doi.org/10.1111/j.1365-3040.2006.01556.x)
  \].

- `Catm`: Global average value for 2002.

- `leafwidth`: Estimate based on large mature leaflets.

- `rateSeneRoot`: Set to 0 to disable root senescence.

- `phi2`: from Sugarcane-BioCro, Jaiswal et al. 2017
  \[[doi:10.1038/nclimate3410](https://doi.org/10.1038/nclimate3410) \].

The following parameters are not used by Soybean-BioCro, but are defined
for compatability with current or historical modules:

- `alpha1`: Needed for the `BioCro:parameter_calculator` module, but not
  used for C3 plants.

- `alphab1`: Needed for the `BioCro:parameter_calculator` module, but
  not used for C3 plants.

- `LeafN`: Needed for the `BioCro:parameter_calculator` module, but not
  used for C3 plants.

- `LeafN_0`: Needed for the `BioCro:parameter_calculator` module, but
  not used for C3 plants.

- `kpLN`: Has no influence on Soybean-BioCro because `lnfun` is set to
  to 0.

- FvCB model (varying Jmax): For compatability with the
  `BioCro:varying_Jmax25` module, `Jmax_at_25_mature` is set to the same
  value as `Jmax_at_25`. `sf_jmax` is also needed for this module.

- `alphaSeneRoot`: Has no influence on Soybean-BioCro because
  `rateSeneRoot` is set to 0.

- `betaSeneRoot`: Has no influence on Soybean-BioCro because
  `rateSeneRoot` is set to 0.

- `km_leaf_litter`: Needed for compatability with the
  `BioCro:litter_cover` module. Estimated based on 2021-2022 Energy Farm
  measurements, where the final leaf litter mass was around 1.5 - 2.5 Mg
  / Ha and covered around half of the ground area near the plants.

## See also

- [`run_biocro`](run_biocro.md)

- [`modules`](modules.md)

- [`crop_model_definitions`](crop_model_definitions.md)

- [`soybean_clock`](soybean_clock.md)
