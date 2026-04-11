---
layout: page
title: Lecture 1: What PATMO Is
subtitle: Student handout for understanding PATMO, its scope, and why the modern sulfur cycle anchors the course.
permalink: /work/patmo/lecture-01/
body_class: patmo-reading-theme
head-extra:
  - patmo-reading-theme.html
description: Lecture 1 of the PATMO student course, introducing PATMO and the role of the modern sulfur cycle.
---

<div class="patmo-course">

<section class="panel">
  <div class="lesson-breadcrumb">
    <a href="{{ '/work/patmo/' | relative_url }}">PATMO Course</a>
    <span>/</span>
    <span>Lecture 1 of 7</span>
  </div>
  <p class="section-label">Student Handout</p>
  <p>
    This page is the student-facing handout for Lecture 1 of the course.
    It is written for you to read directly on a website, not as a teacher's presentation script.
  </p>
  <p>
    This first lecture has one job: to help you understand what <code>PATMO</code> is,
    what it is useful for, what it is not designed to do,
    and why this course uses the <code>modern sulfur cycle</code> as its main case study.
  </p>
  <p>
    If you finish this page and can clearly answer the following three questions, then you have understood the main point of Lecture 1:
  </p>

  <div class="lesson-grid">
    <article class="entry-card">
      <p class="card-label">Question 1</p>
      <p><strong>What kind of model is <code>PATMO</code>?</strong></p>
      <p><code>PATMO</code> is a one-dimensional atmospheric photochemistry model.</p>
    </article>

    <article class="entry-card">
      <p class="card-label">Question 2</p>
      <p><strong>What can <code>PATMO</code> help you do?</strong></p>
      <p>It helps you run controlled computational experiments on atmospheric chemistry problems.</p>
    </article>

    <article class="entry-card">
      <p class="card-label">Question 3</p>
      <p><strong>What will you learn in the rest of this course?</strong></p>
      <p>You will learn how to understand <code>PATMO</code>, run a case, read its outputs, and do a simple comparison experiment.</p>
    </article>
  </div>

  <div class="lesson-anchor-nav">
    <a href="#outline">Outline</a>
    <a href="#goals">Goals</a>
    <a href="#core-takeaway">Takeaway</a>
    <a href="#understanding-what-patmo-is">What PATMO Is</a>
    <a href="#what-patmo-can-help-you-do">Uses</a>
    <a href="#what-patmo-cannot-do">Limits</a>
    <a href="#why-we-use-the-modern-sulfur-cycle">Sulfur Cycle</a>
    <a href="#what-to-expect-from-this-course">Course Scope</a>
    <a href="#how-the-seven-lectures-fit-together">Flow</a>
    <a href="#lecture-summary">Summary</a>
    <a href="#quick-check-quiz">Quiz</a>
  </div>
</section>

<section id="outline" class="section-block">
  <div class="section-heading">
    <h2>Lecture Outline</h2>
  </div>

  <div class="outline-grid">
    <a class="outline-link" href="#core-takeaway">The Most Important Takeaway</a>
    <a class="outline-link" href="#understanding-what-patmo-is">Understanding What PATMO Is</a>
    <a class="outline-link" href="#what-patmo-can-help-you-do">What PATMO Can Help You Do</a>
    <a class="outline-link" href="#what-patmo-cannot-do">What PATMO Cannot Do</a>
    <a class="outline-link" href="#why-we-use-the-modern-sulfur-cycle">Why We Use the Modern Sulfur Cycle</a>
    <a class="outline-link" href="#where-patmo-sits-in-the-research-process">Where PATMO Sits in the Research Process</a>
    <a class="outline-link" href="#what-to-expect-from-this-course">What to Expect From This Course</a>
    <a class="outline-link" href="#how-the-seven-lectures-fit-together">How the Seven Lectures Fit Together</a>
    <a class="outline-link" href="#check-your-understanding">Check Your Understanding</a>
    <a class="outline-link" href="#lecture-summary">Lecture Summary</a>
    <a class="outline-link" href="#after-class-task">After-Class Task</a>
    <a class="outline-link" href="#final-reminder">Final Reminder</a>
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
        <li>explain what kind of model <code>PATMO</code> is</li>
        <li>understand why <code>PATMO</code> is useful as a research tool</li>
        <li>distinguish between what <code>PATMO</code> can and cannot do</li>
        <li>understand why the <code>modern sulfur cycle</code> is used throughout the course</li>
        <li>see how the seven lectures fit together</li>
      </ul>
    </div>

    <div class="takeaway-box">
      <p class="section-label">Reading Goal</p>
      <p>
        This lecture is mainly about orientation.
        It gives you the conceptual frame you need before you touch case files, commands, runtime steps, or outputs.
      </p>
    </div>
  </div>
