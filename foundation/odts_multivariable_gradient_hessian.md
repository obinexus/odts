# ODTS — Multivariable Worked Example: Gradient & Hessian

**Function:**

\[ f(x,y)=x^3+y^3-3xy.\]

This document computes the gradient and Hessian symbolically, finds critical points, classifies them with the Hessian, and includes numeric sanity checks and interpretation suitable for insertion in the ODTS paper or appendix.

---

## 1. Partial derivatives and gradient

\[ \frac{\partial f}{\partial x}=3x^2-3y,\qquad \frac{\partial f}{\partial y}=3y^2-3x.\]

\[ \nabla f(x,y)=\begin{pmatrix}3x^2-3y\\[6pt]3y^2-3x\end{pmatrix}.\]

---

## 2. Hessian (second partial derivatives)

\[ f_{xx}=6x,\qquad f_{yy}=6y,\qquad f_{xy}=f_{yx}=-3.\]

\[ H_f(x,y)=\begin{pmatrix}6x & -3\\[6pt]-3 & 6y\end{pmatrix}.\]

---

## 3. Critical points (solve \(\nabla f=0\))
Solve

\[3x^2-3y=0,\qquad 3y^2-3x=0\]

gives \(y=x^2\) and \(x=y^2\) leading to real critical points \((0,0)\) and \((1,1)\).

---

## 4. Classification via Hessian
- \(H_f(0,0)=\begin{pmatrix}0 & -3\\ -3 & 0\end{pmatrix}\): determinant -9 &nbsp;\(\Rightarrow\) indefinite \(\Rightarrow\) saddle at \((0,0)\).
- \(H_f(1,1)=\begin{pmatrix}6 & -3\\ -3 & 6\end{pmatrix}\): determinant 27, \(f_{xx}=6>0\) &nbsp;\(\Rightarrow\) positive definite \(\Rightarrow\) local minimum at \((1,1)\).

Function values: \(f(0,0)=0\), \(f(1,1)=-1\).

---

## 5. Numeric sanity checks (finite-difference)
Use \(h=10^{-6}\) to compute numerical partials and Hessian entries at chosen points and compare with symbolic values (tolerance \(\sim10^{-8}\) for polynomials in double precision).

---

## 6. ODTS trace metadata recommendation
For each step record: operation type ("partial", "second-partial", "solve"), symbolic result, numeric sample checks, domain, timestamp, and an optional "insight" string for the researcher's intuition.

---

If you want, I can now (1) add a SymPy/Python snippet that computes and verifies all of the above automatically, or (2) merge this section back into the main ODTS canvas document for you. Which do you prefer? I’ll do it without complaining (much).

