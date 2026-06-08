---
layout: page
title: "Lecture 4: Emission and Dry Deposition"
subtitle: Adding emission and dry deposition around the Chapman cycle.
permalink: /work/patmo/lecture-04/
body_class: patmo-reading-theme
head-extra:
  - patmo-reading-theme.html
  - mathjax.html
description: Lecture 4 of the PATMO student course, introducing emission and dry deposition as source and sink processes around the Chapman cycle.
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
    In this lecture, we keep that same Chapman network and add two processes around it:
    emission and dry deposition.
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
    \]

    <p>
      The Chapman cycle gives the chemical production and loss terms.
      Emission adds material. Dry deposition removes material at the lower boundary.
      In a pure Chapman-only exercise, these two extra terms can be set to zero.
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
      <p class="card-label">Dry Deposition</p>
      <p>
        Ozone dry deposition can occur on vegetation, soil, water, snow, building surfaces, and other lower-boundary surfaces.
        The wheat-canopy paper below is only one literature example for learning how to find a deposition velocity.
      </p>
    </article>

    <article class="entry-card">
      <p class="card-label">Emission</p>
      <p>
        For a simple classroom demonstration, use a direct artificial <code>O3</code> emission flux.
        This makes the input format and unit conversion easy to see, even though real lower-atmosphere ozone is usually produced photochemically.
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

<section id="o3-emission-demonstration" class="section-block">
  <div class="section-heading">
    <h2>Finding O3 Emission Data From A Paper</h2>
  </div>

  <div class="panel">
    <p>
      In this lecture, use a direct <code>O3</code> emission only as a training source.
      The goal is not to claim that surface ozone is normally emitted this way.
      The goal is to show how students can find a reported <code>O3</code> emission rate in the literature,
      convert its units, and place it into the lowest model layer.
    </p>
    <p>
      A useful example is
      <a href="https://doi.org/10.1021/acs.estlett.3c00318" target="_blank" rel="noreferrer">Link et al. (2023)</a>,
      which reports ozone generation from a 222 nm germicidal ultraviolet lamp.
      The paper gives an <code>O3</code> generation rate of 1.22 mg h<sup>-1</sup>.
    </p>
  </div>

  <div class="lesson-grid">
    <article class="entry-card">
      <p class="card-label">Step 1: Search The Literature</p>
      <p>
        Use keywords such as <code>ozone emission rate mg h-1</code>, <code>O3 generation rate UV lamp</code>,
        or <code>ozone generator emission rate</code>. Keep papers that report a direct <code>O3</code> rate with units.
      </p>
    </article>

    <article class="entry-card">
      <p class="card-label">Step 2: Copy The Reported Value</p>
      <p>
        From Link et al. (2023), copy the species, source type, value, and unit:
        <code>O3</code>, UV lamp generation, \(R_{\mathrm{mass}} = 1.22\) mg h\(^{-1}\).
      </p>
    </article>

    <article class="entry-card">
      <p class="card-label">Step 3: Choose Model Geometry</p>
      <p>
        The paper reports a total generation rate, not an atmospheric surface flux.
        For a 1D teaching column, assume a column area \(A = 1\) m\(^2\) and lowest-layer thickness
        \(\Delta z_1 = 1000\) m.
      </p>
    </article>

    <article class="entry-card">
      <p class="card-label">Step 4: Convert To Model Units</p>
      <p>
        Convert mg h\(^{-1}\) to molecules cm\(^{-2}\) s\(^{-1}\), then divide by \(\Delta z_1\).
        The final source term should be in molecules cm\(^{-3}\) s\(^{-1}\).
      </p>
    </article>
  </div>

  <div class="panel">
    <p class="section-label">Worked Conversion</p>
    <p>
      First convert the total mass rate to a mass flux over the chosen model column area:
    </p>

    \[
    E_{\mathrm{mass}}
    =
    \frac{R_{\mathrm{mass}}}{A}
    =
    \frac{1.22 \times 10^{-3}\ \mathrm{g\ h^{-1}}}{1\ \mathrm{m^2}}
    =
    1.22 \times 10^{-3}\ \mathrm{g\ m^{-2}\ h^{-1}}
    \]

    <p>
      Then convert the mass flux to a number flux using \(M_{\mathrm{O_3}} = 48.00\) g mol\(^{-1}\):
    </p>

    \[
    F_{\mathrm{O_3}}
    =
    E_{\mathrm{mass}}
    \frac{N_A}{M_{\mathrm{O_3}}}
    \frac{1}{10^4}
    \frac{1}{3600}
    \approx
    4.25 \times 10^{11}
    \ \mathrm{molecules\ cm^{-2}\ s^{-1}}
    \]

    <p>
      Finally divide by the layer thickness \(\Delta z_1 = 1000\) m \(= 1.0 \times 10^5\) cm:
    </p>

    \[
    S_{\mathrm{O_3}}
    =
    \frac{F_{\mathrm{O_3}}}{\Delta z_1}
    =
    \frac{4.25 \times 10^{11}}{1.0 \times 10^5}
    \approx
    4.25 \times 10^6
    \ \mathrm{molecules\ cm^{-3}\ s^{-1}}
    \]

    <p>
      This is the value students can place into the <code>O3</code> source term for the artificial emission demonstration.
      If they choose a different paper value, column area, or model-layer thickness, they must redo the conversion.
    </p>
  </div>

  <div class="takeaway-box">
    <p class="section-label">Teaching Use In This Case</p>
    <p>
      Use this direct <code>O3</code> emission only to teach how a source term is written in PATMO.
      In the physical baseline, the teacher may still set direct <code>O3</code> emission to zero and then turn on this artificial source as a sensitivity test.
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

