---
layout: page
title: Lecture 1: Understanding PATMO
subtitle: PATMO as the central model of this course and the modern sulfur cycle as the training case.
permalink: /work/patmo/lecture-01/
body_class: patmo-reading-theme
head-extra:
  - patmo-reading-theme.html
description: Lecture 1 of the PATMO course, introducing PATMO, its scope, and the role of the modern sulfur cycle.
---

<div class="patmo-course">

<section class="panel">
  <div class="lesson-breadcrumb">
    <a href="{{ '/work/patmo/' | relative_url }}">PATMO Course</a>
    <span>/</span>
    <span>Lecture 1 of 7</span>
  </div>
  <p>
    This lecture introduces <code>PATMO</code> as the main model tool of this course.
    The goal is to understand what kind of model it is, what kinds of questions it can help you explore,
    and why the <code>modern sulfur cycle</code> is used as the main training case throughout the semester.
  </p>
  <div id="main-point" class="takeaway-box">
    <p>
      <strong>Main Point.</strong>
      <code>PATMO</code> is a one-dimensional atmospheric photochemistry model used for controlled and interpretable computational experiments.
    </p>
  </div>
  <p>
    If you finish this page and can clearly answer the three questions in the check section at the end,
    then you have understood the main point of Lecture 1.
  </p>

  <div class="panel">
    <p>After reading this page, you should be able to:</p>
    <ul>
      <li>identify what kind of model <code>PATMO</code> is</li>
      <li>recognize the key files in <code>tests/modern_sulfur_cycle/</code> and what each one does</li>
      <li>understand how the <code>modern sulfur cycle</code> becomes a runnable <code>PATMO</code> case</li>
      <li>know which output files to inspect after a run</li>
    </ul>
  </div>
</section>

<section id="on-this-page" class="section-block">
  <div class="section-heading">
    <h2>On This Page</h2>
  </div>

  <ol>
    <li><a class="outline-link" href="#patmo-as-a-model">PATMO as a model</a></li>
    <li><a class="outline-link" href="#simplified-sulfur-cycle">A simplified modern sulfur cycle</a></li>
    <li><a class="outline-link" href="#working-with-patmo">Modern sulfur cycle input files and initial conditions</a></li>
    <li><a class="outline-link" href="#case-study">How PATMO uses these files in code</a></li>
    <li><a class="outline-link" href="#research-workflow">What PATMO produces after a run</a></li>
    <li><a class="outline-link" href="#check-yourself">Check yourself</a></li>
  </ol>
</section>

