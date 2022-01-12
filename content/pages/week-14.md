---
content_type: page
title: Week 14
uid: 91f42844-de39-df8a-3812-24c45f7815b4
---

Lecture 38: The Discrete Fourier Transform (DFT) and FFT Algorithms
-------------------------------------------------------------------

### Summary

Introduced the [discrete Fourier transform (DFT)](https://en.wikipedia.org/wiki/Discrete_Fourier_transform). Talked about its history (Gauss!), properties (unitarity, convolution theorem), aliasing, special case of the type-1 [discrete cosine transform (DCT)](https://en.wikipedia.org/wiki/Discrete_cosine_transform), and applications (Chebyshev and other spectral methods for integration, PDEs, etcetera; signal processing, [Schönhage-Strassen algorithm](https://en.wikipedia.org/wiki/Sch%C3%B6nhage%E2%80%93Strassen_algorithm)), etc.

A [fast Fourier transform (FFT)](https://en.wikipedia.org/wiki/Fast_Fourier_transform) is an O(_N_ log _N_) algorithm to compute the discrete Fourier transform. There are many such algorithms, the most famous of which is the Cooley-Tukey algorithm (1965, though there were many precursors dating back to Gauss himself).

*   Lecture 38 handout: ![This resource may not render correctly in a screen reader.](/images/inacessible.gif)[Fast Fourier Transform Algorithms (PDF)]({{< baseurl >}}/resources/mit18_335js19_lec38)

Lecture 39: FFT Algorithms and FFTW
-----------------------------------

### Summary

Continued on fast Fourier transform (FFT). Talked about fast Fourier transform in the west (FFTW). The fast Fourier transform in the west is a software library for computing discrete Fourier transforms (DFTs) developed by Matteo Frigo and Steven G. Johnson at MIT. FFTW is known as the fastest free software implementation of the fast Fourier transform (FFT) (upheld by regular benchmarks). Like many other implementations, it can compute transforms of real and complex-valued arrays of arbitrary size and dimension in O(_N_ log _N_) time.

*   Lecture 39 handout: [Fast Fourier Transform and Fast Fourier Transform in the West (PDF - 2.6MB)]({{< baseurl >}}/resources/mit18_335js19_lec39)