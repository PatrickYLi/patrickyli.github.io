---
layout: page
title: "Lecture 6: Run PATMO On An Ubuntu Server"
subtitle: From GitHub download to compiling and running the modern sulfur cycle case.
permalink: /work/patmo/lecture-06/
body_class: patmo-reading-theme
head-extra:
  - patmo-reading-theme.html
description: Lecture 6 of the PATMO student course, guiding students through cloning PATMO on an Ubuntu server, preparing the modern sulfur cycle input files, compiling in build, and running ./test.
---

<div class="patmo-course">

<section class="panel">
  <div class="lesson-breadcrumb">
    <a href="{{ '/work/patmo/' | relative_url }}">PATMO Course</a>
    <span>/</span>
    <span>Lecture 6 of 7</span>
  </div>

  <p class="section-label">Student Handout</p>
  <p>
    Lecture 5 explained how wet deposition is written in the modern sulfur cycle.
    This lecture is the first full server-side operation class:
    log in to an Ubuntu server, download <code>PATMO</code> from GitHub, prepare the
    <code>modern_sulfur_cycle</code> input files, compile the model in <code>build</code>, and run <code>./test</code>.
  </p>
  <p>
    The goal is not to change the science yet. The goal is to make every student able to reach a successful baseline run.
  </p>

  <figure class="panel">
    <img src="{{ '/assets/img/patmo/patmo-server-run-workflow.svg' | relative_url }}" alt="Workflow diagram from Ubuntu server setup to GitHub clone, modern sulfur input files, PATMO preprocessing, build, make, and test run.">
    <figcaption>
      The practical path for this lecture: prepare the server, clone PATMO, prepare the modern sulfur cycle case, compile, and run.
    </figcaption>
  </figure>

  <div class="hero-actions">
    <a class="pixel-button ghost" href="{{ '/work/patmo/lecture-05/' | relative_url }}">Previous Lecture</a>
    <a class="pixel-button secondary" href="{{ '/work/patmo/' | relative_url }}">Course Hub</a>
  </div>
</section>

<section id="references" class="section-block">
  <div class="section-heading">
    <h2>Reference Pages</h2>
  </div>

  <div class="lesson-grid">
    <article class="entry-card">
      <p class="card-label">PATMO User Guide</p>
      <p>
        The guide describes PATMO as a modular 1-D atmospheric model and provides the Ubuntu installation workflow used here.
      </p>
      <p><a href="https://sites.google.com/sophia-atmochem-lab.org/patmo-user-guide" target="_blank" rel="noreferrer">Open PATMO User Guide</a></p>
    </article>

    <article class="entry-card">
      <p class="card-label">Install PATMO</p>
      <p>
        The installation page lists the Ubuntu tools used in this lecture: compilers, Git, Python, and scientific Python libraries.
      </p>
      <p><a href="https://sites.google.com/sophia-atmochem-lab.org/patmo-user-guide/home/getting-started/1-1-install-patmo" target="_blank" rel="noreferrer">Open install guide</a></p>
    </article>

    <article class="entry-card">
      <p class="card-label">GitHub Repository</p>
      <p>
        The class starts by cloning the official PATMO repository from GitHub.
      </p>
      <p><a href="https://github.com/SophiaAtmo/PATMO" target="_blank" rel="noreferrer">Open PATMO GitHub repository</a></p>
    </article>
  </div>
</section>

<section id="server-check" class="section-block">
  <div class="section-heading">
    <h2>Server Checklist</h2>
  </div>

  <div class="panel">
    <p>
      Use an Ubuntu server or an Ubuntu terminal through WSL. The user guide targets Ubuntu 20.04 or newer.
      Students need internet access, permission to install packages, and a working shell.
    </p>

    <pre class="lesson-flow"><code>ssh username@server_address
