---
content_type: page
title: Week 10
uid: 11a637d9-eeaf-6a4b-61b1-048f51780804
---

Lecture 27: Adjoint Methods for Eigenproblems and Recurrence Relations
----------------------------------------------------------------------

### Summary

Discussed adjoint methods for eigenproblems and recurrence relations. A recurrence relation is an equation that recursively defines a sequence or multidimensional array of values, once one or more initial terms are given: each further term of the sequence or array is defined as a function of the preceding terms.

*   Lecture 27 handout: [Adjoint Methods and Sensitivity Analysis for Recurrence Relations (PDF)]({{< baseurl >}}/resources/mit18_335js19_lec27)
*   Lecture 27 notebook: [Recurrence Relation](https://nbviewer.jupyter.org/github/mitmath/18335/blob/master/notes/adjoint/Adjoint-method.ipynb)

Lecture 28: Trust-Regions Methods and the CCSA Algorithm
--------------------------------------------------------

### Summary

Discussed some general concepts in local optimization. Global convergence means convergence to a _local_ optimum from any _feasible_ starting point; explained why finding the feasible region from an _infeasible_ starting point is in general as hard as global optimization. A typical trust region approach is to _locally approximate_ the objective and constraint functions by some _simple functions_ that are easy to optimize, optimize them within some localized trust region around a current point x to obtain a candidate step y, and then either take the step (e.g. if y is an improvement) and/or update the approximations and trust region (e.g. if y was not an improvement or the approximation and exact functions differed greatly). There are many, many algorithms that follow this general outline, but they differ greatly in what approximations they use (e.g. linear, quadratic, ...), what trust region they use, and what methods they use to update the trust region and to evaluate candidate steps. Often, the approximate functions are _convex_ so that convex-optimization methods can be used to solve the _trust-region subproblems_.

Went over a particular example of a nonlinear optimization scheme, solving the full inequality-constrained nonlinear-programming problem: the CCSA algorithms, as refined by Svanberg (2002). This is a surprisingly simple algorithm (the [NLopt](http://ab-initio.mit.edu/nlopt) implementation is only 300 lines of C code), but is robust and provably convergent, and illustrates a number of important ideas in optimization: optimizing an approximation to update the parameters _x_, guarding the approximation with trust regions and penalty terms, and optimizing via the dual function (Lagrange multipliers). Like many optimization algorithms, the general ideas are very straightforward, but getting the details right can be delicate!

Outlined the inner/outer iteration structure of CCSA, and the interesting property that it produces a sequence of feasible iterates from a feasible starting point, which means that you can stop it early and still have a feasible solution (which is very useful for many applications where 99% of optimal is fine, but feasibility is essential).

The inner optimization problem involving the approximate gᵢ functions turns out to be _much_ easier to solve because it is _convex_ and _separable_ (gi = a sum of 1d convex functions of each coordinate xj). Convexity allows us to use the technique of duality to turn the problem into an equivalent "dual" optimization problem, and separability makes this dual problem trivial to formulate and solve. Began discussing the ideas of _Lagrangians_ and duality using the Boyd textbook; we will continue this in the next lecture.

### Further Reading

*   Pages 1–10 of [A Class of Globally Convergent Optimization Methods Based on Conservative Convex Separable Approximations](http://dx.doi.org/10.1137/S1052623499362822) by Krister Svanberg.