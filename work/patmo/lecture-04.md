---
layout: page
title: "Lecture 4: Emission and Deposition"
subtitle: Adding emission, dry deposition, and wet deposition around the Chapman cycle.
permalink: /work/patmo/lecture-04/
body_class: patmo-reading-theme
head-extra:
  - patmo-reading-theme.html
  - mathjax.html
description: Lecture 4 of the PATMO student course, introducing emission, dry deposition, and wet deposition as source and sink processes around the Chapman cycle.
---

<div class="patmo-course">

<section class="panel">
  <div class="lesson-breadcrumb">
    <a href="{{ '/work/patmo/' | relative_url }}">PATMO Course</a>
    <span>/</span>
    <span>Lecture 4 of 7</span>
  </div>

  <p class="section-label">Student Handout</p>
  <p>
    Lecture 2 and Lecture 3 used the Chapman cycle to build a gas-phase reaction network.
    The cycle itself contains thermal reactions and photochemical reactions:
  </p>

  <figure class="panel">
    <img src="{{ '/assets/img/patmo/chapman-cycle-schematic.png' | relative_url }}" alt="Schematic diagram of the Chapman cycle showing oxygen photolysis, ozone formation, ozone photolysis, and ozone loss.">
  </figure>

  <pre class="lesson-flow"><code>R1: O2 + hv -> O + O
R2: O + O2 + M -> O3 + M
R3: O3 + hv -> O2 + O
R4: O + O3 -> O2 + O2</code></pre>

  <p>
    In this lecture, we keep that same Chapman network and add three processes around it:
    emission, dry deposition, and wet deposition.
    These are not new Chapman chemical reactions. They are source and sink terms that add or remove species from the model column.
  </p>

  <div class="hero-actions">
    <a class="pixel-button ghost" href="{{ '/work/patmo/lecture-03/' | relative_url }}">Previous Lecture</a>
    <a class="pixel-button secondary" href="{{ '/work/patmo/' | relative_url }}">Course Hub</a>
  </div>
</section>

<section id="boundary-terms" class="section-block">
  <div class="section-heading">
    <h2>Adding Boundary Terms To Chapman</h2>
  </div>

  <div class="panel">
    <p>
      For one species \(i\), the model tendency can be written as:
    </p>

    \[
    \frac{dn_i}{dt}
    =
    P_i^{\mathrm{chem}}
    -
    L_i^{\mathrm{chem}}
    +
    S_i^{\mathrm{emission}}
    -
    L_i^{\mathrm{dry}}
    -
    L_i^{\mathrm{wet}}
    \]

    <p>
      The Chapman cycle gives the chemical production and loss terms.
      Emission adds material. Dry deposition and wet deposition remove material.
      In a pure Chapman-only exercise, the last three terms can be set to zero.
    </p>
  </div>

  <div class="takeaway-box">
    <p class="section-label">Important Point</p>
    <p>
      Do not write emission or deposition as ordinary gas-phase reactions such as
      <code>O3 -> surface</code> inside the Chapman reaction list.
      They are physical source and sink processes with their own units and model settings.
    </p>
  </div>
</section>

<section id="chapman-training-system" class="section-block">
  <div class="section-heading">
    <h2>How Chapman Should Be Used Here</h2>
  </div>

  <div class="lesson-grid">
    <article class="entry-card">
      <p class="card-label">What Is Realistic</p>
      <p>
        Ozone dry deposition is a meaningful surface sink.
        Near the surface, ozone can be removed by uptake on soil, vegetation, water, and other surfaces.
      </p>
    </article>

    <article class="entry-card">
      <p class="card-label">What Is Only Practice</p>
      <p>
        A direct ozone emission source is usually not the realistic baseline for a Chapman case.
        If we add one in class, it should be labeled as an artificial training source for learning how an emission term works.
      </p>
    </article>

    <article class="entry-card">
      <p class="card-label">What Needs Care</p>
      <p>
        Wet deposition is most useful for soluble or scavenged species.
        Ozone is not the cleanest wet-deposition example, so for a strict Chapman baseline it is reasonable to keep wet deposition off and explain why.
      </p>
    </article>
  </div>
</section>

<section id="emission" class="section-block">
  <div class="section-heading">
    <h2>Emission</h2>
  </div>

  <div class="panel">
    <p>
      Emission means a species is added to the model from a source.
      In a one-dimensional column, the most common teaching form is a surface flux into the lowest model layer.
    </p>

    \[
    S_i^{\mathrm{emission}}
    \approx
    \frac{F_i}{\Delta z_1}
    \]

    <p>
      \(F_i\) is the emission flux, often written in molecules cm\(^{-2}\) s\(^{-1}\).
      \(\Delta z_1\) is the thickness of the lowest model layer.
      After division by \(\Delta z_1\), the source term has volume units such as molecules cm\(^{-3}\) s\(^{-1}\).
    </p>
  </div>

  <div class="takeaway-box">
    <p class="section-label">Chapman Example</p>
    <p>
      For a physically clean Chapman baseline, set direct emissions of <code>O</code>, <code>O2</code>, and <code>O3</code> to zero.
      If the class adds an ozone emission term, treat it as a controlled numerical experiment:
      the purpose is to learn how a source term changes the ozone budget, not to claim ozone is normally emitted this way.
    </p>
  </div>
