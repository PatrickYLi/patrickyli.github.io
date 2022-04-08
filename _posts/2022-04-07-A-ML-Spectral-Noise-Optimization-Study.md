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
> $ SNR = signal / noise $


### The Beer-Lambert Law  
When a beam of light passes through a solution or gas of absorbing molecules, energy is transferred to the absorbing molecules, so the intensity of the light gradually decreases. Scientists Pierre Bouguer and Johann Heinrich Lambert elucidated the relationship between the degree of absorption of light by a substance and the thickness of the absorbing medium in 1729 and 1760, respectively. August Beer proposed a similar relationship between the degree of absorption of light and the concentration of light-absorbing substances in 1852. The two combined to obtain the basic law of light absorption Burger-Lambert-Beer law, referred to as Beer-Lambert law.  

The main content of the Beer-Lambert law is that the decrease in the light intensity, or irradiance (*I*) over the course of a small volume element is proportional to the irradiance of the light entering the element, the concentration of absorbers (*C*), and the length of the path through the absorbing medium (*dx*):  

\begin{equation}
\frac{d I}{d x}=-\varepsilon^{\prime} I C
\end{equation}

### The Interferometry
