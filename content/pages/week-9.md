---
content_type: page
title: Week 9
uid: 79b67382-5292-f6a0-b115-ef7aad0acfff
---

Lecture 24: Sparse-Direct Solvers
---------------------------------

### Summary

**Sparse-direct solvers:** For many problems, there is an intermediate between the dense Θ(m3) solvers of LAPACK and iterative algorithms: for a sparse matrix A, we can sometimes perform an LU or Cholesky factorization while maintaining sparsity, storing and computing only nonzero entries for vast savings in storage and work. In particular, did a MATLAB demo, a few experiments with a simple test case: the "discrete Laplacian" center-difference matrix on uniform grids that we've played with previously in this course. In 1d, this matrix is tridiagonal and LU/Cholesky factorization produces a bidiagonal matrix: Θ(m) storage and work! For a 2d grid, there are 4 off-diagonal elements, and showed how elimination introduces Θ(√m) nonzero entries in each column, or Θ(m3/2) nonzero entries in total. This is still not too bad, but we can do better. First, showed that this "fill-in" of the sparsity depends strongly on the ordering of the degrees of freedom: as an experiment, tried a _random_ reordering, and found that we obtain Θ(m2) nonzero entries (~10% nonzero). Alternatively, one can find re-orderings that greatly reduce the fill-in. One key observation is that the fill-in only depends on the pattern of the matrix, which can be interpreted as a [graph](http://en.wikipedia.org/wiki/Graph_%28mathematics%29): m vertices, and edges for the nonzero entries of A (an [adjacency matrix](http://en.wikipedia.org/wiki/Adjacency_matrix) of the graph), and sparse-direct algorithms are closely related to graph-theory problems. For our simple 2d Laplacian, the graph is just the picture of the grid connecting the points. One simple algorithm is the [nested dissection](https://en.wikipedia.org/wiki/Nested_dissection) algorithm: recursively find a separator of the graph, then re-order the vertices to put the separator last. This reorders the matrix into a mostly block-diagonal form, with large blocks of zeros that are preserved by elimination, and if we do this recursively we greatly reduce the fill-in. Did a crude analysis of the fill-in structure, resulting in the time/space complexity on the last page of the handout, for our 2d grid where separators are obvious; for more general matrices finding separators is a hard and important problem in graph theory.

