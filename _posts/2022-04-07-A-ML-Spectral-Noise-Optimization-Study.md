---
layout: post
title: A ML Spectral Noise Optimization Study
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


![A simple spectral noise optimization example using SNR method](../imgs/MLNoiseOptimization/simpleeg.png)

> SNR (Signal-to-noise ratio) is a widely used sclae in science and engineering field to compare the strength of desired signal with the strength of the background noise. In practical application, havung a larger SNR means tha the data is more reliable.  

### The Beer-Lambert Law  

When a beam of light passes through light-absorbing molecules, energy is transferred to the absorbing molecules, so the intensity of the light gradually decreases. _**Pierre Bouguer**_, a mathematician, geophysicist, geodesist and astronomer, and _**Johann Heinrich Lambert**_, a physicist, mathematician and astronomer, observed that the fraction of light source that is transmitted (*I/I<sub>o</sub>*) is independent of *I<sub>o</sub>*, elucidated the relationship between the degree of absorption of light by a substance and the thickness of the absorbing medium in 1729 and 1760, respectively. _**August Beer**_, a German physicist, chemist and mathematician of Jewish descent, proposed a similar relationship between the degree of absorption of light and the concentration of light-absorbing substances in 1852. The two combined to obtain the basic law of light absorption Burger-Lambert-Beer law, referred to as Beer-Lambert law.  

The main content of the Beer-Lambert law is that the decrease in the light intensity, or **irradiance** (*I*) over the course of a small volume element is proportional to the irradiance of the light entering the element, the **concentration** of absorbers (*C*), and the **length** of the path through the absorbing medium (*dx*):  

\begin{equation}\label{eq1}
\frac{d I}{d x}=-\varepsilon^{\prime} I C
\end{equation}

The proportionality constant (*$ \varepsilon^{\prime} $*) depends on the wavelength of the light source and on the light absorber's properties such as structure, orientation and environment. Integrating Eq. above can show that if the light source with irradiance *I<sub>o</sub>* is incident on a cell of thickness *l*, the irradiance of the transmitted light expression can be shown as:

\begin{equation}
I=I_{o} \exp \left(-\varepsilon^{\prime} C l\right)=I_{o} 10^{-\varepsilon C l} \equiv I_{o} 10^{-A}
\end{equation}

Where *A* is the **absorbance** or **optical density** of the sample ($ A = \varepsilon C l$) and $ \varepsilon $ is called the **molar extinction coefficient** or **molar absorption coefficient** ($ \varepsilon = \varepsilon^{\prime} / ln(10) $). The absorbance is a dimensionaless quantity, so if $ C $ is given in units of molarity (1 **M** = 1 mol/l) and $ c $ in cm, $ \varepsilon $ thus has dimensions of **M**<sup>-1</sup> cm<sup>-1</sup>.

![Derivation of the Beer-Lambert Law](../imgs/MLNoiseOptimization/DerivationBeerLaw.gif)

### The Interferometry
