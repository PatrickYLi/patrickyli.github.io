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

### 1. The Beer-Lambert Law and Absorption Cross-Sections

When a beam of light passes through light-absorbing molecules, energy is transferred to the absorbing molecules, so the intensity of the light gradually decreases. _**Pierre Bouguer**_, a mathematician, geophysicist, geodesist and astronomer, and _**Johann Heinrich Lambert**_, a physicist, mathematician and astronomer, observed that the fraction of light source that is transmitted ($I/I_o$) is independent of $I_o$, elucidated the relationship between the degree of absorption of light by a substance and the thickness of the absorbing medium in 1729 and 1760, respectively. _**August Beer**_, a German physicist, chemist and mathematician of Jewish descent, proposed a similar relationship between the degree of absorption of light and the concentration of light-absorbing substances in 1852. The two combined to obtain the basic law of light absorption Burger-Lambert-Beer law, referred to as Beer-Lambert law.  

The main content of the Beer-Lambert law is that the decrease in the light intensity, or **irradiance** ($I$) over the course of a small volume element is proportional to the irradiance of the light entering the element, the **concentration** of absorbers ($C$), and the **length** of the path through the absorbing medium ($dx$):  

\begin{equation}
\frac{d I}{d x}=-\varepsilon^{\prime} I C
\end{equation}

The proportionality constant (*$ \varepsilon^{\prime} $*) depends on the wavelength of the light source and on the light absorber's properties such as structure, orientation and environment. Integrating Eq. above can show that if the light source with irradiance $I_o$ is incident on a cell of thickness $l$, the irradiance of the transmitted light expression can be shown as:

\begin{equation}
I=I_{o} \exp \left(-\varepsilon^{\prime} C l\right)=I_{o} 10^{-\varepsilon C l} \equiv I_{o} 10^{-A}
\end{equation}

Where $A$ is the **absorbance** or **optical density** of the sample ($ A = \varepsilon C l$) and $ \varepsilon $ is called the **molar extinction coefficient** or **molar absorption coefficient** ($ \varepsilon = \varepsilon^{\prime} / ln(10) $). The absorbance is a dimensionaless quantity, so if $ C $ is given in units of molarity (1 **M** = 1 mol/l) and $ c $ in cm, $ \varepsilon $ thus has dimensions of **M**<sup>-1</sup> cm<sup>-1</sup>.

**How do we apply the Beer-Lambert law to spectral cross-section measurement?**

The Beer-Lambert law can be derived from an appproximation for the absorption coefficient for a molecule by approximating the molecule by an opaque disk whose cross-sectional area, $\sigma$, represents the effective area seen by a photon of frequency $w$. If the frequency of the light is far from resonance, the area is approximately 0, and if $w$ is close to resonance the area is a maximum. Taking an infinitesimal slab, $dz$, of sample:

![Derivation of Beer-Lambert Law](../imgs/MLNoiseOptimization/DerivationBeerLaw.png)

Where $I_o$ is the light intensity entering the sample at $ z = 0 $, $ I_z $ is the intensity enthering the infinitesimal slab at $ z $, $ dI $ is the light intensity absorbed in the slab, and $ I $ is the intensity of light leaving the sample. Then the toal opaue area on the slab due to the absorbers is $\sigma \cdot N \cdot A \cdot dz$. Then, the fraction of photons absorbed will be $\sigma \cdot N \cdot A \cdot dz / A $ so,

\begin{equation}
d I / I_{z}=-\sigma  N  d z
\end{equation}

Integrating this equation from $z = 0$ to $z = b$ gives,

\begin{equation}
ln(I) - ln(I_o) = -\sigma  N  b
\end{equation}

or,

\begin{equation}
-ln(\frac{I}{I_o}) = \sigma  N  b
\end{equation}

Let's organize cross-section $\sigma$ to one side of the equation and replace $b$ with path length $z$ of the measured cell,

\begin{equation}
\sigma=-\frac{1}{N z} \ln \frac{I}{I_{o}}=-\frac{R T}{P N_{A} z} \ln \frac{I}{I_{0}}
\end{equation}

Where $N$ is the number density of absorbing molecules, $z$ is the path length of the cell, $R$ is the gas constant, $T$ is the temperature, $N_{A}$ is the Avagadro number and $P$ is the pressure. Noted the calculation of number density uses the ideal gas law with the elements provided as following. Hence the cross-section $\sigma$ can be easily derived from measuring the incident lignt intensity and detected intensity after energy absorbing.