*   Lecture 24 handout: [Sparse Matrix Algorithms (PDF)]({{< baseurl >}}/resources/mit18_335js19_lec24) (Courtesy of Per-Olof Persson. Used with permission.)
*   Lecture 24 notebook: [Sparse-Direct Solvers in Julia](http://nbviewer.jupyter.org/github/mitmath/18335/blob/master/notes/Nested-Dissection.ipynb)

### Further Reading

*   [Nested Dissection: A Survey and Comparison of Various Nested Dissection Algorithms](http://citeseerx.ist.psu.edu/viewdoc/summary?doi=10.1.1.58.9722) by Manpreet S. Khaira.
*   [Direct Solvers for Sparse Matrices](http://www.cs.utk.edu/~dongarra/etemplates/node388.html) by Jack Dongarra.
*   Davis, Timothy. _Direct Methods for Sparse Linear Systems_. SIAM: Society for Industrial and Applied Mathematics, 2006. ISBN: 9780898716139.

Take-Home Midterm Exam
----------------------

The exam is open notes and open book (including any material posted for the class: pset solutions and handouts). No other materials may be used (closed Internet).

It will cover everything in this course up to and including Lecture 20 and Pset 4.

| Midterm Exams | Solutions |
| --- | --- |
| ![This resource may not render correctly in a screen reader.](/images/inacessible.gif)[Spring 2019 Exam (PDF)]({{< baseurl >}}/resources/mit18_335js19_exam19) | ![This resource may not render correctly in a screen reader.](/images/inacessible.gif)[Spring 2019 Solutions (PDF)]({{< baseurl >}}/resources/mit18_335js19_exam19sol) |
| {{< td-colspan 2 >}}Midterm Exams and Solutions from Previous Years{{< /td-colspan >}} ||
| [Fall 2008 Exam (PDF)]({{< baseurl >}}/resources/mit18_335js19_exam08) | [Fall 2008 Solutions (PDF)]({{< baseurl >}}/resources/mit18_335js19_exam08sol) |
| [Fall 2009 Exam (PDF)]({{< baseurl >}}/resources/mit18_335js19_exam09) | \[No Solutions\] |
| [Fall 2010 Exam (PDF)]({{< baseurl >}}/resources/mit18_335js19_exam10) | [Fall 2010 Solutions (PDF)]({{< baseurl >}}/resources/mit18_335js19_exam10sol) |
| [Fall 2011 Exam (PDF)]({{< baseurl >}}/resources/mit18_335js19_exam11) | [Fall 2011 Solutions (PDF)]({{< baseurl >}}/resources/mit18_335js19_exam11sol) |
| [Fall 2012 Exam (PDF)]({{< baseurl >}}/resources/mit18_335js19_exam12) | [Fall 2012 Solutions (PDF)]({{< baseurl >}}/resources/mit18_335js19_exam12sol) |
| [Fall 2013 Exam (PDF)]({{< baseurl >}}/resources/mit18_335js19_exam13) | ![This resource may not render correctly in a screen reader.](/images/inacessible.gif)[Fall 2013 Solutions (PDF)]({{< baseurl >}}/resources/mit18_335js19_exam13sol) |
| [Spring 2015 Exam (PDF)]({{< baseurl >}}/resources/mit18_335js19_exam15) | [Spring 2015 Solutions (PDF)]({{< baseurl >}}/resources/mit18_335js19_exam15sol) 

Lecture 25: Overview of Optimization Algorithms
-----------------------------------------------

### Summary

Several of the iterative algorithms so far have worked, conceptually at least, by turning the original linear-algebra problem into a minimization problem. It is natural to ask, then, whether we can use similar ideas to solve more general optimization problems, which will be the next major topic in this course.

Broad overview of optimization problems (see handout). The most general formulation is actually quite difficult to solve, so most algorithms (especially the most efficient algorithms) solve various special cases, and it is important to know what the key factors are that distinguish a particular problem. There is also something of an art to the problem formulation itself, e.g. a nondifferentiable minimax problem can be reformulated as a nicer differentiable problem with differentiable constraints.

CG easily generalizes to the [nonlinear conjugate gradient method](https://en.wikipedia.org/wiki/Nonlinear_conjugate_gradient_method) to (locally) minimize an _arbitrary_ twice-differentiable f(x): the only changes are that r=-∇f is not simply b-Ax and that the successive line minimizations min f(x+αd) need to be done numerically (an “easy” 1d optimization problem). The key point being that, near a local minimum of a smooth function, the objective is typically roughly quadratic (via Taylor expansion), and when that happens CG greatly accelerates convergence. (Mentioned Polak–Ribiere heuristic to help "reset" the search direction to the gradient if we are far from the minimum and convergence has stalled; see the Hager survey below for many more.)

Outlined application of nonlinear CG to Hermitian eigenproblems by minimizing the Rayleigh quotient (this is convex, and furthermore we can use the Ritz vectors to shortcut both the conjugacy and the line minimization steps). The generalization of this is the [LOBPCG](http://en.wikipedia.org/wiki/LOBPCG) algorithm.

*   Lecture 25 handout: [A Brief Overview of Optimization Problems (PDF)]({{< baseurl >}}/resources/mit18_335js19_lec25)

### Further Reading

*   Bertsekas, Dimitri P. _Nonlinear Programming_. Athena Scientific, 2016. ISBN: 9781886529052.
*   Online book [_Convex Optimization_](http://web.stanford.edu/~boyd/cvxbook/) by Stephen Boyd and Lieven Vandenberghe.
*   Conn, Andrew R. et al. _Introduction to Derivative-Free Optimization_. SIAM: Society for Industrial and Applied Mathematics, 2009. ISBN: 9780898716689.
*   A useful review of topology-optimization methods can be found in [Topology Optimization Approaches](https://link.springer.com/article/10.1007/s00158-013-0978-6) by Ole Sigmund and Kurt Maute.
*   There are many variants of nonlinear conjugate-gradient, mainly to avoid bad behavior far from the minimum, as surveyed by William Hager and Hongchao Zhang, ![This resource may not render correctly in a screen reader.](/images/inacessible.gif)[A Survey of Nonlinear Conjugate Gradient Methods (PDF)](http://people.cs.vt.edu/~asandu/Public/Qual2011/Optim/Hager_2006_CG-survey.pdf). _Pacific J. Optim._ 2, pp. 35-58 (2006).

Lecture 26: Adjoint Methods
---------------------------

### Summary

Introduction to adjoint methods and the remarkable fact that one can compute the gradient of a complicated function with about the same number of additional operations as computing the function once. The adjoint method is a numerical method for efficiently computing the gradient of a function or operator in a numerical optimization problem. 

*   Lecture 26 handout: [Adjoint Methods (PDF)]({{< baseurl >}}/resources/mit18_335js19_lec26)

### Further Reading

*   [Backpropagation](https://en.wikipedia.org/wiki/Backpropagation) on Wikipedia
*   [Automatic Differentiation (AD)](https://en.wikipedia.org/wiki/Automatic_differentiation) on Wikipedia