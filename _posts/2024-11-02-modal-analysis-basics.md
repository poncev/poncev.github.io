---
layout: post
title: "Modal Analysis: the basics"
author: "Felipe Ponce-Vanegas"
categories: general
tags: [modal analysis]
---

Vibrating mechanical systems are modelled by the equation

$$
\begin{equation}
M\ddot{x} + C\dot{x} + Kx = F,\tag{1}\label{eq:basic_vibration}
\end{equation}
$$

where $x(t) \in \R^N$ is a vector function representing the displacement from equilibrium,
$F$ are external forces,
$M$ is the mass, $C$ is the damping, and $K$ is the stiffness.
The dimension $N$ is referred to as the number of Degrees of Freedom (DoF).

$M$ and $K$ are usually real, symmetric, positive-definite matrices.
This can be justified using the lagrangian formulation of a system of oscillators and
approximating the dynamics to first order, in which case
$M$ and $K$ are jacobian matrices;
however, see Section 2.8 in {% cite ewins00 %}.
$C$ is usually hard to model, and we will impose some conditions on it
for mathematical convenience, but not physically motivated.

### Resonances

A resonance is a function $e^{i\omega t}\phi$ that solves the homogenous system \eqref{eq:basic_vibration} with $C = 0$,
where $\omega > 0$ and $\phi \in \C^N$ is a constant vector,
which are called resonance frequency and mode shape, respectively.
The system \eqref{eq:basic_vibration} always has a full basis of $N$ resonances.

To find the resonances, notice that by the symmetry of $M$ there is an orthonormal matrix $U$ such that $U^t M U = \diag(m_1, \ldots, m_N)$.
If we rescale it as $\Psi := U\diag(1 / \sqrt{m_1}, \ldots, 1/\sqrt{m_N})$,
then $\Psi^t M \Psi = I$.
Now, let us write $y := \Psi^{-1} x$ and multiply \eqref{eq:basic_vibration} by $\Psi^t$ to get

$$
\begin{equation}
\ddot{y} + A\dot{y} + By = 0,
\end{equation}
$$

where $A := \Psi^t C \Psi$ and $B := \Psi^t K \Psi$.
Since we are assuming that $A = 0$ (undamped system), we have to find solutions of the system

$$
\begin{equation}
    (\omega^2 - B)y = 0,
\end{equation}
$$

but by the symmetry of $B$ we can find an orthonormal basis of real vectors $\\{\eta_k\\}_{k=1,\ldots, N}$ with eigenvalues $\\{\omega_k^2 \\}$.
In the original system of coordinates, the eigenvalues are $\phi_k := \Psi \eta_k$,
and they satisfy $\phi_l^t M \phi_k = \delta _{kl}$, where
$\delta _{kl}$ is the Kronecker delta;
it is said that the basis $\\{\phi_k\\}$ is mass-normalized and has units $1/\sqrt{\mathrm{kg}}$.

### Proportional damping

If the symmetric matrices $A$ and $B$ are mutually diagonalizable, that is,
if there is an orthonormal matrix $H = (\eta_1, \ldots, \eta_N)$ such that $H^tAH$ and $H^t BH$ are diagonal, then
writing $y = q_1\eta_1 + \cdots + q_N\eta_N$
we can further decouple the damped system into the $N$ independent equations

$$
\begin{equation}
    \ddot{q}_k + 2\omega_k\zeta_k \dot{q}_k + \omega_k^2q_k = 0,
\end{equation}
$$

where $\zeta_k$ is called the damping ratio.
The mode shapes in the original coordinate system are again $\phi_k := \Psi \eta_k$, which are real vectors.
In this case, it is said that the damping is proportional, and
it is a common and convenient assumption about the matrix $C$.

When is the damping proportional?
Two symmetric matrices are mutually diagonalizable if and only if
$[A, B] := AB - AB = 0$; see Theorem A in the Appendix.
In terms of $C$ and $K$, this is $C\Psi\Psi^t K = K\Psi\Psi^t C$.
Since $M^{-1} = \Psi \Psi^t$, we can re-write the last expression in the most common form

$$
\begin{equation}
    (M^{-1}C)(M^{-1}K) = (M^{-1}K)(M^{-1}C).
\end{equation}
$$

