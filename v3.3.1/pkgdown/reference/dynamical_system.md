# Validating dynamical system inputs

Utility function for checking inputs to `run_biocro` without running it

## Usage

``` r
validate_dynamical_system_inputs(
      initial_values = list(),
      parameters = list(),
      drivers,
      direct_module_names = list(),
      differential_module_names = list(),
      verbose = TRUE
  )
```

## Arguments

- initial_values:

  Identical to the corresponding argument from
  [`run_biocro`](run_biocro.md).

- parameters:

  Identical to the corresponding argument from
  [`run_biocro`](run_biocro.md).

- drivers:

  Identical to the corresponding argument from
  [`run_biocro`](run_biocro.md).

- direct_module_names:

  Identical to the corresponding argument from
  [`run_biocro`](run_biocro.md).

- differential_module_names:

  Identical to the corresponding argument from
  [`run_biocro`](run_biocro.md).

- verbose:

  Identical to the corresponding argument from
  [`run_biocro`](run_biocro.md).

## Details

`validate_dynamical_system_inputs` accepts the same input arguments as
[`run_biocro`](run_biocro.md) with the exception of `ode_solver` (which
is not required to check the validity of a dynamical system).

`validate_dynamical_system_inputs` checks a set of parameters, drivers,
modules, and initial values to see if they can properly define a
dynamical system and can therefore be used as inputs to
[`run_biocro`](run_biocro.md). Although the
[`run_biocro`](run_biocro.md) function performs the same validity
checks, the `validate_dynamical_system_inputs` includes additional
information, such as a list of parameters whose values are not used as
inputs by any modules, since in principle these parameters could be
removed for clarity.

When using one of the pre-defined crop growth models, it may be helpful
to use the `with` command to pass arguments to
`validate_dynamical_system_inputs`; see the documentation for
[`crop_model_definitions`](crop_model_definitions.md) for more
information.

## Value

A boolean indicating whether or not the inputs are valid.

## See also

[`run_biocro`](run_biocro.md)

## Examples

