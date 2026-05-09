---
layout: page
title: "Lecture 2: Building a Reaction Network"
subtitle: Student handout for using the Chapman cycle to learn reaction networks and kinetic data lookup.
permalink: /work/patmo/lecture-02/
body_class: patmo-reading-theme
head-extra:
  - patmo-reading-theme.html
description: Lecture 2 of the PATMO student course, introducing the Chapman cycle and teaching students how to find reaction rate data in JPL and NIST kinetics databases.
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
    This lecture teaches you how to turn atmospheric chemistry into a small reaction network.
    We will use the <code>Chapman cycle</code> as the example because it is compact,
    famous, and just complex enough to show the main habits needed for larger mechanisms.
  </p>
  <p>
    The main practical skill is database reading. By the end of this lecture, you should know how to use
    the <a href="https://jpldataeval.jpl.nasa.gov/" target="_blank" rel="noreferrer">JPL Data Evaluation</a>
    and the <a href="https://kinetics.nist.gov/kinetics/index.jsp" target="_blank" rel="noreferrer">NIST Chemical Kinetics Database</a>
    to find, check, and record reaction rate information for a model-ready network.
  </p>

  <div class="lesson-grid">
    <article class="entry-card">
      <p class="card-label">Question 1</p>
      <p><strong>What is the Chapman cycle?</strong></p>
      <p>A four-reaction oxygen-only mechanism that explains basic stratospheric ozone formation and loss.</p>
    </article>

    <article class="entry-card">
      <p class="card-label">Question 2</p>
      <p><strong>What is a reaction network?</strong></p>
      <p>A structured list of species, reactions, rate laws, and kinetic parameters that a model can integrate.</p>
    </article>

    <article class="entry-card">
      <p class="card-label">Question 3</p>
      <p><strong>What data do we need?</strong></p>
      <p>For thermal reactions we need <code>k(T)</code>. For photolysis reactions we need <code>J</code>, or the inputs needed to calculate <code>J</code>.</p>
    </article>
  </div>

  <div class="lesson-anchor-nav">
    <a href="#outline">Outline</a>
    <a href="#goals">Goals</a>
    <a href="#reaction-network">Reaction Network</a>
    <a href="#chapman-cycle">Chapman Cycle</a>
    <a href="#rate-laws">Rate Laws</a>
    <a href="#database-map">Database Map</a>
    <a href="#using-jpl">JPL</a>
    <a href="#using-nist">NIST</a>
    <a href="#worked-example">Example</a>
    <a href="#network-table">Network Table</a>
    <a href="#common-mistakes">Mistakes</a>
    <a href="#lecture-summary">Summary</a>
    <a href="#quick-check-quiz">Quiz</a>
  </div>

  <div class="hero-actions">
    <a class="pixel-button ghost" href="{{ '/work/patmo/lecture-01/' | relative_url }}">Previous Lecture</a>
    <a class="pixel-button secondary" href="{{ '/work/patmo/' | relative_url }}">Course Hub</a>
  </div>
</section>

<section id="outline" class="section-block">
  <div class="section-heading">
    <h2>Lecture Outline</h2>
  </div>

  <div class="outline-grid">
    <a class="outline-link" href="#goals">What You Should Learn</a>
    <a class="outline-link" href="#reaction-network">What A Reaction Network Means</a>
    <a class="outline-link" href="#chapman-cycle">The Chapman Cycle</a>
    <a class="outline-link" href="#rate-laws">Rate Laws And Units</a>
    <a class="outline-link" href="#database-map">Which Database To Use First</a>
    <a class="outline-link" href="#using-jpl">How To Use The JPL Data Evaluation</a>
    <a class="outline-link" href="#using-nist">How To Use The NIST Kinetics Database</a>
    <a class="outline-link" href="#worked-example">A Worked Chapman Example</a>
    <a class="outline-link" href="#network-table">The Final Reaction Network Table</a>
    <a class="outline-link" href="#common-mistakes">Common Mistakes</a>
    <a class="outline-link" href="#after-class-task">After-Class Task</a>
    <a class="outline-link" href="#quick-check-quiz">Quick Check Quiz</a>
  </div>