If the matrix $A$ commutes with the symmetric matrix $B$, and
all the eigenvalues of $B$ are distinct, then $A = \sum_{j=0}^n c_jB^j$ for
some $n$ and some coefficients $c_j$; see Theorem B in the Appendix.
Again, in terms of $C$ and $K$ we end up with the known formula

$$
\begin{equation}
    (M^{-1}C) = \sum_{j=0}^n c_j(M^{-1}K)^j.
\end{equation}
$$

A more detailed account of these results can be found in {% cite cuaghey65 %}.

### Non-proportional damping

In the general case of \eqref{eq:basic_vibration},
we can still look for solutions of the form $e^z\phi$,
where $z \in \C$ are complex frequencies and $\phi \in \C^N$ are mode shapes.
If we replace $e^z\phi$ in \eqref{eq:basic_vibration} we get

$$
\begin{equation}
    (z^2M + zC + K)\phi = 0.\tag{2}\label{eq:undamped_quad}
\end{equation}
$$

An equivalent problem is to find the eigenvalues and eigenvectors of

$$
\begin{equation}
    \Omega :=
    \begin{bmatrix}
        0 & I \\
        -M^{-1}K & -M^{-1}C
    \end{bmatrix}.
    \tag{3}\label{eq:def_Omega}
\end{equation}
$$

In fact, if $z$ and $\phi$ satisfy \eqref{eq:undamped_quad}, then $(\phi, z\phi)^t$ is an eigenvector of $\Omega$ with eigenvalue $z$.
It is not hard to see that all the eigenvectors have this form.

Since the system matrices are real, then for each eigenvector $(\phi, z\phi)^t$
the vector $(\bar{\phi}, \bar{z}\bar{\phi})^t$ is also an eigenvector.
Hence, for definiteness, we add to the complex frequencies $z$ the condition $\im(z) > 0$, because
the other eigenvector is obtained by conjugation;
under our hypotheses about the system (*i.e.* symmetric, positive-definite matrices), $\im(z) \neq 0$.
We will assume that the mode shapes $\\{\phi_k\\}_k$ form a basis of $\C^N$,
which is true for a generic system (*i.e.*, it is false for set of measure zero).

In general, if $A$ is a matrix and $U = (u_1, \ldots, u_N)$ are the eigenvectors,
then $AU = U\Lambda$, where $\Lambda := \diag(\lambda_1,\ldots,\lambda_N)$ are the eigenvalues.
Then, let us group the mode shapes and eigenvalues into the matrices $\Phi := (\phi_1, \ldots, \phi_N)$ and $Z := \diag(z_1, \ldots, z_N)$.
In this way,

$$
\begin{equation}
    \Omega
    \begin{bmatrix}
    \Phi & \bar{\Phi} \\
    \Phi Z & \bar{\Phi}\bar{Z}
    \end{bmatrix}
    =
    \begin{bmatrix}
    \Phi & \bar{\Phi} \\
    \Phi Z & \bar{\Phi}\bar{Z}
    \end{bmatrix}
    \begin{bmatrix}
        z & \\
         & \bar{z}
    \end{bmatrix},
\end{equation}
$$

or taking inverse at both sides

$$
\begin{equation}
    \begin{bmatrix}
    \Phi & \bar{\Phi} \\
    \Phi Z & \bar{\Phi}\bar{Z}
    \end{bmatrix}^{-1}
    \Omega
    \begin{bmatrix}
    \Phi & \bar{\Phi} \\
    \Phi Z & \bar{\Phi}\bar{Z}
    \end{bmatrix}
    =
    \begin{bmatrix}
        z & \\
         & \bar{z}
    \end{bmatrix},
\end{equation}
$$

so we have diagonalized $\Omega$.
Hence, it is interesting to see if we can find a more
transparent expression for the inverse appearing in the formula.

Considering again the general case $AU = U\Lambda$,
we can deduce that $U^{-1}A = \Lambda U^{-1}$, and then
transposing we get $A^tU^{-t} = U^{-t}\Lambda$.
This means that the columns of $U^{-t}$ are eigenvectors of $A^t$, so
if we solve the spectral problem for $A^t$ and find $V$ such that $A^tV = V\Lambda$,
we would conclude that $U^{-t} = Vd$, where $d$ is a diagonal matrix, and
we would get our desired expression for the inverse of $U$, that is, $U^{-1} = dV^t$.

If we apply the above arguments to $A = \Omega$,
then we must solve the spectral problem for

