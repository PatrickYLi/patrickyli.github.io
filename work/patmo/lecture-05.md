---
layout: page
title: "Lecture 5: Wet Deposition In PATMO"
subtitle: Using the modern sulfur cycle to understand rainout rates, Henry constants, and the wetdep array.
permalink: /work/patmo/lecture-05/
body_class: patmo-reading-theme
head-extra:
  - patmo-reading-theme.html
  - mathjax.html
description: Lecture 5 of the PATMO student course, introducing wet deposition through the modern sulfur cycle rainout calculation in PATMO.
---

<div class="patmo-course">

<section class="panel">
  <div class="lesson-breadcrumb">
    <a href="{{ '/work/patmo/' | relative_url }}">PATMO Course</a>
    <span>/</span>
    <span>Lecture 5 of 7</span>
  </div>

  <p class="section-label">Student Handout</p>
  <p>
    Lecture 4 used the Chapman cycle to introduce emission and dry deposition.
    Wet deposition is different: it is not a useful Chapman-cycle example.
    In this lecture, we switch to the <code>modern sulfur cycle</code>, where soluble sulfur species can be removed by cloud water and precipitation.
  </p>
  <p>
    The goal is to understand how PATMO stores and applies wet deposition rates in the code,
    and how a species-specific rainout rate becomes a loss term in the model equations.
  </p>

  <div class="hero-actions">
    <a class="pixel-button ghost" href="{{ '/work/patmo/lecture-04/' | relative_url }}">Previous Lecture</a>
    <a class="pixel-button secondary" href="{{ '/work/patmo/' | relative_url }}">Course Hub</a>
  </div>
</section>

<section id="patmo-paper-context" class="section-block">
  <div class="section-heading">
    <h2>PATMO Paper Context</h2>
  </div>

  <div class="panel">
    <p>
      The PATMO paper by
      <a href="https://doi.org/10.2343/geochemj.GJ23004" target="_blank" rel="noreferrer">Danielache et al. (2023)</a>
      describes PATMO as a flexible one-dimensional atmospheric chemistry code.
      In that framework, atmospheric composition is controlled not only by chemistry and photolysis,
      but also by boundary and removal processes such as surface emission, dry deposition, wet deposition,
      condensation, sedimentation, diffusion, and escape.
    </p>
    <p>
      This lecture focuses on the wet-deposition part of that physical picture and connects it to the actual PATMO code used in the modern sulfur cycle case.
    </p>
  </div>
</section>

<section id="why-modern-sulfur" class="section-block">
  <div class="section-heading">
    <h2>Why Use The Modern Sulfur Cycle</h2>
  </div>

  <div class="lesson-grid">
    <article class="entry-card">
      <p class="card-label">Chapman Is Not Enough</p>
      <p>
        The Chapman cycle contains <code>O2</code>, <code>O</code>, <code>O3</code>, photons, and a third body <code>M</code>.
        It is useful for gas-phase ozone photochemistry, but it is not a good teaching case for rainout.
      </p>
    </article>

    <article class="entry-card">
      <p class="card-label">Sulfur Has Soluble Species</p>
      <p>
        The modern sulfur cycle includes species such as <code>SO2</code>, <code>SO4</code>, <code>H2S</code>, <code>OCS</code>, and <code>CS2</code>.
        Some of these species can interact with water and be removed by wet deposition.
      </p>
    </article>
  </div>
</section>

<section id="code-map" class="section-block">
  <div class="section-heading">
    <h2>Where Wet Deposition Appears In PATMO</h2>
  </div>

  <table>
    <thead>
      <tr>
        <th>File</th>
        <th>What students should notice</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td><code>src_f90/patmo_parameters.f90</code></td>
        <td><code>wetdep(cellsNumber, chemSpeciesNumber)</code> stores wet-deposition rates for each layer and chemical species.</td>
      </tr>
      <tr>
        <td><code>src_f90/patmo_ode.f90</code></td>
        <td>The ODE system subtracts <code>wetdep(j,i) * n(j,i)</code> from layer <code>j</code>. For layers above the surface, the same amount is added to the layer below.</td>
      </tr>
      <tr>
        <td><code>tests/modern_sulfur_cycle/test.f90</code></td>
        <td>The modern sulfur cycle test initializes <code>wetdep</code> and calls <code>computewetdep(idx, Heff)</code> for selected sulfur species.</td>
      </tr>
      <tr>
        <td><code>tests/modern_sulfur_cycle/Rainout-SO2.txt</code></td>
        <td>The generated rainout file lists altitude <code>ZKM</code> and <code>K_RAIN</code>, the first-order wet-deposition rate.</td>
      </tr>
    </tbody>
  </table>

  <figure class="panel">
    <img src="{{ '/assets/img/patmo/rainout-so2-profile.svg' | relative_url }}" alt="Line plot of SO2 rainout rate K_RAIN versus altitude from Rainout-SO2.txt, showing K_RAIN decreasing with altitude.">
    <figcaption>
      SO<sub>2</sub> rainout-rate profile generated from <code>Rainout-SO2.txt</code>.
      The legend identifies <code>K_RAIN</code> as the first-order wet-deposition rate used by <code>wetdep(j,i)</code>.
    </figcaption>
  </figure>