</section>

<section id="goals" class="section-block">
  <div class="section-heading">
    <h2>What You Should Learn From This Lecture</h2>
  </div>

  <div class="course-overview-grid">
    <div class="panel">
      <p class="section-label">By The End</p>
      <ul>
        <li>explain the four reactions in the <code>Chapman cycle</code></li>
        <li>identify species, reactions, and rate laws in a small reaction network</li>
        <li>distinguish thermal rate constants <code>k</code> from photolysis rates <code>J</code></li>
        <li>use JPL as a first stop for atmospheric model recommendations</li>
        <li>use NIST to inspect literature records and compare kinetic measurements</li>
        <li>record rate expressions with units, temperature ranges, and source information</li>
      </ul>
    </div>

    <div class="takeaway-box">
      <p class="section-label">Main Habit</p>
      <p>
        Never copy only a number. A usable kinetic entry needs the reaction, rate expression,
        units, temperature range, pressure or bath gas when relevant, and source.
      </p>
    </div>
  </div>
</section>

<section id="reaction-network" class="section-block">
  <div class="section-heading">
    <h2>What A Reaction Network Means</h2>
  </div>

  <div class="lesson-grid">
    <article class="entry-card">
      <p class="card-label">Species</p>
      <p>
        Species are the chemical objects whose abundances the model tracks.
        In the Chapman cycle the main species are <code>O<sub>2</sub></code>,
        <code>O</code>, and <code>O<sub>3</sub></code>.
      </p>
    </article>

    <article class="entry-card">
      <p class="card-label">Reactions</p>
      <p>
        Reactions describe how species are produced and destroyed.
        Each reaction becomes one term in the time-evolution equations.
      </p>
    </article>

    <article class="entry-card">
      <p class="card-label">Rate Laws</p>
      <p>
        A rate law tells the model how fast a reaction proceeds.
        It connects a kinetic parameter, such as <code>k(T)</code> or <code>J</code>,
        with species concentrations.
      </p>
    </article>

    <article class="entry-card">
      <p class="card-label">Metadata</p>
      <p>
        Metadata makes the network scientifically traceable:
        source database, reference, temperature range, units, uncertainty, and notes.
      </p>
    </article>
  </div>

  <pre class="lesson-flow"><code>species list
+ reaction list
+ rate laws
+ kinetic data
+ source notes
= reaction network</code></pre>
</section>

<section id="chapman-cycle" class="section-block">
  <div class="section-heading">
    <h2>The Chapman Cycle</h2>
  </div>

  <div class="panel">
    <p>
      The Chapman cycle is the classic oxygen-only mechanism for stratospheric ozone.
      It explains how sunlight can make ozone from molecular oxygen and how ozone is also destroyed.
      NASA's public ozone overview describes the same basic idea: ultraviolet light splits
      <code>O<sub>2</sub></code>, and the resulting oxygen atoms can combine with
      <code>O<sub>2</sub></code> to form <code>O<sub>3</sub></code>.
    </p>
    <p>
      This simple cycle is not the whole chemistry of the real stratosphere.
      Real ozone abundance is also affected by catalytic families such as <code>NOx</code>,
      <code>HOx</code>, <code>ClOx</code>, and <code>BrOx</code>.
      For learning how to build a network, however, Chapman is a clean starting point.
    </p>
  </div>

  <div class="lesson-grid">
    <article class="entry-card">
      <p class="card-label">R1: Oxygen Photolysis</p>
      <pre><code>O2 + photon -> O + O</code></pre>
      <p>
        Shortwave ultraviolet radiation splits molecular oxygen into two oxygen atoms.
        This is a photolysis reaction, so the rate parameter is written as <code>J1</code>, not <code>k1</code>.
      </p>
    </article>

    <article class="entry-card">
      <p class="card-label">R2: Ozone Formation</p>
      <pre><code>O + O2 + M -> O3 + M</code></pre>
      <p>
        An oxygen atom combines with molecular oxygen. The third body <code>M</code>,
        usually mostly <code>N<sub>2</sub></code> and <code>O<sub>2</sub></code> in air,
        carries away excess energy and stabilizes ozone.
      </p>
    </article>

    <article class="entry-card">
      <p class="card-label">R3: Ozone Photolysis</p>
      <pre><code>O3 + photon -> O2 + O</code></pre>
      <p>
        Ozone also absorbs radiation and breaks apart.
        A detailed mechanism may separate products such as <code>O(3P)</code> and <code>O(1D)</code>,
        but this lecture keeps the basic Chapman form.
      </p>
    </article>

    <article class="entry-card">
      <p class="card-label">R4: Ozone Loss</p>
      <pre><code>O + O3 -> O2 + O2</code></pre>
      <p>
        Atomic oxygen reacts with ozone to make two oxygen molecules.
        This is a thermal bimolecular reaction, so it uses a temperature-dependent <code>k4(T)</code>.
      </p>
    </article>
  </div>

  <div class="takeaway-box">
    <p class="section-label">Network View</p>
    <pre class="lesson-flow"><code>O2 --J1--> O + O
