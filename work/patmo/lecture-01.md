---
layout: page
title: Lecture 1: What PATMO Is
subtitle: Student handout for understanding PATMO, its scope, and why the modern sulfur cycle anchors the course.
permalink: /work/patmo/lecture-01/
description: Lecture 1 of the PATMO student course, introducing PATMO and the role of the modern sulfur cycle.
---

<div class="patmo-course" markdown="1">

<section class="panel">
  <div class="lesson-breadcrumb">
    <a href="{{ '/work/patmo/' | relative_url }}">PATMO Course</a>
    <span>/</span>
    <span>Lecture 1 of 7</span>
  </div>
  <p class="section-label">Student Handout</p>
  <p>
    This page is written for direct reading on the website rather than as a teacher's presentation script.
    Its job is simple: to help students understand what <code>PATMO</code> is, what it is useful for,
    what it is not designed to do, and why this course uses the <code>modern sulfur cycle</code> as its main case study.
  </p>
  <div class="lesson-anchor-nav">
    <a href="#learning-goals">Goals</a>
    <a href="#takeaway">Takeaway</a>
    <a href="#what-is-patmo">What PATMO Is</a>
    <a href="#what-patmo-can-do">Uses</a>
    <a href="#modern-sulfur-cycle">Case Study</a>
    <a href="#summary">Summary</a>
  </div>
</section>

<section id="learning-goals" class="course-overview-grid">
  <div class="panel">
    <p class="section-label">By The End</p>
    <h2>What You Should Learn From This Lecture</h2>
    <ul>
      <li>explain what kind of model <code>PATMO</code> is</li>
      <li>understand why <code>PATMO</code> is useful as a research tool</li>
      <li>distinguish between what <code>PATMO</code> can and cannot do</li>
      <li>understand why the <code>modern sulfur cycle</code> is used throughout the course</li>
      <li>see how the seven lectures fit together</li>
    </ul>
  </div>

  <div class="panel">
    <p class="section-label">Check Yourself</p>
    <h2>Three Questions To Keep In Mind</h2>
    <ol>
      <li>What kind of model is <code>PATMO</code>?</li>
      <li>What can <code>PATMO</code> help you do?</li>
      <li>What will you learn in the rest of this course?</li>
    </ol>
    <p>
      If you can answer those three questions clearly by the end of the page, then you have understood the main point of Lecture 1.
    </p>
  </div>
</section>

<section id="takeaway" class="takeaway-box">
  <p class="section-label">Most Important Takeaway</p>
  <p>
    <strong><code>PATMO</code> is a one-dimensional atmospheric photochemistry model that you can use as a platform
    for controlled computational experiments.</strong>
  </p>
  <p>
    That sentence contains the three most important ideas of this lecture:
    <code>one-dimensional</code>, <code>atmospheric photochemistry model</code>, and <code>controlled computational experiments</code>.
    Everything else on this page is an explanation of those three ideas.
  </p>
</section>

<section class="section-block">
  <div class="section-heading">
    <h2>Why The Course Starts With PATMO Itself</h2>
  </div>

  <div class="lesson-grid">
    <div class="panel">
      <p>
        This course does not begin with a broad introduction to atmospheric science,
        and it does not try to teach the full theory of atmospheric sulfur chemistry first.
        It begins with <code>PATMO</code> itself.
      </p>
      <p>
        The reason is practical: if students do not first understand what the model is,
        then the later directories, files, commands, and outputs will feel like a black box.
      </p>
    </div>

    <div class="panel">
      <p class="section-label">Orientation</p>
      <p>Lecture 1 gives direction before technical detail. It clarifies:</p>
      <ul>
        <li>what the course is really about</li>
        <li>what is not the focus of the course</li>
        <li>what <code>PATMO</code> is</li>
        <li>what level students are expected to reach by the end</li>
      </ul>
    </div>
  </div>
</section>

