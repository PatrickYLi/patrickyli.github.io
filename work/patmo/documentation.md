---
layout: page
title: Documentation
subtitle: A code-based reference guide to the current PATMO repository and runtime workflow.
permalink: /work/patmo/documentation/
body_class: patmo-reading-theme patmo-docs-page
head-extra:
  - patmo-reading-theme.html
description: Traditional documentation for the current PATMO codebase, including workflow, inputs, generated code, runtime behavior, outputs, and repository status notes.
excerpt: A reference page for the current PATMO repository, written from the code as it exists now.
---

<div class="patmo-course patmo-docs">

<section class="panel">
  <div class="lesson-breadcrumb">
    <a href="{{ '/work/patmo/' | relative_url }}">PATMO</a>
    <span>/</span>
    <span>Documentation</span>
  </div>
  <p>
    This page documents the <em>current repository state</em> of <code>PATMO</code>.
    It is written from the code in this repo rather than from an external manual,
    so the emphasis is on what is actually implemented, how the workflow is currently wired,
    and which parts should be treated as stable, legacy, or still evolving.
  </p>
  <div class="doc-callout">
    <p class="doc-callout-label">Current Scope</p>
    <p>
      The most complete and up-to-date workflow in this repository is centered on
      <code>tests/modern_sulfur_cycle/</code>, with <code>tests/archean/</code> following the same newer pattern.
      Some older examples are still useful for orientation, but they do not fully match the latest driver interface.
      This page documents the <code>PATMO</code> core codebase only.
    </p>
  </div>
  <div class="doc-pill-row">
    <span class="doc-pill">1D atmospheric model</span>
    <span class="doc-pill">Python code generation + Fortran runtime</span>
    <span class="doc-pill">Sparse DLSODES solver</span>
    <span class="doc-pill doc-pill-warn">Volcano: under development</span>
  </div>
</section>

<div class="patmo-docs-grid">
  <aside class="panel patmo-docs-nav">
    <p class="section-label">On This Page</p>
    <ol>
      <li><a class="outline-link" href="#overview">Overview</a></li>
      <li><a class="outline-link" href="#quick-start">Quick Start</a></li>
      <li><a class="outline-link" href="#repository-map">Repository Map</a></li>
      <li><a class="outline-link" href="#case-files">Case Files</a></li>
      <li><a class="outline-link" href="#options-reference">Options Reference</a></li>
      <li><a class="outline-link" href="#generation-pipeline">Generation Pipeline</a></li>
      <li><a class="outline-link" href="#runtime-model">Runtime Model</a></li>
      <li><a class="outline-link" href="#driver-routines">Driver Routines</a></li>
      <li><a class="outline-link" href="#outputs">Outputs</a></li>
      <li><a class="outline-link" href="#bundled-cases">Bundled Cases</a></li>
      <li><a class="outline-link" href="#volcano">Volcano</a></li>
      <li><a class="outline-link" href="#notes">Notes</a></li>
    </ol>
  </aside>

  <div class="patmo-docs-body">

    <section id="overview" class="section-block">
      <div class="section-heading">
        <h2>Overview</h2>
      </div>

      <div class="panel">
        <p>
          <code>PATMO</code> is a one-dimensional planetary atmosphere model for coupled
          chemistry, transport, and radiative effects. In the current repository, the central idea is not
          “run one fixed solver with one fixed network,” but instead:
        </p>
        <ol class="doc-numbered-list">
          <li>prepare a case with a reaction network, settings, profiles, fluxes, and a Fortran driver</li>
          <li>use Python to generate case-specific Fortran modules into <code>build/</code></li>
          <li>compile the generated code with the fixed runtime modules and DLSODES solver</li>
          <li>run the resulting executable and inspect the output files written by the driver</li>
        </ol>
        <p>
          In other words, the repository is best understood as a <strong>two-stage system</strong>:
          Python performs the preprocessing and code generation, and Fortran performs the time integration.
        </p>
        <figure class="lecture-figure wide-figure">
          <img
            src="{{ '/assets/img/patmo/patmo_architecture_overview.svg' | relative_url }}"
            alt="Architecture overview of PATMO, from case inputs to generated modules, runtime execution, and outputs."
            loading="lazy">
          <figcaption>
            Figure. The current PATMO workflow from case inputs to generated Fortran modules and runtime outputs.
          </figcaption>
        </figure>
        <div class="doc-card-grid">
          <article class="architecture-node">
            <h4>Preprocessing side</h4>
            <p>
              Reads <code>options.opt</code>, case input files, chemical networks, thermochemistry data,
              and photolysis cross-sections.
            </p>
          </article>
          <article class="architecture-node">
            <h4>Generated build</h4>
            <p>
              Writes case-specific Fortran modules into <code>build/</code>, along with copied solver files,
              copied case files, and photochemistry tables.
            </p>
          </article>
          <article class="architecture-node">
            <h4>Runtime side</h4>
            <p>
              Uses sparse Jacobians and <code>DLSODES</code> to integrate chemistry, diffusion,
              optional process terms, and photochemistry.
            </p>
          </article>
        </div>
      </div>
    </section>

    <section id="quick-start" class="section-block">
      <div class="section-heading">
        <h2>Quick Start</h2>
      </div>

      <div class="panel">
        <p class="section-label">Manual Case Run</p>
        <p>
          If a case already has the required plain-text files in place, the shortest current workflow is:
        </p>
        <pre><code class="language-bash">python3 patmo -test=modern_sulfur_cycle