O + O2 + M --k2--> O3 + M
O3 --J3--> O2 + O
O + O3 --k4--> O2 + O2</code></pre>
  </div>
</section>

<section id="rate-laws" class="section-block">
  <div class="section-heading">
    <h2>Rate Laws And Units</h2>
  </div>

  <div class="lesson-grid">
    <article class="entry-card">
      <p class="card-label">Photolysis</p>
      <pre><code>rate = J [species]</code></pre>
      <p>
        <code>J</code> has units of <code>s-1</code>.
        It is not a fixed universal constant because it depends on radiation,
        wavelength-dependent absorption, quantum yield, altitude, solar zenith angle, and shielding.
      </p>
    </article>

    <article class="entry-card">
      <p class="card-label">Bimolecular Reaction</p>
      <pre><code>rate = k [A] [B]</code></pre>
      <p>
        <code>k</code> usually has units of <code>cm3 molecule-1 s-1</code>
        in atmospheric chemistry databases.
      </p>
    </article>

    <article class="entry-card">
      <p class="card-label">Termolecular Reaction</p>
      <pre><code>rate = k [A] [B] [M]</code></pre>
      <p>
        <code>k</code> often has units of <code>cm6 molecule-2 s-1</code>.
        The bath gas and pressure regime matter, so you must read the database notes.
      </p>
    </article>

    <article class="entry-card">
      <p class="card-label">Temperature Dependence</p>
      <pre><code>k(T) = A exp(-E_over_R / T)</code></pre>
      <p>
        Many database entries use an Arrhenius-style form.
        Always check whether the exponent is written with <code>E/R</code>, <code>Ea/R</code>,
        or an energy in <code>J mole-1</code>.
      </p>
    </article>
  </div>

  <div class="panel">
    <p class="section-label">Chapman Rate Terms</p>
    <table>
      <thead>
        <tr>
          <th>Reaction</th>
          <th>Type</th>
          <th>Rate term</th>
          <th>Parameter unit</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <td><code>O<sub>2</sub> + photon -> O + O</code></td>
          <td>photolysis</td>
          <td><code>J1 [O<sub>2</sub>]</code></td>
          <td><code>s-1</code></td>
        </tr>
        <tr>
          <td><code>O + O<sub>2</sub> + M -> O<sub>3</sub> + M</code></td>
          <td>termolecular</td>
          <td><code>k2 [O] [O<sub>2</sub>] [M]</code></td>
          <td><code>cm6 molecule-2 s-1</code></td>
        </tr>
        <tr>
          <td><code>O<sub>3</sub> + photon -> O<sub>2</sub> + O</code></td>
          <td>photolysis</td>
          <td><code>J3 [O<sub>3</sub>]</code></td>
          <td><code>s-1</code></td>
        </tr>
        <tr>
          <td><code>O + O<sub>3</sub> -> O<sub>2</sub> + O<sub>2</sub></code></td>
          <td>bimolecular</td>
          <td><code>k4 [O] [O<sub>3</sub>]</code></td>
          <td><code>cm3 molecule-1 s-1</code></td>
        </tr>
      </tbody>
    </table>
  </div>
