---
content_type: page
title: Week 8
uid: b1ca5a43-353d-d5e4-5a41-cddf22a430cf
---

Lecture 21: Preconditioning Techniques. The Conjugate-Gradient Method
---------------------------------------------------------------------

### Summary

Continued to discuss preconditioning: finding an M such that MA (left preconditioning) or AM (right preconditioning) has clustered eigenvalues (solving MAx=Mb or AMy=b with x=My, respectively). Essentially, one can think of M as a crude approximation for A–1, or rather the inverse of a crude approximation of A that is easy to invert. Brief summary of some preconditioning ideas: multigrid, incomplete LU/Cholesky, Jacobi/block-Jacobi. (Since Jacobi preconditioners only have short-range interactions, they tend not to work well for matrices that come from finite-difference/finite-element discretizations on grids, but they can work well for diagonally dominant matrices that arise in spectral and integral-equation methods.)

**Conjugate-gradient methods:** Began discussing gradient-based iterative solvers for Ax=b linear systems, starting with the case where A is Hermitian positive-definite. Our goal is the conjugate-gradient method, but we start with a simpler technique. First, we cast this as a minimization problem for f(x)=x\*Ax-x\*b-b\*x. Then, we perform 1d line minimizations of f(x+αd) for some direction d. If we choose the directions d to be the steepest-descent directions b-Ax, this gives the steepest-descent method. Discussed successive line minimization of f(x), and in particular the steepest-descent choice of d=b-Ax at each step. Explained how this leads to "zig-zag" convergence by a simple two-dimensional example, and in fact the number of steps is proportional to κ(A). We want to improve this by deriving a Krylov-subspace method that minimizes f(x) over all previous search directions simultaneously.

Derived the conjugate-gradient method, by explicitly requiring that the nth step minimize over the whole Krylov subspace (the span of the previous search directions). This gives rise to an orthogonality ("conjugacy", orthogonality through A) condition on the search directions, and can be enforced with Gram-Schmidt on the gradient directions. Then we will show that a Lanczos-like miracle occurs: most of the Gram-Schmidt inner products are zero, and we only need to keep the previous search direction.

*   Lecture 21 notebook: [Large-Scale Linear Algebra: Dense Matrix Methods](https://nbviewer.jupyter.org/github/mitmath/18335/blob/master/notes/Dense-and-Sparse.ipynb)

### Further Reading

*   Chapter on [Preconditioners](http://www.netlib.org/linalg/html_templates/node51.html) in [_Templates for the Solution of Linear Systems: Building Blocks for Iterative Methods_](http://www.netlib.org/linalg/html_templates/Templates.html) by Richard Barrett et al.
*   Chen, Ke. _Matrix Preconditioning Techniques and Applications_. Cambridge University Press, 2005. ISBN: 9780521838283.
*   On MINRES and SYMMLQ: [Differences in the Effects of Rounding Errors in Krylov Solvers for Symmetric Indefinite Linear Systems](http://citeseerx.ist.psu.edu/viewdoc/summary?doi=10.1.1.31.3064) by Gerard L.G. Sleijpen et al.
*   Online book [_Templates for the Solution of Algebraic Eigenvalue Problems: A Practical Guide_](http://www.cs.utk.edu/~dongarra/etemplates/book.html) by Zhaojun Bai et al.
*   Read “Lecture 38” in the textbook _Numerical Linear Algebra_.
*   [![This resource may not render correctly in a screen reader.](/images/inacessible.gif)An Introduction to the Conjugate Gradient Method without the Agonizing Pain (PDF)](http://www.cs.cmu.edu/~quake-papers/painless-conjugate-gradient.pdf) by Jonathan Richard Shewchuk.

Lecture 22: Convergence of Conjugate Gradient
---------------------------------------------

### Summary

Finished derivation of conjugate gradient, by showing that it reduces to a three-term recurrence.

Discussed convergence of conjugate gradient: useless results like "exact" convergence in m steps (not including roundoff), pessimistic bounds saying that the number of steps goes like the square root of the condition number, and the possibility of superlinear convergence for clustered eigenvalues.

Lecture 23: Biconjugate Gradient Algorithms
-------------------------------------------

### Summary

Derived the preconditioned conjugate gradient method (showing how the apparent non-Hermitian-ness of MA is not actually a problem as long as M is Hermitian positive-definite). Mentioned the connection to approximate Newton methods (which is easy to see if we consider preconditioned steepest-descent with M approximately A\-1).

As an alternative to GMRES for non-Hermitian problems, considered the biCG algorithm. Derived it as in the van der Vorst notes below: as PCG on the Hermitian-indefinite matrix B=\[0,A;A\*,0\] with the "preconditioner" \[0,I;I,0\] (in the unpreconditioned case). Because this is Hermitian, there is still a conjugacy condition and it is still a Krylov method. Because it is indefinite, we are finding a saddle point and not a minimum of a quadratic, and _breakdown_ can occur if one of the denominators (e.g. d\*Bd) becomes zero. (This is the same as algorithm 39.1 in Trefethen, but derived very differently.) Because of this, you should almost never use the plain biCG algorithm. However, the biCG idea was the starting point for several "stabilized" refinements, culminating in biCGSTAB(L) that try to avoid breakdown by combining biCG with elements of GMRES. Another algorithm worth trying is the QMR algorithm.

Concluded with some rules of thumb about which type of solvers to use: LAPACK for small matrices (< 1000s×1000s), sparse-direct for intermediate-size sparse cases, and iterative methods for the largest problems or problems with a fast matrix⋅vector but no sparsity. One important point that we will discuss next time is that sparse-direct algorithms scale much better for sparse matrices that come from discretization of 2d PDEs than 3d PDEs. In general, some experimentation is required to find the best technique for a given problem, so software like MATLAB or the Petsc library is extremely helpful in providing a quick way to explore many algorithms.

### Further Reading

*   Online book [_Templates for the Solution of Linear Systems: Building Blocks for Iterative Methods_](http://www.netlib.org/linalg/html_templates/Templates.html) by Richard Barrett et al.
*   A very nice overview of iterative methods for non-Hermitian problems can be found in these [Lecture Notes on Iterative Methods](http://www.math.uu.nl/people/vorst/lecture.html) by Henk van der Vorst (second section, starting with GMRES).