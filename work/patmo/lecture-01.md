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
    <li><a class="outline-link" href="#working-with-patmo">The case directory you will read</a></li>
    <li><a class="outline-link" href="#case-study">How the modern sulfur cycle enters PATMO</a></li>
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

<section id="working-with-patmo" class="section-block">
  <div class="section-heading">
    <h2>The Case Directory You Will Read</h2>
  </div>

  <div class="panel">
    <p>
      If you want to learn <code>PATMO</code> as a working research model, the best place to start is not the entire source tree.
      The best place to start is one complete case directory.
      In this course, that directory is <code>tests/modern_sulfur_cycle/</code>.
    </p>
    <pre><code>tests/modern_sulfur_cycle/
├── options.opt
├── reaction_network.ntw
├── profile.dat
├── solar_flux.txt
├── test.f90
├── copylist.pcp
├── Rainout-*.txt / deposition files
└── other case-specific helper inputs</code></pre>
  </div>

  <div class="lesson-grid">
    <article class="entry-card">
      <p class="card-label">Why Start Here</p>
      <p>
        This folder already contains the scientific problem, the numerical setup, and the driver program.
        That makes it the most useful entry point for a student who wants to understand how <code>PATMO</code> is actually used.
      </p>
    </article>

    <article class="entry-card">
      <p class="card-label">What This Means in Practice</p>
      <p>
        In Lecture 1, the goal is not to read every solver routine.
        The goal is to recognize how one atmospheric chemistry problem is assembled into files that <code>PATMO</code> can run.
      </p>
    </article>

    <article class="entry-card">
      <p class="card-label">A Small but Important File</p>
      <p>
        <code>copylist.pcp</code> lists the files that must be copied into the build directory for this case.
        This tells you something important about <code>PATMO</code>:
        a model run is built from a case folder, not from one giant monolithic input file.
      </p>
      <pre><code>test.f90
profile.dat
solar_flux.txt
...</code></pre>
      <p>
        In Lecture 1, it is enough to understand that a case is assembled from several input files.
        You do not need to study every helper file yet.
      </p>
    </article>
  </div>
</section>

<section id="case-study" class="section-block">
  <div class="section-heading">
    <h2>How the Modern Sulfur Cycle Enters PATMO</h2>
  </div>

  <div class="panel">
    <p>
      The <code>modern sulfur cycle</code> is useful in this course because it is not just a chemistry diagram.
      In <code>PATMO</code>, it becomes a set of concrete files that control reactions, profiles, radiation, deposition, emissions, and outputs.
    </p>
    <p>
      The practical question for a student is therefore not only “what is sulfur chemistry?”
      It is also “where is that chemistry stored in the code and input files?”
    </p>
  </div>

  <div class="model-note-stack">
    <article class="entry-card">
      <p class="card-label">1. <code>options.opt</code>: Global Settings for This Case</p>
      <pre><code>cellsNumber = 60
zenith_angle = 60
usePhotochemistry = T
emission_species = COS:..., CS2:..., H2S:..., SO2:..., CH3SCH3:...
drydep_species = COS:..., CS2:..., SO2:..., H2S:..., CH3SCH3:...</code></pre>
      <p>
        This file turns the scientific idea into numerical settings.
        It defines the vertical grid, the radiation geometry, whether photochemistry is active,
        and which sulfur species are emitted or dry-deposited.
      </p>
    </article>

    <article class="entry-card">
      <p class="card-label">2. <code>reaction_network.ntw</code>: The Sulfur Chemistry Itself</p>
      <pre><code>13,H2S,OH,,H2O,SH,,...
