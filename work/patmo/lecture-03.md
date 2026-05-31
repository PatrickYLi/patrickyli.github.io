---
layout: page
title: "Lecture 3: Photochemical Calculations"
subtitle: Using ozone photolysis in the Chapman cycle to understand cross sections, photolysis rates, and PATMO settings.
permalink: /work/patmo/lecture-03/
body_class: patmo-reading-theme
head-extra:
  - patmo-reading-theme.html
  - mathjax.html
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
    The same cycle contains two photochemical reactions:
  </p>

  <figure class="panel">
    <img src="{{ '/assets/img/patmo/chapman-cycle-schematic.png' | relative_url }}" alt="Schematic diagram of the Chapman cycle showing oxygen photolysis, ozone formation, ozone photolysis, and ozone loss.">
  </figure>

  <pre class="lesson-flow"><code>R1: O2 + hv -> O + O
R3: O3 + hv -> O2 + O</code></pre>

  <p>
    In this lecture, we will use <code>O3 + hv -> O2 + O</code> as the worked example.
    This reaction shows why photochemistry is different from ordinary thermal kinetics:
    instead of finding one thermal rate constant <code>k(T)</code>, we need to understand how a photolysis rate
    <code>J</code> is calculated from radiation, absorption cross section, and quantum yield.
  </p>

  <div class="hero-actions">
    <a class="pixel-button ghost" href="{{ '/work/patmo/lecture-02/' | relative_url }}">Previous Lecture</a>
    <a class="pixel-button secondary" href="{{ '/work/patmo/' | relative_url }}">Course Hub</a>
  </div>
</section>