</section>

<section id="database-map" class="section-block">
  <div class="section-heading">
    <h2>Which Database To Use First</h2>
  </div>

  <div class="lesson-grid">
    <div class="panel">
      <p class="section-label">JPL Data Evaluation</p>
      <p>
        The JPL evaluation is designed for atmospheric studies and model use.
        It provides critically evaluated kinetic and photochemical data,
        including recommendations, uncertainty information, and notes.
      </p>
      <p>
        For atmospheric modeling, use JPL first when you need a recommended value.
      </p>
      <div class="hero-actions">
        <a class="pixel-button" href="https://jpldataeval.jpl.nasa.gov/" target="_blank" rel="noreferrer">Open JPL</a>
      </div>
    </div>

    <div class="panel">
      <p class="section-label">NIST Chemical Kinetics Database</p>
      <p>
        NIST Standard Reference Database 17 is a searchable compilation of gas-phase kinetics data.
        It is especially useful when you want to inspect individual literature records,
        compare measurements, or find the original authors behind a rate expression.
      </p>
      <div class="hero-actions">
        <a class="pixel-button secondary" href="https://kinetics.nist.gov/kinetics/index.jsp" target="_blank" rel="noreferrer">Open NIST</a>
      </div>
    </div>
  </div>

  <div class="panel">
    <table>
      <thead>
        <tr>
          <th>Your need</th>
          <th>Start with</th>
          <th>What to record</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <td>Recommended atmospheric modeling value</td>
          <td>JPL</td>
          <td>evaluation number, table, note, rate expression, uncertainty</td>
        </tr>
        <tr>
          <td>Photolysis inputs</td>
          <td>JPL</td>
          <td>cross sections, quantum yields, wavelength range, temperature notes</td>
        </tr>
        <tr>
          <td>Original experimental records</td>
          <td>NIST</td>
          <td>authors, year, temperature range, pressure, bath gas, method</td>
        </tr>
        <tr>
          <td>Comparison between several measurements</td>
          <td>NIST, then JPL</td>
          <td>spread of values, category, review status, final selected source</td>
        </tr>
      </tbody>
    </table>
  </div>
</section>

<section id="using-jpl" class="section-block">
  <div class="section-heading">
    <h2>How To Use The JPL Data Evaluation</h2>
  </div>

  <div class="lesson-grid">
    <div class="panel">
      <p class="section-label">Step-By-Step</p>
      <ol>
        <li>Open the <a href="https://jpldataeval.jpl.nasa.gov/" target="_blank" rel="noreferrer">JPL Data Evaluation</a> website.</li>
        <li>Download the latest evaluation PDF from the site, and record the evaluation number.</li>
        <li>Search the PDF for the reaction in more than one form, for example <code>O + O3</code>, <code>O(3P) + O3</code>, and <code>O3 + O</code>.</li>
        <li>Find the table entry and the note attached to that entry.</li>
        <li>Record the rate expression, units, temperature range, uncertainty, and note number.</li>
        <li>For photolysis, search by species name and use the photochemistry tables and notes rather than looking for one universal <code>k</code>.</li>
      </ol>
    </div>

    <div class="takeaway-box">
      <p class="section-label">Why JPL First</p>
      <p>
        JPL values are evaluated for atmospheric modeling.
        That means the database is already asking the same kind of question you are asking:
        what value should a modeler use, and under what conditions?
      </p>
    </div>
  </div>

  <div class="panel">
    <p class="section-label">What A Good JPL Note Looks Like In Your Notebook</p>
    <table>
      <thead>
        <tr>
          <th>Field</th>
          <th>Example entry</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <td>Reaction</td>
          <td><code>O + O<sub>3</sub> -> O<sub>2</sub> + O<sub>2</sub></code></td>
        </tr>
        <tr>
          <td>Type</td>
          <td>bimolecular thermal reaction</td>
        </tr>
        <tr>
          <td>Rate expression</td>
          <td><code>k(T) = 8.0e-12 exp(-2060/T)</code></td>
        </tr>
        <tr>
          <td>Units</td>
          <td><code>cm3 molecule-1 s-1</code></td>
        </tr>
        <tr>
          <td>Database note</td>
          <td>record the JPL evaluation number, table, and note attached to the entry</td>
        </tr>
      </tbody>
    </table>
    <p>
      The numerical entry above is a common JPL-style recommendation for this Chapman loss reaction.
      In a real assignment or model update, you should still check the current JPL PDF and copy the exact table and note information yourself.
    </p>
  </div>
