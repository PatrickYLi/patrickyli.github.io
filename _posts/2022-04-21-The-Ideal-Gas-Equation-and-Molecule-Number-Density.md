---
layout: post
title: The Ideal Gas Equation and Molecule Number Density
tags: [work]
---

<!-- MathJax 公式书写 -->
<head>
    <script src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML" type="text/javascript"></script>
    <script type="text/x-mathjax-config">
        MathJax.Hub.Config({
            tex2jax: {
            skipTags: ['script', 'noscript', 'style', 'textarea', 'pre'],
            inlineMath: [['$','$']]
            }
        });
    </script>
</head>

The gas molecule inside an enclosed vessel follows thermal movement, bringing collsion and exerting a force on the wall of the vessel. The large number of collisions that occur in all gas molecule results in the same pressure at any point in the vessel, regardless of where and in what direction the measurement is made.


One mole of substance contains 6.022×1023 molecules or atoms, speaking of gas, under standard atmosphere condition (101,325 Pa; 273.15 K), one mole of an ideal gas fills a volume of 22.414 L. Back in 1664, Robert Boyle did research on the relationship between gas pressure and gas volume and reached the Boyle-Mariotte law:

\begin{equation}
p \cdot v= \text{ const. }
\end{equation}

Which expresses that the volume of a given quantity of a gas at the constant temperature is inversely proportional to the pressure.

One hundred years later, Jacques Charles and Joseph Gay-Lussac stated the temperature dependence of the volume of a quantity of gas that the volume of a given quantity of gas at a constant pressure is directly proportional to the absolute temperature, which is expressed as the Gay-Lussac’s law:

\begin{equation}
V = \text{ const. } \cdot T
\end{equation}

Subjecting a certain amount of gas to continuous pressure changes and temperature changes, we get:

\begin{equation}
\frac{p \cdot V}{T}=\operatorname{const.}
\end{equation}

When we consider the molecule number of the gas, we obtain that the volume of the gas is proportional to the amount of substance ν of the gas molecules for a given temperature and a given pressure. The expression is:

\begin{equation}
\frac{p \cdot V}{T}= \nu \cdot \text { const. }
\end{equation}

It can also be written as:

\begin{equation}
p \cdot V=\frac{m}{M} \cdot R \cdot T
\end{equation}

Where

| $p$ | Pressure | [Pa] |
| $V$ | Volume | [m<sup>3</sup>] |
| $m$ | Mass | [kg] |
| $M$ | Molar mass | [kg  kmol<sup>-1</sup>] |
| $R$ | General gas constant | [kJ kmol<sup>-1</sup> K<sup>-1</sup>] |
| $T$ | Absolute temperature | [K] |

Besides, the substance amount ν can be indicated as the molecule number in relation to the Avogadro constant N_A, and it is expressed as (also known as the ideal gas equation $I$):

\begin{equation}
p \cdot V=\frac{N}{N_{A}} \cdot R \cdot T=N \cdot k \cdot T \quad \text { where } k=\frac{R}{N_{A}}
\end{equation}

Where

| $N$ | Molecule numbers | |
| $N_{A}$ | Avogadro constant | 6.022×10<sup>23</sup> [mol<sup>-1</sup>] |
| $k$ | Boltzmann constant | 1.381×10<sup>-23</sup> [J K<sup>-1</sup>] |

When both sides of the ideal gas equation are divided by the volume, then we can obtain the ideal gas equation II, which is expressed as:

\begin{equation}
p = n \cdot k \cdot T
\end{equation}

Where

| $n$ | Particle number density | [molecule m<sup>-3</sup>] |

Let's rearrange the equation here and we can obtain the derivation of molecule number density $n$, expressed as:

\begin{equation}
n = \frac{p}{k \cdot T}
\end{equation}

Assume the number of molecules of an ideal gas in a given volume, quoted at standard temperature and pressure, The 2014 CODATA recommended the value 2.6867811(15)×10<sup>25</sup> cm<sup>-3</sup> at 0 &deg;C and 1 atm, named after the physicist Johann Josef Loschmidt, known as the term **Loschmidt Constant** ($n_{0}$).

**Why do we use molecule number density as the expression of pressure?**

In experiments for spectral measurements, the concentration of the sample to be measured is usually reduced in order to avoid complete absorption of the light source signal due to high sample concentration. Also, to avoid the influence of oxygen, carbon dioxide and moisture in the air on the spectrogram, many high-precision spectrometers generate a vacuum-like environment with extremely low air pressure inside. 

