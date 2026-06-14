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
      The subroutine <code>computewetdep</code> first estimates a layer-dependent removal frequency:
    </p>

    \[
    r_{j,i}
    =
    \frac{W_{\mathrm{H2O},j}/55}{N_A w_l 10^{-9} + \left(H_{\mathrm{eff},i} R T_j\right)^{-1}}
    \]

    <p>
      Then it corrects that removal frequency using the precipitation-time factor \(\gamma_j\) and the altitude-dependent factor \(f_z\):
    </p>

    \[
    Q_j
    =
    1 - f_z
    +
    \frac{f_z}{\gamma_j r_{j,i}}
    \left(1 - e^{-r_{j,i}\gamma_j}\right)
    \]

    \[
    K_{\mathrm{RAIN},j,i}
    =
    \frac{1 - e^{-r_{j,i}\gamma_j}}{\gamma_j Q_j}
    \]

    <p>
      This <code>K_RAIN</code> value is what becomes <code>wetdep(j,i)</code> in the ODE system.
    </p>
  </div>
</section>

<section id="output-file" class="section-block">
  <div class="section-heading">
    <h2>Reading A Rainout File</h2>
  </div>

  <div class="panel">
    <p>
      A generated rainout file contains altitude and the first-order rainout rate.
      For <code>SO2</code>, the file <code>Rainout-SO2.txt</code> starts like this:
    </p>

    <pre class="lesson-flow"><code>ZKM      1.0
K_RAIN   6.2733022012625560E-007
ZKM      2.0
K_RAIN   4.6111370206485505E-007
ZKM      3.0
K_RAIN   3.1999166103882194E-007</code></pre>
  </div>

  <div class="takeaway-box">
    <p class="section-label">How To Use It</p>
    <p>
      At 1 km, this example gives \(K_{\mathrm{RAIN}} = 6.27 \times 10^{-7}\ \mathrm{s^{-1}}\).
      The wet-deposition loss term for <code>SO2</code> in that layer is therefore
      \(6.27 \times 10^{-7} [\mathrm{SO_2}]\).
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
      <p>Use <code>computewetdep(idx, Heff)</code> or a prepared <code>Rainout-*.txt</code> file to obtain the layer-dependent rainout rate.</p>
    </article>

    <article class="entry-card">
      <p class="card-label">4. Put It In The ODE</p>
      <p>Interpret <code>wetdep(j,i)</code> as a first-order rate: <code>K_RAIN * [species]</code>.</p>
    </article>
  </div>

  <div class="hero-actions">
    <a class="pixel-button ghost" href="{{ '/work/patmo/' | relative_url }}">Back to course hub</a>
  </div>
</section>

</div>