<section id="example-reaction" class="section-block">
  <div class="section-heading">
    <h2>The Chapman Photochemical Reactions</h2>
  </div>

  <div class="lesson-grid">
    <article class="entry-card">
      <p class="card-label">R1: Oxygen Photolysis</p>
      <pre><code>O2 + hv -> O + O</code></pre>
      <p>
        Molecular oxygen absorbs short-wavelength ultraviolet radiation and breaks into two oxygen atoms.
        This reaction supplies the atomic oxygen needed to start ozone formation.
      </p>
    </article>

    <article class="entry-card">
      <p class="card-label">R3: Ozone Photolysis</p>
      <pre><code>O3 + hv -> O2 + O</code></pre>
      <p>
        Ozone absorbs a photon and dissociates into molecular oxygen and atomic oxygen.
        In a more detailed mechanism, product channels such as <code>O(3P)</code> and <code>O(1D)</code>
        should be treated separately.
      </p>
    </article>
  </div>

  <div class="lesson-grid">
    <article class="entry-card">
      <p class="card-label">Worked Example In This Lecture</p>
      <p>
        We focus on <code>O3 + hv -> O2 + O</code> because ozone has widely used UV/VIS absorption data,
        and it gives a clean example for learning how to search cross-section databases.
      </p>
    </article>

    <article class="entry-card">
      <p class="card-label">Why This Is Not A Normal k</p>
      <p>
        Photolysis depends on the radiation field.
        The same molecule can photolyze quickly or slowly depending on wavelength, altitude,
        solar zenith angle, shielding, cross section, and quantum yield.
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
      For this lecture, use one photolysis-rate equation:
    </p>
    <div class="takeaway-box">
      <p class="section-label">Photolysis Rate</p>
      <p style="font-size: 1.45rem; line-height: 1.8; text-align: center; overflow-x: auto;">
        \[
        J_{i,z} =
        \int_{\lambda_1}^{\lambda_2}
        \phi_{\lambda}(\lambda)
        \sigma_{\lambda}(\lambda)
        I_{\lambda}(z)
        \, d\lambda
        \]
      </p>
    </div>
    <p>
      Here <code>I</code> is the solar irradiance reaching altitude <code>z</code>.
      In a simple attenuation form, it can be connected to optical depth and solar zenith angle:
    </p>
    <div class="takeaway-box">
      <p class="section-label">Irradiance With Optical Depth</p>
      <p style="font-size: 1.35rem; line-height: 1.8; text-align: center; overflow-x: auto;">
        \[
        I_{\lambda}(z) =
        I_{\lambda,0}
        \exp\left[-\frac{\tau_{\lambda}(z)}{\cos\theta}\right]
        \]
      </p>
    </div>
  </div>

  <figure class="panel">
    <img src="{{ '/assets/img/patmo/zenith-angle-schematic.png' | relative_url }}" alt="Schematic diagram showing solar zenith angle measured from the local vertical and the longer atmospheric path at larger zenith angle.">
  </figure>

  <div class="lesson-grid">
    <article class="entry-card">
      <h3>\(J_{i,z}\)</h3>
      <p>
        \(J_{i,z}\) is the photolysis rate of reaction \(i\) at altitude \(z\).
        Its unit is \(\mathrm{s^{-1}}\). For ozone photolysis, this is the \(J(\mathrm{O_3})\) value used in the reaction network.
      </p>
    </article>

    <article class="entry-card">
      <h3>\(\phi_{\lambda}(\lambda)\)</h3>
      <p>
        \(\phi_{\lambda}(\lambda)\) is the quantum yield for the selected product channel.
        It is the probability that photon absorption produces the products written in the photochemical reaction.
      </p>
    </article>

    <article class="entry-card">
      <h3>\(\sigma_{\lambda}(\lambda)\)</h3>
      <p>
        \(\sigma_{\lambda}(\lambda)\) is the absorption cross section at wavelength \(\lambda\).
        It tells us how strongly the molecule absorbs light.
        It is commonly reported in \(\mathrm{cm^2\,molecule^{-1}}\).
      </p>
    </article>

    <article class="entry-card">
      <h3>\(I_{\lambda}(z)\)</h3>
      <p>
        \(I_{\lambda}(z)\) is the solar irradiance at wavelength \(\lambda\) and altitude \(z\).
        It is reduced from the top-of-atmosphere irradiance \(I_{\lambda,0}\) by atmospheric optical depth.
      </p>
    </article>

    <article class="entry-card">
      <h3>\(\lambda_1\), \(\lambda_2\), and \(d\lambda\)</h3>
      <p>
        \(\lambda_1\) and \(\lambda_2\) define the wavelength range of the photolysis calculation.
        The term \(d\lambda\) means the integral is evaluated over wavelength.
      </p>
    </article>

    <article class="entry-card">
      <h3>\(\tau_{\lambda}(z)\)</h3>
      <p>
        \(\tau_{\lambda}(z)\) is the vertical optical depth above altitude \(z\).
        Larger optical depth means stronger attenuation before radiation reaches the layer.
      </p>
    </article>

    <article class="entry-card">
      <h3>\(\theta\) and \(\cos\theta\)</h3>
      <p>
        \(\theta\) is the solar zenith angle measured from the local vertical.
        The factor \(\cos\theta\) converts vertical optical depth into an approximate slant-path optical depth:
        \(\tau_{\mathrm{slant}} = \tau / \cos\theta\).
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
          <td>Solar zenith angle</td>
          <td>The zenith angle <code>&theta;</code>, or equivalently <code>cos(&theta;)</code>.</td>
          <td>This controls the slant path used in <code>&tau;<sub>slant</sub> = &tau; / cos(&theta;)</code>.</td>
        </tr>
        <tr>
          <td>Optical depth</td>
          <td>How the model calculates or reads <code>&tau;<sub>&lambda;</sub>(z)</code>.</td>
          <td>The optical depth controls how much solar irradiance reaches each atmospheric layer.</td>
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
      <li>Explain which setting controls the cross-section file, solar irradiance, optical depth, zenith angle, and wavelength grid.</li>
      <li>Write what must be added or checked in the PATMO input reaction network for this photolysis reaction.</li>
    </ol>
  </div>

  <div class="hero-actions">
    <a class="pixel-button ghost" href="{{ '/work/patmo/' | relative_url }}">Back to course hub</a>
  </div>
</section>

</div>