</section>

<section id="core-takeaway" class="section-block">
  <div class="section-heading">
    <h2>The Most Important Takeaway</h2>
  </div>

  <div class="takeaway-box">
    <p>If you only remember one statement from this lecture, remember this:</p>
    <p>
      <strong><code>PATMO</code> is a one-dimensional atmospheric photochemistry model that you can use as a platform for controlled computational experiments.</strong>
    </p>
    <p>
      That sentence contains the three most important ideas:
      <code>one-dimensional</code>, <code>atmospheric photochemistry model</code>, and <code>controlled computational experiments</code>.
      Everything else in this lecture is just an explanation of those three ideas.
    </p>
  </div>

  <div class="panel">
    <p class="section-label">External Overview</p>
    <p>If you want a quick external overview of common atmospheric model types, you can read:</p>
    <div class="resource-list">
      <a class="outline-link" href="https://ac2.iqfr.csic.es/en/research/climate-modelling" target="_blank" rel="noreferrer">Box modelling</a>
      <a class="outline-link" href="https://ac2.iqfr.csic.es/en/research/climate-modelling" target="_blank" rel="noreferrer">1-dimensional modelling</a>
      <a class="outline-link" href="https://ac2.iqfr.csic.es/en/research/climate-modelling" target="_blank" rel="noreferrer">3-dimensional global modelling</a>
    </div>
  </div>

  <div class="model-comparison">
    <article class="entry-card model-card">
      <p class="card-label">Box Model</p>
      <pre class="model-illustration"><code>+-----------+
|   single  |
| well-mixed|
|    box    |
+-----------+</code></pre>
      <p>
        A box model treats the system as one mixed region.
        It is useful when you want a very simple first approximation and do not need vertical structure.
      </p>
    </article>

    <article class="entry-card model-card">
      <p class="card-label">1D Model</p>
      <pre class="model-illustration"><code>     top
      |
   +-----+
   |layer|
   +-----+
   |layer|
   +-----+
   |layer|
   +-----+
   |layer|
   +-----+
      |
   surface</code></pre>
      <p>
        A 1D model treats the atmosphere as a vertical column divided into layers.
        This is the basic structure used by <code>PATMO</code>.
      </p>
    </article>

    <article class="entry-card model-card">
      <p class="card-label">3D Model</p>
      <pre class="model-illustration"><code>             altitude (z)
                 ^
                 |
          +---+---+---+
         /|  /|  /|  /|
        +---+---+---+ |
        | +---+---+---+
        |/|  /|  /|  /|
        +---+---+---+ |
        | +---+---+---+
        |/  |/  |/  |/
        +---+---+---+
          &lt;---- x ----&gt;
         /
        /
       y</code></pre>
      <p>
        A 3D model resolves structure across longitude, latitude, and altitude at the same time.
        It can represent much more spatial complexity, but it is also much more difficult to build, run, and interpret.
      </p>
    </article>
  </div>
</section>

