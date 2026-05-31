---
layout: page
title: "Lecture 3: Photochemical Calculations"
subtitle: Using ozone photolysis in the Chapman cycle to understand cross sections, photolysis rates, and PATMO settings.
permalink: /work/patmo/lecture-03/
body_class: patmo-reading-theme
head-extra:
  - patmo-reading-theme.html
description: Lecture 3 of the PATMO student course, introducing photochemical calculations through Chapman-cycle ozone photolysis and the MPI-Mainz UV/VIS spectral atlas.
---

<div class="patmo-course">

<section class="panel">
  <div class="lesson-breadcrumb">
    <a href="{{ '/work/patmo/' | relative_url }}">PATMO Course</a>
    <span>/</span>
    <span>Lecture 3 of 7</span>
  </div>

  <p class="section-label">Student Handout</p>
  <p>
    Lecture 2 used the Chapman cycle to introduce a reaction network.
    Lecture 3 focuses on one photochemical reaction from that cycle:
  </p>

  <pre class="lesson-flow"><code>O3 + hv -> O2 + O</code></pre>

  <p>
    This reaction is useful because it shows why photochemistry is different from ordinary thermal kinetics.
    Instead of finding one thermal rate constant <code>k(T)</code>, we need to understand how a photolysis rate
    <code>J</code> is calculated from light intensity, absorption cross section, and quantum yield.
  </p>

  <div class="hero-actions">
    <a class="pixel-button ghost" href="{{ '/work/patmo/lecture-02/' | relative_url }}">Previous Lecture</a>
    <a class="pixel-button secondary" href="{{ '/work/patmo/' | relative_url }}">Course Hub</a>
  </div>
</section>

<section id="example-reaction" class="section-block">
  <div class="section-heading">
    <h2>The Example Reaction</h2>
  </div>

  <div class="lesson-grid">
    <article class="entry-card">
      <p class="card-label">Reaction</p>
      <pre><code>O3 + hv -> O2 + O</code></pre>
      <p>
        Ozone absorbs a photon and dissociates into molecular oxygen and atomic oxygen.
        In a more detailed mechanism, product channels such as <code>O(3P)</code> and <code>O(1D)</code>
        should be treated separately.
      </p>
    </article>

    <article class="entry-card">
      <p class="card-label">Why This Is Not A Normal k</p>
      <p>
        The reaction speed depends on the radiation field.
        The same ozone molecule can photolyze quickly or slowly depending on wavelength, altitude,
        solar zenith angle, shielding, and the chosen cross-section data.
      </p>
    </article>
  </div>

  <div class="takeaway-box">
    <p class="section-label">Main Point</p>
    <p>
      For photochemical reactions, the model calculates a photolysis rate <code>J</code>.
      The database usually gives cross sections and quantum yields, not a single universal <code>J</code>.
    </p>
  </div>
</section>

<section id="photolysis-rate" class="section-block">
  <div class="section-heading">
    <h2>How A Photolysis Rate Is Calculated</h2>
  </div>

  <div class="panel">
    <p>
      A simplified photolysis calculation can be written like this:
    </p>
    <pre class="lesson-flow"><code>J(z) = sum over wavelength [
  actinic_flux(lambda, z)
  * cross_section(lambda, T)
  * quantum_yield(lambda, T)
  * wavelength_step
]</code></pre>
  </div>

  <div class="lesson-grid">
    <article class="entry-card">
      <p class="card-label">Actinic Flux</p>
      <p>
        The amount of radiation available to drive photolysis at each wavelength and altitude.
        It changes with solar spectrum, altitude, absorption by other species, and scattering.
      </p>
    </article>

    <article class="entry-card">
      <p class="card-label">Cross Section</p>
      <p>
        A measure of how strongly a molecule absorbs light at a given wavelength.
        For gas-phase atmospheric chemistry, it is commonly reported in
        <code>cm2 molecule-1</code>.
      </p>
    </article>

    <article class="entry-card">
      <p class="card-label">Quantum Yield</p>
      <p>
        The probability that photon absorption actually produces the photochemical product channel of interest.
        It is dimensionless and may depend on wavelength and temperature.
      </p>
    </article>

    <article class="entry-card">
      <p class="card-label">J Value</p>
      <p>
        The final photolysis rate has units of <code>s-1</code>.
        In the reaction network, the rate term is <code>J[O3]</code>.
      </p>
    </article>
  </div>
</section>

