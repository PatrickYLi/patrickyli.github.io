---
layout: page
title: "Lecture 2: Chapman Cycle and Rate Constants"
subtitle: A direct introduction to the Chapman cycle and how to find kinetic data from JPL and NIST.
permalink: /work/patmo/lecture-02/
body_class: patmo-reading-theme
head-extra:
  - patmo-reading-theme.html
description: Lecture 2 of the PATMO student course, introducing the Chapman cycle and showing how to find reaction rate data in JPL and NIST kinetics databases.
---

<div class="patmo-course">

<section class="panel">
  <div class="lesson-breadcrumb">
    <a href="{{ '/work/patmo/' | relative_url }}">PATMO Course</a>
    <span>/</span>
    <span>Lecture 2 of 7</span>
  </div>

  <p class="section-label">Student Handout</p>
  <p>
    In this lecture, we use the <code>Chapman cycle</code> as the first example of a reaction network.
    The goal is simple: understand the four equations, then learn where to find the rate information needed by a model.
  </p>

  <div class="hero-actions">
    <a class="pixel-button ghost" href="{{ '/work/patmo/lecture-01/' | relative_url }}">Previous Lecture</a>
    <a class="pixel-button secondary" href="{{ '/work/patmo/' | relative_url }}">Course Hub</a>
  </div>
</section>

<section id="chapman-cycle" class="section-block">
  <div class="section-heading">
    <h2>Chapman Cycle</h2>
  </div>

  <div class="panel">
    <p>
      The Chapman cycle is a simple oxygen-only mechanism for explaining the basic formation and destruction of stratospheric ozone.
      It contains two photolysis reactions driven by light and two thermal reactions controlled by rate constants.
    </p>
  </div>

  <div class="takeaway-box">
    <p class="section-label">Four Equations</p>
    <pre class="lesson-flow"><code>R1: O2 + hv -> O + O
R2: O + O2 + M -> O3 + M
R3: O3 + hv -> O2 + O
R4: O + O3 -> O2 + O2</code></pre>
  </div>

  <div class="panel">
    <p>
      Here <code>hv</code> means a photon, and <code>M</code> means a third body molecule such as
      <code>N<sub>2</sub></code> or <code>O<sub>2</sub></code>. The third body is not consumed.
      It removes excess energy so that newly formed <code>O<sub>3</sub></code> can become stable.
    </p>
  </div>
</section>

<section id="equation-by-equation" class="section-block">
  <div class="section-heading">
    <h2>Equation By Equation</h2>
  </div>

  <div class="lesson-grid">
    <article class="entry-card">
      <p class="card-label">R1: Oxygen Photolysis</p>
      <pre><code>O2 + hv -> O + O</code></pre>
      <p>
        Ultraviolet light breaks molecular oxygen into two oxygen atoms.
        This is the starting source of reactive atomic oxygen in the Chapman cycle.
      </p>
      <p>
        Because this reaction is driven by light, its rate is written as <code>J1[O2]</code>.
        The photolysis rate <code>J1</code> has units of <code>s-1</code>.
      </p>
    </article>

    <article class="entry-card">
      <p class="card-label">R2: Ozone Formation</p>
      <pre><code>O + O2 + M -> O3 + M</code></pre>
      <p>
        Atomic oxygen combines with molecular oxygen to form ozone.
        The third body <code>M</code> carries away excess energy.
      </p>
      <p>
        This is a termolecular reaction. Its rate is usually written as
        <code>k2[O][O2][M]</code>, so the rate constant has units of
        <code>cm6 molecule-2 s-1</code>.
      </p>
    </article>

    <article class="entry-card">
      <p class="card-label">R3: Ozone Photolysis</p>
      <pre><code>O3 + hv -> O2 + O</code></pre>
      <p>
        Ozone absorbs light and breaks back into molecular oxygen and atomic oxygen.
        In a more detailed mechanism, the oxygen atom may be separated into different electronic states,
        such as <code>O(3P)</code> and <code>O(1D)</code>.
      </p>
      <p>
        This is also a photolysis reaction, so its rate is written as <code>J3[O3]</code>.
      </p>
    </article>

    <article class="entry-card">
      <p class="card-label">R4: Ozone Loss</p>
      <pre><code>O + O3 -> O2 + O2</code></pre>
      <p>
        Atomic oxygen reacts with ozone and destroys it.
        This reaction closes the oxygen-only ozone cycle.
      </p>
      <p>
        This is a bimolecular thermal reaction. Its rate is written as
        <code>k4[O][O3]</code>, and <code>k4</code> usually has units of
        <code>cm3 molecule-1 s-1</code>.
      </p>
    </article>
  </div>
</section>