<section id="patmo-as-a-model" class="section-block">
  <div class="section-heading">
    <h2>PATMO as a Model</h2>
  </div>

  <div class="panel">
    <p>
      At the most basic level, <code>PATMO</code> is a numerical model for atmospheric chemistry and related physical processes.
      It is a structured way to describe a scientific setup and compute how that setup evolves under clearly stated assumptions.
    </p>
    <figure class="lecture-figure">
      <img src="{{ '/assets/img/patmo/PATMO_1D.png' | relative_url }}" alt="PATMO architecture for the modern sulfur cycle case shown as a one-dimensional atmospheric column with solar flux, chemistry, transport, source, and sink processes.">
      <figcaption>Figure. Architecture of <code>PATMO</code> in the <code>modern sulfur cycle</code> case.</figcaption>
    </figure>
    <p>
      In this course, the word <em>model</em> does not mean “automatic truth.”
      It means a simplified and computable representation of reality that helps you run repeatable and comparable experiments.
    </p>
    <p>
      If you want a short external overview of common atmospheric model families,
      see <a href="https://ac2.iqfr.csic.es/en/research/climate-modelling" target="_blank" rel="noopener">this climate modelling overview from AC2</a>.
    </p>
  </div>

  <div class="model-note-stack">
    <article class="entry-card">
      <p class="card-label">One-Dimensional</p>
      <p>
        When we say that <code>PATMO</code> is one-dimensional, we mean that it focuses on the vertical direction.
        Instead of representing the entire atmosphere in longitude, latitude, and height,
        it treats the atmosphere as a single vertical column divided into layers.
      </p>
      <div class="equation-flow">
        <div class="equation-step">
          <p class="equation-label">Continuous Vertical Form</p>
          <div class="equation">
            <math display="block">
              <mfrac>
                <mrow>
                  <mo>&#x2202;</mo>
                  <msub><mi>n</mi><mi>i</mi></msub>
                  <mo>(</mo><mi>z</mi><mo>,</mo><mi>t</mi><mo>)</mo>
                </mrow>
                <mrow><mo>&#x2202;</mo><mi>t</mi></mrow>
              </mfrac>
              <mo>=</mo>
              <msub><mi>P</mi><mi>i</mi></msub>
              <mo>(</mo><mi>z</mi><mo>,</mo><mi>t</mi><mo>)</mo>
              <mo>&#x2212;</mo>
              <msub><mi>L</mi><mi>i</mi></msub>
              <mo>(</mo><mi>z</mi><mo>,</mo><mi>t</mi><mo>)</mo>
              <mo>&#x2212;</mo>
              <mfrac>
                <mrow>
                  <mo>&#x2202;</mo>
                  <msub><mi>&#x03A6;</mi><mi>i</mi></msub>
                  <mo>(</mo><mi>z</mi><mo>,</mo><mi>t</mi><mo>)</mo>
                </mrow>
                <mrow><mo>&#x2202;</mo><mi>z</mi></mrow>
              </mfrac>
            </math>
          </div>
          <p class="equation-note">
            In a 1D atmospheric column, the abundance of species <em>i</em> changes because of local production,
            local loss, and the vertical divergence of flux.
          </p>
        </div>

        <div class="equation-step">
          <p class="equation-label">Layer-by-Layer Discretization</p>
          <div class="equation">
            <math display="block">
              <mfrac>
                <mrow>
                  <mi>d</mi>
                  <msub>
                    <mi>n</mi>
                    <mrow><mi>i</mi><mo>,</mo><mi>k</mi></mrow>
                  </msub>
                </mrow>
                <mrow><mi>d</mi><mi>t</mi></mrow>
              </mfrac>
              <mo>=</mo>
              <msub><mi>P</mi><mrow><mi>i</mi><mo>,</mo><mi>k</mi></mrow></msub>
              <mo>&#x2212;</mo>
              <msub><mi>L</mi><mrow><mi>i</mi><mo>,</mo><mi>k</mi></mrow></msub>
              <mo>&#x2212;</mo>
              <mfrac>
                <mrow>
                  <msub>
                    <mi>&#x03A6;</mi>
                    <mrow><mi>i</mi><mo>,</mo><mi>k</mi><mo>+</mo><mn>1</mn><mo>/</mo><mn>2</mn></mrow>
                  </msub>
                  <mo>&#x2212;</mo>
                  <msub>
                    <mi>&#x03A6;</mi>
                    <mrow><mi>i</mi><mo>,</mo><mi>k</mi><mo>&#x2212;</mo><mn>1</mn><mo>/</mo><mn>2</mn></mrow>
                  </msub>
                </mrow>
                <mrow><mo>&#x0394;</mo><msub><mi>z</mi><mi>k</mi></msub></mrow>
              </mfrac>
            </math>
          </div>
          <p class="equation-note">
            Here, <em>k</em> = 1, 2, ..., <em>N</em> labels the vertical layers.
            This is the key idea behind a 1D model:
            the atmosphere is represented as a stack of coupled layers,
            and each layer exchanges material with the layers above and below.
          </p>
        </div>
      </div>
      <ul>
        <li>temperature can vary by layer</li>
        <li>density can vary by layer</li>
        <li>species abundances can vary by layer</li>
        <li>radiation conditions can vary by layer</li>
      </ul>
      <p>
        This is why the output of a 1D model is often a profile rather than a single number.
        The model is solving the atmosphere one layer at a time, while still allowing neighboring layers to interact through transport.
      </p>
      <p class="source-note">
        <strong>Textbook note.</strong>
        The use of altitude as a vertical coordinate and the species continuity equation as the basis for layer-by-layer atmospheric modeling
        are discussed in Jacobson, <em>Fundamentals of Atmospheric Modeling, Second Edition</em>,
        Section 5.2 “Altitude coordinate” (p. 143) and Section 7.3 “The species continuity equation” (p. 211).
      </p>
    </article>

    <article class="entry-card">
      <p class="card-label">Photochemistry</p>
      <p>
        When we say that <code>PATMO</code> is a photochemistry model,
        we mean that radiation directly affects chemistry.
        Some molecules absorb photons, change chemically, and then alter the rest of the reaction network.
      </p>
      <p>
        So radiation is not just a background setting in <code>PATMO</code>;
        it is one of the active drivers of chemical evolution.
      </p>
      <div class="equation-flow">
        <div class="equation-step">
          <p class="equation-label">1. Opacity</p>
          <div class="equation">
            <math display="block">
              <msub><mi>&#x03C4;</mi><mi>&#x03BB;</mi></msub>
              <mo>(</mo><mi>z</mi><mo>)</mo>
              <mo>=</mo>
              <msubsup><mo>&#x222B;</mo><mi>z</mi><mi>&#x221E;</mi></msubsup>
              <munder><mo>&#x2211;</mo><mi>i</mi></munder>
              <msub><mi>n</mi><mi>i</mi></msub>
              <mo>(</mo><mrow><mi>z</mi><mo>&#x2032;</mo></mrow><mo>)</mo>
              <msub><mi>&#x03C3;</mi><mrow><mi>i</mi><mo>,</mo><mi>&#x03BB;</mi></mrow></msub>
              <mo>(</mo><mrow><mi>z</mi><mo>&#x2032;</mo></mrow><mo>)</mo>
              <mi>d</mi><mrow><mi>z</mi><mo>&#x2032;</mo></mrow>
            </math>
          </div>
          <p class="equation-note">
            This is the wavelength-dependent optical depth above altitude <em>z</em>.
            It measures how much absorbing material lies overhead and how strongly that material interacts with radiation at wavelength <em>&#x03BB;</em>.
          </p>
        </div>

        <div class="equation-step">
          <p class="equation-label">2. Solar Flux in Each Layer</p>
          <div class="equation">
            <math display="block">
              <msub><mi>I</mi><mi>&#x03BB;</mi></msub>
              <mo>(</mo><mi>z</mi><mo>)</mo>
              <mo>=</mo>
              <msub><mi>I</mi><mi>&#x03BB;</mi></msub>
              <mo>(</mo><mi>&#x221E;</mi><mo>)</mo>
              <msup>
                <mi>e</mi>
                <mrow>
                  <mo>&#x2212;</mo>
                  <mfrac>
                    <mrow>
                      <msub><mi>&#x03C4;</mi><mi>&#x03BB;</mi></msub>
                      <mo>(</mo><mi>z</mi><mo>)</mo>
                    </mrow>
                    <mrow><mi>cos</mi><mi>&#x03B8;</mi></mrow>
                  </mfrac>
                </mrow>
              </msup>
            </math>
          </div>
          <p class="equation-note">
            Once the optical depth is known, the incoming solar radiation is attenuated as it travels downward.
            Larger optical depth means less radiation survives to a given layer.
            The angle <em>&#x03B8;</em> is the solar zenith angle.
          </p>
        </div>

        <div class="equation-step">
          <p class="equation-label">3. Photochemical Rate Constant</p>
          <div class="equation">
            <math display="block">
              <msub><mi>J</mi><mi>i</mi></msub>
              <mo>(</mo><mi>z</mi><mo>)</mo>
              <mo>=</mo>
              <msubsup>
                <mo>&#x222B;</mo>
                <mrow><mi>&#x03BB;</mi><mn>1</mn></mrow>
                <mrow><mi>&#x03BB;</mi><mn>2</mn></mrow>
              </msubsup>
              <msub><mi>&#x03C6;</mi><mi>i</mi></msub>
              <mo>(</mo><mi>&#x03BB;</mi><mo>)</mo>
              <msub><mi>&#x03C3;</mi><mi>i</mi></msub>
              <mo>(</mo><mi>&#x03BB;</mi><mo>)</mo>
              <msub><mi>I</mi><mi>&#x03BB;</mi></msub>
              <mo>(</mo><mi>z</mi><mo>)</mo>
              <mi>d</mi><mi>&#x03BB;</mi>
            </math>
          </div>
          <p class="equation-note">
            This gives the photolysis rate of species <em>i</em> at altitude <em>z</em>.
            It combines three ingredients:
            how efficiently absorbed photons trigger reaction (<em>&#x03C6;</em><sub>i</sub>, the quantum yield),
            how strongly the species absorbs light (<em>&#x03C3;</em><sub>i</sub>, the cross section),
            and how much radiation is actually available locally (<em>I</em><sub>&#x03BB;</sub>).
          </p>
        </div>
      </div>
      <p>
        In short, the logic is:
        opacity controls how much solar radiation survives to a given layer,
        and that surviving radiation sets the local photochemical rate constants.
      </p>
      <p class="source-note">
        <strong>Textbook note.</strong>
        The links among optical depth, solar zenith angle, radiative transfer, and photolysis coefficients are discussed in Jacobson,
        <em>Fundamentals of Atmospheric Modeling, Second Edition</em>,
        Section 9.6 “Optical depth” (p. 313), Section 9.7 “Solar zenith angle” (p. 316),
        Section 9.8 “The radiative transfer equation” (p. 317),
        and Section 9.8.4 “Heating rates and photolysis coefficients” (p. 332).
      </p>
    </article>

    <article class="entry-card">
      <p class="card-label">What PATMO Is Not</p>
      <p>
        <code>PATMO</code> is not an observational instrument, not a database,
        and not a machine that automatically decides whether your scientific interpretation is correct.
      </p>
    </article>
  </div>

  <div class="model-comparison">
    <article class="entry-card">
      <p class="card-label">Box Model</p>
      <div class="model-illustration">
        <pre><code>   atmosphere
      |
      v
 +-----------+
 | one box   |
 | well      |
 | mixed     |
 +-----------+</code></pre>
      </div>
      <p>
        A box model treats the whole system as one mixed compartment.
        There is no vertical structure inside the box.
      </p>
      <ul>
        <li>space resolved: none</li>
        <li>state variables: one value per species</li>
        <li>best for: the simplest chemical thinking</li>
      </ul>
    </article>

    <article class="entry-card">
      <p class="card-label">1D Model</p>
      <div class="model-illustration">
        <pre><code>      top
       ^
 +-----------+
 | layer 4   |
 +-----------+
 | layer 3   |
 +-----------+
 | layer 2   |
 +-----------+
 | layer 1   |
 +-----------+
       ^
    surface</code></pre>
      </div>
      <p>
        A 1D model resolves the atmosphere vertically.
        Each layer can have its own temperature, density, radiation field, and species abundances.
      </p>
      <ul>
        <li>space resolved: altitude only</li>
        <li>state variables: one profile per species</li>
        <li>best for: vertical structure, photochemistry, and diffusion</li>
      </ul>
    </article>

    <article class="entry-card">
      <p class="card-label">3D Model</p>
      <div class="model-illustration">
        <pre><code>          z
          ^
          |
     +----+----+
    /|   /|   /|
   +----+----+ |
   | +--|--+-| +
   |/   |/   |/
   +----+----+ --> x
  /
 v
y</code></pre>
      </div>
      <p>
        A 3D model resolves the atmosphere as a full volume.
        Conditions can vary in longitude, latitude, and height at the same time.
      </p>
      <ul>
        <li>space resolved: horizontal and vertical directions</li>
        <li>state variables: one 3D field per species</li>
        <li>best for: global transport and spatially complex systems</li>
      </ul>
    </article>
  </div>

  <div class="takeaway-box">
    <p><strong>Why this is a good teaching model.</strong></p>
    <p>
      A 1D photochemistry model is more informative than a single box model,
      but still much easier to understand than a full 3D model.
      That makes it a strong choice for learning how model inputs, chemistry, transport, and outputs connect.
    </p>
  </div>
</section>

<section id="simplified-sulfur-cycle" class="section-block">
  <div class="section-heading">
    <h2>A Simplified Modern Sulfur Cycle</h2>
  </div>

  <div class="panel">
    <p>
      Before reading the <code>PATMO</code> case directory, it helps to have one simple picture in mind:
      which sulfur species are released near the surface, which ones can reach the stratosphere,
      and how they are converted into sulfate aerosol.
    </p>
    <figure class="lecture-figure">
      <img src="{{ '/assets/img/patmo/sulfur_cycle.png' | relative_url }}" alt="Simplified atmospheric modern sulfur cycle showing surface emissions of COS, SO2, CS2, H2S, and DMS, oxidation pathways, transport to the stratosphere, conversion to H2SO4 and stratospheric sulfur aerosol, and return to the surface by deposition.">
      <figcaption>Figure. A simplified atmospheric modern sulfur cycle schematic.</figcaption>
    </figure>
  </div>

  <div class="model-note-stack">
    <article class="entry-card">
      <p class="card-label">How to Read This Figure</p>
      <p>
        In this simplified picture, sulfur enters the atmosphere through near-surface release of
        <code>COS</code>, <code>SO2</code>, <code>CS2</code>, <code>H2S</code>, and <code>DMS</code>.
        Among these species, <code>CS2</code>, <code>H2S</code>, and <code>DMS</code> are oxidized in the lower atmosphere
        and feed the production of <code>COS</code> and <code>SO2</code>.
      </p>
      <p>
        <code>SO2</code> is usually oxidized efficiently near the surface and in the troposphere,
        so it does not easily survive long enough to reach the stratosphere.
        In practice, direct injection by strong events such as volcanic eruptions is one important way for <code>SO2</code> to appear there.
      </p>
      <p>
        <code>COS</code> is relatively stable compared with the other reduced sulfur gases.
        That makes it an effective carrier of sulfur into the stratosphere, where it is oxidized through a sequence of reactions that eventually produces <code>SO2</code>.
      </p>
      <p>
        Once <code>SO2</code> is present in the stratosphere, further oxidation produces <code>H2SO4</code>.
        That sulfuric acid then contributes to the formation of <code>stratospheric sulfur aerosol (SSA)</code>.
      </p>
      <p>
        Finally, <code>SSA</code> does not remain suspended forever.
        It is removed through physical processes such as sedimentation and deposition, which return sulfur to lower levels and eventually back toward the surface.
      </p>
    </article>
  </div>

  <div class="takeaway-box">
    <p><strong>Why this figure matters for PATMO.</strong></p>
    <p>
      The rest of Lecture 1 shows how this simplified sulfur-cycle picture is turned into a runnable
      <code>PATMO</code> case through input files, a driver program, and diagnostic outputs.
    </p>
  </div>
</section>

<section id="working-with-patmo" class="section-block">
  <div class="section-heading">
    <h2>Modern Sulfur Cycle Input Files and Initial Conditions</h2>
  </div>

  <div class="panel">
    <p>
      If you want to understand how the <code>modern sulfur cycle</code> is represented in <code>PATMO</code>,
      the most useful place to start is the case directory <code>tests/modern_sulfur_cycle/</code>.
      This folder contains the input package for the case: editable spreadsheet inputs,
      runtime text files, the Fortran driver, and helper files for source and sink processes.
    </p>
    <pre><code>tests/modern_sulfur_cycle/
├── settings.xlsx          -> options.opt
├── profile.xlsx           -> profile.dat
├── solar_flux.xlsx        -> solar_flux.txt
├── reaction_network.xlsx  -> reaction_network.ntw
├── test.f90
├── copylist.pcp
└── helper deposition / rainout files</code></pre>
    <p>
      In this case, the spreadsheet files are convenient for human inspection and editing.
      The runtime files such as <code>options.opt</code>, <code>profile.dat</code>,
      <code>solar_flux.txt</code>, and <code>reaction_network.ntw</code> are the files used by the model during execution.
    </p>
  </div>

  <div class="panel">
    <h3>1. Main input-file types</h3>
    <table class="patmo-data-table">
      <thead>
        <tr>
          <th>File</th>
          <th>Kind of input</th>
          <th>What it contains</th>
          <th>Why it matters</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <td><code>settings.xlsx</code><br><code>options.opt</code></td>
          <td>Case settings</td>
          <td>Grid size, wavelength range, switches, emissions, dry deposition, fixed species</td>
          <td>Controls how the case is configured before the model runs</td>
        </tr>
        <tr>
          <td><code>profile.xlsx</code><br><code>profile.dat</code></td>
          <td>Initial vertical column</td>
          <td>Altitude, <code>Kzz</code>, temperature, and species number densities in each layer</td>
          <td>Defines the initial state of the 1D atmosphere</td>
        </tr>
        <tr>
          <td><code>solar_flux.xlsx</code><br><code>solar_flux.txt</code></td>
          <td>Radiation input</td>
          <td>Top-of-atmosphere spectral solar flux</td>
          <td>Drives opacity, local solar flux, and photolysis rates</td>
        </tr>
        <tr>
          <td><code>reaction_network.xlsx</code><br><code>reaction_network.ntw</code></td>
          <td>Chemical mechanism</td>
          <td>Reactants, products, and rate information for the sulfur chemistry</td>
          <td>Defines which reactions <code>PATMO</code> is allowed to solve</td>
        </tr>
        <tr>
          <td><code>test.f90</code></td>
          <td>Driver code</td>
          <td>The case-specific Fortran program that initializes the model and writes outputs</td>
          <td>Shows how this case is actually executed in code</td>
        </tr>
        <tr>
          <td><code>copylist.pcp</code> and helper <code>.txt</code> files</td>
          <td>Case support files</td>
          <td>Files copied into the build directory, including deposition and rainout inputs</td>
          <td>Provide auxiliary data needed by this specific sulfur-cycle case</td>
        </tr>
      </tbody>
    </table>
  </div>

  <div class="panel">
    <h3>2. Initial conditions in <code>profile.xlsx</code></h3>
    <p>
      <code>profile.xlsx</code> is the clearest place to see what the atmospheric column looks like at the beginning of the simulation.
      The first columns define the vertical structure, and the remaining columns store number densities for individual species.
    </p>
    <table class="compact-data-table">
      <thead>
        <tr>
          <th>Column group</th>
          <th>Meaning</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <td><code>index</code></td>
          <td>Layer number in the 1D column</td>
        </tr>
        <tr>
          <td><code>alt</code></td>
          <td>Altitude in km</td>
        </tr>
        <tr>
          <td><code>dummy</code></td>
          <td>An auxiliary profile column carried with the prepared input file; not the main teaching focus in Lecture 1</td>
        </tr>
        <tr>
          <td><code>Kzz</code></td>
          <td>Eddy diffusion coefficient used for vertical mixing</td>
        </tr>
        <tr>
          <td><code>Tgas</code></td>
          <td>Gas temperature in each layer</td>
        </tr>
        <tr>
          <td>Species columns</td>
          <td>Initial number densities in <code>cm^-3</code> for each species</td>
        </tr>
      </tbody>
    </table>

    <figure class="lecture-figure wide-figure">
      <img src="{{ '/assets/img/patmo/modern_sulfur_cycle_initial_profiles.svg' | relative_url }}" alt="Two-panel plot of initial number-density profiles from profile.xlsx, with altitude in kilometers on the vertical axis and number density on the horizontal axis, showing only the non-zero species.">
      <figcaption>Figure. Initial number-density profiles from <code>profile.xlsx</code>. Only non-zero species are plotted.</figcaption>
    </figure>

    <div class="lesson-grid">
      <article class="entry-card">
        <p class="card-label">What the Plot Shows</p>
        <p>
          The non-zero initial profiles are mainly background gases and oxidants:
          <code>N2</code>, <code>O2</code>, <code>CO2</code>, <code>H2O</code>, <code>O3</code>,
          <code>O</code>, <code>OH</code>, <code>HO2</code>, <code>CO</code>, and total number density <code>M</code>.
        </p>
        <p>
          This is a good reminder that a <code>PATMO</code> case is not just chemistry.
          It also needs a physically defined background atmosphere in which that chemistry can evolve.
        </p>
      </article>

      <article class="entry-card">
        <p class="card-label">What Starts at Zero</p>
        <p>
          In this case, the sulfur-bearing species in the initial profile are set to zero:
          <code>COS</code>, <code>CS2</code>, <code>CS</code>, <code>H2S</code>, <code>SH</code>, <code>S</code>,
          <code>SO</code>, <code>SO2</code>, <code>SO3</code>, <code>HSO</code>, <code>HSO2</code>, <code>HSO3</code>,
          <code>H2SO4</code>, <code>DMS</code>, <code>SO4</code>, <code>CS2E</code>, and <code>CH4O3S</code>.
        </p>
        <p>
          That means the sulfur cycle is not initialized by a pre-existing sulfur column.
          Instead, sulfur is introduced mainly through the source terms defined in the case settings and then evolves through chemistry and transport.
        </p>
      </article>
    </div>
  </div>

  <div class="panel">
    <h3>3. What <code>settings.xlsx</code> controls</h3>
    <p>
      <code>settings.xlsx</code> is the human-readable summary of the case configuration.
      In this workflow, it is converted into <code>options.opt</code>, which is the runtime settings file read by <code>PATMO</code>.
    </p>
    <table class="patmo-data-table settings-detail-table">
      <thead>
        <tr>
          <th>Parameter</th>
          <th>Value in this case</th>
          <th>What it means</th>
          <th>Why it matters here</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <td><code>cellsNumber</code></td>
          <td>60</td>
          <td>Number of vertical layers</td>
          <td>Sets the vertical resolution of the 1D column</td>
        </tr>
        <tr>
          <td><code>cellThickness</code></td>
          <td>1000 m</td>
          <td>Thickness of each model layer</td>
          <td>With 60 layers, this gives a 60 km atmospheric column</td>
        </tr>
        <tr>
          <td><code>photoBinsNumber</code></td>
          <td>4400</td>
          <td>Number of wavelength bins used for photochemistry</td>
          <td>Controls the spectral resolution of the radiation calculation</td>
        </tr>
        <tr>
          <td><code>wavelengMin</code></td>
          <td>120 nm</td>
          <td>Short-wavelength limit of the radiation grid</td>
          <td>Defines the lower bound of the photochemical spectrum</td>
        </tr>
        <tr>
          <td><code>wavelengMax</code></td>
          <td>400 nm</td>
          <td>Long-wavelength limit of the radiation grid</td>
          <td>Defines the upper bound of the photochemical spectrum</td>
        </tr>
        <tr>
          <td><code>zenith_angle</code></td>
          <td>60 degree</td>
          <td>Solar zenith angle</td>
          <td>Enters the attenuation formula for local solar flux</td>
        </tr>
        <tr>
          <td><code>TOA_para</code></td>
          <td>0.5</td>
          <td>Scaling factor applied at the top-of-atmosphere flux boundary</td>
          <td>In this case, the incoming solar flux is reduced by half at the top boundary</td>
        </tr>
        <tr>
          <td><code>usePhotochemistry</code></td>
          <td><code>T</code></td>
          <td>Turns photochemistry on</td>
          <td>Necessary because radiation-driven chemistry is central to this case</td>
        </tr>
        <tr>
          <td><code>useHescape</code></td>
          <td><code>F</code></td>
          <td>Turns hydrogen escape off</td>
          <td>This process is not part of the teaching focus of the modern sulfur cycle case</td>
        </tr>
        <tr>
          <td><code>useWaterRemoval</code></td>
          <td><code>F</code></td>
          <td>Turns water removal off</td>
          <td>Keeps the case focused on the sulfur-cycle processes being introduced here</td>
        </tr>
        <tr>
          <td><code>useAerosolformation</code></td>
          <td><code>T</code></td>
          <td>Allows aerosol formation processes to operate</td>
          <td>Needed because sulfuric-acid production is linked to sulfate aerosol formation</td>
        </tr>
        <tr>
          <td><code>gravity_species</code></td>
          <td><code>SO4:gd(j)</code></td>
          <td>Assigns gravitational settling behavior to <code>SO4</code></td>
          <td>Represents the fact that sulfur aerosol is removed physically as well as chemically</td>
        </tr>
        <tr>
          <td><code>constant_species</code></td>
          <td>12 species</td>
          <td>Species held fixed instead of being fully integrated as prognostic variables</td>
          <td>Simplifies the case while preserving an oxidizing background atmosphere</td>
        </tr>
        <tr>
          <td><code>drydep_species</code></td>
          <td>5 sulfur species</td>
          <td>Dry-deposition velocities at the lower boundary</td>
          <td>Represents surface removal of sulfur gases</td>
        </tr>
        <tr>
          <td><code>emission_species</code></td>
          <td>5 sulfur species</td>
          <td>Surface emission source strengths</td>
          <td>These emissions are the main way sulfur enters the column in this case</td>
        </tr>
      </tbody>
    </table>

    <div class="lesson-grid">
      <article class="entry-card">
        <p class="card-label"><code>emission_species</code></p>
        <table class="compact-data-table">
          <thead>
            <tr>
              <th>Species</th>
              <th>Emission rate</th>
              <th>Unit</th>
            </tr>
          </thead>
          <tbody>
            <tr><td><code>COS</code></td><td><code>8.1001d2</code></td><td><code>molecule cm^-3 s^-1</code></td></tr>
            <tr><td><code>CS2</code></td><td><code>5.9886d2</code></td><td><code>molecule cm^-3 s^-1</code></td></tr>
            <tr><td><code>H2S</code></td><td><code>84.7910d2</code></td><td><code>molecule cm^-3 s^-1</code></td></tr>
            <tr><td><code>SO2</code></td><td><code>615.84d2</code></td><td><code>molecule cm^-3 s^-1</code></td></tr>
            <tr><td><code>CH3SCH3</code> (<code>DMS</code>)</td><td><code>39.50274d3</code></td><td><code>molecule cm^-3 s^-1</code></td></tr>
          </tbody>
        </table>
      </article>

      <article class="entry-card">
        <p class="card-label"><code>drydep_species</code></p>
        <table class="compact-data-table">
          <thead>
            <tr>
              <th>Species</th>
              <th>Deposition velocity</th>
              <th>Unit</th>
            </tr>
          </thead>
          <tbody>
            <tr><td><code>COS</code></td><td><code>9.5d-3</code></td><td><code>cm s^-1</code></td></tr>
            <tr><td><code>CS2</code></td><td><code>4.48d-2</code></td><td><code>cm s^-1</code></td></tr>
            <tr><td><code>SO2</code></td><td><code>1</code></td><td><code>cm s^-1</code></td></tr>
            <tr><td><code>H2S</code></td><td><code>1.7d-1</code></td><td><code>cm s^-1</code></td></tr>
            <tr><td><code>CH3SCH3</code> (<code>DMS</code>)</td><td><code>1.48d-1</code></td><td><code>cm s^-1</code></td></tr>
          </tbody>
        </table>
      </article>

      <article class="entry-card">
        <p class="card-label"><code>constant_species</code></p>
        <p>
          The species held fixed in this case are:
          <code>HO2</code>, <code>N</code>, <code>CO2</code>, <code>H2O</code>, <code>CO</code>,
          <code>O2</code>, <code>N2</code>, <code>OH</code>, <code>O</code>, <code>H2</code>, <code>H</code>, and <code>O3</code>.
        </p>
        <p>
          This keeps a prescribed oxidizing background while the sulfur species respond to emissions,
          photochemistry, transport, and deposition.
        </p>
      </article>
    </div>
  </div>

  <div class="panel">
    <h3>4. Boundary-condition summary for the modern sulfur cycle case</h3>
    <p>
      As a final summary, the table below collects the main lower-boundary sulfur inputs and removal terms in converted teaching units.
      The emission and dry-deposition entries are consistent with the case settings, while the Henry's-constant column is included as supporting boundary-condition context for sulfur uptake and removal.
    </p>
    <table class="patmo-data-table boundary-summary-table">
      <thead>
        <tr>
          <th>Species</th>
          <th>Emission<br>(Tg year<sup>-1</sup>)</th>
          <th>Dry deposition<br>(cm s<sup>-1</sup>)</th>
          <th>Henry's constant<br>(mol L<sup>-1</sup> atm<sup>-1</sup>)</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <td><code>COS</code></td>
          <td>1.3</td>
          <td>9.5 &times; 10<sup>-3</sup></td>
          <td>0.02</td>
        </tr>
        <tr>
          <td><code>CS2</code></td>
          <td>1.2</td>
          <td>4.48 &times; 10<sup>-2</sup></td>
          <td>0.05</td>
        </tr>
        <tr>
          <td><code>SO2</code></td>
          <td>105.4</td>
          <td>1</td>
          <td>4000</td>
        </tr>
        <tr>
          <td><code>H2S</code></td>
          <td>7.72</td>
          <td>1.7 &times; 10<sup>-1</sup></td>
          <td>0.1</td>
        </tr>
        <tr>
          <td><code>DMS</code></td>
          <td>65.57</td>
          <td>1.48 &times; 10<sup>-1</sup></td>
          <td>--</td>
        </tr>
        <tr>
          <td><code>SSA</code></td>
          <td>--</td>
          <td>--</td>
          <td>5 &times; 10<sup>4</sup></td>
        </tr>
      </tbody>
    </table>
    <p>
      Read this table as a compact reminder of how sulfur enters and leaves the model column:
      gaseous sulfur species are supplied at the lower boundary by emissions, some are removed there by dry deposition,
      and highly soluble sulfur species are also connected to aqueous uptake through large Henry's constants.
    </p>
  </div>
</section>

<section id="case-study" class="section-block">
  <div class="section-heading">
    <h2>How PATMO Uses These Files in Code</h2>
  </div>

  <div class="panel">
    <p>
      The spreadsheet files are not the final objects read by the runtime model.
      In this workflow, <code>compile_modern_sulfur_cycle.sh</code> converts the editable spreadsheets
      into the text files that <code>PATMO</code> actually uses when the case is built and executed.
    </p>
    <pre><code>settings.xlsx          -> options.opt
profile.xlsx           -> profile.dat
solar_flux.xlsx        -> solar_flux.txt
reaction_network.xlsx  -> reaction_network.ntw</code></pre>
  </div>

  <div class="model-note-stack">
    <article class="entry-card">
      <p class="card-label">1. <code>options.opt</code> as the switchboard</p>
      <pre><code>cellsNumber = 60
zenith_angle = 60
usePhotochemistry = T
emission_species = COS:..., CS2:..., H2S:..., SO2:..., CH3SCH3:...
drydep_species = COS:..., CS2:..., SO2:..., H2S:..., CH3SCH3:...</code></pre>
      <p>
        Once <code>settings.xlsx</code> has been converted, <code>options.opt</code> becomes the central runtime settings file.
        It tells <code>PATMO</code> how many layers to use, what solar geometry to apply,
        whether photochemistry is active, and which sulfur gases are emitted or removed at the lower boundary.
      </p>
    </article>

    <article class="entry-card">
      <p class="card-label">2. <code>reaction_network.ntw</code> as the chemistry file</p>
      <pre><code>13,H2S,OH,,H2O,SH,,...
26,SO2,O,M,SO3,M,,...
33,SO2,OH,M,HSO3,M,,...
34,SO3,H2O,,H2SO4,,,...
37,SO2,,,SO4,,,0</code></pre>
      <p>
        This file is where the sulfur cycle becomes an explicit mechanism that the solver can evaluate.
        It is the bridge between the conceptual sulfur-cycle diagram and the actual reaction network used in the model.
      </p>
    </article>

    <article class="entry-card">
      <p class="card-label">3. <code>test.f90</code> as the case driver</p>
      <pre><code>call patmo_init()
call patmo_loadInitialProfile("profile.dat",unitH="km",unitX="1/cm3")
call patmo_setFluxBB()
call patmo_dumpJValue("jvalue.dat")
call patmo_dumpOpacity("opacity.dat")
call patmo_dumpAllRates("rates.dat")
call patmo_dumpAllMixingRatioToFile("allNDs.dat")</code></pre>
      <p>
        This is the Fortran driver program for the case.
        It shows, in code, how the runtime inputs are loaded:
        first the model is initialized, then the atmospheric profile is read, then the radiation field is set,
        and finally the diagnostic outputs are written.
        If you want to see where this <code>PATMO</code> run actually starts and ends, this is the file to open.
      </p>
    </article>
  </div>

  <div class="takeaway-box">
    <p><strong>Practical takeaway.</strong></p>
    <p>
      The modern sulfur cycle becomes runnable in <code>PATMO</code> only when these files work together:
      the spreadsheet inputs define the case, the converted text files are read at runtime,
      and <code>test.f90</code> provides the code path that actually launches the experiment.
    </p>
  </div>
</section>

<section id="research-workflow" class="section-block">
  <div class="section-heading">
    <h2>What PATMO Produces After a Run</h2>
  </div>

  <div class="panel">
    <p>
      At the end of <code>tests/modern_sulfur_cycle/test.f90</code>, the case writes four output files that you will repeatedly use in later lectures:
    </p>
    <pre><code>call patmo_dumpJValue("jvalue.dat")
call patmo_dumpOpacity("opacity.dat")
call patmo_dumpAllRates("rates.dat")
call patmo_dumpAllMixingRatioToFile("allNDs.dat")</code></pre>
  </div>

  <div class="lesson-grid">
    <article class="entry-card">
      <p class="card-label"><code>allNDs.dat</code></p>
      <p>
        This is the first file to inspect if you want to know what the atmosphere looks like after the run.
        It contains the vertical abundances or mixing ratios of the modeled species.
        For the modern sulfur cycle case, this is where you begin if you want to see the final profiles of species such as
        <code>H2S</code>, <code>SO2</code>, <code>H2SO4</code>, or <code>SO4</code>.
      </p>
    </article>

    <article class="entry-card">
      <p class="card-label"><code>jvalue.dat</code></p>
      <p>
        This file contains photolysis rates.
        It tells you how quickly radiation-driven reactions proceed in each layer.
      </p>
    </article>

    <article class="entry-card">
      <p class="card-label"><code>opacity.dat</code></p>
      <p>
        This file helps you understand why the photolysis rates look the way they do.
        It records how radiation is attenuated through the atmospheric column.
      </p>
    </article>

    <article class="entry-card">
      <p class="card-label"><code>rates.dat</code></p>
      <p>
        This file is where chemistry becomes interpretable.
        If you want to know which sulfur pathway is important, this is the file you need.
        It helps you move from “the profile changed” to “this particular reaction pathway is responsible.”
      </p>
    </article>
  </div>

  <div class="takeaway-box">
    <p><strong>A useful reading order.</strong></p>
    <p>
      First inspect <code>allNDs.dat</code> to see the resulting sulfur profiles.
      Then look at <code>jvalue.dat</code> and <code>opacity.dat</code> to understand the radiative environment.
      Finally, open <code>rates.dat</code> to identify which reactions are controlling the result.
    </p>
  </div>
</section>

<section id="check-yourself" class="section-block">
  <div class="section-heading">
    <h2>Check Yourself</h2>
  </div>

  <div class="panel">
    <p>
      Before you move on to Lecture 2, answer these three questions.
      If you can answer them correctly, then the main idea of Lecture 1 is already in place.
    </p>
  </div>

  <div class="quiz-block" data-answer="b">
    <h3>1</h3>
    <p>Which spreadsheet is the best place to inspect the altitude-dependent initial number densities of species in this case?</p>
    <div class="quiz-options">
      <button class="quiz-option" data-choice="a" onclick="checkPatmoQuiz(this)">A. <code>settings.xlsx</code></button>
      <button class="quiz-option" data-choice="b" onclick="checkPatmoQuiz(this)">B. <code>profile.xlsx</code></button>
      <button class="quiz-option" data-choice="c" onclick="checkPatmoQuiz(this)">C. <code>reaction_network.xlsx</code></button>
    </div>
    <div class="quiz-feedback"></div>
  </div>

  <div class="quiz-block" data-answer="c">
    <h3>2</h3>
    <p>In this lecture, why are many sulfur species zero in <code>profile.xlsx</code> at the start of the run?</p>
    <div class="quiz-options">
      <button class="quiz-option" data-choice="a" onclick="checkPatmoQuiz(this)">A. Because sulfur chemistry is turned off in this case</button>
      <button class="quiz-option" data-choice="b" onclick="checkPatmoQuiz(this)">B. Because <code>PATMO</code> cannot read sulfur species from a profile file</button>
      <button class="quiz-option" data-choice="c" onclick="checkPatmoQuiz(this)">C. Because sulfur is introduced mainly through emissions and then evolves through chemistry and transport</button>
    </div>
    <div class="quiz-feedback"></div>
  </div>

  <div class="quiz-block" data-answer="a">
    <h3>3</h3>
    <p>Which parameter in <code>settings.xlsx</code> contains the surface source strengths that inject sulfur gases into the atmospheric column?</p>
    <div class="quiz-options">
      <button class="quiz-option" data-choice="a" onclick="checkPatmoQuiz(this)">A. <code>emission_species</code></button>
      <button class="quiz-option" data-choice="b" onclick="checkPatmoQuiz(this)">B. <code>drydep_species</code></button>
      <button class="quiz-option" data-choice="c" onclick="checkPatmoQuiz(this)">C. <code>gravity_species</code></button>
    </div>
    <div class="quiz-feedback"></div>
  </div>

  <div class="hero-actions">
    <a class="pixel-button ghost" href="{{ '/work/patmo/' | relative_url }}">Back to course hub</a>
  </div>
</section>

<script>
  function checkPatmoQuiz(button) {
    const block = button.closest('.quiz-block');
    const answer = block.dataset.answer;
    const choice = button.dataset.choice;
    const feedback = block.querySelector('.quiz-feedback');
    const options = block.querySelectorAll('.quiz-option');

    options.forEach((option) => option.classList.remove('correct-choice'));

    if (choice === answer) {
      button.classList.add('correct-choice');
      feedback.textContent = 'Correct. You can move on.';
      feedback.className = 'quiz-feedback correct';
    } else {
      feedback.textContent = 'Not quite. Try again.';
      feedback.className = 'quiz-feedback incorrect';
    }
  }
</script>

</div>