$$
\begin{equation}
    \Omega^t =
    \begin{bmatrix}
        0 & -KM^{-1} \\
        I & -CM^{-1}
    \end{bmatrix}.
\end{equation}
$$

It is not hard to see that the eigenvectors $v$ have the form

$$
\begin{equation}
    \begin{bmatrix}
        -K\phi \\ z M\phi
    \end{bmatrix},
    \qquad\text{where }
    \phi \text{ satisfies (2).}
\end{equation}
$$

Hence, in this case the matrix of eigenvectors is

$$
\begin{equation}
    V =
    \begin{bmatrix}
        -K & \\
         & M
    \end{bmatrix}
    \begin{bmatrix}
    \Phi & \bar{\Phi} \\
    \Phi Z & \bar{\Phi}\bar{Z}
    \end{bmatrix}
    \qquad\text{and}\qquad
    \begin{bmatrix}
    \Phi & \bar{\Phi} \\
    \Phi Z & \bar{\Phi}\bar{Z}
    \end{bmatrix}^{-1}
    =
    dV^t,
\end{equation}
$$

so we only have to determine the diagonal matrix $d$.
For that, notice that

$$
\begin{align}
    d^{-1} =
    \begin{bmatrix}
    \Phi & \bar{\Phi} \\
    \Phi Z & \bar{\Phi}\bar{Z}
    \end{bmatrix}^t
    V
    =
    \begin{bmatrix}
        -\Phi^t K\Phi + Z\Phi^tM\Phi Z & -\Phi^tK\bar{\Phi} + Z\Phi^tM\bar{\Phi}\bar{Z} \\
        -\bar{\Phi}^tK\Phi + \bar{Z}\bar{\Phi}^tM\Phi Z & -\bar{\Phi}^t K\bar{\Phi} + \bar{Z}\bar{\Phi}^tM\bar{\Phi} \bar{Z}
    \end{bmatrix}.
\end{align}
$$

Incidentally, this expression also provides us with important orthogonality relations
for the mode shapes.
From $-\bar{\Phi}^tK\Phi + \bar{Z}\bar{\Phi}^tM\Phi Z = 0$ we get

$$
\begin{equation}
    \bar{z}_iz_j = \frac{\bar{\phi}_i^tK\phi_j}{\bar{\phi}_i^tM\phi_j},
    \qquad\text{for every } i, j = 1,\ldots, N.
\end{equation}
$$

### Real Model Shapes

We saw above that for undamped systems ($C = 0$),
it is possible to choose the mode shapes $\phi \in \R^N$.
Moreover, we can show that *if all the mode shapes are real, then $C = 0$,*
so when $C \neq 0$ at least one mode shape must be complex.

To see that real mode shapes imply $C = 0$,
let us group the mode shapes and eigenvalues into the matrices $\Phi := (\phi_1, \ldots, \phi_N)$ and $Z := \diag(z_1, \ldots, z_N)$.
The columns of the matrix

$$
\begin{equation}
    \begin{bmatrix}
    \Phi & \Phi \\
    \Phi Z & \Phi\bar{Z}
    \end{bmatrix}
\end{equation}
$$

are the eigenvectors of $\Omega$, so

$$
\begin{equation}
    \Omega
    \begin{bmatrix}
    \Phi & \Phi \\
    \Phi Z & \Phi\bar{Z}
    \end{bmatrix}
    =
    \begin{bmatrix}
    \Phi & \Phi \\
    \Phi Z & \Phi\bar{Z}
    \end{bmatrix}
    \begin{bmatrix}
        Z & \\
         & \bar{Z}
    \end{bmatrix}.
\end{equation}
$$

Since

$$
\begin{equation}
    \begin{bmatrix}
    \Phi & \Phi \\
    \Phi Z & \Phi\bar{Z}
    \end{bmatrix}^{-1}
    =
    \frac{i}{2}
    \begin{bmatrix}
        \im(Z)^{-1} & \\
         & \im(Z)^{-1}
    \end{bmatrix}
    \begin{bmatrix}
    \bar{Z} \Phi^{-1} & -\Phi^{-1} \\
    -Z \Phi^{-1} & \Phi^{-1}
    \end{bmatrix},
\end{equation}
$$

then