</section>

<section id="model-equation" class="section-block">
  <div class="section-heading">
    <h2>How PATMO Uses wetdep</h2>
  </div>

  <div class="panel">
    <p>
      The original PATMO ODE code applies wet deposition like this:
    </p>

    <pre class="lesson-flow"><code>! Wet Deposition
do j = 12, 2, -1
    do i = 1, chemSpeciesNumber
        dn(j,     i) = dn(j,     i) - wetdep(j, i) * n(j, i)
        dn(j - 1, i) = dn(j - 1, i) + wetdep(j, i) * n(j, i)
    end do
end do

do i = 1, chemSpeciesNumber
    dn(1, i) = dn(1, i) - wetdep(1, i) * n(1, i)
end do</code></pre>

    <p>
      In the code, wet deposition is a first-order process. For species <code>i</code> in layer <code>j</code>:
    </p>

    \[
    \left(\frac{dn_{j,i}}{dt}\right)_{\mathrm{wet}}
    =
    -k_{\mathrm{rain},j,i} n_{j,i}
    \]

    <p>
      In PATMO notation, \(k_{\mathrm{rain},j,i}\) is stored as <code>wetdep(j,i)</code>.
      The unit is \(\mathrm{s^{-1}}\). The product <code>wetdep(j,i) * n(j,i)</code>
      has units of molecules cm\(^{-3}\) s\(^{-1}\).
    </p>
  </div>

  <figure class="panel">
    <img src="{{ '/assets/img/patmo/wetdep-vertical-transfer.png' | relative_url }}" alt="Scientific diagram showing PATMO wet deposition vertical transfer from upper layers to lower layers and final removal from the bottom layer.">
    <figcaption>
      Wet deposition in PATMO acts as vertical transfer in upper layers and final removal from the bottom layer.
    </figcaption>
  </figure>

  <div class="lesson-grid">
    <article class="entry-card">
      <p class="card-label">Upper Layers</p>
      <p>
        For <code>j = 12</code> down to <code>2</code>, PATMO subtracts <code>wetdep(j,i) * n(j,i)</code>
        from the current layer and adds the same amount to the layer below.
        This represents rainout moving material downward through the column.
      </p>
    </article>

    <article class="entry-card">
      <p class="card-label">Bottom Layer</p>
      <p>
        For <code>j = 1</code>, there is no lower atmospheric layer.
        The code subtracts <code>wetdep(1,i) * n(1,i)</code> from the model column, so the bottom-layer term is the final removal by wet deposition.
      </p>
    </article>
  </div>

  <div class="takeaway-box">
    <p class="section-label">Vertical Transfer</p>
    <p>
      PATMO does not only delete material from every layer.
      For layers <code>j = 12</code> down to <code>2</code>, material removed from layer <code>j</code> is added to layer <code>j - 1</code>.
      In the bottom layer, wet deposition is a true loss from the model column.
    </p>
  </div>
</section>

<section id="modern-sulfur-example" class="section-block">
  <div class="section-heading">
    <h2>Modern Sulfur Cycle Example</h2>
  </div>

  <div class="panel">
    <p>
      In <code>tests/modern_sulfur_cycle/test.f90</code>, PATMO computes rainout for selected sulfur species by passing an effective Henry constant:
    </p>

    <pre class="lesson-flow"><code>wetdep(:,:) = 0d0

call computewetdep(1,  2.0d-2)  ! OCS
call computewetdep(11, 5.0d-2)  ! CS2
call computewetdep(28, 1.0d-1)  ! H2S
call computewetdep(18, 4.0d3)   ! SO2
call computewetdep(19, 5.0d14)  ! SO4</code></pre>
  </div>

  <div class="lesson-grid">
    <article class="entry-card">
      <p class="card-label">Species Index</p>
      <p>
        The first argument is the PATMO species index.
        For example, <code>18</code> is used for <code>SO2</code> in this generated mechanism.
      </p>
    </article>

    <article class="entry-card">
      <p class="card-label">Heff</p>
      <p>
        The second argument is the effective Henry constant used by <code>computewetdep</code>.
        Larger values generally mean stronger coupling to water and faster rainout.
      </p>
    </article>
  </div>
</section>