</section>

<section id="using-nist" class="section-block">
  <div class="section-heading">
    <h2>How To Use The NIST Kinetics Database</h2>
  </div>

  <div class="lesson-grid">
    <div class="panel">
      <p class="section-label">Step-By-Step</p>
      <ol>
        <li>Open the <a href="https://kinetics.nist.gov/kinetics/index.jsp" target="_blank" rel="noreferrer">NIST Chemical Kinetics Database</a>.</li>
        <li>Use the quick search form for simple reactant or product searches.</li>
        <li>Use the advanced reaction search when the quick search returns too many records.</li>
        <li>Search for several reaction spellings, such as <code>O3</code> with <code>O</code>, or <code>O2</code> with <code>O</code> and product <code>O3</code>.</li>
        <li>Open individual records and compare temperature range, pressure, bath gas, category, and data type.</li>
        <li>Prefer review records when you need a broad comparison, but always keep the original citation trail.</li>
      </ol>
    </div>

    <div class="takeaway-box">
      <p class="section-label">Important Difference</p>
      <p>
        NIST often gives many records, not one final answer.
        Your job is to decide which record is relevant to your modeling condition,
        then compare it with the evaluated recommendation from JPL.
      </p>
    </div>
  </div>

  <div class="panel">
    <p class="section-label">What To Look For On A NIST Record</p>
    <table>
      <thead>
        <tr>
          <th>Field</th>
          <th>Why it matters</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <td>Reaction order</td>
          <td>tells you whether the units should be first, second, or third order</td>
        </tr>
        <tr>
          <td>Temperature range</td>
          <td>shows whether the expression is valid for your atmosphere layer</td>
        </tr>
        <tr>
          <td>Pressure and bath gas</td>
          <td>especially important for termolecular reactions</td>
        </tr>
        <tr>
          <td>Category and data type</td>
          <td>helps distinguish direct experiment, review, theory, and fitted mechanisms</td>
        </tr>
        <tr>
          <td>Authors and year</td>
          <td>needed because NIST recommends citing original authors when you use data</td>
        </tr>
      </tbody>
    </table>
  </div>
</section>

<section id="worked-example" class="section-block">
  <div class="section-heading">
    <h2>A Worked Chapman Example</h2>
  </div>

  <div class="panel">
    <p>
      Imagine you are preparing a small Chapman cycle file for a model.
      The goal is not just to write reactions, but to create a traceable table that connects each reaction to a kinetic source.
    </p>
  </div>

  <div class="panel">
    <table>
      <thead>
        <tr>
          <th>ID</th>
          <th>Reaction</th>
          <th>Parameter</th>
          <th>Database strategy</th>
          <th>Model note</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <td>R1</td>
          <td><code>O<sub>2</sub> + photon -> O + O</code></td>
          <td><code>J1</code></td>
          <td>Use JPL photochemistry data for <code>O<sub>2</sub></code>; do not search for a fixed thermal <code>k</code>.</td>
          <td><code>J1</code> depends on radiation and altitude.</td>
        </tr>
        <tr>
          <td>R2</td>
          <td><code>O + O<sub>2</sub> + M -> O<sub>3</sub> + M</code></td>
          <td><code>k2(T, M)</code></td>
          <td>Start with JPL termolecular or pressure-dependent tables; use NIST to inspect records and bath gases.</td>
          <td>Keep <code>[M]</code> explicit unless your model converts this to an effective second-order rate.</td>
        </tr>
        <tr>
          <td>R3</td>
          <td><code>O<sub>3</sub> + photon -> O<sub>2</sub> + O</code></td>
          <td><code>J3</code></td>
          <td>Use JPL photochemistry data for <code>O<sub>3</sub></code>; note product channel if <code>O(1D)</code> is included.</td>
          <td>A more detailed network separates <code>O(3P)</code> and <code>O(1D)</code>.</td>
        </tr>
        <tr>
          <td>R4</td>
          <td><code>O + O<sub>3</sub> -> O<sub>2</sub> + O<sub>2</sub></code></td>
          <td><code>k4(T)</code></td>
          <td>Use JPL for the recommended expression; use NIST to compare experimental and review records.</td>
          <td>Common form: <code>k4 = 8.0e-12 exp(-2060/T)</code>.</td>
        </tr>
      </tbody>
    </table>
  </div>

  <div class="takeaway-box">
    <p class="section-label">Mini Calculation</p>
    <p>
      At <code>T = 298 K</code>, the common Chapman loss expression gives
      <code>k4 = 8.0e-12 exp(-2060/298) approximately 8.0e-15 cm3 molecule-1 s-1</code>.
      This simple calculation is useful because it lets you check whether a copied table value is in the right range.
    </p>
  </div>