<section id="setting-xlsx" class="section-block">
  <div class="section-heading">
    <h2>Photochemical Settings In tests/setting.xlsx</h2>
  </div>

  <div class="panel">
    <p>
      In the PATMO test case, look at <code>tests/setting.xlsx</code> before running a photochemical calculation.
      The exact sheet or row names may differ between versions, but the photochemistry-related settings should answer these questions.
    </p>
  </div>

  <div class="panel">
    <table>
      <thead>
        <tr>
          <th>Setting group</th>
          <th>What students should check</th>
          <th>Why it matters for <code>O3 + hv -> O2 + O</code></th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <td>Photochemistry switch</td>
          <td>Whether photochemical reactions are enabled for the run.</td>
          <td>If this is off, the model will not calculate <code>J[O3]</code>.</td>
        </tr>
        <tr>
          <td>Radiation source</td>
          <td>The solar or stellar spectrum used as input radiation.</td>
          <td>Different spectra change the amount of light available for ozone photolysis.</td>
        </tr>
        <tr>
          <td>Wavelength grid</td>
          <td>The wavelength range and resolution used by the calculation.</td>
          <td>The cross-section file must cover the wavelength region used by the model.</td>
        </tr>
        <tr>
          <td>Cross-section file</td>
          <td>The path or filename for the <code>O3</code> absorption cross-section data.</td>
          <td>This is the database file students will find from MPI-Mainz or another source.</td>
        </tr>
        <tr>
          <td>Quantum yield file</td>
          <td>The product-channel yield used for ozone photolysis.</td>
          <td>Absorption alone is not enough; the model also needs the product probability.</td>
        </tr>
        <tr>
          <td>Atmospheric profile</td>
          <td>Altitude, temperature, and density information used by the run.</td>
          <td>Temperature can affect cross sections and quantum yields, and altitude affects radiation.</td>
        </tr>
        <tr>
          <td>Output control</td>
          <td>Whether photolysis rates or radiation diagnostics are written to output files.</td>
          <td>Students need this output to check whether <code>J(O3)</code> was actually calculated.</td>
        </tr>
      </tbody>
    </table>
  </div>
</section>

<section id="mpi-mainz" class="section-block">
  <div class="section-heading">
    <h2>Finding Cross Sections In MPI-Mainz</h2>
  </div>

  <div class="panel">
    <p>
      The <a href="https://uvvis.mpch-mainz.gwdg.de/uvvis/cross_sections/" target="_blank" rel="noreferrer">MPI-Mainz UV/VIS Spectral Atlas cross-section page</a>
      is a common entry point for gas-phase atmospheric absorption cross sections.
      The atlas also provides <a href="https://uvvis.mpch-mainz.gwdg.de/uvvis/quantum_yields/" target="_blank" rel="noreferrer">quantum-yield data</a>
      through a separate section.
    </p>
  </div>

  <div class="lesson-grid">
    <article class="entry-card">
      <p class="card-label">Step 1: Open Cross Sections</p>
      <p>
        Go to the MPI-Mainz cross-section page.
        You can browse by molecular category or use the species search.
      </p>
    </article>

    <article class="entry-card">
      <p class="card-label">Step 2: Search Ozone</p>
      <p>
        For this lecture, search for <code>O3</code> or open the <code>Ozone</code> category.
        The ozone page lists many measurements with different wavelength ranges and temperatures.
      </p>
    </article>

    <article class="entry-card">
      <p class="card-label">Step 3: Choose A Data Set</p>
      <p>
        Choose a data set that matches the model wavelength range and temperature as closely as possible.
        Record author, year, temperature, wavelength coverage, and file name.
      </p>
    </article>

    <article class="entry-card">
      <p class="card-label">Step 4: Download And Inspect</p>
      <p>
        Numerical files are ASCII text files. The first column is wavelength in <code>nm</code>,
        and the second column is cross section in <code>cm2 molecule-1</code> or quantum yield when using the quantum-yield section.
      </p>
    </article>
  </div>
</section>

<section id="patmo-workflow" class="section-block">
  <div class="section-heading">
    <h2>From Database To PATMO Input</h2>
  </div>

  <div class="panel">
    <p>
      For <code>O3 + hv -> O2 + O</code>, students should connect the database data to the model setup in this order:
    </p>
    <pre class="lesson-flow"><code>choose photolysis reaction
find O3 cross-section data
find or confirm quantum yield data
check tests/setting.xlsx photochemistry settings
point PATMO to the correct input files
run and inspect J(O3) output</code></pre>
  </div>

  <div class="takeaway-box">
    <p class="section-label">Do Not Skip This Check</p>
    <p>
      A correct reaction equation is not enough.
      For photochemistry, the reaction network, cross-section data, quantum-yield data,
      wavelength grid, and radiation settings must be consistent with each other.
    </p>
  </div>
</section>

<section id="after-class-task" class="section-block">
  <div class="section-heading">
    <h2>After-Class Task</h2>
  </div>

  <div class="panel">
    <p>
      Complete a short photochemistry setup note for the Chapman reaction
      <code>O3 + hv -> O2 + O</code>.
    </p>
    <ol>
      <li>Find one suitable <code>O3</code> absorption cross-section data set in MPI-Mainz.</li>
      <li>Record the author, year, temperature, wavelength range, units, and filename.</li>
      <li>Identify the photochemistry-related settings in <code>tests/setting.xlsx</code>.</li>
      <li>Explain which setting controls the cross-section file and which setting controls the radiation or wavelength grid.</li>
      <li>Write what must be added or checked in the PATMO input reaction network for this photolysis reaction.</li>
    </ol>
  </div>

  <div class="hero-actions">
    <a class="pixel-button ghost" href="{{ '/work/patmo/' | relative_url }}">Back to course hub</a>
  </div>
</section>

</div>