<section id="model-network" class="section-block">
  <div class="section-heading">
    <h2>From Equations To A Network</h2>
  </div>

  <div class="panel">
    <p>
      A reaction network is the model-ready version of these equations.
      For each reaction, the model needs the equation, the rate law, the kinetic parameter, the unit, and the source.
    </p>
  </div>

  <div class="panel">
    <table>
      <thead>
        <tr>
          <th>ID</th>
          <th>Reaction</th>
          <th>Type</th>
          <th>Rate term</th>
          <th>What to look up</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <td>R1</td>
          <td><code>O<sub>2</sub> + hv -> O + O</code></td>
          <td>photolysis</td>
          <td><code>J1[O<sub>2</sub>]</code></td>
          <td>photolysis data for <code>O<sub>2</sub></code></td>
        </tr>
        <tr>
          <td>R2</td>
          <td><code>O + O<sub>2</sub> + M -> O<sub>3</sub> + M</code></td>
          <td>termolecular</td>
          <td><code>k2[O][O<sub>2</sub>][M]</code></td>
          <td>temperature and pressure dependent <code>k2</code></td>
        </tr>
        <tr>
          <td>R3</td>
          <td><code>O<sub>3</sub> + hv -> O<sub>2</sub> + O</code></td>
          <td>photolysis</td>
          <td><code>J3[O<sub>3</sub>]</code></td>
          <td>photolysis data for <code>O<sub>3</sub></code></td>
        </tr>
        <tr>
          <td>R4</td>
          <td><code>O + O<sub>3</sub> -> O<sub>2</sub> + O<sub>2</sub></code></td>
          <td>bimolecular</td>
          <td><code>k4[O][O<sub>3</sub>]</code></td>
          <td>temperature dependent <code>k4</code></td>
        </tr>
      </tbody>
    </table>
  </div>
</section>

<section id="find-rate-data" class="section-block">
  <div class="section-heading">
    <h2>One Worked Database Example</h2>
  </div>

  <div class="panel">
    <p>
      We will only demonstrate the database workflow with one reaction:
    </p>
    <pre><code>O + O3 -> O2 + O2</code></pre>
    <p>
      The remaining Chapman reactions should be found by students using the same method.
    </p>
  </div>

  <div class="lesson-grid">
    <article class="entry-card">
      <p class="card-label">Step 1: Search JPL</p>
      <p>
        Open the <a href="https://jpldataeval.jpl.nasa.gov/" target="_blank" rel="noreferrer">JPL Data Evaluation</a>.
        Search the latest evaluation PDF for <code>O + O3</code>, <code>O3 + O</code>, or <code>O(3P) + O3</code>.
      </p>
      <p>
        JPL is the first stop because it gives evaluated recommendations for atmospheric modeling.
      </p>
    </article>

    <article class="entry-card">
      <p class="card-label">Step 2: Record The JPL Entry</p>
      <p>
        For this reaction, a common evaluated expression is:
      </p>
      <pre><code>k(T) = 8.0e-12 exp(-2060/T)</code></pre>
      <p>
        Record the units <code>cm3 molecule-1 s-1</code>, the temperature range, and the JPL note number.
      </p>
    </article>

    <article class="entry-card">
      <p class="card-label">Step 3: Search NIST</p>
      <p>
        Open the <a href="https://kinetics.nist.gov/kinetics/index.jsp" target="_blank" rel="noreferrer">NIST Chemical Kinetics Database</a>.
        Search for the same reactants and products.
      </p>
      <p>
        NIST is useful for checking the literature records behind the reaction.
      </p>
    </article>

    <article class="entry-card">
      <p class="card-label">Step 4: Compare</p>
      <p>
        Compare the NIST records with the JPL recommendation.
        Pay attention to temperature range, reaction order, data type, authors, and year.
      </p>
      <p>
        If you use the value in a model, cite the database and keep the original literature trail.
      </p>
    </article>
  </div>

  <div class="takeaway-box">
    <p class="section-label">What Students Should Copy</p>
    <p>
      Do not copy only the number. Copy the reaction, rate expression, units, temperature range,
      database name, evaluation or record information, and citation notes.
    </p>
  </div>
</section>