<section id="what-is-patmo" class="section-block">
  <div class="section-heading">
    <h2>What PATMO Is</h2>
  </div>

  <div class="lesson-grid">
    <article class="entry-card">
      <p class="card-label">Basic Definition</p>
      <p>
        At the most basic level, <code>PATMO</code> is a numerical model for studying chemistry and related physical processes in atmospheres.
      </p>
      <p>
        It lets you describe a scientific setup in a structured way and then compute how that setup evolves.
      </p>
    </article>

    <article class="entry-card">
      <p class="card-label">What It Is Not</p>
      <ul>
        <li>an observational instrument</li>
        <li>a database</li>
        <li>a plotting tool</li>
        <li>a program that automatically gives final scientific truth</li>
      </ul>
    </article>
  </div>

  <div class="panel">
    <p class="section-label">Questions PATMO Can Represent</p>
    <ul>
      <li>How does the vertical distribution of a species change under a given radiation field?</li>
      <li>What happens if I change an emission term?</li>
      <li>What happens if I modify a deposition setting?</li>
    </ul>
    <p>
      In other words, <code>PATMO</code> gives you a way to turn scientific questions into controlled numerical experiments.
    </p>
  </div>

  <div class="lesson-grid">
    <article class="entry-card">
      <p class="card-label">What A Scientific Model Means Here</p>
      <p>
        A model is not a perfect copy of reality and it is not a machine that automatically gives the correct answer.
        It is a simplified representation of reality that includes selected processes and ignores many others.
      </p>
      <ul>
        <li>model output depends on assumptions</li>
        <li>model output depends on inputs</li>
        <li>model output still needs interpretation</li>
      </ul>
    </article>

    <article class="entry-card">
      <p class="card-label">Why Models Matter</p>
      <p>
        The value of a model is not that it replaces reality. Its value is that it lets you run repeatable,
        editable, and comparable experiments under clearly defined conditions.
      </p>
      <p>
        That is exactly why models are useful in both research and teaching.
      </p>
    </article>

    <article class="entry-card">
      <p class="card-label">What One-Dimensional Means</p>
      <p>
        When we say <code>PATMO</code> is one-dimensional, we mainly mean that it focuses on the vertical direction.
        The atmosphere is treated as a single vertical column divided into layers.
      </p>
      <ul>
        <li>temperature can vary with altitude</li>
        <li>density can vary with altitude</li>
        <li>species abundances can vary with altitude</li>
        <li>radiation conditions can vary with altitude</li>
      </ul>
    </article>

    <article class="entry-card">
      <p class="card-label">What Photochemistry Means</p>
      <p>
        Photochemistry means radiation directly affects chemistry. Some molecules absorb photons
        at specific wavelengths and then break apart or change chemically, which reshapes the reaction network.
      </p>
      <ul>
        <li>light conditions change with altitude</li>
        <li>different molecules respond to different parts of the spectrum</li>
        <li>photochemistry can strongly affect which processes dominate</li>
      </ul>
    </article>
  </div>

  <div class="takeaway-box">
    <p class="section-label">Why A 1D Photochemistry Model Is Worth Learning</p>
    <p>
      A one-dimensional model is more realistic than a simple box model, but still far easier to understand
      than a full 3D model. That makes <code>PATMO</code> especially useful when the goal is not only to generate output,
      but to understand how and why that output changes.
    </p>
  </div>
</section>

<section id="what-patmo-can-do" class="section-block">
  <div class="section-heading">
    <h2>What PATMO Can Help You Do</h2>
  </div>

  <div class="lesson-grid">
    <article class="entry-card">
      <p class="card-label">1. Vertical Profiles</p>
      <p>
        If you provide a reaction network, an initial atmospheric profile, and relevant parameters,
        the model can compute how species evolve across altitude.
      </p>
    </article>

    <article class="entry-card">
      <p class="card-label">2. Compare Setups</p>
      <p>
        You can change emission parameters, deposition parameters, radiation-related settings,
        or other case-specific inputs and compare how the results respond.
      </p>
    </article>

    <article class="entry-card">
      <p class="card-label">3. Sensitivity Experiments</p>
      <p>
        A sensitivity experiment means changing one factor while keeping the rest as fixed as possible,
        then examining how the result changes. This is one of the most important uses of <code>PATMO</code> in the course.
      </p>
    </article>

    <article class="entry-card">
      <p class="card-label">4. Practice A Workflow</p>
      <p>
        <code>PATMO</code> is useful not only because it produces numbers. It helps students practice a basic research workflow.
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