26,SO2,O,M,SO3,M,,...
33,SO2,OH,M,HSO3,M,,...
34,SO3,H2O,,H2SO4,,,...
37,SO2,,,SO4,,,0</code></pre>
      <p>
        This file is where the sulfur cycle becomes an explicit chemical mechanism.
        Here you can see species such as <code>H2S</code>, <code>SO2</code>, <code>SO3</code>, <code>H2SO4</code>, and <code>SO4</code>
        connected by reaction lines that the model can evaluate.
      </p>
    </article>

    <article class="entry-card">
      <p class="card-label">3. <code>profile.dat</code> and <code>solar_flux.txt</code>: The Column and the Radiation Field</p>
      <pre><code>index alt ... Kzz Tgas ... COS CS2 H2S SO2 SO3 H2SO4 SO4 ...
1     1   ... 1.0E5 282  ... 0   0   0   0   0   0     0</code></pre>
      <p>
        <code>profile.dat</code> gives the vertical atmospheric structure layer by layer.
        It contains altitude, mixing strength <code>Kzz</code>, temperature, and species abundances.
        <code>solar_flux.txt</code> provides the incoming radiation spectrum that drives photochemistry.
      </p>
    </article>

    <article class="entry-card">
      <p class="card-label">4. <code>test.f90</code>: The Driver of the Whole Experiment</p>
      <pre><code>call patmo_init()
call patmo_loadInitialProfile("profile.dat",unitH="km",unitX="1/cm3")
call patmo_setFluxBB()
call patmo_dumpJValue("jvalue.dat")
call patmo_dumpOpacity("opacity.dat")
call patmo_dumpAllRates("rates.dat")
call patmo_dumpAllMixingRatioToFile("allNDs.dat")</code></pre>
      <p>
        This is the Fortran driver program for the case.
        It initializes the model, loads the atmospheric profile, sets the radiation field, and writes the main outputs.
        If you want to see where a <code>PATMO</code> run actually starts and ends, this is the file to open.
      </p>
    </article>
  </div>

  <div class="takeaway-box">
    <p><strong>Practical takeaway.</strong></p>
    <p>
      In this course, the <code>modern sulfur cycle</code> is not just a topic.
      It is a concrete <code>PATMO</code> case that you can read file by file.
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
    <p>Which file in <code>tests/modern_sulfur_cycle/</code> contains the sulfur reaction list used by the model?</p>
    <div class="quiz-options">
      <button class="quiz-option" data-choice="a" onclick="checkPatmoQuiz(this)">A. <code>solar_flux.txt</code></button>
      <button class="quiz-option" data-choice="b" onclick="checkPatmoQuiz(this)">B. <code>reaction_network.ntw</code></button>
      <button class="quiz-option" data-choice="c" onclick="checkPatmoQuiz(this)">C. <code>allNDs.dat</code></button>
    </div>
    <div class="quiz-feedback"></div>
  </div>

  <div class="quiz-block" data-answer="a">
    <h3>2</h3>
    <p>Which file acts as the driver of the case by calling <code>patmo_init</code>, loading the profile, and writing the output files?</p>
    <div class="quiz-options">
      <button class="quiz-option" data-choice="a" onclick="checkPatmoQuiz(this)">A. <code>test.f90</code></button>
      <button class="quiz-option" data-choice="b" onclick="checkPatmoQuiz(this)">B. <code>profile.dat</code></button>
      <button class="quiz-option" data-choice="c" onclick="checkPatmoQuiz(this)">C. <code>copylist.pcp</code></button>
    </div>
    <div class="quiz-feedback"></div>
  </div>

  <div class="quiz-block" data-answer="a">
    <h3>3</h3>
    <p>If you want to see the final vertical profiles of sulfur species after a run, which file should you open first?</p>
    <div class="quiz-options">
      <button class="quiz-option" data-choice="a" onclick="checkPatmoQuiz(this)">A. <code>allNDs.dat</code></button>
      <button class="quiz-option" data-choice="b" onclick="checkPatmoQuiz(this)">B. <code>opacity.dat</code></button>
      <button class="quiz-option" data-choice="c" onclick="checkPatmoQuiz(this)">C. <code>copylist.pcp</code></button>
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