</section>

<section id="dry-deposition" class="section-block">
  <div class="section-heading">
    <h2>Dry Deposition</h2>
  </div>

  <div class="panel">
    <p>
      Dry deposition is removal at the lower boundary without precipitation.
      It is usually written with a deposition velocity \(v_{d,i}\):
    </p>

    \[
    F_i^{\mathrm{dry}}
    =
    v_{d,i} n_i(z_1)
    \]

    \[
    L_i^{\mathrm{dry}}
    \approx
    \frac{F_i^{\mathrm{dry}}}{\Delta z_1}
    =
    \frac{v_{d,i} n_i(z_1)}{\Delta z_1}
    \]

    <p>
      \(n_i(z_1)\) is the species concentration in the lowest layer.
      \(v_{d,i}\) has units of length per time, such as cm s\(^{-1}\).
      A larger deposition velocity means faster surface removal.
    </p>
  </div>

  <div class="takeaway-box">
    <p class="section-label">Chapman Example</p>
    <p>
      Apply dry deposition to <code>O3</code> as the worked Chapman example.
      This creates a surface sink that competes with photolysis and the gas-phase loss reaction
      <code>O + O3 -> O2 + O2</code>.
    </p>
  </div>
</section>

<section id="wet-deposition" class="section-block">
  <div class="section-heading">
    <h2>Wet Deposition</h2>
  </div>

  <div class="panel">
    <p>
      Wet deposition is removal by cloud water, rainout, or washout.
      In a simple model exercise, it can be represented as a first-order loss:
    </p>

    \[
    L_i^{\mathrm{wet}}
    =
    k_{i}^{\mathrm{wet}} n_i
    \]

    <p>
      \(k_i^{\mathrm{wet}}\) is a wet-removal rate with units of s\(^{-1}\).
      It summarizes how efficiently the species is scavenged by cloud or precipitation processes.
    </p>
  </div>

  <div class="takeaway-box">
    <p class="section-label">Chapman Example</p>
    <p>
      For a strict Chapman ozone baseline, wet deposition can be turned off and documented as
      <code>not used for this simple O3 example</code>.
      The formula is still worth learning now because it becomes important for soluble sulfur species later in the course.
    </p>
  </div>
</section>

<section id="class-setup" class="section-block">
  <div class="section-heading">
    <h2>Class Setup</h2>
  </div>

  <table>
    <thead>
      <tr>
        <th>Model part</th>
        <th>What students should specify</th>
        <th>Chapman teaching choice</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td>Chemical network</td>
        <td>Reaction equations and rate information</td>
        <td>Keep the four Chapman reactions from Lectures 2 and 3</td>
      </tr>
      <tr>
        <td>Emission</td>
        <td>Species, source type, flux or rate, units, layer</td>
        <td>Baseline: set direct ozone emission to zero; optional: artificial O3 source for testing</td>
      </tr>
      <tr>
        <td>Dry deposition</td>
        <td>Species, deposition velocity, units, surface-layer treatment</td>
        <td>Add dry deposition for O3</td>
      </tr>
      <tr>
        <td>Wet deposition</td>
        <td>Species, wet-removal rate or scavenging setting, units</td>
        <td>Baseline: keep off for O3 and explain why</td>
      </tr>
    </tbody>
  </table>
</section>

<section id="after-class-task" class="section-block">
  <div class="section-heading">
    <h2>After-Class Task</h2>
  </div>

  <div class="panel">
    <p>
      Starting from your Chapman reaction network, prepare a boundary-process version of the case.
    </p>
    <ol>
      <li>Keep the four Chapman chemical reactions unchanged.</li>
      <li>Decide whether the case uses any emission term. For the physical baseline, set direct ozone emission to zero. If you add an artificial O3 source, label it clearly as a training experiment.</li>
      <li>Add dry deposition for <code>O3</code>. Record the deposition velocity, units, and whether the removal applies only to the lowest layer.</li>
      <li>Decide whether wet deposition is off or included. For a strict Chapman case, keep it off and explain that O3 is not the main wet-deposition example.</li>
      <li>Submit the updated PATMO input files together with a short table listing each boundary process, species, value, units, and reason for using or not using it.</li>
    </ol>
  </div>

  <div class="takeaway-box">
    <p class="section-label">Final Output</p>
    <p>
      The result should show both parts of the model setup:
      the gas-phase Chapman reaction network and the external source/sink settings.
    </p>
  </div>

  <div class="hero-actions">
    <a class="pixel-button ghost" href="{{ '/work/patmo/' | relative_url }}">Back to course hub</a>
  </div>
</section>

</div>