<section id="understanding-what-patmo-is" class="section-block">
  <div class="section-heading">
    <h2>Understanding What PATMO Is</h2>
  </div>

  <div class="lesson-grid">
    <article class="entry-card">
      <p class="card-label">Why Start Here</p>
      <p>
        This course starts with <code>PATMO</code> itself because later parts of the workflow only make sense
        if you already know what kind of tool you are dealing with.
      </p>
    </article>

    <article class="entry-card">
      <p class="card-label">Basic Definition</p>
      <p>
        At the most basic level, <code>PATMO</code> is a numerical model for atmospheric chemistry and related physical processes.
        It is a structured way to describe a scientific setup and compute how that setup evolves.
      </p>
    </article>

    <article class="entry-card">
      <p class="card-label">What It Is Not</p>
      <p>
        It is not an observational instrument, not a database,
        and not a machine that automatically produces scientific truth.
      </p>
    </article>

    <article class="entry-card">
      <p class="card-label">What A Model Means Here</p>
      <p>
        In this course, a model means a simplified representation of reality
        that lets you run repeatable and comparable experiments under clearly defined assumptions.
      </p>
    </article>
  </div>

  <div class="lesson-grid">
    <article class="entry-card">
      <p class="card-label">One-Dimensional</p>
      <p>
        When we say that <code>PATMO</code> is one-dimensional, we mean that it focuses on the vertical direction.
        Instead of representing the entire atmosphere in longitude, latitude, and height,
        it treats the atmosphere as a single vertical column divided into layers.
      </p>
      <ul>
        <li>temperature can vary by layer</li>
        <li>density can vary by layer</li>
        <li>species abundances can vary by layer</li>
        <li>radiation conditions can vary by layer</li>
      </ul>
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
    </article>
  </div>

  <div class="takeaway-box">
    <p class="section-label">Why This Model Is Good For Learning</p>
    <p>
      A 1D photochemistry model is much more structured than a single box model,
      but still much easier to understand than a full 3D model.
      It gives you a way to study mechanisms clearly without getting buried in spatial complexity too early.
    </p>
  </div>
</section>

<section id="what-patmo-can-help-you-do" class="section-block">
  <div class="section-heading">
    <h2>What PATMO Can Help You Do</h2>
  </div>

  <div class="lesson-grid">
    <article class="entry-card">
      <p class="card-label">1. Compute Vertical Variation</p>
      <p>
        If you provide a reaction network, an initial atmospheric profile, and relevant parameters,
        the model can compute how species evolve across altitude.
      </p>
    </article>

    <article class="entry-card">
      <p class="card-label">2. Compare Setups</p>
      <p>You can change case-specific settings and compare how the results respond:</p>
      <ul>
        <li>emission parameters</li>
        <li>deposition parameters</li>
        <li>radiation-related settings</li>
        <li>other case-specific inputs</li>
      </ul>
    </article>

    <article class="entry-card">
      <p class="card-label">3. Perform Sensitivity Experiments</p>
      <p>
        A sensitivity experiment means changing one factor while keeping the rest as fixed as possible,
        then examining how the result changes.
      </p>
      <p>This is one of the most important uses of <code>PATMO</code> in this course.</p>
    </article>

    <article class="entry-card">
      <p class="card-label">4. Practice A Research Workflow</p>
      <p>
        <code>PATMO</code> is not only useful because it produces numbers.
        It is useful because it helps you practice a basic research workflow.
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

<section id="what-patmo-cannot-do" class="section-block">
  <div class="section-heading">
    <h2>What PATMO Cannot Do</h2>
  </div>

  <div class="lesson-grid">
    <article class="entry-card">
      <p class="card-label">1. Not A Full 3D Global Model</p>
      <p>
        <code>PATMO</code> is not designed to directly answer global weather
        or full three-dimensional transport questions.
      </p>
    </article>

    <article class="entry-card">
      <p class="card-label">2. Not A Replacement For Observations</p>
      <p>
        Model output is not the same thing as reality.
        Observations and models support each other, but one does not replace the other.
      </p>
    </article>

    <article class="entry-card">
      <p class="card-label">3. Not Meaningful Without Meaningful Inputs</p>
      <p>
        If the inputs, parameters, or boundary conditions are poorly chosen,
        the output will not become scientifically useful by itself.
      </p>
    </article>

    <article class="entry-card">
      <p class="card-label">4. It Does Not Interpret For You</p>
      <p>
        <code>PATMO</code> computes results under a chosen setup.
        It does not decide whether those results are scientifically reasonable or how they should be explained.
      </p>
    </article>
  </div>

  <div class="takeaway-box">
    <p><strong><code>PATMO</code> is a tool, not a judge.</strong></p>
    <p>It helps you run a controlled experiment, but it does not think on your behalf.</p>
  </div>