mkdir -p ~/patmo_course
cd ~/patmo_course</code></pre>
  </div>

  <div class="table-wrap">
    <table>
      <thead>
        <tr>
          <th>Tool</th>
          <th>Why it is needed</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <td><code>git</code></td>
          <td>Download the PATMO source code from GitHub.</td>
        </tr>
        <tr>
          <td><code>gfortran</code>, <code>gcc</code>, <code>g++</code>, <code>make</code></td>
          <td>Compile the Fortran source files and build the <code>test</code> executable.</td>
        </tr>
        <tr>
          <td><code>python3</code> and Python libraries</td>
          <td>Run the PATMO preprocessor and convert spreadsheet-style input files into model input files.</td>
        </tr>
      </tbody>
    </table>
  </div>
</section>

<section id="install-tools" class="section-block">
  <div class="section-heading">
    <h2>Install The Required Tools</h2>
  </div>

  <div class="panel">
    <p>
      Install the basic compilers, Git, Make, and Python packages before cloning PATMO.
      On a teaching server, this step may already be done by the instructor.
    </p>

    <pre class="lesson-flow"><code>sudo apt update
sudo apt install -y git make gfortran gcc g++ \
    python3 python3-pip \
    python3-numpy python3-scipy python3-matplotlib \
    python3-pandas python3-openpyxl</code></pre>
  </div>

  <div class="panel">
    <p>
      Check that the important commands are visible to the shell:
    </p>

    <pre class="lesson-flow"><code>git --version
gfortran --version
python3 --version
python3 -c "import numpy, scipy, matplotlib, pandas, openpyxl; print('Python libraries OK')"</code></pre>
  </div>
</section>

<section id="clone-patmo" class="section-block">
  <div class="section-heading">
    <h2>Download PATMO From GitHub</h2>
  </div>

  <div class="panel">
    <p>
      Clone the repository and enter the PATMO root directory.
      In the examples below, the root directory is called <code>PATMO</code>.
    </p>

    <pre class="lesson-flow"><code>git clone https://github.com/SophiaAtmo/PATMO.git
cd PATMO
pwd
ls</code></pre>
  </div>

  <div class="takeaway-box">
    <p class="section-label">Where You Should Be</p>
    <p>
      The commands below assume that the terminal is in the PATMO root directory,
      the same level that contains <code>patmo</code>, <code>compile.sh</code>, <code>build</code>, <code>src_f90</code>, and <code>tests</code>.
    </p>
  </div>
</section>

<section id="case-files" class="section-block">
  <div class="section-heading">
    <h2>Find The Modern Sulfur Cycle Files</h2>
  </div>

  <div class="panel">
    <p>
      The prepared case is stored in <code>tests/modern_sulfur_cycle</code>.
      Before running anything, students should inspect the files and connect them to previous lectures.
    </p>

    <pre class="lesson-flow"><code>ls tests/modern_sulfur_cycle</code></pre>
  </div>

  <div class="table-wrap">
    <table>
      <thead>
        <tr>
          <th>File</th>
          <th>Role in this run</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <td><code>reaction_network.xlsx</code></td>
          <td>Editable reaction-network spreadsheet.</td>
        </tr>
        <tr>
          <td><code>reaction_network.ntw</code></td>
          <td>PATMO-readable reaction network generated from the spreadsheet.</td>
        </tr>
        <tr>
          <td><code>settings.xlsx</code></td>
          <td>Editable model settings used to create <code>options.opt</code>.</td>
        </tr>
        <tr>
          <td><code>profile.xlsx</code> and <code>profile.dat</code></td>
          <td>Atmospheric vertical profile used to initialize the model column.</td>
        </tr>
        <tr>
          <td><code>solar_flux.xlsx</code> and <code>solar_flux.txt</code></td>
          <td>Solar flux input for photochemistry.</td>
        </tr>
        <tr>
          <td><code>test.f90</code></td>
          <td>The Fortran driver program compiled into the final <code>test</code> executable.</td>
        </tr>
        <tr>
          <td><code>Rainout-*.txt</code> and sulfur deposition files</td>
          <td>Wet deposition, aerosol, and related sulfur-cycle input data used by <code>test.f90</code>.</td>
        </tr>
      </tbody>
    </table>
  </div>
</section>