$$
\begin{align}
    \Omega &=
    \frac{i}{2}
    \begin{bmatrix}
    \Phi & \Phi \\
    \Phi Z & \Phi\bar{Z}
    \end{bmatrix}
    \begin{bmatrix}
        Z & \\
         & \bar{Z}
    \end{bmatrix}
    \begin{bmatrix}
        \im(Z)^{-1} & \\
         & \im(Z)^{-1}
    \end{bmatrix}
    \begin{bmatrix}
    \bar{Z} \Phi^{-1} & -\Phi^{-1} \\
    -Z \Phi^{-1} & \Phi^{-1}
    \end{bmatrix} \\
    &=
    \begin{bmatrix}
        0 & I \\
        -\Phi |Z|^2\Phi^{-1} & 2\Phi \re(Z)\Phi^{-1}
    \end{bmatrix}.
\end{align}
$$

Comparing this expression with the definition of $\Omega$ in \eqref{eq:def_Omega},
we see that

$$
\begin{equation}
    M^{-1}K = \Phi |Z|^2\Phi^{-1}
    \qquad\text{and}\qquad
    M^{-1}C = -2\Phi\re(Z)\Phi^{-1}.
\end{equation}
$$

These two matrices commute, so
the damping is proportional.
Hence, proportional damping is basically equivalent to all mode shapes being real.

## Appendix

We sketch proofs of some results of linear algebra we used above. 

**Theorem A**

Let $A$ and $B$ be two diagonalizable matrices.
Then, $[A, B] = 0$ if and only if
there exists a common basis that diagonalizes both matrices.

 
*Proof*

It is not hard to see that mutually diagonalizable matrices commute.

Let $\\{\lambda_k\\}_k$ be the distinct eigenvalues of $A$, and $V_k$ the corresponding subspaces of eigenvectors.
Then, for $v \in V_k$ we have

$$
\begin{equation}
    AB v = BA v = \lambda_k Bv,
\end{equation}
$$

so $Bv$ is an eigenvector and $Bv \in V_k$.
Since $B$ is also diagonalizable, we can partition each
$V_k = W_{k1} + \cdots + W_{kl_k}$ into subspaces of eigenvectors of $B$.
Hence, a basis of vectors $\\{\psi_k\\}$ with $\psi_k \in W_{ij}$ diagonalizes both operators. $$\Box$$

If the eigenvalues of one of the matrices are distinct, then
it implies that the other matrix is diagonalizable as well.

**Theorem B**

Let $A$ be a diagonalizable matrix with distinct eigenvalues.
Then, $[A, B] = 0$ if and only if $B = p(A)$ for some polynomial $p$.

*Proof*

If $B = p(A)$, then clearly $B$ commutes with $A$.
Conversely, let us assume, using Theorem A, that

$$
\begin{equation}
    A =
    \begin{bmatrix}
        \lambda_1 \\
         & \ddots & \\
         & & \lambda_N
    \end{bmatrix}
    \qquad\text{and}\qquad
    B =
    \begin{bmatrix}
        \mu_1 \\
         & \ddots & \\
         & & \mu_N
    \end{bmatrix},
\end{equation}
$$

so for any polynomial $p(\lambda) = \sum_{j=0}^n c_j\lambda^j$ we have

$$
\begin{equation}
    p(A) =
    \begin{bmatrix}
        p(\lambda_1) \\
         & \ddots & \\
         & & p(\lambda_N)
    \end{bmatrix}.
\end{equation}
$$

To prove the theorem, we have to find numbers $c_j$ such that

$$
\begin{equation}
    p(\lambda_k) = \sum_j c_j\lambda_k^j = \mu_k.
\end{equation}
$$

Then, we can try to look for solutions of

$$
\begin{equation}
    \begin{bmatrix}
        1 & \lambda_1 & \cdots & \lambda_1^{N-1} \\
        \vdots & & & \vdots \\
        1 & \lambda_N & \cdots & \lambda_N^{N-1}
    \end{bmatrix}
    \begin{bmatrix}
        c_0 \\ \vdots \\ c_{N-1}
    \end{bmatrix}
    =
    \begin{bmatrix}
        \mu_1 \\ \vdots \\ \mu_N
    \end{bmatrix}.
\end{equation}
$$

The matrix at the left is known as the <a href="https://en.wikipedia.org/wiki/Vandermonde_matrix" target="_blank"> Vandermonde matrix</a>,
and it is invertible when the $\lambda_k$'s are distinct,
so the system can be solved,
which proves the existence of the polynomial in the statement of the theorem. $\Box$

## References

{% bibliography --cited %}
