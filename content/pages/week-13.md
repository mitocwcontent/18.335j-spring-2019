---
content_type: page
title: Week 13
uid: f0c2e826-9307-e9b8-dfff-15f04276fcac
---

Lecture 35: Chebyshev Approximation
-----------------------------------

### Summary

Explained connection of Clenshaw-Curtis quadrature and cosine series to Chebyshev polynomials. This leads into the general topic of Chebyshev approximation, and how we can approximate any smooth function on a finite interval by a polynomial with exponential accuracy (in the degree of the polynomial) as long as we interpolate via Chebyshev points.

Using Chebyshev approximation, explained how lots of problems can be solved by first approximating a nasty function via a polynomial, at which point one can just use easy methods for polynomials. Showed examples of root finding, minimization, integration, and solving ODEs via the [ApproxFun](https://github.com/ApproxFun/ApproxFun.jl) package for Julia, which implements a modern version of these ideas (following in the tracks of the pioneering [Chebfun](http://www.chebfun.org/) package for MATLAB).

### Further Reading

*   [Chebyshev polynomials](http://en.wikipedia.org/wiki/Chebyshev_polynomials) on Wikipedia.
*   Online book [Chebyshev and Fourier Spectral Methods](http://www-personal.umich.edu/~jpboyd/BOOK_Spectral2000.html) by John P. Boyd.
*   [Numerical Complex Analysis](http://www.maths.usyd.edu.au/u/olver/teaching/NCA/) by Sheehan Olver.

Lecture 36: Integration with Weight Functions, and Gaussian Quadrature
----------------------------------------------------------------------

### Summary

Discussed integration with weight functions: I = ∫w(x)f(x)dx ≈ In = ∑wif(xi). Even if the "weight" w(x) is "nasty" (discontinuous, singular, highly oscillatory…), we can do some (possibly expensive) precomputations of wi and xi _once_ and re-use them for integrating many "nice" (smooth) functions f(x) with only a few quadrature points.

One approach to this is weighted Clenshaw-Curtis quadrature: expand f(x) in Chebyshev polynomials, i.e. f(cosθ) in a cosine series, as before via a trapezoidal rule (DCT), and then compute the integrals ∫w(cosθ)cos(kθ)sin(θ)dθ in whatever way you can … possibly by a very slowly converging method, but you only need to compute these weight integrals _once_ and can then re-use them for many different f(x).

An alternative approach (for real w ≥ 0, and w > 0 almost everywhere) is [Gaussian quadrature](https://en.wikipedia.org/wiki/Gaussian_quadrature): fit f(x) to a polynomial of degree n–1 by evaluating at n points consisting of the roots of an orthogonal polynomial qn+1 of degree n, where the polynomials {q1,q2,…} are formed via Gram-Schmidt orthogonalization of the basis {1,x,x2,…} with the inner product (p,q)=∫w(x)p(x)q(x)dx. There is a beautiful proof that this integrates polynomials exactly up to degree 2n–1, it takes w(x) into account analytically, and by a more complicated analysis it converges exponentially for analytic f. Moreover, it turns out that finding {q1,q2,…} is equivalent to performing the Lanczos iteration with the real-symmetric linear operator x, leading to a three-term recurrence. We already saw one such three-term recurrence for the Chebyshev polynomials Tn(x), corresponding to the weight \\(\\text{w(x)}=1/\\sqrt{1-\\text{x}^2}\\), and other weight functions give rise to other orthogonal polynomials with their own 3-term recurrences: w=1 gives [Legendre polynomials](https://en.wikipedia.org/wiki/Legendre_polynomials), but there are also [Gegenbauer polynomials](https://en.wikipedia.org/wiki/Gegenbauer_polynomials), [Laguerre polynomials](https://en.wikipedia.org/wiki/Laguerre_polynomials) for w=e\-x on 0…\\(\\infty\\), [Hermite polynomials](https://en.wikipedia.org/wiki/Hermite_polynomials) for w=exp(-x2) on -\\(\\infty\\)…+\\(\\infty\\), and others. Moreover, the Lanczos tridiagonal eigenvalues turn out to be precisely the roots of qn+1 that we want, while the eigenvectors turn out to give the weights, and for a long time this surprising(!) O(n2) "Golub-Welsch" method was the method of choice for finding Gaussian quadrature nodes and weights.

### Further Reading

*   [A Comparison of Some Methods for the Evaluation of Highly Oscillatory Integrals](https://doi.org/10.1016/S0377-0427(99)00213-7) (by G.A. Evans and J.R.Webster) describes the weighted Clenshaw-Curtis approach to oscillatory integrals.
*   [![This resource may not render correctly in a screen reader.](/images/inacessible.gif)Numerical Approximation of Highly Oscillatory Integrals (PDF)](http://www.maths.usyd.edu.au/u/olver/papers/OlverThesis.pdf) by Sheehan Olver.
*   Read “Lecture 37” in the textbook _Numerical Linear Algebra_.
*   O(n) methods have been discovered to find the Gaussian quadrature points and weights; see the references in the Julia [FastGaussQuadrature](https://github.com/JuliaApproximation/FastGaussQuadrature.jl) package.
*   An influential recent comparison of Clenshaw-Curtis and Gaussian quadrature is: Trefethen, Lloyd N. [Is Gauss Quadrature Better Than Clenshaw-Curtis?](http://citeseerx.ist.psu.edu/viewdoc/summary?doi=10.1.1.157.4174), _SIAM Review_ 50 (1), 67-87 (2008).

Lecture 37: Adaptive and Multidimensional Quadrature
----------------------------------------------------

### Summary

Discussed the importance of nested quadrature rules for _a posteriori_ error estimation (Clenshaw-Curtis, Gauss-Kronrod, Gauss-Patterson) and adaptive quadrature. Discussed p-adaptive vs. h-adaptive adaptive schemes. Talked about multidimensional integration and the curse of dimensionality.