### 2. From Raw Signal to Cross-Sections
The general workflow of calculating cross-sections from the raw data is simplely illustrated as the figure below. 

![Workflow of Calculating Cross-Sections](../imgs/MLNoiseOptimization/Workflow.png)

#### 2.1 Drifting Effect Precaution

The measuring procedure (Step 2 above) is designed to work against the deuterium lamp (the UV light souce) intensity drifting. Considering the light intensity keeps decreasing during the measurements, the same time interval is used to measure the background signal and the sample signal sequentially, and the average of the two adjacent background signals is used to represent the reference background signal of the sample signal measured in the middle of the two background signal measurements. So that this can reduce the drifting effect.

#### 2.2 Signal-to-Noise Ratio and Dynamic Range Definition

The Signal-to-Noise Ratio (SNR) and Dynamic Range (DR) are two common parameters used to spefify the electrical performance of a spectrometer. Although a consensus seems to have been reached, it is very rare to find decent description of how to calculating noise. Here we will talk about how they are defined and how to measure and calculte them in the spectral measurements.

![Definitions of SNR and SR](../imgs/MLNoiseOptimization/SNRandSR.png)

The [Ibsen photonics Inc. ducument](https://ibsen.com/wp-content/uploads/Tech-Note-The-Signal-to-Noise-Ratio-SNR-and-Dynamic-Range-DR.pdf) gives delicate illustrations of SNR and DR as shown above. It points out that the range if digital signal output from the spectromeyter is usually $0$ to $2^{N}-1$, where $N$ is the number of bits in the Analogue-to-Digital (A/D) converter on the electronics. And typically, the $N$ number range from 10 to 16, i.e., the maximum signal level range between 1,023 to 65,535 counts.

Noise is the random variation of the signal around the mean value. The left panel of the above figure shows a spectrum with a sigle peak in wavelength and time. The peak signal level will fluctuate a small amount around the average value due to noise from the electronics. The noise is measured by the Root-Mean-Square (RMS) value of the fluctuations.

SNR is defined as signal level divided by noise level. It is suggested that in order to get an accurate result for teh SNR usually it is required to measure over 25-50 time samples of the spectrum. The figure below shows an example how to calculate average signal and noise using excel.

![Example of calculation of Signal and Noise for 25 measurements in Excel](../imgs/MLNoiseOptimization/CalculationofSNR.png)

The dynamic range is defined as the maximum possible signal level divided by the noise level when no light enters the spectrometer, and is called dark noise of the spectrometer. It is measured by taking 25-50 dark level measurements and calculating the RMS level with standard deviation function for each wavelength. The DR is then calculated as $2^{N}-1$ divided by the dark noise.

### 3. Data Filtering Using Gradient Descent

#### 3.1 Linearity of Absorbance and Concentration

We assume a beam of light passes through sample cell in the spectrometer, and when part of the energy, which represents a specific wavelength of light, is absorbed by the sample, the intensity of the light emitted from the sample cell $I$ will be lower than the incident light intensity $I_{o}$.

![Simple Illustration of a Sample Cell](../imgs/MLNoiseOptimization/SampleCell.png)

The term absorbance is used for illustrating the phenomenon, given the symbol, A. Expressed as

\begin{equation}
A= log_{10}(\frac{I_{o}}{I})=\epsilon l c
\end{equation}

Where $A$ is the absorbance, $ε$ is the molar attenuation coefficient or absorptivity of the attenuating species, $l$ is the optical path length in cm, and $c$ is the concentration of the attenuating species. From the above equation, it can be deduced that absorbance $A$ is directly proportional to the concentration $c$.

![Linearity of Absorbance and Concentration](../imgs/MLNoiseOptimization/AbsoConc.png)

In the actual absorption spectroscopy measurements, intercepting the spectra data at a particular wavelength under all concentration environments yields a plot demonstrating a linear relationship between absorbance and concentration, and the fitted linear relationship has an intercept of 0. During high resolution measurements, tens of thousand lines of dataset needs to be analyzed, generizing numerous dataset comparison and filtering work, which is ideal for machine learning workflow.

#### 3.2 Linear Regression using LAD algorithm 


<!-- ### The Interferometry -->