<section class="section-block">
  <div class="section-heading">
    <h2>What PATMO Cannot Do</h2>
  </div>

  <div class="lesson-grid">
    <article class="entry-card">
      <p class="card-label">1. Not A Full 3D Global Model</p>
      <p>
        <code>PATMO</code> is not designed to directly answer global weather questions
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
        If inputs, parameters, or boundary conditions are poorly chosen,
        the output will not become scientifically useful by itself.
      </p>
    </article>

    <article class="entry-card">
      <p class="card-label">4. Not Its Own Interpreter</p>
      <p>
        <code>PATMO</code> computes results under a chosen setup.
        It does not decide whether those results are scientifically reasonable or how they should be explained.
      </p>
    </article>
  </div>

  <div class="takeaway-box">
    <p class="section-label">Important Reminder</p>
    <p><strong><code>PATMO</code> is a tool, not a judge.</strong></p>
    <p>
      It helps you run a controlled experiment, but it does not think on your behalf.
    </p>
  </div>
</section>

<section id="modern-sulfur-cycle" class="section-block">
  <div class="section-heading">
    <h2>Why This Course Uses The Modern Sulfur Cycle</h2>
  </div>

  <div class="lesson-grid">
    <div class="panel">
      <p>
        This course uses the <code>modern sulfur cycle</code> as its running case study,
        but that does not mean the course is trying to teach the entire theory of modern sulfur chemistry in full detail.
      </p>
      <p>
        Instead, the case is used because it is practical, scientifically meaningful,
        and rich enough to show several important parts of <code>PATMO</code> without becoming overwhelming for beginners.
      </p>
    </div>

    <div class="panel">
      <p class="section-label">Why It Works As A Teaching Case</p>
      <ul>
        <li>it is connected to a real research topic</li>
        <li>it involves several important <code>PATMO</code> components</li>
        <li>it is rich enough to feel realistic</li>
        <li>it is still manageable for beginners</li>
      </ul>
    </div>
  </div>

  <div class="panel">
    <p class="section-label">What Students Can See In This One Case</p>
    <ul>
      <li>a reaction network</li>
      <li>vertical profile inputs</li>
      <li>radiation-related inputs</li>
      <li>source and sink settings</li>
      <li>model outputs that can be compared and interpreted</li>
    </ul>
    <p>
      So the <code>modern sulfur cycle</code> is not the whole course.
      It is the main case through which students learn the model.
    </p>
  </div>

  <div class="lesson-grid">
    <article class="entry-card">
      <p class="card-label">How The Case Reappears</p>
      <ul>
        <li>Lecture 1: the scientific problem the course centers on</li>
        <li>Later lectures: the concrete example for files and inputs</li>
        <li>Later still: the first case students actually run</li>
        <li>Final stage: the basis for a simple sensitivity experiment</li>
      </ul>
    </article>

    <article class="entry-card">
      <p class="card-label">Where PATMO Sits In Research</p>
      <p>
        Running the model is not the whole research activity.
        <code>PATMO</code> sits in the middle of a larger process.
      </p>
    </article>
  </div>

  <pre class="lesson-flow"><code>scientific question
-> choose relevant species and processes
-> prepare model inputs
-> run PATMO
-> obtain outputs
-> compare experiments
-> interpret the results</code></pre>
</section>

<section class="section-block">
  <div class="section-heading">
    <h2>What You Do Not Need To Know Yet</h2>
  </div>

  <div class="lesson-grid">
    <article class="entry-card">
      <p class="card-label">Not Required Yet</p>
      <ul>
        <li><code>Fortran</code></li>
        <li><code>Python</code> programming</li>
        <li>advanced atmospheric chemistry</li>
        <li>advanced numerical analysis</li>
        <li>advanced command-line skills</li>
      </ul>
    </article>

    <article class="entry-card">
      <p class="card-label">What You Do Need</p>
      <ul>
        <li>willingness to learn from one case step by step</li>
        <li>willingness to connect files, workflow, and output meaningfully</li>
        <li>willingness to ask questions when something is unclear</li>
      </ul>
    </article>

    <article class="entry-card">
      <p class="card-label">Expected By The End Of The Course</p>
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
        <li>a full theory course on the modern sulfur cycle</li>
        <li>mathematical derivations of numerical solvers</li>
        <li>3D global model systems</li>
        <li>advanced source-code development</li>
      </ul>
    </article>
  </div>
</section>