<section id="prepare-inputs" class="section-block">
  <div class="section-heading">
    <h2>Compile The Modern Sulfur Cycle Input Files</h2>
  </div>

  <div class="panel">
    <p>
      In this lecture, <strong>compile input files</strong> means converting the editable classroom files into the files PATMO actually reads,
      then asking the PATMO preprocessor to prepare the <code>build</code> directory for this test case.
    </p>
    <p>
      Copy the modern sulfur compile script to the PATMO root directory and run it there:
    </p>

    <pre class="lesson-flow"><code>cp tests/modern_sulfur_cycle/compile_modern_sulfur_cycle.sh .
chmod +x ./compile_modern_sulfur_cycle.sh
./compile_modern_sulfur_cycle.sh</code></pre>
  </div>

  <div class="panel">
    <p>
      When the script asks for the folder name under <code>tests</code>, type:
    </p>

    <pre class="lesson-flow"><code>modern_sulfur_cycle</code></pre>

    <p>
      The script converts <code>reaction_network.xlsx</code> to <code>reaction_network.ntw</code>,
      converts <code>settings.xlsx</code> to <code>options.opt</code>, prepares profile and solar-flux input files,
      creates <code>copylist.pcp</code>, validates output settings in <code>test.f90</code>, and runs:
    </p>

    <pre class="lesson-flow"><code>python3 patmo -test="modern_sulfur_cycle"</code></pre>
  </div>

  <div class="takeaway-box">
    <p class="section-label">Do Not Skip This Step</p>
    <p>
      Running <code>make</code> before the PATMO preprocessor has prepared <code>build</code> usually means the build folder is using old files,
      missing files, or a different test case.
    </p>
  </div>
</section>

<section id="build-and-run" class="section-block">
  <div class="section-heading">
    <h2>Build And Run</h2>
  </div>

  <div class="panel">
    <p>
      After the input/preprocessor step finishes, enter the <code>build</code> folder, compile the executable, and run it.
    </p>

    <pre class="lesson-flow"><code>cd build
make clean
make
./test</code></pre>
  </div>

  <div class="lesson-grid">
    <article class="entry-card">
      <p class="card-label"><code>make</code></p>
      <p>
        Compiles PATMO source files and the selected <code>test.f90</code> driver into an executable named <code>test</code>.
      </p>
    </article>

    <article class="entry-card">
      <p class="card-label"><code>./test</code></p>
      <p>
        Runs the modern sulfur cycle case. Students should watch the terminal for progress, mass information, and any error message.
      </p>
    </article>
  </div>

  <div class="takeaway-box">
    <p class="section-label">Successful Baseline</p>
    <p>
      A successful class run means the executable is created, <code>./test</code> starts, and the model reaches the end of the run without a crash.
      Scientific interpretation comes after this operational baseline is stable.
    </p>
  </div>
</section>

<section id="troubleshooting" class="section-block">
  <div class="section-heading">
    <h2>Troubleshooting</h2>
  </div>

  <div class="table-wrap">
    <table>
      <thead>
        <tr>
          <th>Problem</th>
          <th>Likely fix</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <td><code>Permission denied</code> when running the script</td>
          <td>Run <code>chmod +x ./compile_modern_sulfur_cycle.sh</code>.</td>
        </tr>
        <tr>
          <td><code>Folder not found: ./tests/...</code></td>
          <td>Run the script from the PATMO root directory and enter <code>modern_sulfur_cycle</code> exactly.</td>
        </tr>
        <tr>
          <td><code>ModuleNotFoundError</code> for Python packages</td>
          <td>Install <code>python3-pandas</code> and <code>python3-openpyxl</code>, or use the Python setup recommended by the server administrator.</td>
        </tr>
        <tr>
          <td><code>gfortran: command not found</code></td>
          <td>Install <code>gfortran</code> with <code>sudo apt install -y gfortran</code>.</td>
        </tr>
        <tr>
          <td><code>./test: No such file or directory</code></td>
          <td>Make sure <code>make</code> finished successfully inside <code>build</code>.</td>
        </tr>
      </tbody>
    </table>
  </div>

  <div class="hero-actions">
    <a class="pixel-button ghost" href="{{ '/work/patmo/lecture-05/' | relative_url }}">Previous Lecture</a>
    <a class="pixel-button secondary" href="{{ '/work/patmo/' | relative_url }}">Back to course hub</a>
  </div>
</section>

</div>