cd build
make
./test</code></pre>
        <p>
          The first command generates the build directory for the selected case.
          Compilation happens only after that, inside <code>build/</code>.
        </p>
      </div>

      <div class="panel">
        <p class="section-label">Spreadsheet-Assisted Workflow</p>
        <p>
          The repository also contains helper scripts such as <code>compile.sh</code> and the case-specific
          scripts in <code>tests/modern_sulfur_cycle/</code> and <code>tests/archean/</code>.
          In the current code, those scripts can:
        </p>
        <ul>
          <li>convert <code>reaction_network.xlsx</code> into <code>reaction_network.ntw</code></li>
          <li>convert <code>settings.xlsx</code> into <code>options.opt</code></li>
          <li>convert <code>profile.xlsx</code> into <code>profile.dat</code></li>
          <li>interpolate <code>solar_flux.xlsx</code> into <code>solar_flux.txt</code></li>
          <li>validate <code>patmo_dumpDensityToFile</code> calls in <code>test.f90</code></li>
          <li>append default volcano options when a <code>volcano_events.dat</code> file is present</li>
          <li>invoke <code>python3 patmo -test=&lt;case&gt;</code> and enter <code>build/</code></li>
        </ul>
        <pre><code class="language-bash">chmod +x ./compile.sh
./compile.sh
make
./test</code></pre>
        <p class="source-note">
          The helper script does not replace compilation; it prepares the case and build directory, then leaves you in
          <code>build/</code> so you can run <code>make</code> and then <code>./test</code>.
        </p>
      </div>
    </section>

    <section id="repository-map" class="section-block">
      <div class="section-heading">
        <h2>Repository Map</h2>
      </div>

      <div class="panel">
        <table class="patmo-data-table compact-data-table">
          <thead>
            <tr>
              <th>Path</th>
              <th>Role In The Current Workflow</th>
            </tr>
          </thead>
          <tbody>
            <tr>
              <td><code>patmo</code></td>
              <td>Main Python entry point. Generates case-specific build files for <code>-test=&lt;case&gt;</code>.</td>
            </tr>
            <tr>
              <td><code>src_py/</code></td>
              <td>Python preprocessing, network loading, option parsing, photochemistry preparation, Jacobian and ODE code generation.</td>
            </tr>
            <tr>
              <td><code>src_f90/</code></td>
              <td>Fortran templates and fixed runtime modules that are copied or instantiated into <code>build/</code>.</td>
            </tr>
            <tr>
              <td><code>solvers/</code></td>
              <td>Legacy Fortran solver sources used for the final executable, including <code>DLSODES</code> support.</td>
            </tr>
            <tr>
              <td><code>data/</code></td>
              <td>Reference datasets: KIDA chemistry, thermochemistry, and SWRI photolysis cross-section files.</td>
            </tr>
            <tr>
              <td><code>tests/</code></td>
              <td>Case directories. Each case provides its own driver program and input files.</td>
            </tr>
            <tr>
              <td><code>build/</code></td>
              <td>Generated working directory for the selected case. This is the directory you compile and run.</td>
            </tr>
            <tr>
              <td><code>tools/</code></td>
              <td>Small auxiliary tools, such as helper interpolation scripts.</td>
            </tr>
          </tbody>
        </table>
      </div>
    </section>

    <section id="case-files" class="section-block">
      <div class="section-heading">
        <h2>Case Files</h2>
      </div>

      <div class="panel">
        <p>
          A runnable PATMO case lives under <code>tests/&lt;case&gt;/</code>. The current code does not treat the case
          directory as passive data only. It treats it as the main scientific setup for the run, including
          the input files and the driver program that decides what to load, what to set, and what to dump.
        </p>
        <table class="patmo-data-table">
          <thead>
            <tr>
              <th>File</th>
              <th>Status</th>
              <th>What It Does</th>
            </tr>
          </thead>
          <tbody>
            <tr>
              <td><code>copylist.pcp</code></td>
              <td>Required</td>
              <td>Lists which case files are copied into <code>build/</code> before generation and compilation.</td>
            </tr>
            <tr>
              <td><code>options.opt</code></td>
              <td>Required</td>
              <td>Stores grid, photochemistry, and process switches that the Python side reads before code generation.</td>
            </tr>
            <tr>
              <td><code>reaction_network.ntw</code></td>
              <td>Usually required</td>
              <td>Explicit reaction network in PATMO text format. Often generated from <code>reaction_network.xlsx</code>.</td>
            </tr>
            <tr>
              <td><code>test.f90</code></td>
              <td>Required</td>
              <td>Case-specific driver. Calls the public PATMO routines, sets the experiment, and decides which outputs to write.</td>
            </tr>
            <tr>
              <td><code>profile.dat</code></td>
              <td>Case dependent</td>
              <td>Initial atmospheric profile loaded by <code>patmo_loadInitialProfile()</code>.</td>
            </tr>
            <tr>
              <td><code>solar_flux.txt</code></td>
              <td>Required when photochemistry is used</td>
              <td>Top-of-atmosphere photon flux sampled on the active wavelength grid.</td>
            </tr>
            <tr>
              <td>Auxiliary text tables</td>
              <td>Optional</td>
              <td>Case-specific files for rainout, aerosol, condensation, deposition, or other custom source/sink logic used by <code>test.f90</code>.</td>
            </tr>
            <tr>
              <td><code>volcano_events.dat</code></td>
              <td>Optional</td>
              <td>Current experimental event file for volcanic gas and ash forcing. This feature should be treated as under development.</td>
            </tr>
          </tbody>
        </table>
      </div>

      <div class="panel">
        <p class="section-label">Network File Format</p>
        <p>
          The explicit network file is a structured comma-separated text format.
          The current parser in <code>src_py/patmo_network.py</code> recognizes:
        </p>
        <ul>
          <li><code>#@var:T=Tgas</code> style variable aliases</li>
          <li><code>@format:</code> lines that define how each reaction row is parsed</li>
          <li><code>@ghost:</code> lines for additional ghost species</li>
          <li>reaction rows containing indexed reactants, products, and one rate expression</li>
        </ul>
        <pre><code class="language-text">#@var:T=Tgas