<section id="deposition-paper-example" class="section-block">
  <div class="section-heading">
    <h2>Finding A Deposition Value From A Paper</h2>
  </div>

  <div class="panel">
    <p>
      Use vegetation as the example surface and search for a recent paper with these keywords:
      <code>ozone dry deposition velocity wheat field</code>, <code>O3 deposition flux vegetation</code>, or
      <code>Vd ozone cropland</code>.
    </p>
    <p>
      A good example is
      <a href="https://doi.org/10.5194/acp-24-12323-2024" target="_blank" rel="noreferrer">Zhang et al. (2024)</a>,
      which measured ozone deposition over wheat fields in the North China Plain during the 2023 wheat growing season.
    </p>
  </div>

  <div class="lesson-grid">
    <article class="entry-card">
      <p class="card-label">Step 1: Identify The Surface</p>
      <p>
        Record the land surface before copying any number.
        In this paper, the surface is a wheat canopy, not bare soil, open water, or forest.
      </p>
    </article>

    <article class="entry-card">
      <p class="card-label">Step 2: Find The Variable</p>
      <p>
        Search inside the paper for <code>deposition velocity</code>, <code>Vd</code>, and <code>flux</code>.
        For PATMO, the number we need for the simple dry-deposition setting is usually a deposition velocity.
      </p>
    </article>

    <article class="entry-card">
      <p class="card-label">Step 3: Copy Units</p>
      <p>
        Zhang et al. report deposition velocity in cm s\(^{-1}\).
        The reported mean value for the main wheat growing season is \(V_d = 0.29 \pm 0.33\) cm s\(^{-1}\).
      </p>
    </article>

    <article class="entry-card">
      <p class="card-label">Step 4: Keep Context</p>
      <p>
        The same paper reports higher daytime \(V_d\) than nighttime \(V_d\).
        Do not treat one value as universal: record site, season, vegetation type, method, and whether the value is daytime, nighttime, or all-period average.
      </p>
    </article>
  </div>

  <div class="panel">
    <p class="section-label">Worked Conversion For Model</p>
    <p>
      The reported value \(V_d = 0.29\) cm s\(^{-1}\) is already a deposition velocity.
      If PATMO asks directly for a dry-deposition velocity, students can enter:
    </p>

    \[
    v_{d,\mathrm{O_3}}
    =
    0.29\ \mathrm{cm\ s^{-1}}
    \]

    <p>
      If the model setup needs a lowest-layer first-order loss rate, divide by the lowest-layer thickness.
      With \(\Delta z_1 = 1000\) m \(= 1.0 \times 10^5\) cm:
    </p>

    \[
    k_{\mathrm{dep,O_3}}
    =
    \frac{v_{d,\mathrm{O_3}}}{\Delta z_1}
    =
    \frac{0.29}{1.0 \times 10^5}
    \approx
    2.9 \times 10^{-6}\ \mathrm{s^{-1}}
    \]

    <p>
      The volume loss term is then this first-order rate multiplied by the lowest-layer ozone number density, written as \([\mathrm{O_3}]\):
    </p>

    \[
    L_{\mathrm{dry,O_3}}
    =
    k_{\mathrm{dep,O_3}} [\mathrm{O_3}]
    =
    2.9 \times 10^{-6} [\mathrm{O_3}]
    \]

    <p>
      In the model tendency equation, dry deposition is a sink, so it is subtracted from the <code>O3</code> budget.
      If students use a different \(V_d\) or layer thickness, they must redo the \(k_{\mathrm{dep,O_3}}\) conversion.
    </p>
  </div>

  <div class="takeaway-box">
    <p class="section-label">Teaching Use In This Case</p>
    <p>
      For a first Chapman dry-deposition exercise, students may use \(v_{d,\mathrm{O_3}} = 0.29\) cm s\(^{-1}\)
      as a demonstration value for ozone deposition to a wheat canopy.
      They should also calculate \(k_{\mathrm{dep,O_3}} = 2.9 \times 10^{-6}\) s\(^{-1}\) for a 1000 m lowest layer.
      The submission must cite the paper and state that this value is surface-, season-, and method-dependent.
    </p>
  </div>

  <div class="hero-actions">
    <a class="pixel-button ghost" href="{{ '/work/patmo/' | relative_url }}">Back to course hub</a>
  </div>
</section>

</div>
