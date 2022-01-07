---
content_type: page
title: Week 2
uid: e81728f3-82ce-c39a-1a67-9d027dc38ee9
---

Lecture 3: Floating-Point Summation and Backwards Stability
-----------------------------------------------------------

### Summary

Analyzed the accumulated floating-point roundoff errors (see handouts below), explaining the results that we observed experimentally in the Julia notebook of the previous lecture. In those experiments, all of the inputs were non-negative, so that the "condition number" factor in the derivation equalled 1. In general, though, if you have _cancellations_ between summands of opposite signs, the same analysis shows that the relative error of the output can be arbitrarily large.

**Stability:** Gave the obvious definition of accuracy, what we might call "forwards stability" = almost the right answer for the right input. Showed that this is often too strong; e.g. adding a sequence of numbers is not forwards stable. (Basically because the denominator in the relative forwards error, which is the exact sum, can be made arbitrarily small via cancellations.)

Define asymptotic notation O(ε): _f_(ε) is O(_g_(ε)) if there exist some constants C, ε0 > 0 such that |_f_(ε)| < C|_g_(ε)| for all |ε|<ε0. That is, _g_(ε) is an asymptotic _upper bound_ for _f_(ε) as ε goes to zero, ignoring constant factors C. (A similar notation is used in computational complexity theory, but in the limit of large arguments _n_.) In the definitions of stability, we technically require [uniform convergence](https://en.wikipedia.org/wiki/Uniform_convergence): we must have O(ε) errors with the same constants C and ε0 independent of the inputs _x_. (The constants can depend on the dimension of _x_, however.)

More generally, we apply a weaker condition: "stability" = almost the right answer for almost the right input.

Often, it is sufficient to prove "backwards stability" = right answer for almost the right input. Showed that, in our example of adding a sequence of numbers, backwards stability seems to work where forwards stability failed. Then more rigorously proved that floating-point summation of _n_ numbers is backwards stable.

*   Lecture 3 handout 1: ![This resource may not render correctly in a screen reader.](/images/inacessible.gif)[Notes on the Accuracy of Naive Summation (PDF)]({{< baseurl >}}/resources/mit18_335js19_lec3-1)
*   Lecture 3 handout 2: ![This resource may not render correctly in a screen reader.](/images/inacessible.gif)[Backwards Stability of Recursive Summation (PDF)]({{< baseurl >}}/resources/mit18_335js19_lec3-2)

### Further Reading

*   [What Every Computer Scientist Should Know About Floating Point Arithmetic](http://citeseerx.ist.psu.edu/viewdoc/summary?doi=10.1.1.22.6768) by David Goldberg.
*   [How Java’s Floating-Point Hurts Everyone Everywhere (PDF)](http://www.cs.berkeley.edu/~wkahan/JAVAhurt.pdf) by William Kahan and Joseph Darcy. This article contains a nice discussion of floating-point myths and misconceptions.
*   Read “Lectures 3 and 13–15” in the textbook _Numerical Linear Algebra_.
*   Wikipedia article on "[Big O Notation](https://en.wikipedia.org/wiki/Big_O_notation)"; note that for expressions like O(ε) we are looking in the limit of small arguments rather than of large arguments (as in complexity theory), but otherwise the ideas are the same.

Lecture 4: Norms on Vector Spaces
---------------------------------

### Summary

When quantifying errors, a central concept is a norm, and we saw in our proof of backwards stability of summation that the choice of norm seems important. Defined norms (as in Lecture 3 of Trefethen): for a vector space _V_, a norm takes any _v_ ∈ _V_ and gives you a real number ‖_v_‖ satisfying three properties:

*   Positive definite: ‖_v_‖ ≥ 0, and = 0 if and only if _v_ = 0
*   Scaling: ‖α_v_‖ = |α|⋅‖_v_‖ for any scalar α.
*   [Triangle inequality](https://en.wikipedia.org/wiki/Triangle_inequality): ‖_v_+_w_‖ ≤ ‖_v_‖ + ‖_w_‖

There are many norms, for many different vector spaces. Gave examples of norms of column vectors: L_p_ norms (usually _p_ \= 1, 2, or \\(\\infty\\)) and weighted L2 norms. A complete (= continuous, essentially) normed vector space is called a [Banach space](https://en.wikipedia.org/wiki/Banach_space), and these error concepts generalize to _f_(_x_) when _f_ and _x_ are in any Banach spaces (including scalars, column vectors, matrices, …though infinite-dimensional Banach spaces are trickier).

Especially important in numerical analysis are functions where the inputs and/or outputs are matrices, and for these cases we need matrix norms. The most important matrix norms are those that are related to matrix operations. Started with the Frobenius norm. Related the Frobenius norm ‖_A_‖_F_ to the square root of the sum of eigenvalues of _A_\*_A_, which are called the _singular values_ σ2; we will do much more on singular values later, but for now noted that they equal the squared eigenvalues of _A_ if _A_\*=_A_ (Hermitian). Also defined the induced matrix norm, and bounded it below by the maximum eigenvalue magnitude of _A_ (if _A_ is square). For the L2 induced norm, related it (without proof for now) to the maximum singular value. A useful property of the induced norm is ‖_AB_‖ ≤ ‖_A_‖⋅‖_B_‖. Multiplication by a unitary matrix _Q_ (_Q_\* = _Q_\-1) preserves both the Frobenius and L2 induced norms.

**Equivalence of norms:** Described fact that any two norms are equivalent up to a constant bound, and showed that this means that stability in one norm implies stability in all norms. Sketched proof (_only skimmed this_): (i) show we can reduce the problem to proving any norm is equivalent to e.g. L1 on (ii) the unit circle; (iii) show any norm is continuous; and (ii) use a result from real analysis: a continuous function on a closed/bounded (compact) set achieves its maximum and minimum, the [extreme value theorem](http://en.wikipedia.org/wiki/Extreme_value_theorem). See handout below.

*   Lecture 4 handout: [Notes on the Equivalence of Norms (PDF)]({{< baseurl >}}/resources/mit18_335js19_lec4)

### Further Reading

*   Read “Lecture 3” in the textbook _Numerical Linear Algebra_.
*   If you don't immediately recognize that A\*A has nonnegative real eigenvalues (it is positive semidefinite), now is a good time to review your linear algebra; see also “Lecture 24” in the textbook _Numerical Linear Algebra_.

Lecture 5: Condition Numbers
----------------------------

### Summary

Related backwards error to forwards error, and backwards stability to forwards error (or "accuracy" as the book calls it). Show that, in the limit of high precision, the forwards error can be bounded by the backwards error multiplied by a quantity κ, the relative condition number. The nice thing about κ is that it involves only exact linear algebra and calculus, and is completely separate from considerations of floating-point roundoff. Showed that, for differentiable functions, κ can be written in terms of the induced norm of the Jacobian matrix.

Calculated condition number for square root, summation, and matrix-vector multiplication, as well as solving _Ax_ \= _b_, similar to the book. Defined the condition number of a matrix: for _f_(_x_) = _Ax_, the condition number is ‖_A_‖⋅‖_x_‖/‖_Ax_‖, which is bounded above by κ(_A_) = ‖_A_‖⋅‖_A_\-1‖.

Related matrix L2 norm to eigenvalues of _B_ \= _A_\*_A_. _B_ is obviously Hermitian (_B_\* = _B_), and with a little more work showed that it is positive semidefinite: _x_\*_Bx_ ≥ 0 for any _x_. Reviewed and re-derived properties of eigenvalues and eigenvectors of Hermitian and positive-semidefinite matrices. Hermitian means that the eigenvalues are real, the eigenvectors are orthogonal (or can be chosen orthogonal). Also, a Hermitian matrix must be diagonalizable (I skipped the proof for this; we will prove it later in a more general setting). Positive semidefinite means that the eigenvalues are nonnegative.

Proved that, for a Hermitian matrix _B_, the Rayleigh quotient R(_x_)=_x_\*_Bx_/_x_\*_x_ is bounded above and below by the largest and smallest eigenvalues of _B_ (the "min–max theorem"). Hence showed that the L2 induced norm of _A_ is the square root of the largest eigenvalue of _B_ \= _A_\*_A_. Similarly, showed that the L2 induced norm of _A_\-1, or more generally the supremum of ‖_x_‖/‖_Ax_‖, is equal to the square root of the inverse of the smallest eigenvalue of _A_\*_A_.

Understanding norms and condition numbers of matrices therefore reduces to understanding the eigenvalues of _A_\*_A_ (or _AA_\*). However, looking at it this way is unsatisfactory for several reasons. First, we would like to solve one eigenproblem, not two. Second, working with things like _A_\*_A_ explicitly is often bad numerically, because it squares the condition number \[showed that κ(_A_\*_A_) = κ(_A_)2\] and hence exacerbates roundoff errors. Third, we would really like to get some better understanding of _A_ itself. All of these concerns are addressed by the singular value decomposition or SVD, which we will derive next time.

### Assignment

*   [Problem set 2 (PDF)]({{< baseurl >}}/resources/mit18_335js19_pset2)
*   [Problem set 2 solutions (PDF)]({{< baseurl >}}/resources/mit18_335js19_pset2sol)

### Further Reading

*   Read “Lectures 12, 14, 15, and 24” in the textbook _Numerical Linear Algebra_.
*   Any linear-algebra textbook for a review of eigenvalue problems, especially Hermitian/real-symmetric ones.
*   [Errors, Norms, and Condition Numbers](https://nbviewer.jupyter.org/github/stevengj/1806/blob/fall18/lectures/Conditioning.ipynb)