<section class="section-block">
  <div class="section-heading">
    <h2>How The Seven Lectures Fit Together</h2>
  </div>

  <div class="course-roadmap">
    <article class="entry-card course-roadmap-card is-live">
      <p class="card-label">Lecture 1</p>
      <h3>What PATMO Is</h3>
      <p>Orientation: model identity, scope, and the role of the modern sulfur cycle.</p>
      <span class="course-status is-live">Current lecture</span>
    </article>

    <article class="entry-card course-roadmap-card is-upcoming">
      <p class="card-label">Lecture 2</p>
      <h3>The Overall Architecture of PATMO</h3>
      <p>How the major parts of the model fit together.</p>
      <span class="course-status">Planned</span>
    </article>

    <article class="entry-card course-roadmap-card is-upcoming">
      <p class="card-label">Lecture 3</p>
      <h3>The Input File System of PATMO</h3>
      <p>What the main files are and why each one matters.</p>
      <span class="course-status">Planned</span>
    </article>

    <article class="entry-card course-roadmap-card is-upcoming">
      <p class="card-label">Lecture 4</p>
      <h3>How the Modern Sulfur Cycle Is Written into PATMO</h3>
      <p>How the scientific case becomes model-ready inputs.</p>
      <span class="course-status">Planned</span>
    </article>

    <article class="entry-card course-roadmap-card is-upcoming">
      <p class="card-label">Lecture 5</p>
      <h3>The Runtime Workflow and Output System</h3>
      <p>What the model does when it runs and where the outputs appear.</p>
      <span class="course-status">Planned</span>
    </article>

    <article class="entry-card course-roadmap-card is-upcoming">
      <p class="card-label">Lecture 6</p>
      <h3>Hands-On Practice</h3>
      <p>Run the prepared modern sulfur cycle case for the first time.</p>
      <span class="course-status">Planned</span>
    </article>

    <article class="entry-card course-roadmap-card is-upcoming">
      <p class="card-label">Lecture 7</p>
      <h3>First Research-Style Exercise</h3>
      <p>Modify a case, rerun it, and compare a simple sensitivity experiment.</p>
      <span class="course-status">Planned</span>
    </article>
  </div>

  <pre class="lesson-flow"><code>meet PATMO
-> understand PATMO
-> run PATMO
-> use PATMO in the simplest research-style way</code></pre>
</section>

<section id="summary" class="section-block">
  <div class="section-heading">
    <h2>Check Your Understanding</h2>
  </div>

  <div class="lesson-grid">
    <div class="panel">
      <p>
        By the end of this lecture, students should be able to answer these questions in their own words:
      </p>
      <ol>
        <li>What is <code>PATMO</code>?</li>
        <li>Why is <code>PATMO</code> useful in research?</li>
        <li>Why is the <code>modern sulfur cycle</code> used in this course?</li>
        <li>What are you expected to be able to do by the end of the course?</li>
      </ol>
      <p>
        If the answers are not clear yet, read the key sections again and try to reduce each answer to one or two sentences.
      </p>
    </div>

    <div class="panel">
      <p class="section-label">Lecture Summary</p>
      <ol>
        <li><code>PATMO</code> is a one-dimensional atmospheric photochemistry model.</li>
        <li>It is used to study chemistry and related processes in a vertical atmospheric column.</li>
        <li>Its value lies in controlled and comparable computational experiments.</li>
        <li>The <code>modern sulfur cycle</code> is the running case study of the course, not the whole course content.</li>
        <li>The rest of the course moves step by step from understanding the model to running it and then using it in a simple experiment.</li>
      </ol>
    </div>
  </div>

  <div class="takeaway-box">
    <p class="section-label">After-Class Task</p>
    <p>
      Write a short answer, in no more than 200 words, to the following two questions:
    </p>
    <ol>
      <li>What is <code>PATMO</code> in your own understanding?</li>
      <li>What role does the <code>modern sulfur cycle</code> play in this course?</li>
    </ol>
    <p>
      Use your own words. The answer does not need to be perfect, but it should show understanding.
    </p>
  </div>

  <section class="panel">
    <p class="section-label">Final Reminder</p>
    <p>
      At this stage, students do not need to know how to run the model yet, they do not need to know the file system yet,
      and they do not need solver details yet. What they need from Lecture 1 is orientation:
      what <code>PATMO</code> is, why it matters, and how the course is structured.
    </p>
    <div class="hero-actions">
      <a class="pixel-button ghost" href="{{ '/work/patmo/' | relative_url }}">Back to course hub</a>
    </div>
  </section>
</section>

</div>