</section>

<section id="why-we-use-the-modern-sulfur-cycle" class="section-block">
  <div class="section-heading">
    <h2>Why We Use the Modern Sulfur Cycle</h2>
  </div>

  <div class="lesson-grid">
    <div class="panel">
      <p>
        This course uses the <code>modern sulfur cycle</code> as its running case study,
        but it is important to understand what that means.
      </p>
      <p>
        The course is not trying to teach the entire theory of sulfur chemistry in full detail.
        Instead, it uses this case because it is scientifically meaningful and also practical for training.
      </p>
    </div>

    <article class="entry-card">
      <p class="card-label">Why It Is A Good Teaching Case</p>
      <ul>
        <li>a reaction network</li>
        <li>vertical profile inputs</li>
        <li>radiation-related inputs</li>
        <li>source and sink settings</li>
        <li>outputs that can be compared and interpreted</li>
      </ul>
    </article>
  </div>

  <div class="panel">
    <p>
      It is also useful because the same case can be revisited throughout the course.
      That means you do not need to constantly adapt to new scientific contexts.
      Instead, you can move step by step from recognizing the case, to reading its files, to running it, and finally to modifying it.
    </p>
    <p>
      So the <code>modern sulfur cycle</code> is not the whole course.
      It is the main case through which you learn how to work with <code>PATMO</code>.
    </p>
  </div>
</section>

<section id="where-patmo-sits-in-the-research-process" class="section-block">
  <div class="section-heading">
    <h2>Where PATMO Sits in the Research Process</h2>
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
      <p class="section-label">Why This Matters</p>
      <p>
        That way of thinking will matter throughout the course.
        It will help you remember that output is not the end of the task.
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

<section id="what-to-expect-from-this-course" class="section-block">
  <div class="section-heading">
    <h2>What to Expect From This Course</h2>
  </div>

  <div class="lesson-grid">
    <article class="entry-card">
      <p class="card-label">You Do Not Need Yet</p>
      <p>
        Before moving on, it helps to be clear about the starting point.
        You do <strong>not</strong> need to know <code>Fortran</code>, <code>Python</code>,
        advanced atmospheric chemistry, numerical analysis, or advanced command-line skills before continuing.
      </p>
    </article>

    <article class="entry-card">
      <p class="card-label">What You Do Need</p>
      <p>
        You need something much simpler:
        willingness to learn step by step from one case,
        and willingness to connect files, workflow, and output meaningfully.
      </p>
    </article>

    <article class="entry-card">
      <p class="card-label">Target By The End</p>
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
      <p class="card-label">Outside The Main Path</p>
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
  </div>
</section>