</section>

<section id="network-table" class="section-block">
  <div class="section-heading">
    <h2>The Final Reaction Network Table</h2>
  </div>

  <div class="panel">
    <p>
      A model-ready network should look like a research notebook, not just a list of reactions.
      Use a table like this before writing anything into a model input file.
    </p>
  </div>

  <div class="panel">
    <table>
      <thead>
        <tr>
          <th>ID</th>
          <th>Reaction</th>
          <th>Type</th>
          <th>Rate law</th>
          <th>Units</th>
          <th>Source</th>
          <th>Notes</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <td>R1</td>
          <td><code>O<sub>2</sub> + photon -> O + O</code></td>
          <td>photolysis</td>
          <td><code>J1 [O<sub>2</sub>]</code></td>
          <td><code>s-1</code></td>
          <td>JPL photochemistry</td>
          <td>requires radiation field, cross section, quantum yield</td>
        </tr>
        <tr>
          <td>R2</td>
          <td><code>O + O<sub>2</sub> + M -> O<sub>3</sub> + M</code></td>
          <td>termolecular</td>
          <td><code>k2 [O] [O<sub>2</sub>] [M]</code></td>
          <td><code>cm6 molecule-2 s-1</code></td>
          <td>JPL plus NIST check</td>
          <td>bath gas and pressure regime matter</td>
        </tr>
        <tr>
          <td>R3</td>
          <td><code>O<sub>3</sub> + photon -> O<sub>2</sub> + O</code></td>
          <td>photolysis</td>
          <td><code>J3 [O<sub>3</sub>]</code></td>
          <td><code>s-1</code></td>
          <td>JPL photochemistry</td>
          <td>track product channel in detailed networks</td>
        </tr>
        <tr>
          <td>R4</td>
          <td><code>O + O<sub>3</sub> -> O<sub>2</sub> + O<sub>2</sub></code></td>
          <td>bimolecular</td>
          <td><code>k4 [O] [O<sub>3</sub>]</code></td>
          <td><code>cm3 molecule-1 s-1</code></td>
          <td>JPL recommendation; NIST records</td>
          <td>temperature-dependent loss reaction</td>
        </tr>
      </tbody>
    </table>
  </div>

  <div class="lesson-grid">
    <article class="entry-card">
      <p class="card-label">Good Network Entry</p>
      <p>Can be traced back to a database, a note, and a valid temperature or pressure range.</p>
    </article>

    <article class="entry-card">
      <p class="card-label">Weak Network Entry</p>
      <p>Contains only a reaction and a number with no unit, no source, and no validity range.</p>
    </article>
  </div>
</section>

