---
content_type: page
title: Week 11
uid: 29b83d59-c75d-f1e9-d0bc-358816146102
---

Lecture 29: Lagrange Dual Problems
----------------------------------

### Summary

Started by reviewing the basic idea of Lagrange multipliers to find an extremum of one function f0(x) and one equality constraint h1(x)=0. We instead find an extremum of L(x,ν1)=f0(x)+ν1h1(x) over x and the _Lagrange multiplier_ ν1. The ν1 partial derivative of L ensures h1(x)=0, in which case L=f0 and the remaining derivatives extremize f0 along the constraint surface. Noted that ∇L=0 then enforces ∇f0\=0 in the direction parallel to the constraint, whereas perpendicular to the constraint ν1 represents a "force" that prevents x from leaving the h1(x)=0 constraint surface.

Generalized to the Lagrangian L(x,λ,ν) of the general optimization problem (the "primal" problem) with both inequality and equality constraints, following chapter 5 of the Boyd and Vandenberghe book (see below) (section 5.1.1).

Described the KKT conditions for a (local) optimum/extremum (Boyd, section 5.5.3). These are true in problems with strong duality, as pointed out by Boyd, but they are actually true in much more general conditions. For example, they hold under the "LICQ" condition in which the gradients of all the active constraints are linearly independents. Gave a simple graphical example to illustrate why violating LICQ requires a fairly weird optimum, at a cusp of two constraints.

Lecture 29 handout: [Lagrangian, Lagrange Dual Function and Dual Problem (PDF)](https://github.com/mitmath/18335/blob/master/notes/boyd-ch5-slides.pdf) in the online book [_Convex Optimization_](http://www.stanford.edu/~boyd/cvxbook/) by Stephen Boyd and Lieven Vandenberghe.

### Further Reading

*   Chapter 5 in the online book [_Convex Optimization_](http://www.stanford.edu/~boyd/cvxbook/) by Stephen Boyd and Lieven Vandenberghe.
*   There are many sources on [Lagrange multipliers](http://en.wikipedia.org/wiki/Lagrange_multipliers) (the special case of equality constraints) online that can be found by googling.

Lecture 30: Quasi-Newton Methods and the BFGS Algorithm; Lecture 31: Derivation of the BFGS Update
--------------------------------------------------------------------------------------------------

### Summary

Began discussing quasi-Newton methods in general, and the Broyden–Fletcher–Goldfarb–Shanno (BFGS) algorithm in particular, following the handout below. Quasi-Newton methods are methods used to either find zeroes or local maxima and minima of functions, as an alternative to Newton's method. In numerical optimization, the BFGS algorithm is an iterative method for solving unconstrained nonlinear optimization problems.

*   Lectures 30–31 handout: ![This resource may not render correctly in a screen reader.](/images/inacessible.gif)[Quasi-Newton Optimization: Origin of the BFGS Update (PDF)]({{< baseurl >}}/resources/mit18_335js19_lec30)

### Further Reading

*   [Quasi-Newton method](http://en.wikipedia.org/wiki/Quasi-Newton_methods) on Wikipedia
*   [Broyden-Fletcher-Goldfarb-Shanno algorithm](http://en.wikipedia.org/wiki/BFGS_method) on Wikipedia
*   [![This resource may not render correctly in a screen reader.](/images/inacessible.gif)A New Derivation of Symmetric Positive Definite Secant Updates (PDF - 1.4MB)](https://www.sciencedirect.com/science/article/pii/B9780124686625500124) by J.E. Dennis, Jr and Robert B. Schnabel.
*   [![This resource may not render correctly in a screen reader.](/images/inacessible.gif)The Linear Algebra of Block Quasi-Newton Algorithms (PDF)](http://www.cs.umd.edu/~oleary/reprints/j39.pdf) by Dianne P. O'Leary and A. Yeremin.