<section id="rainout-rate" class="section-block">
  <div class="section-heading">
    <h2>How K_RAIN Is Calculated</h2>
  </div>

  <div class="panel">
    <p>
      This calculation answers one question:
      <strong>how fast should species <code>i</code> be removed by rainout in layer <code>j</code>?</strong>
      The answer is a first-order rate constant called <code>K_RAIN</code>.
    </p>
    <p>
      Read the calculation in three steps: estimate contact with water, correct for precipitation timing, then convert the result into a first-order loss rate.
    </p>
  </div>

  <div class="lesson-grid">
    <article class="entry-card">
      <p class="card-label">1. Water Available</p>
      <p>
        <code>WH2O</code> represents the layer-dependent water removal term used by the rainout calculation.
        More available liquid-water removal generally gives stronger wet deposition.
      </p>
    </article>

    <article class="entry-card">
      <p class="card-label">2. Solubility</p>
      <p>
        <code>Heff</code> is the effective Henry constant for the species.
        A more soluble species is more easily transferred into water and can rain out faster.
      </p>
    </article>

    <article class="entry-card">
      <p class="card-label">3. Timing</p>
      <p>
        <code>gamma</code> is a precipitation/non-precipitation time factor, and <code>fz</code> is an altitude-dependent factor.
        Together they prevent the simple removal frequency from being used too directly.
      </p>
    </article>
  </div>

  <div class="panel">
    <p>
      First, <code>computewetdep</code> estimates a raw layer-dependent removal frequency \(r_{j,i}\).
      This is not the final rainout rate yet. It is an intermediate estimate of how quickly species <code>i</code>
      can be transferred into water in layer <code>j</code>:
    </p>

    \[
    r_{j,i}
    =
    \frac{W_{\mathrm{H2O},j}/55}{N_A w_l 10^{-9} + \left(H_{\mathrm{eff},i} R T_j\right)^{-1}}
    \]

    <p>
      In this expression, \(W_{\mathrm{H2O},j}\) comes from the vertical water profile,
      \(H_{\mathrm{eff},i}\) is species-specific, \(T_j\) is the layer temperature,
      and \(R\) is the gas constant in the unit system used by the code.
    </p>

    <p>
      Second, the code computes a correction factor \(Q_j\).
      This correction accounts for the fact that rainout is not simply continuous removal at the raw frequency \(r_{j,i}\):
    </p>

    \[
    Q_j
    =
    1 - f_z
    +
    \frac{f_z}{\gamma_j r_{j,i}}
    \left(1 - e^{-r_{j,i}\gamma_j}\right)
    \]

    <p>
      Third, the corrected first-order rainout rate is calculated as:
    </p>

    \[
    K_{\mathrm{RAIN},j,i}
    =
    \frac{1 - e^{-r_{j,i}\gamma_j}}{\gamma_j Q_j}
    \]

    <p>
      This <code>K_RAIN</code> value is what becomes <code>wetdep(j,i)</code> in the ODE system.
      Once it is stored there, PATMO uses it exactly like any other first-order sink:
      <code>K_RAIN * [species]</code>.
    </p>
  </div>

  <div class="takeaway-box">
    <p class="section-label">Reading Shortcut</p>
    <p>
      You do not need to memorize every term in the rainout formula.
      For this lecture, remember the workflow:
      choose a species, provide <code>Heff</code>, let <code>computewetdep</code> calculate <code>K_RAIN(z)</code>,
      and use <code>K_RAIN</code> as the first-order wet-deposition rate in each layer.
    </p>
  </div>
</section>

<section id="modern-sulfur-code" class="section-block">
  <div class="section-heading">
    <h2>Actual Wet Deposition Code In The Modern Sulfur Cycle</h2>
  </div>

  <div class="panel">
    <p>
      In the modern sulfur cycle test, wet deposition is assigned directly in
      <code>tests/modern_sulfur_cycle/test.f90</code>. The model first clears all wet-deposition rates,
      then assigns rainout to selected sulfur species.
    </p>

    <pre class="lesson-flow"><code>wetdep(:,:) = 0d0

! calculate wet deposition
call computewetdep(1,  2.0d-2)  ! OCS
call computewetdep(11, 5.0d-2)  ! CS2
call computewetdep(28, 1.0d-1)  ! H2S
call computewetdep(18, 4.0d3)   ! SO2
! call computewetdep(26, 5d14)  ! H2SO4
call computewetdep(19, 5d14)    ! SO4