``` r
# Example 1: missing a parameter and an initial value
validate_dynamical_system_inputs(
  within(soybean$initial_values, rm(Leaf)),         # remove the initial `Leaf` value
  within(soybean$parameters, rm(leaf_reflectance)), # remove `leaf_reflectance`
  soybean_weather$'2002',
  soybean$direct_modules,
  soybean$differential_modules
)
#> Warning: object 'leaf_reflectance' not found
#> 
#> Checking the validity of the system inputs:
#> 
#> [pass] No quantities were defined multiple times in the inputs
#> 
#> [fail] The following module inputs were not defined:
#>  Leaf from the 'maintenance_respiration_calculator' module
#>  Leaf from the 'parameter_calculator' module
#>  Leaf from the 'senescence_logistic' module
#>  Leaf from the 'partitioning_growth' module
#> 
#> [fail] The following differential module outputs were not part of the initial values:
#>  Leaf from the 'senescence_logistic' module
#>  Leaf from the 'maintenance_respiration' module
#>  Leaf from the 'partitioning_growth' module
#> 
#> [pass] There are no cyclic dependencies among the direct modules.
#> 
#> System inputs are not valid
#> 
#> Printing additional information about the system inputs:
#> 
#> The direct modules are in a suitable order for evaluation.
#> 
#> The following quantities were each required by at least one module:
#>  Catm
#>  DVI
#>  Grain
#>  Grain_mr_rate
#>  Gs_min
#>  Gstar_Ea
#>  Gstar_c
#>  Jmax_Ea
#>  Jmax_at_25
#>  Jmax_c
#>  Kc_Ea
#>  Kc_c
#>  Ko_Ea
#>  Ko_c
#>  Leaf
#>  LeafN
#>  LeafN_0
#>  LeafWS
#>  Leaf_mr_rate
#>  O2
#>  RL_Ea
#>  RL_at_25
#>  RL_c
#>  Rhizome
#>  Rhizome_mr_rate
#>  Rmax_emrV0
#>  Root
#>  Root_mr_rate
#>  Shell
#>  Shell_mr_rate
#>  Sp
#>  Sp_thermal_time_decay
#>  Stem
#>  Stem_mr_rate
#>  StomataWS
#>  TTc
#>  TTemr_threshold
#>  Tbase_emr
#>  Tmax_R0R1
#>  Tmax_R1R7
#>  Tmax_emrV0
#>  Tmin_R0R1
#>  Tmin_R1R7
#>  Tmin_emrV0
#>  Topt_R0R1
#>  Topt_R1R7
#>  Topt_emrV0
#>  Tp_Ha
#>  Tp_Hd
#>  Tp_S
#>  Tp_at_25
#>  Tp_c
#>  Vcmax_Ea
#>  Vcmax_at_25
#>  Vcmax_c
#>  absorbed_longwave
#>  alpha1
#>  alphaLeaf
#>  alphaRhizome
#>  alphaRoot
#>  alphaSeneLeaf
#>  alphaSeneRhizome
#>  alphaSeneRoot
#>  alphaSeneStem
#>  alphaShell
#>  alphaStem
#>  alphab1
#>  atmospheric_pressure
#>  atmospheric_scattering
#>  atmospheric_transmittance
#>  b0
#>  b1
#>  betaLeaf
#>  betaRhizome
#>  betaRoot
#>  betaSeneLeaf
#>  betaSeneRhizome
#>  betaSeneRoot
#>  betaSeneStem
#>  betaShell
#>  betaStem
#>  beta_PSII
#>  canopy_assimilation_molar_flux
#>  canopy_assimilation_rate
#>  canopy_gross_assimilation_molar_flux
#>  canopy_height
#>  canopy_non_photorespiratory_CO2_release_molar_flux
#>  canopy_photorespiration_molar_flux
#>  canopy_transpiration_rate
#>  chil
#>  cosine_zenith_angle
#>  cws1
#>  cws2
#>  day_length
#>  development_rate_per_hour
#>  dry_biomass_per_carbon
#>  electrons_per_carboxylation
#>  electrons_per_oxygenation
#>  emissivity_sky
#>  fractional_doy
#>  gbw_canopy
#>  grc_grain
#>  grc_leaf
#>  grc_rhizome
#>  grc_root
#>  grc_shell
#>  grc_stem
#>  growth_respiration_fraction
#>  height_layer_0
#>  height_layer_1
#>  height_layer_2
#>  height_layer_3
#>  height_layer_4
#>  height_layer_5
#>  height_layer_6
#>  height_layer_7
#>  height_layer_8
#>  height_layer_9
#>  heightf
#>  hydrDist
#>  iSp
#>  irradiance_diffuse_fraction
#>  irradiance_direct_fraction
#>  kGrain
#>  kLeaf
#>  kRhizome
#>  kRhizome_emr
#>  kRhizome_emr_DVI
#>  kRoot
#>  kSeneLeaf
#>  kSeneRhizome
#>  kSeneRoot
#>  kSeneStem
#>  kShell
#>  kStem
#>  k_diffuse
#>  kpLN
#>  lai
#>  lat
#>  leaf_reflectance_nir
#>  leaf_reflectance_par
#>  leaf_transmittance_nir
#>  leaf_transmittance_par
#>  leafwidth
#>  lnfun
#>  longitude
#>  maturity_group
#>  min_gbw_canopy
#>  mrc_grain
#>  mrc_leaf
#>  mrc_rhizome
#>  mrc_root
#>  mrc_shell
#>  mrc_stem
#>  net_assimilation_rate_grain
#>  net_assimilation_rate_leaf
#>  net_assimilation_rate_rhizome
#>  net_assimilation_rate_root
#>  net_assimilation_rate_shell
#>  net_assimilation_rate_stem
#>  par_energy_content
#>  par_energy_fraction
#>  par_incident_diffuse
#>  par_incident_direct
#>  phi1
#>  phi2
#>  phi_PSII_0
#>  phi_PSII_1
#>  phi_PSII_2
#>  precip
#>  rateSeneLeaf
#>  rateSeneRhizome
#>  rateSeneRoot
#>  rateSeneStem
#>  remobilization_fraction
#>  retrans
#>  retrans_rhizome
#>  rfl
#>  rh
#>  rsdf
#>  rsec
#>  shaded_Assim_layer_0
#>  shaded_Assim_layer_1
#>  shaded_Assim_layer_2
#>  shaded_Assim_layer_3
#>  shaded_Assim_layer_4
#>  shaded_Assim_layer_5
#>  shaded_Assim_layer_6
#>  shaded_Assim_layer_7
#>  shaded_Assim_layer_8
#>  shaded_Assim_layer_9
#>  shaded_GrossAssim_layer_0
#>  shaded_GrossAssim_layer_1
#>  shaded_GrossAssim_layer_2
#>  shaded_GrossAssim_layer_3
#>  shaded_GrossAssim_layer_4
#>  shaded_GrossAssim_layer_5
#>  shaded_GrossAssim_layer_6
#>  shaded_GrossAssim_layer_7
#>  shaded_GrossAssim_layer_8
#>  shaded_GrossAssim_layer_9
#>  shaded_Gs_layer_0
#>  shaded_Gs_layer_1
#>  shaded_Gs_layer_2
#>  shaded_Gs_layer_3
#>  shaded_Gs_layer_4
#>  shaded_Gs_layer_5
#>  shaded_Gs_layer_6
#>  shaded_Gs_layer_7
#>  shaded_Gs_layer_8
#>  shaded_Gs_layer_9
#>  shaded_RL_layer_0
#>  shaded_RL_layer_1
#>  shaded_RL_layer_2
#>  shaded_RL_layer_3
#>  shaded_RL_layer_4
#>  shaded_RL_layer_5
#>  shaded_RL_layer_6
#>  shaded_RL_layer_7
#>  shaded_RL_layer_8
#>  shaded_RL_layer_9
#>  shaded_Rp_layer_0
#>  shaded_Rp_layer_1
#>  shaded_Rp_layer_2
#>  shaded_Rp_layer_3
#>  shaded_Rp_layer_4
#>  shaded_Rp_layer_5
#>  shaded_Rp_layer_6
#>  shaded_Rp_layer_7
#>  shaded_Rp_layer_8
#>  shaded_Rp_layer_9
#>  shaded_TransR_layer_0
#>  shaded_TransR_layer_1
#>  shaded_TransR_layer_2
#>  shaded_TransR_layer_3
#>  shaded_TransR_layer_4
#>  shaded_TransR_layer_5
#>  shaded_TransR_layer_6
#>  shaded_TransR_layer_7
#>  shaded_TransR_layer_8
#>  shaded_TransR_layer_9
#>  shaded_absorbed_ppfd_layer_0
#>  shaded_absorbed_ppfd_layer_1
#>  shaded_absorbed_ppfd_layer_2
#>  shaded_absorbed_ppfd_layer_3
#>  shaded_absorbed_ppfd_layer_4
#>  shaded_absorbed_ppfd_layer_5
#>  shaded_absorbed_ppfd_layer_6
#>  shaded_absorbed_ppfd_layer_7
#>  shaded_absorbed_ppfd_layer_8
#>  shaded_absorbed_ppfd_layer_9
#>  shaded_absorbed_shortwave_layer_0
#>  shaded_absorbed_shortwave_layer_1
#>  shaded_absorbed_shortwave_layer_2
#>  shaded_absorbed_shortwave_layer_3
#>  shaded_absorbed_shortwave_layer_4
#>  shaded_absorbed_shortwave_layer_5
#>  shaded_absorbed_shortwave_layer_6
#>  shaded_absorbed_shortwave_layer_7
#>  shaded_absorbed_shortwave_layer_8
#>  shaded_absorbed_shortwave_layer_9
#>  shaded_fraction_layer_0
#>  shaded_fraction_layer_1
#>  shaded_fraction_layer_2
#>  shaded_fraction_layer_3
#>  shaded_fraction_layer_4
#>  shaded_fraction_layer_5
#>  shaded_fraction_layer_6
#>  shaded_fraction_layer_7
#>  shaded_fraction_layer_8
#>  shaded_fraction_layer_9
#>  soil_air_entry
#>  soil_b_coefficient
#>  soil_clod_size
#>  soil_depth1
#>  soil_depth2
#>  soil_depth3
#>  soil_field_capacity
#>  soil_reflectance
#>  soil_sand_content
#>  soil_saturated_conductivity
#>  soil_saturation_capacity
#>  soil_transmission
#>  soil_water_content
#>  soil_wilting_point
#>  solar
#>  sowing_fractional_doy
#>  specific_heat_of_air
#>  sunlit_Assim_layer_0
#>  sunlit_Assim_layer_1
#>  sunlit_Assim_layer_2
#>  sunlit_Assim_layer_3
#>  sunlit_Assim_layer_4
#>  sunlit_Assim_layer_5
#>  sunlit_Assim_layer_6
#>  sunlit_Assim_layer_7
#>  sunlit_Assim_layer_8
#>  sunlit_Assim_layer_9
#>  sunlit_GrossAssim_layer_0
#>  sunlit_GrossAssim_layer_1
#>  sunlit_GrossAssim_layer_2
#>  sunlit_GrossAssim_layer_3
#>  sunlit_GrossAssim_layer_4
#>  sunlit_GrossAssim_layer_5
#>  sunlit_GrossAssim_layer_6
#>  sunlit_GrossAssim_layer_7
#>  sunlit_GrossAssim_layer_8
#>  sunlit_GrossAssim_layer_9
#>  sunlit_Gs_layer_0
#>  sunlit_Gs_layer_1
#>  sunlit_Gs_layer_2
#>  sunlit_Gs_layer_3
#>  sunlit_Gs_layer_4
#>  sunlit_Gs_layer_5
#>  sunlit_Gs_layer_6
#>  sunlit_Gs_layer_7
#>  sunlit_Gs_layer_8
#>  sunlit_Gs_layer_9
#>  sunlit_RL_layer_0
#>  sunlit_RL_layer_1
#>  sunlit_RL_layer_2
#>  sunlit_RL_layer_3
#>  sunlit_RL_layer_4
#>  sunlit_RL_layer_5
#>  sunlit_RL_layer_6
#>  sunlit_RL_layer_7
#>  sunlit_RL_layer_8
#>  sunlit_RL_layer_9
#>  sunlit_Rp_layer_0
#>  sunlit_Rp_layer_1
#>  sunlit_Rp_layer_2
#>  sunlit_Rp_layer_3
#>  sunlit_Rp_layer_4
#>  sunlit_Rp_layer_5
#>  sunlit_Rp_layer_6
#>  sunlit_Rp_layer_7
#>  sunlit_Rp_layer_8
#>  sunlit_Rp_layer_9
#>  sunlit_TransR_layer_0
#>  sunlit_TransR_layer_1
#>  sunlit_TransR_layer_2
#>  sunlit_TransR_layer_3
#>  sunlit_TransR_layer_4
#>  sunlit_TransR_layer_5
#>  sunlit_TransR_layer_6
#>  sunlit_TransR_layer_7
#>  sunlit_TransR_layer_8
#>  sunlit_TransR_layer_9
#>  sunlit_absorbed_ppfd_layer_0
#>  sunlit_absorbed_ppfd_layer_1
#>  sunlit_absorbed_ppfd_layer_2
#>  sunlit_absorbed_ppfd_layer_3
#>  sunlit_absorbed_ppfd_layer_4
#>  sunlit_absorbed_ppfd_layer_5
#>  sunlit_absorbed_ppfd_layer_6
#>  sunlit_absorbed_ppfd_layer_7
#>  sunlit_absorbed_ppfd_layer_8
#>  sunlit_absorbed_ppfd_layer_9
#>  sunlit_absorbed_shortwave_layer_0
#>  sunlit_absorbed_shortwave_layer_1
#>  sunlit_absorbed_shortwave_layer_2
#>  sunlit_absorbed_shortwave_layer_3
#>  sunlit_absorbed_shortwave_layer_4
#>  sunlit_absorbed_shortwave_layer_5
#>  sunlit_absorbed_shortwave_layer_6
#>  sunlit_absorbed_shortwave_layer_7
#>  sunlit_absorbed_shortwave_layer_8
#>  sunlit_absorbed_shortwave_layer_9
#>  sunlit_fraction_layer_0
#>  sunlit_fraction_layer_1
#>  sunlit_fraction_layer_2
#>  sunlit_fraction_layer_3
#>  sunlit_fraction_layer_4
#>  sunlit_fraction_layer_5
#>  sunlit_fraction_layer_6
#>  sunlit_fraction_layer_7
#>  sunlit_fraction_layer_8
#>  sunlit_fraction_layer_9
#>  tbase
#>  temp
#>  theta_0
#>  theta_1
#>  theta_2
#>  time
#>  time_zone_offset
#>  whole_plant_growth_respiration_molar_flux
#>  windspeed
#>  windspeed_height
#>  windspeed_layer_0
#>  windspeed_layer_1
#>  windspeed_layer_2
#>  windspeed_layer_3
#>  windspeed_layer_4
#>  windspeed_layer_5
#>  windspeed_layer_6
#>  windspeed_layer_7
#>  windspeed_layer_8
#>  windspeed_layer_9
#>  wsFun
#>  year
#> 
#> The following parameters were not used as inputs to any module:
#>  Jmax_at_25_mature
#>  km_leaf_litter
#>  sf_jmax
#>  soil_bulk_density
#>  soil_clay_content
#>  soil_silt_content
#> You may want to consider removing them for clarity
#> 
#> The following drivers were not used as inputs to any module:
#>  dw_solar
#>  netsolar
#>  up_solar
#>  zen
#> You may want to consider removing them for clarity
#> 
#> All quantities in the initial values have associated derivatives
#> 
#> Derivatives for the following quantities are each determined by more than one module:
#>  Grain
#>  Grain
#>  Leaf
#>  Leaf
#>  Rhizome
#>  Rhizome
#>  Root
#>  Root
#>  Shell
#>  Shell
#>  Stem
#>  Stem
#> 
#> No direct modules require a fixed step size Euler ode_solver
#> 
#> No differential modules require a fixed step size Euler ode_solver
#> 
#> All modules in the direct module list are direct modules
#> 
#> All modules in the differential module list are differential modules
#> 
#> [1] FALSE

# Example 2: a valid set of input arguments
validate_dynamical_system_inputs(
  soybean$initial_values,
  soybean$parameters,
  soybean_weather$'2002',
  soybean$direct_modules,
  soybean$differential_modules
)
#> 
#> Checking the validity of the system inputs:
#> 
#> [pass] No quantities were defined multiple times in the inputs
#> 
#> [pass] All module inputs were properly defined
#> 
#> [pass] All differential module outputs were included in the initial values
#> 
#> [pass] There are no cyclic dependencies among the direct modules.
#> 
#> System inputs are valid
#> 
#> Printing additional information about the system inputs:
#> 
#> The direct modules are in a suitable order for evaluation.
#> 
#> The following quantities were each required by at least one module:
#>  Catm
#>  DVI
#>  Grain
#>  Grain_mr_rate
#>  Gs_min
#>  Gstar_Ea
#>  Gstar_c
#>  Jmax_Ea
#>  Jmax_at_25
#>  Jmax_c
#>  Kc_Ea
#>  Kc_c
#>  Ko_Ea
#>  Ko_c
#>  Leaf
#>  LeafN
#>  LeafN_0
#>  LeafWS
#>  Leaf_mr_rate
#>  O2
#>  RL_Ea
#>  RL_at_25
#>  RL_c
#>  Rhizome
#>  Rhizome_mr_rate
#>  Rmax_emrV0
#>  Root
#>  Root_mr_rate
#>  Shell
#>  Shell_mr_rate
#>  Sp
#>  Sp_thermal_time_decay
#>  Stem
#>  Stem_mr_rate
#>  StomataWS
#>  TTc
#>  TTemr_threshold
#>  Tbase_emr
#>  Tmax_R0R1
#>  Tmax_R1R7
#>  Tmax_emrV0
#>  Tmin_R0R1
#>  Tmin_R1R7
#>  Tmin_emrV0
#>  Topt_R0R1
#>  Topt_R1R7
#>  Topt_emrV0
#>  Tp_Ha
#>  Tp_Hd
#>  Tp_S
#>  Tp_at_25
#>  Tp_c
#>  Vcmax_Ea
#>  Vcmax_at_25
#>  Vcmax_c
#>  absorbed_longwave
#>  alpha1
#>  alphaLeaf
#>  alphaRhizome
#>  alphaRoot
#>  alphaSeneLeaf
#>  alphaSeneRhizome
#>  alphaSeneRoot
#>  alphaSeneStem
#>  alphaShell
#>  alphaStem
#>  alphab1
#>  atmospheric_pressure
#>  atmospheric_scattering
#>  atmospheric_transmittance
#>  b0
#>  b1
#>  betaLeaf
#>  betaRhizome
#>  betaRoot
#>  betaSeneLeaf
#>  betaSeneRhizome
#>  betaSeneRoot
#>  betaSeneStem
#>  betaShell
#>  betaStem
#>  beta_PSII
#>  canopy_assimilation_molar_flux
#>  canopy_assimilation_rate
#>  canopy_gross_assimilation_molar_flux
#>  canopy_height
#>  canopy_non_photorespiratory_CO2_release_molar_flux
#>  canopy_photorespiration_molar_flux
#>  canopy_transpiration_rate
#>  chil
#>  cosine_zenith_angle
#>  cws1
#>  cws2
#>  day_length
#>  development_rate_per_hour
#>  dry_biomass_per_carbon
#>  electrons_per_carboxylation
#>  electrons_per_oxygenation
#>  emissivity_sky
#>  fractional_doy
#>  gbw_canopy
#>  grc_grain
#>  grc_leaf
#>  grc_rhizome
#>  grc_root
#>  grc_shell
#>  grc_stem
#>  growth_respiration_fraction
#>  height_layer_0
#>  height_layer_1
#>  height_layer_2
#>  height_layer_3
#>  height_layer_4
#>  height_layer_5
#>  height_layer_6
#>  height_layer_7
#>  height_layer_8
#>  height_layer_9
#>  heightf
#>  hydrDist
#>  iSp
#>  irradiance_diffuse_fraction
#>  irradiance_direct_fraction
#>  kGrain
#>  kLeaf
#>  kRhizome
#>  kRhizome_emr
#>  kRhizome_emr_DVI
#>  kRoot
#>  kSeneLeaf
#>  kSeneRhizome
#>  kSeneRoot
#>  kSeneStem
#>  kShell
#>  kStem
#>  k_diffuse
#>  kpLN
#>  lai
#>  lat
#>  leaf_reflectance_nir
#>  leaf_reflectance_par
#>  leaf_transmittance_nir
#>  leaf_transmittance_par
#>  leafwidth
#>  lnfun
#>  longitude
#>  maturity_group
#>  min_gbw_canopy
#>  mrc_grain
#>  mrc_leaf
#>  mrc_rhizome
#>  mrc_root
#>  mrc_shell
#>  mrc_stem
#>  net_assimilation_rate_grain
#>  net_assimilation_rate_leaf
#>  net_assimilation_rate_rhizome
#>  net_assimilation_rate_root
#>  net_assimilation_rate_shell
#>  net_assimilation_rate_stem
#>  par_energy_content
#>  par_energy_fraction
#>  par_incident_diffuse
#>  par_incident_direct
#>  phi1
#>  phi2
#>  phi_PSII_0
#>  phi_PSII_1
#>  phi_PSII_2
#>  precip
#>  rateSeneLeaf
#>  rateSeneRhizome
#>  rateSeneRoot
#>  rateSeneStem
#>  remobilization_fraction
#>  retrans
#>  retrans_rhizome
#>  rfl
#>  rh
#>  rsdf
#>  rsec
#>  shaded_Assim_layer_0
#>  shaded_Assim_layer_1
#>  shaded_Assim_layer_2
#>  shaded_Assim_layer_3
#>  shaded_Assim_layer_4
#>  shaded_Assim_layer_5
#>  shaded_Assim_layer_6
#>  shaded_Assim_layer_7
#>  shaded_Assim_layer_8
#>  shaded_Assim_layer_9
#>  shaded_GrossAssim_layer_0
#>  shaded_GrossAssim_layer_1
#>  shaded_GrossAssim_layer_2
#>  shaded_GrossAssim_layer_3
#>  shaded_GrossAssim_layer_4
#>  shaded_GrossAssim_layer_5
#>  shaded_GrossAssim_layer_6
#>  shaded_GrossAssim_layer_7
#>  shaded_GrossAssim_layer_8
#>  shaded_GrossAssim_layer_9
#>  shaded_Gs_layer_0
#>  shaded_Gs_layer_1
#>  shaded_Gs_layer_2
#>  shaded_Gs_layer_3
#>  shaded_Gs_layer_4
#>  shaded_Gs_layer_5
#>  shaded_Gs_layer_6
#>  shaded_Gs_layer_7
#>  shaded_Gs_layer_8
#>  shaded_Gs_layer_9
#>  shaded_RL_layer_0
#>  shaded_RL_layer_1
#>  shaded_RL_layer_2
#>  shaded_RL_layer_3
#>  shaded_RL_layer_4
#>  shaded_RL_layer_5
#>  shaded_RL_layer_6
#>  shaded_RL_layer_7
#>  shaded_RL_layer_8
#>  shaded_RL_layer_9
#>  shaded_Rp_layer_0
#>  shaded_Rp_layer_1
#>  shaded_Rp_layer_2
#>  shaded_Rp_layer_3
#>  shaded_Rp_layer_4
#>  shaded_Rp_layer_5
#>  shaded_Rp_layer_6
#>  shaded_Rp_layer_7
#>  shaded_Rp_layer_8
#>  shaded_Rp_layer_9
#>  shaded_TransR_layer_0
#>  shaded_TransR_layer_1
#>  shaded_TransR_layer_2
#>  shaded_TransR_layer_3
#>  shaded_TransR_layer_4
#>  shaded_TransR_layer_5
#>  shaded_TransR_layer_6
#>  shaded_TransR_layer_7
#>  shaded_TransR_layer_8
#>  shaded_TransR_layer_9
#>  shaded_absorbed_ppfd_layer_0
#>  shaded_absorbed_ppfd_layer_1
#>  shaded_absorbed_ppfd_layer_2
#>  shaded_absorbed_ppfd_layer_3
#>  shaded_absorbed_ppfd_layer_4
#>  shaded_absorbed_ppfd_layer_5
#>  shaded_absorbed_ppfd_layer_6
#>  shaded_absorbed_ppfd_layer_7
#>  shaded_absorbed_ppfd_layer_8
#>  shaded_absorbed_ppfd_layer_9
#>  shaded_absorbed_shortwave_layer_0
#>  shaded_absorbed_shortwave_layer_1
#>  shaded_absorbed_shortwave_layer_2
#>  shaded_absorbed_shortwave_layer_3
#>  shaded_absorbed_shortwave_layer_4
#>  shaded_absorbed_shortwave_layer_5
#>  shaded_absorbed_shortwave_layer_6
#>  shaded_absorbed_shortwave_layer_7
#>  shaded_absorbed_shortwave_layer_8
#>  shaded_absorbed_shortwave_layer_9
#>  shaded_fraction_layer_0
#>  shaded_fraction_layer_1
#>  shaded_fraction_layer_2
#>  shaded_fraction_layer_3
#>  shaded_fraction_layer_4
#>  shaded_fraction_layer_5
#>  shaded_fraction_layer_6
#>  shaded_fraction_layer_7
#>  shaded_fraction_layer_8
#>  shaded_fraction_layer_9
#>  soil_air_entry
#>  soil_b_coefficient
#>  soil_clod_size
#>  soil_depth1
#>  soil_depth2
#>  soil_depth3
#>  soil_field_capacity
#>  soil_reflectance
#>  soil_sand_content
#>  soil_saturated_conductivity
#>  soil_saturation_capacity
#>  soil_transmission
#>  soil_water_content
#>  soil_wilting_point
#>  solar
#>  sowing_fractional_doy
#>  specific_heat_of_air
#>  sunlit_Assim_layer_0
#>  sunlit_Assim_layer_1
#>  sunlit_Assim_layer_2
#>  sunlit_Assim_layer_3
#>  sunlit_Assim_layer_4
#>  sunlit_Assim_layer_5
#>  sunlit_Assim_layer_6
#>  sunlit_Assim_layer_7
#>  sunlit_Assim_layer_8
#>  sunlit_Assim_layer_9
#>  sunlit_GrossAssim_layer_0
#>  sunlit_GrossAssim_layer_1
#>  sunlit_GrossAssim_layer_2
#>  sunlit_GrossAssim_layer_3
#>  sunlit_GrossAssim_layer_4
#>  sunlit_GrossAssim_layer_5
#>  sunlit_GrossAssim_layer_6
#>  sunlit_GrossAssim_layer_7
#>  sunlit_GrossAssim_layer_8
#>  sunlit_GrossAssim_layer_9
#>  sunlit_Gs_layer_0
#>  sunlit_Gs_layer_1
#>  sunlit_Gs_layer_2
#>  sunlit_Gs_layer_3
#>  sunlit_Gs_layer_4
#>  sunlit_Gs_layer_5
#>  sunlit_Gs_layer_6
#>  sunlit_Gs_layer_7
#>  sunlit_Gs_layer_8
#>  sunlit_Gs_layer_9
#>  sunlit_RL_layer_0
#>  sunlit_RL_layer_1
#>  sunlit_RL_layer_2
#>  sunlit_RL_layer_3
#>  sunlit_RL_layer_4
#>  sunlit_RL_layer_5
#>  sunlit_RL_layer_6
#>  sunlit_RL_layer_7
#>  sunlit_RL_layer_8
#>  sunlit_RL_layer_9
#>  sunlit_Rp_layer_0
#>  sunlit_Rp_layer_1
#>  sunlit_Rp_layer_2
#>  sunlit_Rp_layer_3
#>  sunlit_Rp_layer_4
#>  sunlit_Rp_layer_5
#>  sunlit_Rp_layer_6
#>  sunlit_Rp_layer_7
#>  sunlit_Rp_layer_8
#>  sunlit_Rp_layer_9
#>  sunlit_TransR_layer_0
#>  sunlit_TransR_layer_1
#>  sunlit_TransR_layer_2
#>  sunlit_TransR_layer_3
#>  sunlit_TransR_layer_4
#>  sunlit_TransR_layer_5
#>  sunlit_TransR_layer_6
#>  sunlit_TransR_layer_7
#>  sunlit_TransR_layer_8
#>  sunlit_TransR_layer_9
#>  sunlit_absorbed_ppfd_layer_0
#>  sunlit_absorbed_ppfd_layer_1
#>  sunlit_absorbed_ppfd_layer_2
#>  sunlit_absorbed_ppfd_layer_3
#>  sunlit_absorbed_ppfd_layer_4
#>  sunlit_absorbed_ppfd_layer_5
#>  sunlit_absorbed_ppfd_layer_6
#>  sunlit_absorbed_ppfd_layer_7
#>  sunlit_absorbed_ppfd_layer_8
#>  sunlit_absorbed_ppfd_layer_9
#>  sunlit_absorbed_shortwave_layer_0
#>  sunlit_absorbed_shortwave_layer_1
#>  sunlit_absorbed_shortwave_layer_2
#>  sunlit_absorbed_shortwave_layer_3
#>  sunlit_absorbed_shortwave_layer_4
#>  sunlit_absorbed_shortwave_layer_5
#>  sunlit_absorbed_shortwave_layer_6
#>  sunlit_absorbed_shortwave_layer_7
#>  sunlit_absorbed_shortwave_layer_8
#>  sunlit_absorbed_shortwave_layer_9
#>  sunlit_fraction_layer_0
#>  sunlit_fraction_layer_1
#>  sunlit_fraction_layer_2
#>  sunlit_fraction_layer_3
#>  sunlit_fraction_layer_4
#>  sunlit_fraction_layer_5
#>  sunlit_fraction_layer_6
#>  sunlit_fraction_layer_7
#>  sunlit_fraction_layer_8
#>  sunlit_fraction_layer_9
#>  tbase
#>  temp
#>  theta_0
#>  theta_1
#>  theta_2
#>  time
#>  time_zone_offset
#>  whole_plant_growth_respiration_molar_flux
#>  windspeed
#>  windspeed_height
#>  windspeed_layer_0
#>  windspeed_layer_1
#>  windspeed_layer_2
#>  windspeed_layer_3
#>  windspeed_layer_4
#>  windspeed_layer_5
#>  windspeed_layer_6
#>  windspeed_layer_7
#>  windspeed_layer_8
#>  windspeed_layer_9
#>  wsFun
#>  year
#> 
#> The following parameters were not used as inputs to any module:
#>  Jmax_at_25_mature
#>  km_leaf_litter
#>  sf_jmax
#>  soil_bulk_density
#>  soil_clay_content
#>  soil_silt_content
#> You may want to consider removing them for clarity
#> 
#> The following drivers were not used as inputs to any module:
#>  dw_solar
#>  netsolar
#>  up_solar
#>  zen
#> You may want to consider removing them for clarity
#> 
#> All quantities in the initial values have associated derivatives
#> 
#> Derivatives for the following quantities are each determined by more than one module:
#>  Grain
#>  Grain
#>  Leaf
#>  Leaf
#>  Rhizome
#>  Rhizome
#>  Root
#>  Root
#>  Shell
#>  Shell
#>  Stem
#>  Stem
#> 
#> No direct modules require a fixed step size Euler ode_solver
#> 
#> No differential modules require a fixed step size Euler ode_solver
#> 
#> All modules in the direct module list are direct modules
#> 
#> All modules in the differential module list are differential modules
#> 
#> [1] TRUE
```
