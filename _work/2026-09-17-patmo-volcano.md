---
title: PATMO Volcano
subtitle: Inputs, physical processes, and a practical guide to volcanic forcing in PATMO.
date: 2026-09-17 12:00:00 +0900
status: Technical guide
stack: Atmospheric chemistry / Volcanic forcing / Model documentation
permalink: /work/patmo-volcano/
body_class: patmo-reading-theme
head-extra:
  - patmo-reading-theme.html
  - mathjax.html
description: English documentation of PATMO volcanic SO2 injection, ash and sulfate optical budgets, transport equations, input provenance, and diagnostic outputs.
excerpt: A guide to the current PATMO volcano module, covering its inputs and sources, physical equations, optical pre-run, full chemistry workflow, and output files.
---

<style>
.volcano-guide { line-height: 1.75; }
.volcano-guide h2 { margin-top: 2.8rem; scroll-margin-top: 6rem; }
.volcano-guide h3 { margin-top: 1.8rem; }
.volcano-guide code { overflow-wrap: anywhere; }
.volcano-guide pre code { overflow-wrap: normal; }
.volcano-guide table { display: block; max-width: 100%; overflow-x: auto; font-size: .95em; }
.volcano-guide th, .volcano-guide td { min-width: 9rem; padding: .7rem; vertical-align: top; }
.volcano-equation { overflow-x: auto; padding: .7rem 0; margin: 1rem 0; }
.volcano-guide .guide-note { border-left: 4px solid #0f5f73; padding: 1rem 1.3rem; background: #edf2ef; }
.volcano-guide .guide-nav { padding: 1.1rem 1.5rem; background: #efe4d2; border-radius: .7rem; }
@media print { .volcano-guide table { display: table; font-size: 9pt; } .volcano-guide pre { white-space: pre-wrap; } }
</style>

<div class="volcano-guide" markdown="1">

[Work]({{ '/work/' | relative_url }}) / **PATMO Volcano**

This guide describes the implementation in **PATMO-main as inspected on 17 September 2026**, using `tests/volcano_pinatubo_1991` as the reference case. It explains what the module calculates, where its inputs come from, how to run it, and how to read its outputs. Earlier notes may use the former case name `modern_sulfur_volcano`; the commands below use the current directory name.

<div class="guide-note" markdown="1">
**Scope.** This is a prescribed, one-dimensional volcanic forcing model for sensitivity experiments. It combines a chemical SO₂ source with evolving ash and sulfate aerosol optical budgets. It is informed by Pinatubo observations, but it is not an observation-fitted reconstruction of the 1991 eruption. Here, **SSA means sulfate aerosol**, not single-scattering albedo.
</div>

<nav class="guide-nav" aria-label="Guide contents" markdown="1">
**Contents**

1. [What the module does](#overview)
2. [Inputs and information sources](#inputs)
3. [Physical processes and equations](#physics)
4. [How to run the model](#usage)
5. [Output files and interpretation](#outputs)
6. [Validation and limitations](#validation)
7. [References and implementation map](#references)
</nav>

## 1. What the module does {#overview}

The module follows two connected calculation paths:

~~~text
SO2 mass + averaging area + duration
    → prescribed SO2 source at altitude → PATMO chemical evolution

Ash budget + sulfate precursor potential
    → injection, ascent, mixing, formation, settling, and removal
    → ash + formed sulfate optical depth
    → direct solar attenuation → photolysis rates → chemical evolution
~~~

Chemical SO₂ is injected into a Gaussian altitude profile. With positive particle rise times, ash and sulfate precursor are supplied continuously at a fixed vent layer, then carried upward by a prescribed flow. The precursor forms sulfate locally; it is not itself an optical absorber or a settling particle.

**The two sulfur representations are independent.** `sulfate_tau_550` is a prescribed optical potential. It is not calculated from SO₂ mass, and its conversion does not remove SO₂ from the chemical network. Conversely, changing chemical SO₂ does not automatically change this sulfate opacity. The optical scheme therefore conserves optical potential under its idealized assumptions, not elemental sulfur mass.

There are two executables:

| Executable | Purpose | Chemistry |
| --- | --- | --- |
| `./test_volcano` | Fast optical pre-run: inspect particle profiles and wavelength-dependent shielding. | Does not integrate the chemical ODE. |
| `./test` | Background spin-up followed by the volcanic chemistry experiment. | Integrates the configured PATMO network and records selected histories. |

## 2. Inputs and information sources {#inputs}

### 2.1 File ownership

Keep lasting changes in `tests/volcano_pinatubo_1991/`. The `build/` directory is generated and can be overwritten. The root `compile.sh` refreshes spreadsheet-derived inputs before generating the model; the case-local build script also compiles it.

| Case file | Contents and role |
| --- | --- |
| `settings.xlsx` → `options.opt` | Grid, radiation settings, fixed species, emissions, and deposition settings. |
| `reaction_network.xlsx` → `reaction_network.ntw` | Reaction identities, reactants, products, and rate expressions. |
| `profile.xlsx` → `profile.dat` | Initial atmospheric profile, including the background eddy mixing coefficient, Kzz. |
| `solar_flux.xlsx` → `solar_flux.txt` | Wavelength and incident photon spectral flux. Generation aligns bin-average flux to the actual `xsecs/photoMetric.dat` grid. Units are photons cm⁻² s⁻¹ nm⁻¹. |
| `volcano_events.dat` | Event timing, SO₂ loading and area, plume geometry, ash and sulfate optical/transport parameters. Used by both executables. |
| `volcano_prerun.in` | Pre-run file paths, time and wavelength sampling, optical threshold, and optional fixed end time. |
| `volcano_run.in` | Full-model spin-up limit, equilibrium criterion, and volcanic simulation duration. |
| `volcano_history.in` | Species/reaction selections, sampling intervals, output windows, and history filename prefix. |
| `partial_H2SO4.txt`, `vapor_H2SO4.txt`, `SO4_deposition_rate.txt` | Auxiliary physical tables read by the full chemistry driver. They are separate from the prescribed volcanic optical precursor. |
| `copylist.pcp` | Manifest of case inputs and drivers to copy into `build/`. |

The background atmosphere and chemistry are inherited from the modern sulfur-cycle setup. They are not identified as a measured Pinatubo-day atmosphere. The volcano event file does not establish the original provenance of every chemical rate, cross section, or solar spectrum. For a reproducible scientific application, archive the actual workbooks, cross-section data, physical tables, generated options, and their original dataset references alongside the run.

### 2.2 Reference event and provenance

The following values are the **active case settings**, not universal module defaults. The literature constrains selected scales; the remaining parameters define the sensitivity experiment.

| Quantity / input | Reference value | Basis and interpretation |
| --- | --- | --- |
| `start_day` | 0 | Start at the beginning of the volcanic phase, after background equilibrium. |
| `duration_hour` | 9 h | Uniform-emission approximation informed by the climactic eruption duration described by [Guo et al. (2004b)](https://doi.org/10.1029/2003GC000655). |
| `so2_mass_tg` | 20 Tg SO₂ | Rounded Pinatubo-scale loading. [Guo et al. (2004a)](https://doi.org/10.1029/2003GC000654) report retrieval-dependent estimates and uncertainties; 20 Tg is not an exact measured value. |
| `plume_radius_km` | 2500 km | Assumed regional averaging radius, not a measured vent plume radius or a fixed observing site. |
| `plume_bottom_km`, `plume_top_km` | 20, 25 km | Informed by the main early aerosol layer reported by [McCormick and Veiga (1992)](https://doi.org/10.1029/91GL02790). These define the particle ascent taper and Gaussian SO₂ geometry, not a hard cloud boundary. |
| `ash_rise_start_km`, `sulfate_rise_start_km` | 0 km | Idealized ground coordinate, not measured vent elevation. |
| `ash_rise_time_hour`, `sulfate_rise_time_hour` | 0.5 h | Assumed nominal ascent scale; distinct from the 9 h emission duration. |
| `ash_tau_550`, `sulfate_tau_550` | 0.4, 0.15 | Prescribed integrated ash budget and sulfate precursor potential. Neither is a guaranteed realized local peak AOD. |
| Ash / sulfate radius | 2 / 0.4 μm | Fixed spherical sensitivity radii. A single settling radius is not interchangeable with a retrieved size-distribution effective radius. |
| Ash / sulfate density | 2.5 / 1.6 g cm⁻³ | Assumed particle densities for the settling estimate. |
| Ash / sulfate Cunningham factor | 1.9 / 6.3 | Fixed reference-height slip corrections; not recalculated with altitude. |
| Gravity / air viscosity | 980.665 cm s⁻² / 1.7 × 10⁻⁴ g cm⁻¹ s⁻¹ | Fixed values in the reference settling calculation. |
| Ash / sulfate settling speed | ≈ 0.21044 / 0.017863 km day⁻¹ | Derived from the selected radii, densities, viscosity, gravity, and slip factors. |
| `ash_vertical_diffusion_cm2_s`, `sulfate_vertical_diffusion_cm2_s` | `background` | Use the atmospheric Kzz profile. Positive numeric values override it; zero disables mixing. |
| `sulfate_formation_day` | 25 days | First-order conversion approximation informed by observed SO₂ removal times in [Guo et al. (2004a)](https://doi.org/10.1029/2003GC000654). Observed removal is not a uniquely measured aerosol-formation rate. |
| Ash / sulfate horizontal lifetime | 1 / 14 days | Assumed export from the selected column, not global atmospheric residence times. |
| `ash_lifetime_day` | 2 days | Additional residual ash removal, applied after emission ends. |
| `sulfate_lifetime_day` | −1 | Disable additional particle loss; explicit settling and horizontal export still operate. |
| Ash / sulfate wavelength exponent | 0.2 / 0.8 | Assumed power-law spectral slopes, not a fit to the UV extinction spectrum. |

The reference grid has 60 layers of 1 km and 4400 photochemical bins over 120–400 nm. The zenith angle is 60° and the top-of-atmosphere flux multiplier is 0.5. These belong to the current case configuration and can change when `settings.xlsx` is edited. In particular, 550 nm is an optical reference wavelength outside this photochemical interval; UV opacity is extrapolated using the prescribed slopes.

### 2.3 Event-file syntax

Use **one complete event per non-comment line**, with `key=value` pairs. Spaces around `=` and comma separators are accepted. `#` and `!` introduce comments. Multiple event lines are supported; the reference case has one active event. Fortran values such as `0.5d0` mean 0.5 in double precision.

Specify a positive duration for the documented SO₂ mass/column workflow. Use one SO₂ source representation:

| Input | Meaning |
| --- | --- |
| `so2_flux_cm2_s` | Constant source in molecules cm⁻² s⁻¹. |
| `so2_column_cm2` | Total molecular column, divided by event duration. |
| `so2_mass_tg` with `plume_radius_km` or `injection_area_km2` | Total mass converted to a column using the selected area. |

If competing inputs are supplied, explicit flux takes precedence over column, and column over mass; explicit area takes precedence over radius. Using only one representation avoids ambiguity. Time aliases include `start_hour`, `start_s`, `duration_day`, and `duration_s`.

Use `plume_center_km` and `plume_sigma_km`, or let bottom/top values define them. An optional `plume_fwhm_km` sets sigma to FWHM / 2.35482 when sigma is omitted. Explicit center/sigma take precedence over the derived geometry. The parser also accepts `sulfate_start_delay_day` (or `_hour`, `_s`) to delay the precursor source relative to the event; the reference has no delay.

For each particle type, a positive `*_settling_cm_s` (also `_m_s` or `_km_day`) overrides the Stokes estimate. Otherwise the radius-based calculation is used. To disable settling, set both the direct speed and particle radius to zero. A horizontal lifetime ≤ 0 disables horizontal export. For residual `*_lifetime_day`, a negative value disables loss, while zero removes the budget immediately once the relevant loss period begins. These two lifetime controls therefore treat zero differently.

## 3. Physical processes and equations {#physics}

### 3.1 Notation and units

Layer index *j* increases upward; *N* is the top layer. Altitude *z* and layer thickness Δ*z* are in cm in the internal calculations. Time is in seconds. Let Aⱼ be layer-integrated ash optical depth at 550 nm, Pⱼ unconverted sulfate optical potential, and Sⱼ formed sulfate optical depth. All three budgets are dimensionless, but **only Aⱼ and Sⱼ contribute to opacity**. Let *n*air be air number density in cm⁻³.

### 3.2 SO₂ mass, column, and Gaussian injection

For mass *M* in Tg, area *A*area in km², molar mass 64.066 g mol⁻¹, Avogadro constant *N*A, and event duration *D* in seconds:

{% raw %}
<div class="volcano-equation">
\[
A_{\rm area}=\pi R^2,\qquad
N_{\rm col}=\frac{M\,10^{12}N_A}{64.066\,A_{\rm area}\,10^{10}},\qquad
F_{\rm SO_2}=\frac{N_{\rm col}}{D}.
\]
</div>
{% endraw %}

The area factors convert Tg to g and km² to cm². The reference 20 Tg, 2500 km radius, and 9 h duration give approximately 9.57 × 10¹⁷ molecules cm⁻² and 2.95 × 10¹³ molecules cm⁻² s⁻¹.

The discrete Gaussian is normalized over the actual model layers:

{% raw %}
<div class="volcano-equation">
\[
g_j=\exp\!\left[-\frac{(z_j-z_c)^2}{2\sigma^2}\right],\qquad
W=\sum_j g_j\Delta z_j,\qquad
Q_{{\rm SO_2},j}(t)=F_{\rm SO_2}\frac{g_j}{W}\,I_{[t_0,t_0+D)}(t).
\]
</div>
{% endraw %}

Here *I* is one during emission and zero otherwise. The volumetric source Q has units molecules cm⁻³ s⁻¹, and its vertical integral equals the prescribed column flux. When bottom/top geometry is used, *z*c = (*z*top + *z*bottom)/2 and σ = (*z*top − *z*bottom)/4. The reference gives 22.5 km and 1.25 km. The Gaussian has tails beyond 20–25 km; those input heights do not truncate it.

This source is added to the network species named `SO2`. It does not automatically distribute volcanic sulfur among SO₂ isotopologues. Any isotope-specific injection requires a separately defined treatment.

### 3.3 Continuous optical injection

For an event optical budget B and an integration interval [*t*, *t* + Δ*t*], the emitted increment is

{% raw %}
<div class="volcano-equation">
\[
\Delta B=B\,\frac{\max\{0,\min(t+\Delta t,t_0+D)-\max(t,t_0)\}}{D}.
\]
</div>
{% endraw %}

Apply this independently to ash and precursor. With positive rise time and a target center above the vent, the increment enters the layer nearest the fixed source altitude. A vent below the grid maps to the lowest layer. The source stays at that layer throughout the event. With zero rise time, the increment is placed directly into the Gaussian profile with layer fraction *g*ⱼΔ*z*ⱼ/*W*. There is no direct formed-SSA source.

### 3.4 Prescribed ascent

For each particle family, define the nominal upward speed and taper heights:

{% raw %}
<div class="volcano-equation">
\[
u_0=\frac{\max(z_c-z_v,0)}{T_{\rm rise}},\qquad
z_L=\max(z_v,z_c-2\sigma),\qquad z_U=z_c+2\sigma.
\]
\[
f(z)=\begin{cases}
0,&z<z_v,\\
1,&z_v\le z\le z_L,\\
(z_U-z)/(z_U-z_L),&z_L<z<z_U,\\
0,&z\ge z_U.
\end{cases}
\qquad u(z,t)=u_0 f(z)\,I_{[t_0,t_0+D+T_{\rm rise})}(t).
\]
</div>
{% endraw %}

The implementation enforces a minimum 1 cm separation between taper heights for degenerate geometry. In the reference case, ascent operates for 9.5 h but emission lasts only 9 h. The last 0.5 h transports the final emissions without adding material. Tapering means the specified rise time is not an exact arrival time. The flow itself does not carry particles above the upper taper, although mixing can do so. Ash and sulfate have independent ascent settings; precursor and formed SSA share the sulfate airflow.

### 3.5 Gravitational settling

The Stokes–Cunningham estimate is

{% raw %}
<div class="volcano-equation">
\[
v_s=\frac{2r^2\rho_p g C_c}{9\eta}.
\]
</div>
{% endraw %}

Here *r* is radius in cm, ρₚ particle density in g cm⁻³, *g* gravitational acceleration, *C*c the slip correction, and η dynamic viscosity in g cm⁻¹ s⁻¹. The code neglects air buoyancy in this expression. With upward-positive velocity, ash and formed SSA move at *v* = *u* − *v*s; precursor moves at *v* = *u*. Radius, density, viscosity, and slip correction are fixed per event. The calculation does not evolve particle size or altitude-dependent drag.

### 3.6 Vertical mixing and conservative transport

For any layer budget qⱼ ∈ {Aⱼ, Pⱼ, Sⱼ}, define its optical-budget density *c*ⱼ = qⱼ/Δ*z*ⱼ. The continuum form represented by the transport operator is

{% raw %}
<div class="volcano-equation">
\[
\Phi=v c-K_{zz}n_{\rm air}\frac{\partial(c/n_{\rm air})}{\partial z},\qquad
\frac{dq_j}{dt}=\Phi_{j-1/2}-\Phi_{j+1/2}.
\]
</div>
{% endraw %}

The diffusive flux acts on the air-relative quantity *c*/*n*air, rather than on an unweighted layer difference. Internal faces use arithmetic averages of Kzz and air density. Each face has a single shared flux, so transfer out of one layer enters its neighbor. Ash uses its mixing setting; precursor and formed SSA use the sulfate setting.

Advection uses a monotonized-central slope limiter and upwind face reconstruction. Transport advances with a two-stage strong-stability-preserving Runge–Kutta method:

{% raw %}
<div class="volcano-equation">
\[
q^{(1)}=q^n+hL(q^n),\qquad
q^{n+1}=\tfrac12 q^n+\tfrac12\left[q^{(1)}+hL(q^{(1)})\right].
\]
</div>
{% endraw %}

The internal step *h* is restricted by advective and diffusive rates. The top boundary has zero flux. Downward settling through the bottom leaves the column; boundary diffusion is zero. Without sources, formation, or losses, the column budget changes only through this bottom outflow. This is a conservation statement about the optical budget, not a sulfur-mass closure test.

### 3.7 Sulfate formation

At each layer, precursor is converted by a first-order process with time constant *T*f:

{% raw %}
<div class="volcano-equation">
\[
\frac{dP_j}{dt}=-\frac{P_j}{T_f},\qquad
\frac{dS_j}{dt}=\frac{P_j}{T_f}.
\]
\[
P_j'=P_j e^{-\Delta t/T_f},\qquad
S_j'=S_j+P_j(1-e^{-\Delta t/T_f}).
\]
</div>
{% endraw %}

An isolated parcel converts 63.2% after one formation time, not 100%. The exact exponential conversion is split with transport and loss operations. Formation occurs where precursor is located; the code never copies the ash profile into SSA. For a nonpositive formation time, the conversion routine transfers all available precursor during a positive step.

### 3.8 Horizontal export and residual removal

Positive loss times produce exponential decay:

{% raw %}
<div class="volcano-equation">
\[
q'=q\exp\!\left(-\frac{\Delta t_h}{T_h}-\frac{\Delta t_r}{T_r}\right).
\]
</div>
{% endraw %}

Δ*t*h and Δ*t*r are the portions of the step for which each loss is active. Horizontal export acts from source onset on ash and on both sulfate budgets. Residual ash removal begins after ash emission ends. Residual sulfate removal acts only on formed particles, from their formation onward; it never removes precursor directly. Disabled terms are omitted.

For the reference ash case after emission, export and residual loss alone give an effective e-folding time of (1/1 day + 1/2 days)⁻¹ = 16 h, in addition to settling. Rapid optical decay therefore cannot be attributed entirely to gravitational fallout. Horizontal export represents leaving the selected column; it does not imply removal from the global stratosphere.

### 3.9 Wavelength-dependent opacity and photolysis

For a single event, the volcanic vertical optical depth at layer *j* is

{% raw %}
<div class="volcano-equation">
\[
\tau_{{\rm volc},b,j}=\sum_{k=j}^{N}\left[
A_k\left(\frac{\lambda_b}{550\,{\rm nm}}\right)^{-\alpha_A}
+S_k\left(\frac{\lambda_b}{550\,{\rm nm}}\right)^{-\alpha_S}\right].
\]
</div>
{% endraw %}

Multiple events are summed using their own spectral exponents. The current layer and all layers above it contribute, including the top layer. Precursor is excluded. Volcanic opacity is added to the background/gas optical depth before photolysis is calculated.

The direct photon spectrum used by the model is

{% raw %}
<div class="volcano-equation">
\[
F_{b,j}=C_{\rm TOA} F_{0,b}\exp(-\tau_{{\rm total},b,j}/\mu),
\qquad \mu=\cos\theta>0.
\]
\[
J_{r,j}=\sum_b X_{r,b}\,F_{b,j}\,\Delta\lambda_b,\qquad
\Delta\lambda_b=10^7hc\left(\frac{1}{E_{L,b}}-\frac{1}{E_{R,b}}\right).
\]
</div>
{% endraw %}

*F*₀ is the incident bin-average photon spectral flux; *C*TOA is the configured scaling factor. *X*r,b is the reaction-specific cross-section array used by PATMO; any quantum-yield or branching treatment must be represented consistently in the generated photochemical setup. The width formula uses *h* in eV s, *c* in cm s⁻¹, and energy edges in eV, giving nm. **Every bin has its own wavelength width.** A uniform energy grid is not a uniform wavelength grid.

Direct flux is zero when μ ≤ 0. This treatment does not calculate diffuse or multiply scattered actinic flux. Extinction of the direct beam is therefore not equivalent to a complete radiative-transfer solution.

### 3.10 Coupling to chemistry and equilibrium

The chemical network retains the usual number-density balance:

{% raw %}
<div class="volcano-equation">
\[
\frac{dn_i}{dt}=\sum_r\nu_{ir}R_r+Q_i+\mathcal{T}_i-\mathcal{D}_i,
\qquad R_r=k_r\prod_m n_m^{a_{mr}}.
\]
</div>
{% endraw %}

ν is net stoichiometry, Q includes volcanic SO₂ injection, and the last terms represent the configured gas transport and deposition. For a photolysis reaction, the coefficient is *J*. The volcanic particle ascent/export rules above do not automatically apply to chemical gases; gases retain PATMO's own transport and loss terms.

Before volcanic forcing starts, the full driver checks the maximum relative daily change over every chemical species and layer:

{% raw %}
<div class="volcano-equation">
\[
\epsilon=\max_{i,j}\frac{|n_{i,j}^{\rm new}-n_{i,j}^{\rm old}|}
{\max(|n_{i,j}^{\rm new}|,|n_{i,j}^{\rm old}|,n_{{\rm air},j}f_{\rm floor})}.
\]
</div>
{% endraw %}

The reference requires ε &lt; 10⁻⁴ for 30 consecutive daily checks, with *f*floor = 10⁻¹⁵. The current network includes the `M` pseudo-species, and the driver obtains physical air density as half the sum over its chemical-species array. Recheck that convention if the network representation changes.

The volcano clock is held during spin-up and reset when the criterion passes. The maximum spin-up is 40 years; failure to equilibrate stops the run without initiating the eruption. During volcanic evolution, outer steps resolve event boundaries and output times, and photolysis is refreshed on every model call.

## 4. How to run the model {#usage}

### 4.1 Prepare and compile

The build requires Python 3 with `numpy`, `pandas`, and `openpyxl`, a Fortran compiler, and `make`. From the PATMO repository root:

~~~bash
./tests/volcano_pinatubo_1991/compile_volcano_pinatubo_1991.sh
~~~

This refreshes spreadsheet inputs, generates code and spectral inputs, verifies the copied case files, and compiles both executables. It does not launch a simulation. The equivalent explicit sequence is:

~~~bash
./compile.sh volcano_pinatubo_1991
cd build
make
~~~

The root `compile.sh` generates the model but does not itself run `make`. Running `python3 patmo -test=volcano_pinatubo_1991` directly uses the existing converted options, network, and profile; it does not replace the spreadsheet conversion step.

### 4.2 Inspect the optical pre-run

Set `volcano_prerun.in` in the case directory before regeneration. For a first 72-hour inspection, keep its file-path settings and use:

~~~text
output_time_step_hour = 1d0
output_early_time_step_s = 300d0
output_early_until_hour = 1d0
output_wavelength_step_nm = 5d0
tau_floor = 1d-6
output_end_day = 3d0
~~~

Then, from `build/`, run:

~~~bash
./test_volcano
~~~

This samples every five minutes in the first hour and hourly afterward. The wavelength request selects approximately spaced model bins; read the actual wavelengths in the header. A nonpositive wavelength step writes every internal bin. Set the early step to zero to disable additional early sampling. The main interval can instead use `_s` or `_day`; this also determines the displayed time unit.

The shipped case uses `output_end_day = -1d0` for automatic termination. Auto-end waits until all events finish and an upper bound on current and future particle opacity, including unconverted precursor, is below `tau_floor` at every model wavelength. The end is searched in daily increments. This is a numerical threshold, not an observed impact duration. A case that does not decay needs a fixed end; failure to find one within 100 years raises an error.

To inspect local particle structure, run the viewer from the repository root:

~~~bash
python3 tools/volcano_optical_depth_viewer.py \
  --input build/volcano_ash_profile.dat
~~~

The viewer requires its plotting dependencies, including NumPy and Matplotlib. It offers a browser fallback when Tk is unavailable. Start with absolute local optical-depth values. Its `Column floor` defaults to 10⁻⁶ and is applied before optional normalization; set it to zero to inspect weak tails. A visible nonzero tail is not necessarily a significant aerosol layer.

### 4.3 Configure the full chemistry run

`volcano_run.in` is a Fortran namelist. Retain the group name and closing slash:

~~~fortran
&volcano_run
  spinup_max_years = 40d0
  volcano_duration_day = 730d0
  equilibrium_relative_limit = 1d-4
  equilibrium_mixing_floor = 1d-15
  stable_days_required = 30
/
~~~

The 730-day phase is counted from the start of the volcano clock; event start times and requested history windows must fit inside it. A year means 365 days here. Spin-up can end before its maximum if equilibrium is reached.

`volcano_history.in` separately controls the records:

~~~fortran
&history_output
  species_names = 'SO2'
  species_every_s = 3600d0
  species_end_s = 259200d0
  reaction_ids = 0
  reaction_every_s = 3600d0
  reaction_end_s = 259200d0
  solar_every_s = 21600d0
  solar_end_s = 259200d0
  history_prefix = 'volcano_history'
/
~~~

These settings record SO₂ and every reaction hourly for 72 h, and all-wavelength solar flux every 6 h for 72 h. `reaction_ids = 0` selects all reactions; otherwise supply the desired IDs. Species names must exist in the generated network. Set an interval ≤ 0 to disable that output group. The final requested time is included even when it is not a multiple of the interval. The 72 h output window does not shorten the 730-day model run; extend it to examine slower sulfate evolution. Dense solar histories can become very large.

After regenerating and compiling the case, run from `build/`:

~~~bash
./test
~~~

Archive earlier outputs before rerunning because standard filenames are reused. If the program reports `Volcano forcing was not started`, no new volcanic phase has run; existing history files may belong to an earlier simulation. Generation and compilation alone do not refresh simulation results.

## 5. Output files and interpretation {#outputs}

All filenames below are relative to `build/`. History names use the default prefix; `history_prefix` can change them. Text headers beginning with `#` define fields and units. Read those headers rather than assuming a fixed sampling interval or wavelength order.

### 5.1 Optical pre-run files

| File | Row structure | Meaning |
| --- | --- | --- |
| `volcano_optical_depth.dat` | One time/layer pair, followed by `total_tau_*nm`, `ash_tau_*nm`, and `sulfate_tau_*nm` triplets for selected wavelengths. | Dimensionless cumulative volcanic shielding from the current layer through the top. `total` means ash + formed SSA only; background gas opacity is not included in this file. |
| `volcano_ash_profile.dat` | One time/layer pair with altitude and seven budget fields, listed below. | Local particle budgets and cumulative 550 nm shielding, plus unconverted precursor potential. |

The local-profile columns, in order, are:

~~~text
time  layer  altitude_km
ash_local_tau550  ash_column_tau550
sulfate_local_tau550  sulfate_column_tau550
total_local_tau550  total_column_tau550
sulfate_precursor_potential
~~~

`time` is elapsed time since the first event, in the header's `time_unit`. `*_local_tau550` is a layer-integrated contribution, not number concentration, mass concentration, or extinction per unit length. To obtain an extinction coefficient, divide by that layer's thickness. `*_column_tau550` includes the current and all higher layers. The precursor field is local tau550-equivalent potential, **not opacity**. Neither pre-run file contains a chemical SO₂ concentration or SO₂ source-rate field.

Useful consistency checks are `total_local = ash_local + sulfate_local`, `total_column = ash_column + sulfate_column`, and bottom-layer cumulative optical depth equal to the sum of local layer contributions. With upward layer numbering, consecutive cumulative differences recover local contributions.

### 5.2 Full chemistry histories

| File | Contents and units |
| --- | --- |
| `volcano_history_species.dat` | `time_s`, layer, altitude in km, species name, number density in cm⁻³, and mixing ratio in pptv. Mixing ratio = 10¹² × species density / air density. |
| `volcano_history_reactions.dat` | `time_s`, layer, altitude, reaction ID, coefficient kind (`k` or `J`), coefficient, and instantaneous event rate in cm⁻³ s⁻¹. |
| `volcano_history_reaction_map.dat` | Reaction ID, kind, number of density factors, coefficient units, and reaction text. Use it to interpret IDs and stoichiometry. |
| `volcano_history_solar.dat` | One time/layer row with direct photon spectral flux for every model bin, in photons cm⁻² s⁻¹ nm⁻¹. Includes attenuation from the full optical-depth calculation. |
| `volcano_history_wavelengths.dat` | Bin ID, wavelength in nm, incident spectral flux, and that bin's integration width in nm. Maps the solar columns and supports reconstruction of J. |
| `volcano_initial_state.dat`, `volcano_final_state.dat` | Snapshots of altitude, SO₂ source rate in cm⁻³ s⁻¹, and local/cumulative ash, sulfate, and total tau550. They are diagnostic snapshots, not restart files. |

History `time_s = 0` is the first eruption after spin-up. The header also records its offset on the volcano clock. This distinction matters if `start_day` is not zero. The initial-state snapshot is taken at the beginning of the volcanic phase and can precede a delayed first event.

A photolysis coefficient *J* has units s⁻¹. A general coefficient with *m* density factors has units cm³⁽ᵐ⁻¹⁾ s⁻¹. The reported event rate is the coefficient multiplied by its density factors. It is **not** an interval-integrated reaction flux or a species net loss: use the reaction stoichiometry and sum production/loss pathways to obtain a species budget. Integrate the event rate over time if an accumulated reaction count is required.

The full driver also writes `background_equilibrium_species_number_density.csv` after spin-up and `final_species_number_density.csv` after the volcanic phase, together with ordinary PATMO diagnostics such as rainout outputs. These are separate from the volcanic histories; case-directory `Rainout-*.txt` files are historical diagnostics, not authoritative runtime inputs. An endpoint file cannot replace a time-resolved history.

## 6. Validation and limitations {#validation}

The repository provides checks for optical evolution, transport, radiation, generation, and the history scheduler. After building, the relevant commands are:

~~~bash
# From the repository root
python3 tests/volcano_pinatubo_1991/test_prerun.py
python3 tests/volcano_pinatubo_1991/test_generation.py

# From build/
make check_volcano_transport
make check_volcano_radiation
~~~

Transport checks address conservation, positivity, bottom loss, density-weighted equilibrium, nonuniform layer volumes, diffusion, formation, and grid convergence. Radiation checks address per-bin integration and agreement between J and the diagnostic spectrum. These are numerical checks; they do not demonstrate an equilibrated atmosphere or agreement with observations. Use a separate checkout/build directory if a production run is active.

The present model does not resolve plume buoyancy, entrainment, thermal energy, overshoot, lateral detrainment, evolving size distributions, ash–ice aggregation, altitude-dependent slip correction, or multiple scattering. Fixed extinction efficiency is implicit in transporting optical budgets. The prescribed sulfate potential is not chemically or mass coupled to SO₂. Fixed-background species and the configured chemistry/deposition assumptions also constrain interpretation.

Before drawing chemical conclusions, run a fresh spin-up and volcanic calculation with the current code, check time and grid sensitivity, and compare a matched no-eruption control. Outputs made before the **17 September 2026 spectral-integration correction** should not be presented as results from the corrected photochemistry. This documentation update did not run a new full chemistry simulation.

## 7. References and implementation map {#references}

The guide uses the implementation for algorithmic details and primary literature for selected observational context. The cited studies do not validate the entire parameter set.

- **Guo et al. (2004a).** *Re-evaluation of SO₂ release of the 15 June 1991 Pinatubo eruption using ultraviolet and infrared satellite sensors.* [DOI: 10.1029/2003GC000654](https://doi.org/10.1029/2003GC000654). Basis for the approximate SO₂ loading and removal timescale; the optical formation parameter remains a modeling approximation.
- **Guo et al. (2004b).** *Particles in the great Pinatubo volcanic cloud of June 1991: The role of ice.* [DOI: 10.1029/2003GC000655](https://doi.org/10.1029/2003GC000655). Context for eruption duration and ash/ice evolution; [author-hosted full text](https://pages.mtu.edu/~gbluth/Publications/guo_2004b_g3.pdf). This is a different paper from the SO₂ re-evaluation above.
- **McCormick and Veiga (1992).** *SAGE II measurements of early Pinatubo aerosols.* [DOI: 10.1029/91GL02790](https://doi.org/10.1029/91GL02790). Context for the main 20–25 km aerosol layer; observed material also extended above that interval.

For readers with the PATMO checkout, the source map is:

| Repository path | What to inspect |
| --- | --- |
| `src_f90/patmo_volc.f90` | Event parser, source normalization, optical injection, ascent, settling, losses, spectral opacity, and pre-run writers. |
| `src_f90/patmo_volc_transport.f90` | Finite-volume transport and exact precursor conversion. |
| `src_f90/patmo_photoRates.f90` | Direct spectrum, per-bin wavelength widths, and photolysis integration. |
| `src_f90/patmo.f90`, `src_f90/patmo_ode.f90` | Model-step coupling, opacity updates, and addition of the SO₂ source to the chemical equations. |
| `tests/volcano_pinatubo_1991/test.f90` | Spin-up gate, volcanic run, history configuration, scheduler, and writers. |
| `tests/volcano_pinatubo_1991/test_volcano.f90` | Standalone optical pre-run and its configuration reader. |
| `tests/volcano_pinatubo_1991/README.md` | Case workflow and implementation-specific notes. |
| `tools/volcano_optical_depth_viewer.py` | Interactive inspection and figure/movie export. |

[Back to Work]({{ '/work/' | relative_url }}) · [PATMO course]({{ '/work/patmo/' | relative_url }})

</div>
