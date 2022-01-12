---
content_type: page
title: Week 6
uid: 442a963d-d193-26e6-068e-1510d4c866d4
---

Lecture 15: Eigensolver Algorithms: Companion Matrices, Ill-Conditioning, and Hessenberg Factorization
------------------------------------------------------------------------------------------------------

### Summary

Pointed out that an "LU-like" algorithm for eigenproblems, which computes the exact eigenvalues/eigenvectors (in exact arithmetic, neglecting roundoff) in a finite number of steps involving addition, subtraction, multiplication, division, and roots, is impossible. The reason is that no such algorithm exists (or can ever exist) to find roots of polynomials with degree greater than 4, thanks to a theorem by Abel, Galois and others in the 19th century. Used the [companion matrix](http://en.wikipedia.org/wiki/Companion_matrix) to show that polynomial root finding is equivalent to the problem of finding eigenvalues. Mentioned the connection to other classic problems of antiquity (squaring the circle, trisecting an angle, doubling the cube), which were also proved impossible in the 19th century.

As a result, all eigenproblem methods must be iterative: they must consist of improving an initial guess, in successive steps, so that it converges towards the exact result to any desired accuracy, but never actually reaches the exact answer in general. A simple example of such a method is Newton's method, which can be applied to iteratively approximate a root of any nonlinear function to any desired accuracy, given a sufficiently good initial guess.

However, finding roots of the characteristic polynomial is generally a terrible way to find eigenvalues. Actually computing the characteristic polynomial coefficients and then finding the roots somehow (Newton's method?) is a disaster, incredibly ill-conditioned: gave the example of [Wilkinson's polynomial](http://en.wikipedia.org/wiki/Wilkinson%27s_polynomial). If we can compute the characteristic polynomial values implicitly somehow, directly from the determinant, then it is not too bad (if you are looking only for eigenvalues in some known interval, for example), but we haven't learned an efficient way to do that yet. The way LAPACK and MATLAB actually compute eigenvalues, the QR method and its descendants, wasn't discovered until 1960.

The key to making most of the eigensolver algorithms efficient is reducing A to Hessenberg form: A=QHQ\* where H is upper triangular plus one nonzero value below each diagonal. Unlike Schur form, Hessenberg factorization can be done exactly in a finite number \[Θ(m3)\] of steps (in exact arithmetic). H and A are similar: they have the same eigenvalues, and the eigenvector are related by Q. And once we reduce to Hessenberg form, all the subsequent operations we might want to do (determinants, LU or QR factorization, etcetera), will be fast. In the case of Hermitian A, showed that H is tridiagonal; in this case, most subsequent operations (even LU and QR factorization) will be Θ(m).

*   Lecture 15 handout: [Hessenberg Factorization (PDF)]({{< baseurl >}}/resources/mit18_335js19_lec15)

### Further Reading

*   Read “Lectures 24 and 25” in the textbook _Numerical Linear Algebra_.
*   See [Studying Wilkinson’s Polynomial in Julia](https://nbviewer.jupyter.org/github/mitmath/18335/blob/spring15/notes/Wilkinson-Polynomial.ipynb) for some experiments with polynomial roots.
*   [Eigenvalues: The Key Idea](https://nbviewer.jupyter.org/github/stevengj/1806/blob/fall18/lectures/Eigenvalue-Polynomials.ipynb)

Lecture 16: The Power Method and the QR Algorithm
-------------------------------------------------

### Summary

Reviewed power method for biggest-|λ| eigenvector/eigenvalue and its convergence rate.

Discussed how to use the power method to get multiple eigenvalues/vectors of Hermitian matrices by "deflation" (using orthogonality of eigenvectors). Discussed how, in principle, QR factorization of _An_ for large _n_ will give the eigenvectors and eigenvalues in descending order of magnitude, but how this is killed by roundoff errors.

Unshifted QR method: proved that repeatedly forming A=QR, then replacing A with RQ (as in Problem set 3) is equivalent to QR factorizing _An_. But since we do this while only multiplying repeatedly by unitary matrices, it is well conditioned and we get the eigenvalues accurately.

To make the QR method faster, we first reduce to Hessenberg form; you will show in Problem set 3 that this is especially fast when A is Hermitian and the Hessenberg form is tridiagonal. Second, we use shifts. In particular, the worst case for the QR method, just as for the power method, is when eigenvalues are nearly equal. We can fix this by shifting.

Discussed inverse iteration and shifted-inverse iteration. Discussed Rayleigh-quotient iteration (shifted-inverse iteration with the Rayleigh-quotient eigenvalue estimate as the shift) and its convergence rate in the Hermitian case. How, for Hermitian matrix the eigenvalue estimate has a much smaller error than the eigenvector (the error is squared) due to the fact that the eigenvalue is an extremum.

### Further Reading

*   Read “Lectures 27–30” in the textbook _Numerical Linear Algebra_.
*   [The QR Algorithm I (PDF)]({{< baseurl >}}/resources/mit18_335js19_lec16_reading) (Courtesy of Per-Olof Persson. Used with permission.)
*   [The QR Algorithm II (PDF)]({{< baseurl >}}/resources/mit18_335js19_lec16_reading2) (Courtesy of Per-Olof Persson. Used with permission.)

Lecture 17: Shifted QR and Rayleigh Quotients
---------------------------------------------

### Summary

Brief discussion of shifted QR method.

There are a number of additional tricks to further improve things, the most important of which is probably the Wilkinson shift: estimating μ from a little 2×2 problem from the last two columns to avoid problems in cases e.g. where there are two equal and opposite eigenvalues. Some of these tricks (e.g. the Wilkinson shift) are described in the book, and some are only in specialized publications. If you want the eigenvectors as well as eigenvalues, it turns out to be more efficient to use a more recent "divide and conquer" algorithm, summarized in the book, but where the details are especially tricky and important. However, at this point I don't want to cover more gory details in this course. Although it is good to know the general structure of modern algorithms, especially the fact that they look nothing like the characteristic-polynomial algorithm you learn as an undergraduate, as a practical matter you are always just going to call LAPACK if the problem is small enough to solve directly. Matters are different for much larger problems, where the algorithms are not so bulletproof and one might need to get into the guts of the algorithms; this will lead us into the next topic of the course, iterative algorithms for large systems, in subsequent lectures.

Briefly discussed Golub-Kahn bidiagonalization method for SVD, just to get the general flavor. At this point, however, we are mostly through with details of dense linear-algebra techniques: the important thing is to grasp the fundamental ideas rather than zillions of little details, since in practice you're just going to use LAPACK anyway.

Start discussing (at a very general level) a new topic: iterative algorithms, usually for sparse matrices, and in general for matrices where you have a fast way to compute _Ax_ matrix-vector products but cannot (practically) mess around with the specific entries of _A_.

Emphasized that there are many iterative methods, and that there is no clear "winner" or single bulletproof library that you can use without much thought (unlike LAPACK for dense direct solvers). It is problem-dependent and often requires some trial and error. Then there is the whole topic of preconditioning, which we will discuss more later, which is even more problem-dependent. Briefly listed several common techniques for linear systems (_Ax_\=_b_) and eigenproblems (_Ax_\=_λx_ or _Ax_\=_λBx_).

### Further Reading

*   Read “Lectures 27–30” in the textbook _Numerical Linear Algebra_.
*   [The QR Algorithm I (PDF)]({{< baseurl >}}/resources/mit18_335js19_lec16_reading) (Courtesy of Per-Olof Persson. Used with permission.)
*   [The QR Algorithm II (PDF)]({{< baseurl >}}/resources/mit18_335js19_lec16_reading2) (Courtesy of Per-Olof Persson. Used with permission.)