According to the equations above, the pressure is proportional to the molecule number density. Notes that even at a pressure of 10<sup>-12</sup> hPa, there is still 26,500 molecule cm<sup>-3</sup> be present. It is increasingly ineffective to express pressure in the unit of Pascal at extremely low pressure, it is better to be recorded by the particle number density.

<center> Conversion table for unit of pressure </center>

|        |    Pa    |    bar    |    hPa    |   µbar   |    Torr   |  micron  |    atm    |     at    |   mm WS   |    psi    |    psf    |
|:------:|:--------:|:---------:|:---------:|:--------:|:---------:|:--------:|:---------:|:---------:|:---------:|:---------:|:---------:|
| Pa     | 1        | 1·10<sup>-5</sup>    | 1·10<sup>-2</sup>    | 10       | 7.5·10<sup>-3</sup>  | 7.5      | 9.87·10<sup>-6</sup> | 1.02·10<sup>-5</sup> | 0.102     | 1.45·10<sup>-4</sup> | 2.09·10<sup>-2</sup> |
| bar    | 1·10<sup>5</sup>    | 1         | 1·10<sup>3</sup>     | 1·10<sup>6</sup>    | 750       | 7.5·10<sup>5</sup>  | 0.987     | 1.02      | 1.02·10<sup>4</sup>  | 14.5      | 2.09·10<sup>3</sup>  |
| hPa    | 100      | 1·10<sup>-3</sup>    | 1         | 1,000    | 0.75      | 750      | 9.87·10<sup>-4</sup> | 1.02·10<sup>-3</sup> | 10.2      | 1.45·10<sup>-2</sup> | 2.09      |
| µbar   | 0.1      | 1·10<sup>-6</sup>    | 1·10<sup>-3</sup>    | 1        | 7.5·10<sup>-4</sup>  | 0.75     | 9.87·10<sup>-7</sup> | 1.02·10<sup>-6</sup> | 1.02·10<sup>-2</sup> | 1.45·10<sup>-5</sup> | 2.09·10<sup>-3</sup> |
| Torr   | 1.33·10<sup>2</sup> | 1.33·10<sup>-3</sup> | 1.33      | 1,330    | 1         | 1,000    | 1.32·10<sup>-3</sup> | 1.36·10<sup>-3</sup> | 13.6      | 1.93·10<sup>-2</sup> | 2.78      |
| micron | 0.133    | 1.33·10<sup>-6</sup> | 1.33·10<sup>-3</sup> | 1.33     | 1·10<sup>-3</sup>    | 1        | 1.32·10<sup>-6</sup> | 1.36·10<sup>-6</sup> | 1.36·10<sup>-2</sup> | 1.93·10<sup>-5</sup> | 2.78·10<sup>-3</sup> |
| atm    | 1.01·10<sup>5</sup> | 1.013     | 1,013     | 1.01·10<sup>6</sup> | 760       | 7.6·10<sup>5</sup>  | 1         | 1.03      | 1.03·10<sup>4</sup>  | 14.7      | 2.12·10<sup>3</sup>  |
| at     | 9.81·10<sup>4</sup> | 0.981     | 981       | 9.81·10<sup>5</sup> | 735.6     | 7.36·10<sup>5</sup> | 0.968     | 1         | 1·10<sup>-4</sup>    | 14.2      | 2.04·10<sup>3</sup>  |
| mm WS  | 9.81     | 9.81·10<sup>-5</sup> | 9.81·10<sup>-2</sup> | 98.1     | 7.36·10<sup>-2</sup> | 73.6     | 9.68·10<sup>-5</sup> | 1·10<sup>-4</sup>    | 1         | 1.42·10<sup>-3</sup> | 0.204     |
| psi    | 6.89·10<sup>3</sup> | 6.89·10<sup>-2</sup> | 68.9      | 6.89·10<sup>4</sup> | 51.71     | 5.17·10<sup>4</sup> | 6.8·10<sup>-2</sup>  | 7.02·10<sup>-2</sup> | 702       | 1         | 144       |
| psf    | 47.8     | 4.78·10<sup>-4</sup> | 0.478     | 478      | 0.359     | 359      | 4.72·10<sup>-4</sup> | 4.87·10<sup>-4</sup> | 4.87      | 6.94·10<sup>-3</sup> | 1         |