<section id="common-mistakes" class="section-block">
  <div class="section-heading">
    <h2>Common Mistakes</h2>
  </div>

  <div class="lesson-grid">
    <article class="entry-card">
      <p class="card-label">Mistake 1</p>
      <p>
        Treating photolysis as if it has one universal thermal rate constant.
        Photolysis uses <code>J</code>, and <code>J</code> depends on the radiation environment.
      </p>
    </article>

    <article class="entry-card">
      <p class="card-label">Mistake 2</p>
      <p>
        Copying <code>k</code> without units.
        A third-order <code>cm6 molecule-2 s-1</code> value is not interchangeable with a second-order
        <code>cm3 molecule-1 s-1</code> value.
      </p>
    </article>

    <article class="entry-card">
      <p class="card-label">Mistake 3</p>
      <p>
        Ignoring temperature range.
        A rate expression measured at high temperature may not be appropriate for a cold stratospheric layer.
      </p>
    </article>

    <article class="entry-card">
      <p class="card-label">Mistake 4</p>
      <p>
        Mixing <code>O</code>, <code>O(3P)</code>, and <code>O(1D)</code> without checking the product channel.
        Excited oxygen can open different chemistry.
      </p>
    </article>

    <article class="entry-card">
      <p class="card-label">Mistake 5</p>
      <p>
        Assuming NIST gives one recommended answer.
        NIST is a searchable compilation, so you must compare records and decide which one fits your use case.
      </p>
    </article>

    <article class="entry-card">
      <p class="card-label">Mistake 6</p>
      <p>
        Forgetting to record the database version or evaluation number.
        Kinetic recommendations can change, so source metadata matters.
      </p>
    </article>
  </div>
</section>

<section id="references" class="section-block">
  <div class="section-heading">
    <h2>Useful Source Links</h2>
  </div>

  <div class="resource-list">
    <a class="outline-link" href="https://science.nasa.gov/earth/earth-observatory/ozone/" target="_blank" rel="noreferrer">NASA Earth Observatory: Ozone</a>
    <a class="outline-link" href="https://svs.gsfc.nasa.gov/823" target="_blank" rel="noreferrer">NASA SVS: O2 Broken Up And Reforming As O3</a>
    <a class="outline-link" href="https://jpldataeval.jpl.nasa.gov/" target="_blank" rel="noreferrer">JPL Chemical Kinetics And Photochemical Data</a>
    <a class="outline-link" href="https://kinetics.nist.gov/kinetics/index.jsp" target="_blank" rel="noreferrer">NIST Chemical Kinetics Database</a>
    <a class="outline-link" href="https://kinetics.nist.gov/kinetics/getting_started.jsp" target="_blank" rel="noreferrer">NIST Getting Started</a>
  </div>
</section>

<section id="lecture-summary" class="section-block">
  <div class="section-heading">
    <h2>Lecture Summary</h2>
  </div>

  <div class="lesson-grid">
    <div class="panel">
      <p>The main ideas of Lecture 2 can be reduced to six statements:</p>
      <ol>
        <li>The Chapman cycle is a four-reaction oxygen mechanism for basic stratospheric ozone formation and loss.</li>
        <li>A reaction network connects species, reactions, rate laws, and kinetic data.</li>
        <li>Thermal reactions use <code>k(T)</code>; photolysis reactions use <code>J</code>.</li>
        <li>JPL is the best first stop for evaluated atmospheric modeling recommendations.</li>
        <li>NIST is useful for searching and comparing gas-phase kinetic literature records.</li>
        <li>A useful network entry must include units, source, temperature range, and notes.</li>
      </ol>
    </div>

    <div class="takeaway-box">
      <p>
        If you can build the Chapman table yourself, then you are ready to build larger atmospheric reaction networks.
      </p>
    </div>
  </div>
</section>

<section id="after-class-task" class="section-block">
  <div class="section-heading">
    <h2>After-Class Task</h2>
  </div>

  <div class="lesson-grid">
    <div class="panel">
      <p>Build your own Chapman reaction network table with the following columns:</p>
      <ol>
        <li>reaction ID</li>
        <li>reaction equation</li>
        <li>reaction type</li>
        <li>rate law</li>
        <li>rate expression or photolysis data requirement</li>
        <li>units</li>
        <li>database source</li>
        <li>notes and validity range</li>
      </ol>
    </div>

    <div class="panel">
      <p class="section-label">Required Checks</p>
      <ul>
        <li>Use JPL to find the recommended information for <code>O + O<sub>3</sub> -> O<sub>2</sub> + O<sub>2</sub></code>.</li>
        <li>Use NIST to find at least one record for the same reaction.</li>
        <li>Write three sentences explaining why the JPL and NIST pages are useful in different ways.</li>
        <li>State clearly why <code>O<sub>2</sub></code> and <code>O<sub>3</sub></code> photolysis use <code>J</code> instead of a fixed <code>k</code>.</li>
      </ul>
    </div>
  </div>
