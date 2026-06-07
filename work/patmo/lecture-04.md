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
      <p class="card-label">Dry Deposition</p>
      <p>
        Ozone dry deposition can occur on vegetation, soil, water, snow, building surfaces, and other lower-boundary surfaces.
        The wheat-canopy paper below is only one literature example for learning how to find a deposition velocity.
      </p>
    </article>

    <article class="entry-card">
      <p class="card-label">Emission</p>
      <p>
        Surface ozone is usually not emitted directly. It is mainly produced photochemically from precursors such as
        <code>NOx</code> and <code>VOCs</code>. In a Chapman-only exercise, direct <code>O3</code> emission is therefore an artificial training source.
      </p>
    </article>

    <article class="entry-card">
      <p class="card-label">Wet Deposition</p>
      <p>
        Wet deposition removes species by cloud water and precipitation.
        It is important for soluble or efficiently scavenged species, but it is not a useful main sink for the simple Chapman <code>O3</code> example.
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

<section id="emission-paper-example" class="section-block">
  <div class="section-heading">
    <h2>Finding Emission Data From A Paper</h2>
  </div>

  <div class="panel">
    <p>
      When students search for <code>ozone emission</code>, they should quickly check whether the paper is talking about direct <code>O3</code>
      emission or emissions of ozone precursors. In most lower-atmosphere cases, the useful search target is precursor emission.
    </p>
    <p>
      A good example is
      <a href="https://doi.org/10.5194/acp-23-1043-2023" target="_blank" rel="noreferrer">Guion et al. (2023)</a>,
      which connects biogenic isoprene emissions, ozone dry deposition velocity, and surface ozone in southwestern Europe.
    </p>
  </div>

  <div class="lesson-grid">
    <article class="entry-card">
      <p class="card-label">Step 1: Search The Right Species</p>
      <p>
        Use keywords such as <code>ozone precursor emissions</code>, <code>isoprene emissions surface ozone</code>,
        <code>VOC emissions ozone formation</code>, or <code>NOx emissions ozone</code>.
      </p>
    </article>

    <article class="entry-card">
      <p class="card-label">Step 2: Identify The Emitted Species</p>
      <p>
        In Guion et al. (2023), the emission variable is biogenic isoprene, written as <code>C5H8</code>, not <code>O3</code>.
        Students should copy the emitted species name before copying any number.
      </p>
      <p>
        There is no direct stoichiometric conversion such as <code>1 C5H8 -> x O3</code>.
        <code>C5H8</code> is an ozone precursor: it can affect <code>O3</code> only through a larger <code>VOC-NOx</code> photochemical mechanism.
        In a Chapman-only network, <code>C5H8</code> cannot be converted into <code>O3</code> because the required precursor chemistry is not included.
      </p>
    </article>

    <article class="entry-card">
      <p class="card-label">Step 3: Copy Units And Source Type</p>
      <p>
        Guion et al. show daily mean <code>C5H8</code> emissions in units of g m<sup>-2</sup> h<sup>-1</sup>.
        Record whether the source is biogenic, anthropogenic, biomass burning, oceanic, volcanic, or another category.
      </p>
    </article>

    <article class="entry-card">
      <p class="card-label">Step 4: Decide If Chapman Can Use It</p>
      <p>
        A pure Chapman network has only <code>O2</code>, <code>O</code>, <code>O3</code>, photons, and a third body <code>M</code>.
        If the paper reports <code>VOC</code> or <code>NOx</code> emissions, the data are useful for learning emission lookup, but they require a larger chemical mechanism.
      </p>
    </article>
  </div>

  <div class="panel">
    <p class="section-label">Worked Conversion</p>
    <p>
      For a simple exercise, take a representative isoprene emission flux from the order of magnitude shown in the paper:
      \(E_{\mathrm{mass}} = 1.0 \times 10^{-3}\) g m\(^{-2}\) h\(^{-1}\) for <code>C5H8</code>.
      This is a surface mass flux. A chemistry model usually needs a number-based source term.
    </p>
    <p>
      The map in Guion et al. (2023) is spatially variable. High-emission regions are on the order of
      \(1.5 \times 10^{-3}\) g m\(^{-2}\) h\(^{-1}\), so students should first choose the grid cell,
      region, or condition they want to represent before converting units.
    </p>

    \[
    F_{\mathrm{num}}
    =
    E_{\mathrm{mass}}
    \frac{N_A}{M_{\mathrm{C_5H_8}}}
    \frac{1}{10^4}
    \frac{1}{3600}
    \]

    <p>
      Using \(N_A = 6.022 \times 10^{23}\) molecules mol\(^{-1}\) and
      \(M_{\mathrm{C_5H_8}} = 68.12\) g mol\(^{-1}\):
    </p>

    \[
    F_{\mathrm{num}}
    \approx
    2.46 \times 10^{11}
    \ \mathrm{molecules\ cm^{-2}\ s^{-1}}
    \]

    <p>
      If the lowest model layer thickness is \(\Delta z_1 = 1000\) m \(= 1.0 \times 10^5\) cm,
      convert the surface flux to a volume source term:
    </p>

    \[
    S_{\mathrm{C_5H_8}}
    =
    \frac{F_{\mathrm{num}}}{\Delta z_1}
    \approx
    2.46 \times 10^6
    \ \mathrm{molecules\ cm^{-3}\ s^{-1}}
    \]

    <p>
      This final value is the type of number-density tendency that can be compared with chemical production and loss terms.
      If students instead use \(1.5 \times 10^{-3}\) g m\(^{-2}\) h\(^{-1}\), the same calculation gives
      \(F_{\mathrm{num}} \approx 3.69 \times 10^{11}\) molecules cm\(^{-2}\) s\(^{-1}\) and,
      for \(\Delta z_1 = 1000\) m, \(S_{\mathrm{C_5H_8}} \approx 3.69 \times 10^6\) molecules cm\(^{-3}\) s\(^{-1}\).
      If students choose a different emission flux or a different lowest-layer thickness, they must redo this conversion.
    </p>
  </div>

  <div class="takeaway-box">
    <p class="section-label">Teaching Use In This Case</p>
    <p>
      For the Chapman exercise, keep direct <code>O3</code> emission at zero in the physical baseline.
      Use the paper example to teach the search workflow: identify emitted species, source type, units, region, period,
      convert the reported flux to molecules cm<sup>-2</sup> s<sup>-1</sup>, and then convert it to molecules cm<sup>-3</sup> s<sup>-1</sup>
      using the thickness of the model layer receiving the emission.
      Do not reinterpret the resulting <code>C5H8</code> source term as an <code>O3</code> source term unless a separate ozone-production yield is explicitly defined for a larger chemical mechanism.
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

  <div class="takeaway-box">
    <p class="section-label">Teaching Use In This Case</p>
    <p>
      For a first Chapman dry-deposition exercise, students may use \(v_{d,\mathrm{O_3}} = 0.29\) cm s\(^{-1}\)
      as a demonstration value for ozone deposition to a wheat canopy.
      The submission must cite the paper and state that this value is surface-, season-, and method-dependent.
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
        <td>Baseline: set direct O3 emission to zero; use precursor-emission papers only as a lookup example</td>
      </tr>
      <tr>
        <td>Dry deposition</td>
        <td>Species, deposition velocity, units, surface-layer treatment</td>
        <td>Add dry deposition for O3; vegetation is one example surface, not the only possible surface</td>
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
      <li>For the physical baseline, set direct ozone emission to zero. Then use one paper, such as Guion et al. (2023), to explain how ozone-related precursor emission data are searched, recorded, and converted from mass flux to molecules cm<sup>-3</sup> s<sup>-1</sup>.</li>
      <li>Add dry deposition for <code>O3</code>. Use the vegetation example from Zhang et al. (2024), or find another paper and record the deposition velocity, units, surface type, site, season, and whether the removal applies only to the lowest layer.</li>
      <li>Keep wet deposition off for the strict Chapman <code>O3</code> case and explain that wet removal is mainly useful for soluble or efficiently scavenged species.</li>
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
