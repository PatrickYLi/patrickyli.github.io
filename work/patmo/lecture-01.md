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
      <li>explain what <code>PATMO</code> can help you do and what it is not built for</li>
      <li>understand why the <code>modern sulfur cycle</code> is the main case of this course</li>
    </ul>
  </div>
</section>

<section id="on-this-page" class="section-block">
  <div class="section-heading">
    <h2>On This Page</h2>
  </div>

  <ol>
    <li><a class="outline-link" href="#patmo-as-a-model">PATMO as a model</a></li>
    <li><a class="outline-link" href="#working-with-patmo">Working with PATMO</a></li>
    <li><a class="outline-link" href="#case-study">Why this course uses the modern sulfur cycle</a></li>
    <li><a class="outline-link" href="#research-workflow">PATMO in research work</a></li>
    <li><a class="outline-link" href="#course-expectations">What this course will teach you</a></li>
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
      <div class="equation-step">
        <p class="equation-label">Layer-by-Layer Representation</p>
        <div class="equation">
          n<sub>i</sub>(z, t) &rarr; { n<sub>i,1</sub>(t), n<sub>i,2</sub>(t), ..., n<sub>i,N</sub>(t) }
        </div>
        <p class="equation-note">
          Here, <em>i</em> is a chemical species and <em>k = 1, 2, ..., N</em> labels the vertical layers.
          Instead of storing one single abundance for the whole atmosphere,
          <code>PATMO</code> stores one value for each species in each layer.
        </p>
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
            &tau;<sub>k,&lambda;</sub> &asymp; &Sigma;<sub>m=k</sub><sup>N</sup> &Sigma;<sub>i</sub>
            n<sub>i,m</sub> &middot; &sigma;<sub>i,&lambda;</sub> &middot; &Delta;z<sub>m</sub>
          </div>
          <p class="equation-note">
            This is the wavelength-dependent opacity above layer <em>k</em>.
            It tells you how much material lies above that layer and how strongly that material absorbs radiation at wavelength <em>&lambda;</em>.
          </p>
        </div>

        <div class="equation-step">
          <p class="equation-label">2. Solar Flux in Each Layer</p>
          <div class="equation">
            I<sub>&lambda;</sub>(z<sub>k</sub>) = I<sub>&lambda;</sub>(&infin;) &middot;
            e<sup>-&tau;<sub>k,&lambda;</sub> / cos &theta;</sup>
          </div>
          <p class="equation-note">
            Once the opacity is known, the incoming solar flux is attenuated as it travels downward.
            Larger opacity means less radiation reaches the layer.
            The factor <em>&theta;</em> is the solar zenith angle.
          </p>
        </div>

        <div class="equation-step">
          <p class="equation-label">3. Photochemical Rate Constant</p>
          <div class="equation">
            J<sub>i,k</sub> = &int;<sub>&lambda;1</sub><sup>&lambda;2</sup>
            &phi;<sub>i</sub>(&lambda;) &middot; &sigma;<sub>i</sub>(&lambda;) &middot; I<sub>&lambda;</sub>(z<sub>k</sub>) d&lambda;
          </div>
          <p class="equation-note">
            This gives the photolysis rate of species <em>i</em> in layer <em>k</em>.
            It combines three ingredients: how efficiently photons cause reaction
            (<em>quantum yield</em>), how strongly the species absorbs radiation
            (<em>cross section</em>), and how much radiation is actually available in that layer.
          </p>
        </div>
      </div>
      <p>
        In short, the logic is:
        opacity controls how much solar radiation survives to a given layer,
        and that surviving radiation sets the local photochemical rate constants.
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
    <h2>Working with PATMO</h2>
  </div>

  <div class="lesson-grid">
    <article class="entry-card">
      <p class="card-label">What You Can Study</p>
      <p>
        If you provide a reaction network, an atmospheric profile, and relevant parameters,
        <code>PATMO</code> can compute how species evolve across altitude.
      </p>
    </article>

    <article class="entry-card">
      <p class="card-label">What You Can Compare</p>
      <p>You can change case-specific settings and compare how the results respond.</p>
      <ul>
        <li>emission parameters</li>
        <li>deposition parameters</li>
        <li>radiation-related settings</li>
        <li>other case-specific inputs</li>
      </ul>
    </article>

    <article class="entry-card">
      <p class="card-label">Sensitivity Experiments</p>
      <p>
        One of the most useful habits in this course is changing one factor while keeping the rest as fixed as possible,
        then examining how the result changes.
      </p>
    </article>

    <article class="entry-card">
      <p class="card-label">What It Is Not Built For</p>
      <p>
        <code>PATMO</code> is not designed to answer full 3D weather or global transport questions,
        and it does not replace observations or scientific judgment.
      </p>
    </article>

    <article class="entry-card">
      <p class="card-label">Why Inputs Matter</p>
      <p>
        Model output only becomes useful when the chosen inputs, parameters, and boundary conditions are meaningful.
        A model cannot rescue a poor setup by itself.
      </p>
    </article>

    <article class="entry-card">
      <p class="card-label">Why This Matters</p>
      <p>
        The value of <code>PATMO</code> is not only that it produces numbers.
        Its value is that it lets you run controlled, comparable, and interpretable computational experiments.
      </p>
    </article>
  </div>

  <pre class="lesson-flow"><code>ask a question
-> organize model inputs
-> run the model
-> inspect outputs
-> compare experiments
-> interpret the results</code></pre>
</section>

