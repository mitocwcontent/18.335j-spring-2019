---
content_type: page
title: Week 12
uid: dd105dfc-a5c9-7a7d-ef21-71a11a6c112d
---

Lecture 32: Derivative-Free Optimization by Linear and Quadratic Approximations
-------------------------------------------------------------------------------

### Summary

Introduced derivative-free optimization algorithms, for the common case where you don't have too many parameters (tens or hundreds) and computing the gradient is inconvenient (complicated programming, even if adjoint methods are theoretically applicable) or impossible (non-differentiable objectives). Started by discussing methods based on linear interpolation of simplices, such as the COBYLA algorithm of Powell.

Discussed derivative-free optimization based on quadratic approximation by symmetric Broyden updates (as in BOBYQA/NEWUOA algorithm of Powell, for example). Updating the Hessian turns into a quadratic programming (QP) problem, and discussed solution of QPs by construction of the dual, turning it into either an unconstrained QP (and hence a linear system) for equality constrained problems, or a bound-constrained QP for inequality-constrained QPs.

Briefly discussed global optimization (GO). For general objectives, there are no "good" GO algorithms; it is easy to devise an objective for which GO is arbitrarily hard (the problem is NP-hard in general). Many (but not all) GO algorithms have a proof (often trivial) that they converge to the global optimum asymptotically, but little can be said about how _fast_ they converge, and in general there is no way to know when the global optimum has been found—stopping criteria for global optimization tend to be "stop when the design is good enough" or "stop when you run out of patience". In high dimensions, GO is often hopeless unless you have a function that is very "nice" (has only a few local optima). There are a few categories of GO algorithms, depending on how they perform the _global_ and _local_ portions of their search.

*   _Stochastic global & local search:_ Algorithms like genetic algorithms, simulated annealing, and so on, typically rely on randomized algorithms for both exploring the global search space and for refining local optimum. These algorithms are easy to implement, assume little or nothing about the objective, are easy to parallelize, and have evocative natural analogies. As a result, they perhaps have a disproportionate mind share: they should often be treated as methods of last resort, because they don't exploit any mathematical structure of the objective (e.g. smoothness or even continuity).
*   _Stochastic global & deterministic local search:_ The typical algorithm in this category is a "multistart" algorithm: perform repeated local optimizations from randomized starting points. This sort of algorithm can take advantage of very efficient local optimizers, especially if you have a differentiable function and the gradient is available. On the other hand, effective use often requires some tweaking to choose the stopping criteria of the local search, and the algorithms will often search the same local optimum many times. One attempt to deal with the latter problem is the MLSL algorithm, which is a multistart algorithm with a "filter" to prevent repeated searches of the same optimum (the filtering rule for starting points is not perfect, but it provably leads to each optimum being searched a finite number of times asymptotically).
*   _Deterministic global & local search:_ These are typically "branch-and-bound" algorithms that systematically divide up the search domain and search the whole thing. For a black-box objective, one must asymptotically search the entire domain, but heuristics can be used to identify which subregions to search first, and a typical algorithm of this sort is DIRECT. Given more knowledge of the objective function, one can ideally devise a method to compute a _lower bound_ of the objective in each subregion, and in this way subregions can be eliminated from the search if their lower bound is above the best value found so far. A typical tool to construct such bounds is Taylor arithmetic, e.g. via the COSY INFINITY software. A typical example of branch-and-bound GO software based on analytical lower bounds is the BARON program.

### Further Reading

*   Conn, Andrew R. et al. _Introduction to Derivative-Free Optimization_. SIAM: Society for Industrial and Applied Mathematics, 2009. ISBN: 9780898716689.
*   See also [NLopt Algorithms](https://nlopt.readthedocs.io/en/latest/NLopt_Algorithms/) and the references thereof.
*   Several more stochastic algorithms in Julia are implemented in the [BlackBoxOptim](https://github.com/robertfeldt/BlackBoxOptim.jl) package.

Lecture 33: Numerical Integration and the Convergence of the Trapezoidal Rule
-----------------------------------------------------------------------------

### Summary

New topic: numerical integration (numerical quadrature). Began by basic definition of the problem (in 1d) and differences from general ODE problems. Then gave trapezoidal quadrature rule, and simple argument why the error generally decreases with the square of the number of function evaluations.

Showed numerical experiment (see handout) demonstrating that sometimes the trapezoidal rule can do much better than this: it can even have exponential convergence with the number of points! To understand this at a deeper level, I analyze the problem using Fourier cosine series (see handout), and show that the error in the trapezoidal rule is directly related to the convergence rate of the Fourier series. Claimed that this convergence rate is related to the smoothness of the periodic extension of the function, and in fact an analytic periodic function has Fourier coefficients that vanish exponentially fast, and thus the trapezoidal rule converges exponentially in that case. Proved by integration by parts of the Fourier series. In fact, we find that only the _odd_\-order derivatives at the endpoints need to be periodic to get accelerated convergence.

*   Lecture 33 handout 1: ![This resource may not render correctly in a screen reader.](/images/inacessible.gif)[Numerical Integration and the Redemption of the Trapezoidal Rule (PDF)]({{< baseurl >}}/resources/mit18_335js19_lec33_1)
*   Lecture 33 handout 2: ![This resource may not render correctly in a screen reader.](/images/inacessible.gif)[Fourier Cosine Series Examples (PDF)]({{< baseurl >}}/resources/mit18_335js19_lec33_2)

Lecture 34: Clenshaw-Curtis Quadrature
--------------------------------------

### Summary

Explained the idea of Clenshaw-Curtis quadrature as a change of variables + a cosine series to turn the integral of _any_ function into the integral of periodic functions. This way, functions only need to be analytic on the interior of the integration interval in order to get exponential convergence. 

Also explained (as in the handout) how to precompute the weights in terms of a discrete cosine transform, rather than cosine-transforming the function values every time one needs an integral, via a simple transposition trick.

Discussed the importance of nested quadrature rules for _a posteriori_ error estimation and adaptive quadrature. Discussed p-adaptive vs. h-adaptive adaptive schemes.

### Further Reading

*   Trefethen, Lloyd N. [Is Gauss Quadrature Better Than Clenshaw-Curtis?](http://citeseerx.ist.psu.edu/viewdoc/summary?doi=10.1.1.157.4174), _SIAM Review_ 50 (1), 67–87 (2008).
*   Gonnet, Pedro. [A Review of Error Estimation in Adaptive Quadrature](http://dl.acm.org/citation.cfm?id=2333117), _ACM Computing Surveys_ 44, article 22 (2012).