<section id="reference-data" class="section-block">
  <div class="section-heading">
    <h2>Reference Data To Check</h2>
  </div>

  <div class="panel">
    <p>
      The table below is only a reference target. Students still need to use JPL and NIST themselves
      to confirm the entries and complete the source information.
    </p>
  </div>

  <div class="panel">
    <table>
      <thead>
        <tr>
          <th>ID</th>
          <th>Reaction</th>
          <th>Reference data</th>
          <th>Student task</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <td>R1</td>
          <td><code>O<sub>2</sub> + hv -> O + O</code></td>
          <td>No fixed thermal <code>k</code>; use <code>J1</code> from photolysis inputs.</td>
          <td>Find JPL photolysis data for <code>O<sub>2</sub></code>.</td>
        </tr>
        <tr>
          <td>R2</td>
          <td><code>O + O<sub>2</sub> + M -> O<sub>3</sub> + M</code></td>
          <td>Common low-pressure form: <code>k2 = 6.0e-34 (T/300)^-2.4</code>.</td>
          <td>Check units, third-body assumptions, and temperature range.</td>
        </tr>
        <tr>
          <td>R3</td>
          <td><code>O<sub>3</sub> + hv -> O<sub>2</sub> + O</code></td>
          <td>No fixed thermal <code>k</code>; use <code>J3</code> from photolysis inputs.</td>
          <td>Find JPL photolysis data and note product channels.</td>
        </tr>
        <tr>
          <td>R4</td>
          <td><code>O + O<sub>3</sub> -> O<sub>2</sub> + O<sub>2</sub></code></td>
          <td><code>k4 = 8.0e-12 exp(-2060/T)</code>, units <code>cm3 molecule-1 s-1</code>.</td>
          <td>Use this as the worked example, then compare with NIST records.</td>
        </tr>
      </tbody>
    </table>
  </div>
</section>

<section id="after-class-task" class="section-block">
  <div class="section-heading">
    <h2>After-Class Task</h2>
  </div>

  <div class="panel">
    <p>
      We demonstrated the database workflow with only one reaction:
      <code>O + O<sub>3</sub> -> O<sub>2</sub> + O<sub>2</sub></code>.
      Now complete the remaining entries yourself.
    </p>
    <ol>
      <li>Use JPL to find the photolysis information for <code>O<sub>2</sub></code>.</li>
      <li>Use JPL and NIST to check <code>O + O<sub>2</sub> + M -> O<sub>3</sub> + M</code>.</li>
      <li>Use JPL to find the photolysis information for <code>O<sub>3</sub></code>.</li>
      <li>Make a final Chapman table with reaction, rate term, units, source, and notes.</li>
    </ol>
  </div>
</section>

<section id="quick-check-quiz" class="section-block">
  <div class="section-heading">
    <h2>Quick Check</h2>
  </div>

  <div class="quiz-block" data-answer="b">
    <h3>Question 1</h3>
    <p>Which equation forms ozone in the Chapman cycle?</p>
    <div class="quiz-options">
      <button class="quiz-option" data-choice="a" onclick="checkPatmoLecture2Quiz(this)">A. <code>O<sub>3</sub> + hv -> O<sub>2</sub> + O</code></button>
      <button class="quiz-option" data-choice="b" onclick="checkPatmoLecture2Quiz(this)">B. <code>O + O<sub>2</sub> + M -> O<sub>3</sub> + M</code></button>
      <button class="quiz-option" data-choice="c" onclick="checkPatmoLecture2Quiz(this)">C. <code>O + O<sub>3</sub> -> O<sub>2</sub> + O<sub>2</sub></code></button>
    </div>
    <div class="quiz-feedback"></div>
  </div>

  <div class="quiz-block" data-answer="a">
    <h3>Question 2</h3>
    <p>What type of parameter is used for photolysis reactions?</p>
    <div class="quiz-options">
      <button class="quiz-option" data-choice="a" onclick="checkPatmoLecture2Quiz(this)">A. <code>J</code>, with units of <code>s-1</code></button>
      <button class="quiz-option" data-choice="b" onclick="checkPatmoLecture2Quiz(this)">B. <code>k</code>, always with units of <code>cm6 molecule-2 s-1</code></button>
      <button class="quiz-option" data-choice="c" onclick="checkPatmoLecture2Quiz(this)">C. No rate parameter is needed</button>
    </div>
    <div class="quiz-feedback"></div>
  </div>

  <div class="hero-actions">
    <a class="pixel-button ghost" href="{{ '/work/patmo/' | relative_url }}">Back to course hub</a>
  </div>
</section>

<script>
  function checkPatmoLecture2Quiz(button) {
    const block = button.closest('.quiz-block');
    const answer = block.dataset.answer;
    const choice = button.dataset.choice;
    const feedback = block.querySelector('.quiz-feedback');
    const options = block.querySelectorAll('.quiz-option');

    options.forEach((option) => option.classList.remove('correct-choice'));

    if (choice === answer) {
      button.classList.add('correct-choice');
      feedback.textContent = 'Correct.';
      feedback.className = 'quiz-feedback correct';
    } else {
      feedback.textContent = 'Not quite. Try again.';
      feedback.className = 'quiz-feedback incorrect';
    }
  }
</script>

</div>