<section id="case-study" class="section-block">
  <div class="section-heading">
    <h2>Why This Course Uses the Modern Sulfur Cycle</h2>
  </div>

  <div class="lesson-grid">
    <div class="panel">
      <p>
        This course uses the <code>modern sulfur cycle</code> as its main case study,
        but that does not mean the course is trying to teach the full theory of sulfur chemistry.
      </p>
      <p>
        Instead, this case is used because it is scientifically meaningful and also practical for learning how to work with <code>PATMO</code>.
      </p>
    </div>

    <article class="entry-card">
      <p class="card-label">Why This Case Works Well</p>
      <ul>
        <li>it contains a real reaction network</li>
        <li>it uses vertical profile inputs</li>
        <li>it includes radiation-related inputs</li>
        <li>it contains source and sink settings</li>
        <li>it produces outputs that can be compared and interpreted</li>
      </ul>
    </article>
  </div>

  <div class="panel">
    <p>
      It is also useful because the same case can be revisited throughout the course.
      You do not need to keep adapting to a different scientific context in every lecture.
      Instead, you can move step by step from recognizing the case, to reading its files, to running it, and finally to modifying it.
    </p>
    <p>
      The <code>modern sulfur cycle</code> is therefore not the whole content of the course.
      It is the main training case through which you learn how to work with <code>PATMO</code>.
    </p>
  </div>
</section>

<section id="research-workflow" class="section-block">
  <div class="section-heading">
    <h2>PATMO in Research Work</h2>
  </div>

  <div class="lesson-grid">
    <div class="panel">
      <p>
        A common misunderstanding is to think that “running the model” is the whole research activity.
        It is not.
      </p>
      <p>
        <code>PATMO</code> is not the beginning and it is not the end.
        It is the middle step that connects a scientific question to interpretable results.
      </p>
    </div>

    <div class="panel">
      <p>
        This way of thinking matters because model output is not the end of the task.
        Output is the start of analysis.
      </p>
    </div>
  </div>

  <pre class="lesson-flow"><code>scientific question
-> choose relevant species and processes
-> prepare model inputs
-> run PATMO
-> obtain outputs
-> compare experiments
-> interpret the results</code></pre>
</section>

<section id="course-expectations" class="section-block">
  <div class="section-heading">
    <h2>What This Course Will Teach You</h2>
  </div>

  <div class="lesson-grid">
    <article class="entry-card">
      <p class="card-label">By the End of the Course</p>
      <ul>
        <li>explain what <code>PATMO</code> is for</li>
        <li>recognize the structure of a <code>PATMO</code> case directory</li>
        <li>understand the role of the major input files</li>
        <li>run a prepared case</li>
        <li>find the main output files</li>
        <li>make a simple change to a case</li>
        <li>compare a baseline experiment and a control experiment</li>
      </ul>
    </article>

    <article class="entry-card">
      <p class="card-label">What Is Not the Focus Here</p>
      <ul>
        <li>a full atmospheric chemistry course</li>
        <li>a full sulfur-cycle theory course</li>
        <li>mathematical derivations of numerical solvers</li>
        <li>3D global model systems</li>
        <li>advanced source-code development</li>
      </ul>
      <p>
        You may also hear brief mention of modules that are still under development,
        such as volcano-related functionality, but those will not be major topics in this course.
      </p>
    </article>

    <div class="takeaway-box">
      <p>
        If these points are already clear to you, then you are ready for Lecture 2.
      </p>
    </div>
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
    <p>What kind of model is <code>PATMO</code> in this course?</p>
    <div class="quiz-options">
      <button class="quiz-option" data-choice="a" onclick="checkPatmoQuiz(this)">A. A full 3D global weather prediction model</button>
      <button class="quiz-option" data-choice="b" onclick="checkPatmoQuiz(this)">B. A one-dimensional atmospheric photochemistry model</button>
      <button class="quiz-option" data-choice="c" onclick="checkPatmoQuiz(this)">C. A database of sulfur chemistry observations</button>
    </div>
    <div class="quiz-feedback"></div>
  </div>

  <div class="quiz-block" data-answer="c">
    <h3>2</h3>
    <p>What is one of the main reasons to use <code>PATMO</code>?</p>
    <div class="quiz-options">
      <button class="quiz-option" data-choice="a" onclick="checkPatmoQuiz(this)">A. It automatically tells you which scientific interpretation is correct</button>
      <button class="quiz-option" data-choice="b" onclick="checkPatmoQuiz(this)">B. It replaces real atmospheric observations</button>
      <button class="quiz-option" data-choice="c" onclick="checkPatmoQuiz(this)">C. It lets you run controlled and comparable computational experiments</button>
    </div>
    <div class="quiz-feedback"></div>
  </div>

  <div class="quiz-block" data-answer="a">
    <h3>3</h3>
    <p>Why does this course use the <code>modern sulfur cycle</code>?</p>
    <div class="quiz-options">
      <button class="quiz-option" data-choice="a" onclick="checkPatmoQuiz(this)">A. It is the main training case through which you learn how to work with <code>PATMO</code></button>
      <button class="quiz-option" data-choice="b" onclick="checkPatmoQuiz(this)">B. It is the only atmospheric chemistry topic that <code>PATMO</code> can simulate</button>
      <button class="quiz-option" data-choice="c" onclick="checkPatmoQuiz(this)">C. It replaces the need to understand model inputs and outputs</button>
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