@format:idx,R,R,R,P,P,P,rate
1,COS,OH,,CO2,SH,,1.1E-13*exp(-1200/T)
2,COS,O,,CO,SO,,2.1E-11*exp(-2200/T)</code></pre>
        <p class="source-note">
          If <code>options.network</code> is left empty and <code>options.species</code> is provided,
          the Python side can also construct a reduced network directly from the KIDA database.
        </p>
      </div>
    </section>

    <section id="options-reference" class="section-block">
      <div class="section-heading">
        <h2>Options Reference</h2>
      </div>

      <div class="panel">
        <p>
          The current option parser lives in <code>src_py/patmo_options.py</code>.
          It supports integers, floats, booleans, comma-separated lists, and <code>key:value</code> dictionaries.
          Boolean flags are read from <code>T</code> and <code>F</code>.
        </p>
      </div>

      <div class="panel">
        <p class="section-label">Core Setup</p>
        <table class="patmo-data-table settings-detail-table">
          <thead>
            <tr>
              <th>Key</th>
              <th>Meaning</th>
              <th>Current Behavior</th>
            </tr>
          </thead>
          <tbody>
            <tr>
              <td><code>network</code></td>
              <td>Path to the explicit reaction network file.</td>
              <td>If set, <code>loadNetwork()</code> reads that file directly.</td>
            </tr>
            <tr>
              <td><code>species</code></td>
              <td>Comma-separated species list.</td>
              <td>If used with an empty <code>network</code>, KIDA reactions are filtered to this species set.</td>
            </tr>
            <tr>
              <td><code>cellsNumber</code></td>
              <td>Number of vertical layers.</td>
              <td>Copied into generated Fortran commons and used throughout the grid arrays.</td>
            </tr>
            <tr>
              <td><code>cellThickness</code></td>
              <td>Nominal cell thickness.</td>
              <td>Used in generated dry deposition terms and related source/sink scaling.</td>
            </tr>
            <tr>
              <td><code>photoBinsNumber</code></td>
              <td>Number of photochemistry bins.</td>
              <td>Defines the size of <code>photoMetric.dat</code> and photolysis-related arrays.</td>
            </tr>
            <tr>
              <td><code>wavelengMin</code>, <code>wavelengMax</code></td>
              <td>Wavelength range in nm.</td>
              <td>The current photochemistry pipeline uses these values to derive the active energy grid.</td>
            </tr>
            <tr>
              <td><code>energyMin</code>, <code>energyMax</code></td>
              <td>Legacy energy-range fields.</td>
              <td>Still parsed, but the current photochemistry builder is driven primarily by wavelength limits when photochemistry is enabled.</td>
            </tr>
            <tr>
              <td><code>zenith_angle</code></td>
              <td>Solar zenith angle in degrees.</td>
              <td>Converted to <code>mu = cos(theta)</code> inside generated photo-rate code.</td>
            </tr>
            <tr>
              <td><code>TOA_para</code></td>
              <td>Top-of-atmosphere scaling coefficient.</td>
              <td>Written into the generated photo-rate module as a multiplicative factor.</td>
            </tr>
            <tr>
              <td><code>plotRates</code></td>
              <td>Whether rate plotting should be enabled.</td>
              <td>Currently used as a light build-time flag; plotting support depends on <code>matplotlib</code>.</td>
            </tr>
            <tr>
              <td><code>useEntropyProduction</code></td>
              <td>Entropy-production control flag.</td>
              <td>Affects reverse-reaction handling and whether entropy production is part of the intended workflow.</td>
            </tr>
          </tbody>
        </table>
      </div>

      <div class="panel">
        <p class="section-label">Process Switches And Boundary Terms</p>
        <table class="patmo-data-table settings-detail-table">
          <thead>
            <tr>
              <th>Key</th>
              <th>Meaning</th>
              <th>Current Behavior</th>
            </tr>
          </thead>
          <tbody>
            <tr>
              <td><code>usePhotochemistry</code></td>
              <td>Enable the photolysis preprocessing and runtime photo-rate path.</td>
              <td>Controls cross-section loading, photo metric creation, and photolysis rate evaluation.</td>
            </tr>
            <tr>
              <td><code>useHescape</code></td>
              <td>Enable hydrogen escape terms.</td>
              <td>Turns on conditional blocks in the generated ODE and main runtime module.</td>
            </tr>
            <tr>
              <td><code>useWaterRemoval</code></td>
              <td>Enable water condensation/removal logic.</td>
              <td>Activates conditional removal terms in <code>patmo_ode</code>.</td>
            </tr>
            <tr>
              <td><code>useAerosolformation</code></td>
              <td>Enable aerosol conversion terms.</td>
              <td>Activates the current sulfuric-acid-to-aerosol conversion block in the generated ODE.</td>
            </tr>
            <tr>
              <td><code>constant_species</code></td>
              <td>Comma-separated list of species that should remain fixed.</td>
              <td>Generated as <code>dn = 0</code> constraints for those species.</td>
            </tr>
            <tr>
              <td><code>gravity_species</code></td>
              <td>Dictionary of species to settling expressions.</td>
              <td>Injected into generated ODE code as gravity-settling source/sink terms.</td>
            </tr>
            <tr>
              <td><code>drydep_species</code></td>
              <td>Dictionary of surface dry deposition rates.</td>
              <td>Applied in the bottom layer through generated dry-deposition code.</td>
            </tr>
            <tr>
              <td><code>emission_species</code></td>
              <td>Dictionary of lower-boundary emission terms.</td>
              <td>Added as explicit source terms in the first atmospheric layer.</td>
            </tr>
            <tr>
              <td><code>useVolcano</code></td>
              <td>Enable volcanic forcing.</td>
              <td>Turns on the experimental <code>patmo_volcano</code> integration path.</td>
            </tr>
            <tr>
              <td><code>volcanoFile</code></td>
              <td>Volcanic event file name.</td>
              <td>Passed directly into <code>patmo_volcano_init()</code>.</td>
            </tr>
            <tr>
              <td><code>volcanoAshSettling</code></td>
              <td>Ash settling coefficient.</td>
              <td>Used in the ash transport/settling update inside <code>patmo_volcano</code>.</td>
            </tr>
            <tr>
              <td><code>volcanoAshDecay</code></td>
              <td>Ash decay coefficient.</td>
              <td>Used in the ash opacity decay update inside <code>patmo_volcano</code>.</td>
            </tr>
          </tbody>
        </table>
      </div>
    </section>

    <section id="generation-pipeline" class="section-block">
      <div class="section-heading">
        <h2>Generation Pipeline</h2>
      </div>

      <div class="panel">
        <p>
          The current main orchestration logic is in the repository root file <code>patmo</code>.
          That script performs the following build sequence for <code>-test=&lt;case&gt;</code>:
        </p>
        <div class="architecture-map">
          <div class="architecture-stage stage-python">
            <h3>Python Build Steps</h3>
            <div class="architecture-runtime-flow">
              <article class="architecture-node">
                <h4>1. Copy case files</h4>
                <p><code>patmo_test.buildTest()</code> reads <code>copylist.pcp</code> and copies the selected case files into <code>build/</code>.</p>
              </article>
              <div class="architecture-arrow-inline">→</div>
              <article class="architecture-node">
                <h4>2. Read options</h4>
                <p><code>patmo_options.options()</code> parses the run configuration and process flags.</p>
              </article>
              <div class="architecture-arrow-inline">→</div>
              <article class="architecture-node">
                <h4>3. Build the network</h4>
                <p><code>patmo_network</code> loads the explicit network file and any KIDA-derived chemistry selected by the options.</p>
              </article>
              <div class="architecture-arrow-inline">→</div>
              <article class="architecture-node">
                <h4>4. Prepare photochemistry</h4>
                <p><code>patmo_photochemistry</code> loads cross-sections, defines the logarithmic metric, and writes interpolated files.</p>
              </article>
              <div class="architecture-arrow-inline">→</div>
              <article class="architecture-node">
                <h4>5. Generate Fortran</h4>
                <p>Utility, rate, reverse-rate, ODE, commons, sparsity, Jacobian, and main modules are generated into <code>build/</code>.</p>
              </article>
            </div>
          </div>
        </div>
      </div>

      <div class="panel">
        <p class="section-label">Generated And Copied Build Files</p>
        <table class="patmo-data-table compact-data-table">
          <thead>
            <tr>
              <th>Category</th>
              <th>Files</th>
            </tr>
          </thead>
          <tbody>
            <tr>
              <td>Generated Fortran modules</td>
              <td><code>patmo.f90</code>, <code>patmo_ode.f90</code>, <code>patmo_commons.f90</code>, <code>patmo_rates.f90</code>, <code>patmo_photo.f90</code>, <code>patmo_photoRates.f90</code>, <code>patmo_reverseRates.f90</code>, <code>patmo_jacobian.f90</code>, <code>patmo_sparsity.f90</code>, <code>patmo_utils.f90</code></td>
            </tr>
            <tr>
              <td>Copied fixed runtime files</td>
              <td><code>Makefile</code>, <code>patmo_constants.f90</code>, <code>patmo_parameters.f90</code>, <code>patmo_volcano.f90</code></td>
            </tr>
            <tr>
              <td>Copied solver files</td>
              <td><code>opkda1.f</code>, <code>opkda2.f</code>, <code>opkdmain.f</code></td>
            </tr>
            <tr>
              <td>Generated chemistry diagnostics</td>
              <td><code>reactionsVerbatim.dat</code>, <code>xsecs/photoMetric.dat</code>, <code>xsecs/*.dat</code></td>
            </tr>
            <tr>
              <td>Copied case data</td>
              <td>The files listed in <code>copylist.pcp</code>, including the case-specific <code>test.f90</code>.</td>
            </tr>
          </tbody>
        </table>
      </div>
    </section>

    <section id="runtime-model" class="section-block">
      <div class="section-heading">
        <h2>Runtime Model</h2>
      </div>

      <div class="panel">
        <p>
          After compilation, the runtime behavior is centered on the generated <code>patmo</code> module and the
          case-specific <code>test.f90</code> driver.
        </p>
        <div class="doc-card-grid">
          <article class="architecture-node">
            <h4><code>patmo_init()</code></h4>
            <p>
              Loads photochemistry metrics and cross-sections when enabled, zeroes rates and cumulative arrays,
              loads verbatim reaction names, and initializes volcanic state when the volcano path is active.
            </p>
          </article>
          <article class="architecture-node">
            <h4><code>patmo_run(dt, convergence)</code></h4>
            <p>
              Computes optical depth, rates, reverse rates, and sparse structure if needed, then advances the
              full 1D state using <code>DLSODES</code>.
            </p>
          </article>
          <article class="architecture-node">
            <h4><code>patmo_ode:fex</code></h4>
            <p>
              Builds the right-hand side from chemistry, diffusion, eddy transport, wet deposition,
              and any enabled optional source/sink terms.
            </p>
          </article>
          <article class="architecture-node">
            <h4>Jacobian path</h4>
            <p>
              The generated <code>patmo_jacobian</code> and <code>patmo_sparsity</code> modules provide the sparse structure
              required by the solver.
            </p>
          </article>
        </div>
      </div>

      <div class="panel">
        <p class="section-label">Photochemistry Path</p>
        <p>
          The current photolysis workflow is split across Python preprocessing and Fortran runtime:
        </p>
        <ul>
          <li><code>src_py/patmo_photochemistry.py</code> finds SWRI cross-sections and interpolates them onto the active log-energy metric</li>
          <li>the interpolated cross-section tables needed by the runtime are written into <code>build/xsecs/</code></li>
          <li><code>build/xsecs/photoMetric.dat</code> and <code>build/xsecs/*.dat</code> are written for the runtime side</li>
          <li><code>patmo_photo</code> loads those files at initialization time</li>
          <li><code>patmo_photoRates</code> integrates cross-sections against the current optical depth field and flux scaling</li>
        </ul>
      </div>

      <div class="panel">
        <p class="section-label">Process Terms In The Current ODE</p>
        <p>
          The generated ODE system contains the reaction network terms and then layers additional process terms on top.
          In the current source, those include:
        </p>
        <ul>
          <li>molecular and eddy diffusion between neighboring cells</li>
          <li>constant-species constraints</li>
          <li>gravity settling for configured species</li>
          <li>dry deposition in the bottom layer</li>
          <li>lower-boundary emissions</li>
          <li>wet deposition</li>
          <li>optional water removal</li>
          <li>optional aerosol formation</li>
          <li>optional hydrogen escape</li>
          <li>optional volcanic gas and ash forcing</li>
        </ul>
      </div>
    </section>

    <section id="driver-routines" class="section-block">
      <div class="section-heading">
        <h2>Driver Routines</h2>
      </div>

      <div class="panel">
        <p>
          The public routines currently exposed by <code>src_f90/patmo.f90</code> are the routines a case driver usually calls.
          The table below focuses on the ones that appear most often in the bundled cases.
        </p>
        <table class="patmo-data-table">
          <thead>
            <tr>
              <th>Routine</th>
              <th>Typical Use</th>
            </tr>
          </thead>
          <tbody>
            <tr>
              <td><code>patmo_init()</code></td>
              <td>Initialize the generated runtime, photochemistry tables, and reaction metadata.</td>
            </tr>
            <tr>
              <td><code>patmo_loadInitialProfile()</code></td>
              <td>Load the initial vertical profile from <code>profile.dat</code> with unit conversion.</td>
            </tr>
            <tr>
              <td><code>patmo_setFluxBB()</code></td>
              <td>Set a blackbody stellar flux field. Used by several bundled examples.</td>
            </tr>
            <tr>
              <td><code>patmo_setGravity()</code></td>
              <td>Set the gravitational acceleration used by transport terms.</td>
            </tr>
            <tr>
              <td><code>patmo_hydrostaticEquilibrium()</code></td>
              <td>Construct or update the hydrostatic structure from a chosen ground pressure.</td>
            </tr>
            <tr>
              <td><code>patmo_setDiffusionDzzAll()</code>, <code>patmo_setEddyKzzAll()</code></td>
              <td>Set layer-wide transport coefficients.</td>
            </tr>
            <tr>
              <td><code>patmo_setChemistryAll()</code>, <code>patmo_setTgasAll()</code></td>
              <td>Directly set species abundances or temperatures for every cell.</td>
            </tr>
            <tr>
              <td><code>patmo_run(dt, convergence)</code></td>
              <td>Advance the model by one time step and return the current convergence estimate.</td>
            </tr>
            <tr>
              <td><code>patmo_dumpHydrostaticProfile()</code>, <code>patmo_dumpOpacity()</code>, <code>patmo_dumpJValue()</code>, <code>patmo_dumpAllRates()</code></td>
              <td>Write standard diagnostic files after or during a run.</td>
            </tr>
            <tr>
              <td><code>patmo_dumpDensityToFile()</code>, <code>patmo_dumpMixingRatioToFile()</code>, <code>patmo_dumpAllMixingRatioToFile()</code></td>
              <td>Write species-specific or full-column output files.</td>
            </tr>
            <tr>
              <td><code>patmo_getTotalMass()</code>, <code>patmo_getEntropyProduction()</code></td>
              <td>Return scalar diagnostics that can be used inside a custom driver loop.</td>
            </tr>
          </tbody>
        </table>
        <p class="source-note">
          Important current-state note: the main source file now defines <code>patmo_run(dt, convergence)</code>.
          The <code>hello</code>, <code>benchmark</code>, and <code>entropy</code> examples still call an older one-argument form,
          so they should be read as legacy examples rather than as the most current driver template.
        </p>
      </div>
    </section>

    <section id="outputs" class="section-block">
      <div class="section-heading">
        <h2>Outputs</h2>
      </div>

      <div class="panel">
        <p>
          Output behavior is split between fixed PATMO dump routines and whatever the case-specific driver writes.
          The most common current output files are:
        </p>
        <table class="patmo-data-table compact-data-table">
          <thead>
            <tr>
              <th>File</th>
              <th>Where It Comes From</th>
              <th>Meaning</th>
            </tr>
          </thead>
          <tbody>
            <tr>
              <td><code>hydrostat.out</code></td>
              <td>Driver call</td>
              <td>Hydrostatic structure near the start of the run.</td>
            </tr>
            <tr>
              <td><code>hydrostatEnd.out</code></td>
              <td>Driver call</td>
              <td>Hydrostatic structure at the end of the run.</td>
            </tr>
            <tr>
              <td><code>allNDs.dat</code></td>
              <td><code>patmo_dumpAllMixingRatioToFile()</code></td>
              <td>All-species column output written at the end of a run.</td>
            </tr>
            <tr>
              <td><code>opacity.dat</code></td>
              <td><code>patmo_dumpOpacity()</code></td>
              <td>Current wavelength- or energy-resolved opacity diagnostic.</td>
            </tr>
            <tr>
              <td><code>jvalue.dat</code></td>
              <td><code>patmo_dumpJValue()</code></td>
              <td>Photolysis rate diagnostic for the active photo reactions.</td>
            </tr>
            <tr>
              <td><code>rates.dat</code></td>
              <td><code>patmo_dumpAllRates()</code></td>
              <td>All reaction rates for the generated forward, photochemical, and reverse network.</td>
            </tr>
            <tr>
              <td>Species-specific dump files</td>
              <td><code>patmo_dumpDensityToFile()</code> or <code>patmo_dumpMixingRatioToFile()</code></td>
              <td>Case-selected diagnostic outputs for individual species.</td>
            </tr>
            <tr>
              <td><code>build/reactionsVerbatim.dat</code></td>
              <td>Python preprocessing</td>
              <td>Human-readable reaction list corresponding to the generated network.</td>
            </tr>
          </tbody>
        </table>
      </div>
    </section>

    <section id="bundled-cases" class="section-block">
      <div class="section-heading">
        <h2>Bundled Cases</h2>
      </div>

      <div class="panel">
        <p>
          The repository currently contains five bundled case folders. They are not all at the same maturity level.
        </p>
        <table class="patmo-data-table">
          <thead>
            <tr>
              <th>Case</th>
              <th>Role</th>
              <th>Current Status In This Repo</th>
            </tr>
          </thead>
          <tbody>
            <tr>
              <td><code>tests/modern_sulfur_cycle/</code></td>
              <td>Main teaching and reference case.</td>
              <td>Most complete current workflow. Matches the newer helper-script and two-argument driver style.</td>
            </tr>
            <tr>
              <td><code>tests/archean/</code></td>
              <td>Extended sulfur/archean scenario.</td>
              <td>Also uses the newer workflow structure and the current <code>patmo_run(dt, convergence)</code> signature.</td>
            </tr>
            <tr>
              <td><code>tests/hello/</code></td>
              <td>Minimal orientation example.</td>
              <td>Useful for reading, but it still uses the older one-argument <code>patmo_run(dt)</code> form and energy-range options.</td>
            </tr>
            <tr>
              <td><code>tests/benchmark/</code></td>
              <td>Longer legacy benchmark case.</td>
              <td>Legacy-style driver; use it carefully as a reference, not as the primary current template.</td>
            </tr>
            <tr>
              <td><code>tests/entropy/</code></td>
              <td>Entropy-production example.</td>
              <td>Legacy-style driver; still useful conceptually, but not the cleanest match to the current runtime interface.</td>
            </tr>
          </tbody>
        </table>
      </div>
    </section>

    <section id="volcano" class="section-block">
      <div class="section-heading">
        <h2>Volcano</h2>
      </div>

      <div class="panel doc-under-development">
        <p class="doc-callout-label">Under Development</p>
        <p>
          Volcano-related functionality exists in the current codebase, but it should be treated as an
          <strong>under-development path</strong> rather than as a fully settled public workflow.
          The repository currently exposes a <code>patmo_volcano.f90</code> module, option flags,
          and example event files, but this documentation intentionally presents them as experimental.
        </p>
      </div>

      <div class="panel">
        <p class="section-label">What The Current Code Already Does</p>
        <ul>
          <li>reads <code>volcano_events.dat</code> at initialization time when <code>useVolcano = T</code></li>
          <li>supports <code>GAS</code> and <code>ASH</code> event rows</li>
          <li>maps event altitude ranges from km to the existing model grid</li>
          <li>adds volcanic gas source terms inside the ODE right-hand side</li>
          <li>adds gray ash opacity to the optical-depth field</li>
          <li>updates ash by source input, decay, and simple settling after each step</li>
        </ul>
      </div>

      <div class="panel">
        <p class="section-label">Current Event File Shape</p>
        <pre><code class="language-text"># kind species t_start_s t_end_s zmin_km zmax_km source_rate
GAS SO2 0.0 5.1840e6 12.0 25.0 2.50e3
GAS H2S 0.0 5.1840e6 10.0 22.0 3.00e2
ASH ASH 0.0 1.2960e7 14.0 30.0 2.00e-8</code></pre>
        <p class="source-note">
          This format reflects the current source code and the example file shipped in
          <code>tests/modern_sulfur_cycle/volcano_events.dat</code>. It should not yet be treated as a frozen interface.
        </p>
      </div>
    </section>

    <section id="notes" class="section-block">
      <div class="section-heading">
        <h2>Notes</h2>
      </div>

      <div class="panel">
        <ul>
          <li><code>build/</code> is a generated workspace. It is not the source of truth; the source of truth is the case directory plus the Python and Fortran templates.</li>
          <li>The repository mixes current workflows with older examples. This page prioritizes the currently consistent path rather than flattening those differences away.</li>
          <li>When photochemistry is enabled, wavelength-based configuration is the more current path in this repository.</li>
          <li>The external wiki can still be useful, but this page is meant to describe the code snapshot currently present in this repo.</li>
        </ul>
      </div>
    </section>

  </div>
</div>

</div>