<section id="how-the-seven-lectures-fit-together" class="section-block">
  <div class="section-heading">
    <h2>How the Seven Lectures Fit Together</h2>
  </div>

  <p>The course follows a deliberate sequence:</p>

  <div class="lecture-flowchart">
    <article class="entry-card flow-step is-live">
      <p class="card-label">Lecture 1</p>
      <h3>What PATMO Is</h3>
      <p>Orientation: model identity, scope, and the role of the modern sulfur cycle.</p>
      <span class="course-status is-live">Current lecture</span>
    </article>

    <div class="flow-arrow" aria-hidden="true">↓</div>

    <article class="entry-card flow-step">
      <p class="card-label">Lecture 2</p>
      <h3>The Overall Architecture of <code>PATMO</code></h3>
      <p>See how the major parts of the model fit together.</p>
    </article>

    <div class="flow-arrow" aria-hidden="true">↓</div>

    <article class="entry-card flow-step">
      <p class="card-label">Lecture 3</p>
      <h3>The Input File System of <code>PATMO</code></h3>
      <p>Learn how the main input files are organized and what each one does.</p>
    </article>

    <div class="flow-arrow" aria-hidden="true">↓</div>

    <article class="entry-card flow-step">
      <p class="card-label">Lecture 4</p>
      <h3>How the <code>modern sulfur cycle</code> Case Is Written into <code>PATMO</code></h3>
      <p>Connect the scientific case to model-ready inputs.</p>
    </article>

    <div class="flow-arrow" aria-hidden="true">↓</div>

    <article class="entry-card flow-step">
      <p class="card-label">Lecture 5</p>
      <h3>The Runtime Workflow and Output System of <code>PATMO</code></h3>
      <p>Understand what happens when the model runs and where the outputs appear.</p>
    </article>

    <div class="flow-arrow" aria-hidden="true">↓</div>

    <article class="entry-card flow-step">
      <p class="card-label">Lecture 6</p>
      <h3>Hands-On Practice: Running the <code>modern sulfur cycle</code> Case</h3>
      <p>Run the prepared case and connect setup to output.</p>
    </article>

    <div class="flow-arrow" aria-hidden="true">↓</div>

    <article class="entry-card flow-step">
      <p class="card-label">Lecture 7</p>
      <h3>A First Research-Style Exercise</h3>
      <p>Modify a case and run a simple sensitivity experiment.</p>
    </article>
  </div>

  <div class="takeaway-box flow-summary">
    <p class="section-label">Course Logic</p>
    <pre class="lesson-flow"><code>meet PATMO
-> understand PATMO
-> run PATMO
-> use PATMO in the simplest research-style way</code></pre>
  </div>
</section>

<section id="check-your-understanding" class="section-block">
  <div class="section-heading">
    <h2>Check Your Understanding</h2>
  </div>

  <div class="lesson-grid">
    <div class="panel">
      <p>By the end of this lecture, you should be able to answer these questions in your own words:</p>
      <ol>
        <li>What is <code>PATMO</code>?</li>
        <li>Why is <code>PATMO</code> useful in research?</li>
        <li>Why is the <code>modern sulfur cycle</code> used in this course?</li>
        <li>What are you expected to be able to do by the end of the course?</li>
      </ol>
    </div>

    <div class="panel">
      <p class="section-label">Reading Advice</p>
      <p>
        If you cannot answer them yet, that is fine.
        Read the key sections again and try to reduce the answers to one or two sentences each.
      </p>
    </div>
  </div>
</section>

<section id="lecture-summary" class="section-block">
  <div class="section-heading">
    <h2>Lecture Summary</h2>
  </div>

  <div class="lesson-grid">
    <div class="panel">
      <p>The main ideas of Lecture 1 can be reduced to five statements:</p>
      <ol>
        <li><code>PATMO</code> is a one-dimensional atmospheric photochemistry model.</li>
        <li>It is used to study chemistry and related processes in a vertical atmospheric column.</li>
        <li>Its value lies in controlled and comparable computational experiments.</li>
        <li>The <code>modern sulfur cycle</code> is the running case study of this course, not the whole course content.</li>
        <li>The rest of the course will move step by step from understanding the model to running it and then using it in a simple experiment.</li>
      </ol>
    </div>

    <div class="takeaway-box">
      <p>
        If those five ideas are clear to you, then you are ready for Lecture 2.
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
      <p>Write a short answer, in no more than 200 words, to the following two questions:</p>
      <ol>
        <li>What is <code>PATMO</code> in your own understanding?</li>
        <li>What role does the <code>modern sulfur cycle</code> play in this course?</li>
      </ol>
    </div>

    <div class="panel">
      <p class="section-label">Requirements</p>
      <ul>
        <li>use your own words</li>
        <li>do not simply copy section headings</li>
        <li>your answer does not need to be perfect, but it should show understanding</li>
      </ul>
    </div>
  </div>
