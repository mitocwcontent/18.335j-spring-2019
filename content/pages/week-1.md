---
content_type: page
title: Week 1
uid: 6950d4a0-73bc-0d04-aa63-962ae31c42a2
---

Lecture 1: Course Overview, Newton's Method for Root-Finding
------------------------------------------------------------

### Summary

Brief overview of the huge field of numerical methods and outline of the small portion that this course will cover. Key new concerns in numerical analysis, which don't appear in more abstract mathematics, are (i) performance (traditionally, arithmetic counts, but now memory access often dominates) and (ii) accuracy (both floating-point roundoff errors and also convergence of intrinsic approximations in the algorithms).

As a starting example, we considered the convergence of Newton's method (as applied to square roots); see the handout and Julia notebook below.

*   Lecture 1 handout: [Square Roots via Newton's Method (PDF)]({{< baseurl >}}/resources/mit18_335js19_lec1)
*   Lecture 1 notebook: [Square Roots](http://nbviewer.jupyter.org/github/mitmath/18335/blob/master/notes/Newton-Square-Roots.ipynb)

### Assignment

*   [Problem set 1 (PDF)]({{< baseurl >}}/resources/mit18_335js19_pset1)
*   Problem set 1 notebook: [Problem set 1](https://nbviewer.jupyter.org/github/mitmath/18335/blob/master/psets/pset1.ipynb)
*   [Problem set 1 solutions (PDF)]({{< baseurl >}}/resources/mit18_335js19_pset1sol)
*   Problem set 1 solutions notebook: [Problem set 1 solutions](http://nbviewer.jupyter.org/github/mitmath/18335/blob/master/psets/pset1sol.ipynb)

### Further Reading

*   [Newton's method](https://en.wikipedia.org/wiki/Newton's_method) from Wikipedia is a reasonable starting point. Googling "Newton's method" can find lots of references.
*   Beware that the terminology for the [rate of convergence](https://en.wikipedia.org/wiki/Rate_of_convergence) (linear, quadratic, etc.) is somewhat different in this context from the terminology for discretization schemes (first-order, second-order, etc.); see e.g. the linked Wikipedia article.

Lecture 2: Floating-Point Arithmetic
------------------------------------

### Summary

The basic issue is that, for computer arithmetic to be fast, it has to be done in hardware, operating on numbers stored in a fixed, finite number of digits (bits). As a consequence, only a _finite subset_ of the real numbers can be represented, and the question becomes _which subset_ to store, how arithmetic on this subset is defined, and how to analyze the errors compared to theoretical exact arithmetic on real numbers.

In floating-point arithmetic, we store both an integer coefficient and an exponent in some base: essentially, scientific notation. This allows large dynamic range and fixed _relative_ accuracy: if fl(_x_) is the closest floating-point number to any real _x_, then |fl(_x_)-_x_| < ε|_x_| where ε is the _machine precision_. This makes error analysis much easier and makes algorithms mostly insensitive to overall scaling or units, but has the disadvantage that it requires specialized floating-point hardware to be fast. Nowadays, all general-purpose computers, and even many little computers like your cell phones, have floating-point units.

Went through some simple examples in Julia (see notebook below), illustrating basic syntax and a few interesting tidbits, in particular on the accuracy of summation algorithms, that we will investigate in more detail later.

Overview of floating-point representations, focusing on the IEEE 754 standard (see also handout from previous lecture). The key point is that the nearest floating-point number to _x_, denoted fl(_x_), has the property of _uniform relative precision_ (for |_x_| and 1/|_x_| less than some _range_, ≈10308 for double precision) that |fl(_x_)−_x_| ≤ εmachine|_x_|, where εmachine is the relative "machine precision" (about 10−16 for double precision). There are also a few special values: ±Inf (e.g. for [overflow](https://en.wikipedia.org/wiki/Arithmetic_overflow)), [NaN](https://en.wikipedia.org/wiki/NaN), and ±0 (e.g. for [underflow](https://en.wikipedia.org/wiki/Arithmetic_underflow)).

*   Lecture 2 handout: [Floating-Point Arithmetic, the IEEE Standard (PDF)]({{< baseurl >}}/resources/mit18_335js19_lec2) (Courtesy of Per-Olof Persson. Used with permission.)
*   Lecture 2 notebook: [Floating-Point Arithmetic](http://nbviewer.jupyter.org/github/mitmath/18335/blob/master/notes/Floating-Point-Intro.ipynb)
*   [Some Myths about Floating-Point Arithmetic (PDF)]({{< baseurl >}}/resources/mit18_335js19_lec2_supp)

### Further Reading

*   [What Every Computer Scientist Should Know About Floating Point Arithmetic](http://citeseerx.ist.psu.edu/viewdoc/summary?doi=10.1.1.22.6768) by David Goldberg.
*   [![This resource may not render correctly in a screen reader.](/images/inacessible.gif)How Java’s Floating-Point Hurts Everyone Everywhere (PDF)](http://www.cs.berkeley.edu/~wkahan/JAVAhurt.pdf) by William Kahan and Joseph Darcy. This article contains a nice discussion of floating-point myths and misconceptions.
*   Read “Lecture 13” in the textbook _Numerical Linear Algebra_.

Julia Tutorial
--------------

We introduced [the Julia Programming Language](http://julialang.org/) that we will use this term.

*   [Julia & IJulia Cheat-Sheet (PDF)]({{< baseurl >}}/resources/julia-cheatsheet)
*   [Introduction to Julia (PDF)]({{< baseurl >}}/resources/julia-intro)
*   [Julia for Numerical Computation in MIT Courses](https://github.com/mitmath/julia-mit/blob/master/README.md)