</section>

<section id="quick-check-quiz" class="section-block">
  <div class="section-heading">
    <h2>Quick Check Quiz</h2>
  </div>

  <div class="panel">
    <p>
      Use this short quiz to check whether the main ideas of Lecture 2 are clear.
    </p>
  </div>

  <div class="quiz-block" data-answer="b">
    <h3>Question 1</h3>
    <p>Which reaction is the ozone formation step in the Chapman cycle?</p>
    <div class="quiz-options">
      <button class="quiz-option" data-choice="a" onclick="checkPatmoLecture2Quiz(this)">A. <code>O<sub>3</sub> + photon -> O<sub>2</sub> + O</code></button>
      <button class="quiz-option" data-choice="b" onclick="checkPatmoLecture2Quiz(this)">B. <code>O + O<sub>2</sub> + M -> O<sub>3</sub> + M</code></button>
      <button class="quiz-option" data-choice="c" onclick="checkPatmoLecture2Quiz(this)">C. <code>O + O<sub>3</sub> -> O<sub>2</sub> + O<sub>2</sub></code></button>
    </div>
    <div class="quiz-feedback"></div>
  </div>

  <div class="quiz-block" data-answer="a">
    <h3>Question 2</h3>
    <p>What parameter is normally used for photolysis reactions?</p>
    <div class="quiz-options">
      <button class="quiz-option" data-choice="a" onclick="checkPatmoLecture2Quiz(this)">A. <code>J</code>, with units of <code>s-1</code></button>
      <button class="quiz-option" data-choice="b" onclick="checkPatmoLecture2Quiz(this)">B. A third-order <code>k</code> with units of <code>cm6 molecule-2 s-1</code></button>
      <button class="quiz-option" data-choice="c" onclick="checkPatmoLecture2Quiz(this)">C. A pressure value only</button>
    </div>
    <div class="quiz-feedback"></div>
  </div>

  <div class="quiz-block" data-answer="c">
    <h3>Question 3</h3>
    <p>Why should you start with JPL for atmospheric model rate recommendations?</p>
    <div class="quiz-options">
      <button class="quiz-option" data-choice="a" onclick="checkPatmoLecture2Quiz(this)">A. Because it only contains raw, unevaluated laboratory records</button>
      <button class="quiz-option" data-choice="b" onclick="checkPatmoLecture2Quiz(this)">B. Because it replaces the need to record units</button>
      <button class="quiz-option" data-choice="c" onclick="checkPatmoLecture2Quiz(this)">C. Because it provides evaluated kinetic and photochemical data for atmospheric studies</button>
    </div>
    <div class="quiz-feedback"></div>
  </div>

  <div class="quiz-block" data-answer="b">
    <h3>Question 4</h3>
    <p>What is NIST especially useful for?</p>
    <div class="quiz-options">
      <button class="quiz-option" data-choice="a" onclick="checkPatmoLecture2Quiz(this)">A. Calculating solar actinic flux automatically for every model layer</button>
      <button class="quiz-option" data-choice="b" onclick="checkPatmoLecture2Quiz(this)">B. Searching and comparing gas-phase kinetic literature records</button>
      <button class="quiz-option" data-choice="c" onclick="checkPatmoLecture2Quiz(this)">C. Removing the need for original citations</button>
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
      feedback.textContent = 'Correct. You can move on.';
      feedback.className = 'quiz-feedback correct';
    } else {
      feedback.textContent = 'Not quite. Try again.';
      feedback.className = 'quiz-feedback incorrect';
    }
  }
</script>

</div>