</section>

<section id="final-reminder" class="section-block">
  <div class="section-heading">
    <h2>Final Reminder</h2>
  </div>

  <div class="panel">
    <p>At this stage, you do not need to know how to run the model yet.</p>
    <p>You do not need to know the file system yet.</p>
    <p>You do not need to know the solver details yet.</p>
    <p>What you need from Lecture 1 is orientation:</p>
    <ul>
      <li>what <code>PATMO</code> is</li>
      <li>why it matters</li>
      <li>how this course is structured</li>
    </ul>
    <p>Once that orientation is clear, the rest of the course will make much more sense.</p>

    <div class="hero-actions">
      <a class="pixel-button ghost" href="{{ '/work/patmo/' | relative_url }}">Back to course hub</a>
    </div>
  </div>
</section>

<section id="quick-check-quiz" class="section-block">
  <div class="section-heading">
    <h2>Quick Check Quiz</h2>
  </div>

  <div class="panel">
    <p>
      Before you move on to Lecture 2, use this short quiz to check whether the main ideas of Lecture 1 are clear.
    </p>
  </div>

  <div class="quiz-block" data-answer="b">
    <h3>Question 1</h3>
    <p>What kind of model is <code>PATMO</code> in this course?</p>
    <div class="quiz-options">
      <button class="quiz-option" data-choice="a" onclick="checkPatmoQuiz(this)">A. A full 3D global weather prediction model</button>
      <button class="quiz-option" data-choice="b" onclick="checkPatmoQuiz(this)">B. A one-dimensional atmospheric photochemistry model</button>
      <button class="quiz-option" data-choice="c" onclick="checkPatmoQuiz(this)">C. A database of sulfur chemistry observations</button>
    </div>
    <div class="quiz-feedback"></div>
  </div>

  <div class="quiz-block" data-answer="c">
    <h3>Question 2</h3>
    <p>What is one of the main values of <code>PATMO</code>?</p>
    <div class="quiz-options">
      <button class="quiz-option" data-choice="a" onclick="checkPatmoQuiz(this)">A. It automatically tells you which scientific interpretation is correct</button>
      <button class="quiz-option" data-choice="b" onclick="checkPatmoQuiz(this)">B. It replaces real atmospheric observations</button>
      <button class="quiz-option" data-choice="c" onclick="checkPatmoQuiz(this)">C. It lets you run controlled and comparable computational experiments</button>
    </div>
    <div class="quiz-feedback"></div>
  </div>

  <div class="quiz-block" data-answer="a">
    <h3>Question 3</h3>
    <p>Why does this course use the <code>modern sulfur cycle</code>?</p>
    <div class="quiz-options">
      <button class="quiz-option" data-choice="a" onclick="checkPatmoQuiz(this)">A. It is the main training case through which you learn how to work with <code>PATMO</code></button>
      <button class="quiz-option" data-choice="b" onclick="checkPatmoQuiz(this)">B. It is the only atmospheric chemistry topic that <code>PATMO</code> can simulate</button>
      <button class="quiz-option" data-choice="c" onclick="checkPatmoQuiz(this)">C. It replaces the need to understand model inputs and outputs</button>
    </div>
    <div class="quiz-feedback"></div>
  </div>

  <div class="quiz-block" data-answer="b">
    <h3>Question 4</h3>
    <p>What should you expect to be able to do by the end of the course?</p>
    <div class="quiz-options">
      <button class="quiz-option" data-choice="a" onclick="checkPatmoQuiz(this)">A. Develop new PATMO modules from scratch</button>
      <button class="quiz-option" data-choice="b" onclick="checkPatmoQuiz(this)">B. Understand a case, run it, inspect outputs, and perform a simple comparison experiment</button>
      <button class="quiz-option" data-choice="c" onclick="checkPatmoQuiz(this)">C. Replace all atmospheric chemistry seminars with PATMO alone</button>
    </div>
    <div class="quiz-feedback"></div>
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
