# A proof of Colin Barker's recurrence conjecture for OEIS A192346

**Lean 4 / Mathlib formalization** | [OEIS A192346](https://oeis.org/A192346) | [Lean source](A192346.lean)

This repository presents a proof of the fourth-order recurrence conjectured by **Colin Barker (2013)** for the integer sequence **A192346**, introduced by **Clark Kimberling (2011)**. The proof uses the quadratic relation defining the polynomial reduction, a two-coordinate linear recurrence, and its characteristic polynomial.

The OEIS edit proposing this proof was submitted for editorial review on **November 23, 2026**. Editorial acceptance and historical priority are separate questions from the mathematical correctness of the argument.

## Statement

Let

\[
p_n(X)=\frac{(X+\sqrt{X+2})^n+(X-\sqrt{X+2})^n}{2}.
\]

Reduce each polynomial using \(X^2=X+1\), and let \(a(n+1)\) be the constant coefficient of the resulting degree-at-most-one polynomial. The OEIS offset is 1, with initial terms

\[
a(1),a(2),a(3),a(4)=1,0,3,4.
\]

The conjectured identity is

\[
\boxed{a(n)=2a(n-1)+6a(n-2)-2a(n-3)-a(n-4)\quad(n\geq5).}
\]

## Mathematical proof

Set \(r_\pm=X\pm\sqrt{X+2}\). Their sum is \(2X\), and their product is \(X^2-X-2\). Consequently, the polynomials satisfy

\[
p_{n+2}=2Xp_{n+1}+(X+2-X^2)p_n.
\]

In the quadratic quotient \(\mathbb Z[X]/(X^2-X-1)\), the relation \(X^2=X+1\) reduces this to

\[
p_{n+2}=2Xp_{n+1}+p_n.
\]

Write the unique degree-at-most-one representative as \(u_n+v_nX\), and let \(w_n=(u_n,v_n)^T\). Multiplication by \(X\) sends \((u,v)\) to \((v,u+v)\), so

\[
w_{n+2}=2Mw_{n+1}+w_n,\qquad
M=\begin{pmatrix}0&1\\1&1\end{pmatrix}.
\]

The associated characteristic polynomial is

\[
\det(t^2I-2tM-I)=t^4-2t^3-6t^2+2t+1.
\]

Applying this polynomial to the vector recurrence gives

\[
u_{n+4}=2u_{n+3}+6u_{n+2}-2u_{n+1}-u_n.
\]

Since \(a(n+1)=u_n\), this is Barker's recurrence.

## Formal verification

The file [`A192346.lean`](A192346.lean) contains the following Lean theorems:

- `coeff_recurrence`: fourth-order recurrence for the coefficient pair.
- `oeisPoly_eval_bridge`: evaluation of the recursively defined integer polynomial at an element satisfying \(X^2=X+1\).
- `oeisPoly_radical_bridge`: identity between twice the polynomial evaluation and the two radical powers.
- `oeisA192346_recurrence`: the OEIS-indexed recurrence.

These theorems were reported to compile successfully in Google Colab with Lean 4 / Mathlib on November 23, 2026. The axiom report contained only `propext`, `Classical.choice`, and `Quot.sound` (as applicable). The source does **not** include an explicit theorem using `Polynomial.modByMonic` to identify the canonical polynomial remainder. That last identification is supplied in the mathematical argument above, not as a separate Lean theorem.

To check the source in a Mathlib project:

```bash
lake env lean A192346.lean
```

For reproducibility, record the Mathlib commit and Lean version used in the successful Colab run; they are not pinned in this repository at present.

## Attribution and scope

- **Sequence:** Clark Kimberling, [OEIS A192346](https://oeis.org/A192346), 2011.
- **Recurrence conjecture:** Colin Barker, OEIS formula comment, January 17, 2013.
- **Proof and Lean formalization submitted to OEIS:** Shreyas Samir Kolte, November 23, 2026, with AI assistance.

The argument is an application of standard finite-dimensional linear-recurrence and quadratic-algebra techniques. It does **not** claim a new general theorem about quadratic algebras, nor does it claim that no prior unpublished or differently formulated proof exists.

## References

1. [OEIS A192346](https://oeis.org/A192346) and its [revision history](https://oeis.org/history?seq=A192346).
2. [A192346 Lean source](A192346.lean).
