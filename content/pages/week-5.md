---
content_type: page
title: Week 5
uid: a4d71ca2-18aa-0275-e844-5a9cbf900105
---

Lecture 12: Cache-Oblivious Algorithms and Spatial Locality
-----------------------------------------------------------

### Summary

Introduced the concept of optimal cache-oblivious algorithms. Discussed cache-oblivious matrix multiplication in theory and in practice (see handout and [Cache-Oblivious Algorithms](http://citeseerx.ist.psu.edu/viewdoc/summary?doi=10.1.1.34.7911) by Matteo Frigo et al below).

Discussion of spatial locality and cache lines, with examples of dot products and matrix additions (both of which are "level 1 BLAS" operations with no temporal locality); you'll do more on this in Problem set 3.

*   Lecture 12 handout: [Experiments with Cache-Oblivious Matrix Multiplication (PDF)]({{< baseurl >}}/resources/mit18_335js19_lec12)
*   Lecture 12 notebook: [Experiments with Memory Access and Matrices](http://nbviewer.jupyter.org/github/mitmath/18335/blob/master/notes/Memory-and-Matrices.ipynb)

### Further Reading

*   [Automatically Tuned Linear Algebra Software (ATLAS)](http://math-atlas.sourceforge.net/)
*   [Cache-Oblivious Algorithms](http://citeseerx.ist.psu.edu/viewdoc/summary?doi=10.1.1.34.7911) by Matteo Frigo, et al.
*   [![This resource may not render correctly in a screen reader.](/images/inacessible.gif)Register Allocation in Kernel Generators (PDF)](http://cscads.rice.edu/workshops/july2007/autotune-slides-07/Frigo.pdf) by Matteo Frigo
*   [Row- and column-major order](http://en.wikipedia.org/wiki/Row-major_order) on Wikipedia

Lecture 13: LU Factorization and Partial Pivoting
-------------------------------------------------

### Summary

Review of Gaussian elimination. Reviewed the fact that this gives an A=LU factorization, and that we then solve Ax=b by solving Ly=b (doing the same steps to b that we did to A during elimination to get y) and then solving Ux=y (back substitution). Emphasized that you should almost never compute A\-1 explicitly. It is just as cheap to keep L and U around, since triangular solves are essentially the same cost as a matrix-vector multiplication. Computing A\-1 is usually a mistake: you can't do anything with A\-1 that you couldn't do with L and U, and you are wasting both computations and accuracy in computing A\-1. A\-1 is useful in abstract manipulations, but whenever you see "x=A\-1b" you should interpret it for computational purposes as solving Ax=b by LU or some other method.

Introduced partial pivoting, and pointed out (omitting bookkeeping details) that this can be expressed as a PA=LU factorization where P is a permutation. Began to discuss backwards stability of LU, and mentioned example where U matrix grows exponentially fast with m to point out that the backwards stability result is practically useless here, and that the (indisputable) practicality of Gaussian elimination is more a result of the types of matrices that arise in practice.

### Further Reading

*   Read “Lectures 20–23” in the textbook _Numerical Linear Algebra_.

Lecture 14: Cholesky Factorization and other Specialized Solvers. Eigenproblems and Schur Factorizations
--------------------------------------------------------------------------------------------------------

### Summary

Briefly discussed Cholesky factorization, which is Gaussian elimination for the special case of Hermitian positive-definite matrices, where we can save a factor of two in time and memory. More generally, if the matrix A has a special form, one can sometimes take advantage of this to have a more efficient Ax=b solver, for example: Hermitian positive-definite (Cholesky), tridiagonal or banded (linear-time solvers), lower/upper triangular (forward/back substitution), sparse (mostly zero—sparse-direct and iterative solvers, to be discussed later; typically only worthwhile when the matrix is much bigger than 1000×1000).

New topic: eigenproblems. Reviewed the usual formulation of eigenproblems and the characteristic polynomial, mentioned extensions to generalized eigenproblems and SVDs. Discussed diagonalization, defective matrices, and the generalization of the Schur factorization.

Discussed diagonalization, defective matrices, and the generalization of the Schur factorization. Proved (by induction) that every (square) matrix has a Schur factorization, and that for Hermitian matrices the Schur form is real and diagonal.

### Further Reading

*   Read “Lectures 20–23” in the textbook _Numerical Linear Algebra_.
*   See all of the special cases of LAPACK's [Linear Equations](http://www.netlib.org/lapack/lug/node38.html).