! Turco H2SO4
wetdep(12,26) = 1.77E-06
wetdep(11,26) = 3.54E-06
wetdep(10,26) = 5.31E-06
wetdep(9,26)  = 7.08E-06
wetdep(8,26)  = 8.85E-06
wetdep(7,26)  = 1.06E-05
wetdep(6,26)  = 1.24E-05
wetdep(5,26)  = 1.42E-05
wetdep(4,26)  = 1.59E-05
wetdep(3,26)  = 1.77E-05
wetdep(2,26)  = 1.95E-05
wetdep(1,26)  = 2.12E-05</code></pre>
  </div>

  <div class="table-wrap">
    <table>
      <thead>
        <tr>
          <th>Species</th>
          <th>PATMO index</th>
          <th>How wet deposition is assigned</th>
          <th>What students should notice</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <td><code>OCS</code></td>
          <td><code>1</code></td>
          <td><code>computewetdep(1, 2.0d-2)</code></td>
          <td>The second argument is the effective Henry constant used to calculate <code>K_RAIN(z)</code>.</td>
        </tr>
        <tr>
          <td><code>CS2</code></td>
          <td><code>11</code></td>
          <td><code>computewetdep(11, 5.0d-2)</code></td>
          <td>Less soluble species receive smaller rainout rates than highly soluble sulfur species.</td>
        </tr>
        <tr>
          <td><code>H2S</code></td>
          <td><code>28</code></td>
          <td><code>computewetdep(28, 1.0d-1)</code></td>
          <td>The species index must match the generated PATMO mechanism.</td>
        </tr>
        <tr>
          <td><code>SO2</code></td>
          <td><code>18</code></td>
          <td><code>computewetdep(18, 4.0d3)</code></td>
          <td>This is the example connected to the <code>Rainout-SO2.txt</code> profile shown above.</td>
        </tr>
        <tr>
          <td><code>SO4</code></td>
          <td><code>19</code></td>
          <td><code>computewetdep(19, 5d14)</code></td>
          <td>A very large effective Henry constant makes sulfate strongly coupled to wet removal.</td>
        </tr>
        <tr>
          <td><code>H2SO4</code></td>
          <td><code>26</code></td>
          <td>Manual values: <code>wetdep(layer,26)</code></td>
          <td>The <code>computewetdep</code> call is commented out, so this test prescribes an H2SO4 vertical profile directly.</td>
        </tr>
      </tbody>
    </table>
  </div>

  <div class="lesson-grid">
    <article class="entry-card">
      <p class="card-label">Computed Species</p>
      <p>
        For <code>OCS</code>, <code>CS2</code>, <code>H2S</code>, <code>SO2</code>, and <code>SO4</code>, the code supplies
        <code>computewetdep</code> with a species index and an effective Henry constant.
        PATMO then calculates the layer-dependent <code>wetdep(j,i)</code> profile.
      </p>
    </article>

    <article class="entry-card">
      <p class="card-label">Prescribed H2SO4</p>
      <p>
        For <code>H2SO4</code>, the code uses explicit values from layer <code>12</code> to layer <code>1</code>.
        These numbers are already first-order wet-deposition rates in <code>s-1</code>, so they can be placed directly into
        <code>wetdep(layer,26)</code>.
      </p>
    </article>
  </div>

  <div class="takeaway-box">
    <p class="section-label">Model Meaning</p>
    <p>
      Once these lines are executed, each value of <code>wetdep(j,i)</code> is used by the ODE as a first-order term.
      For example, the <code>SO2</code> loss or transfer rate in layer <code>j</code> is
      <code>wetdep(j,18) * n(j,18)</code>.
    </p>
  </div>
</section>

<section id="workflow" class="section-block">
  <div class="section-heading">
    <h2>Workflow For Students</h2>
  </div>

  <div class="lesson-grid">
    <article class="entry-card">
      <p class="card-label">1. Choose Species</p>
      <p>Start with a sulfur species in the modern sulfur mechanism, such as <code>SO2</code> or <code>SO4</code>.</p>
    </article>

    <article class="entry-card">
      <p class="card-label">2. Find Heff</p>
      <p>Find or justify an effective Henry constant for the species. Record the source and units.</p>
    </article>

    <article class="entry-card">
      <p class="card-label">3. Compute K_RAIN</p>
      <p>Use <code>computewetdep(idx, Heff)</code> or assign <code>wetdep(layer,idx)</code> directly when a prescribed vertical profile is used.</p>
    </article>

    <article class="entry-card">
      <p class="card-label">4. Put It In The ODE</p>
      <p>Interpret <code>wetdep(j,i)</code> as a first-order rate: <code>K_RAIN * [species]</code>.</p>
    </article>
  </div>

  <div class="hero-actions">
    <a class="pixel-button" href="{{ '/work/patmo/lecture-06/' | relative_url }}">Continue to Lecture 6</a>
    <a class="pixel-button ghost" href="{{ '/work/patmo/' | relative_url }}">Back to course hub</a>
  </div>
</section>

</div>
