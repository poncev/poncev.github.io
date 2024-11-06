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
M\ddot{x} + C\dot{x} + Kx = F,\label{eq:basic_vibration}
\end{equation}
$$

where $x(t) \in \R^N$ is a vector function representing the displacement from equilibrium,
$F$ are external forces,
$M$ is the mass, $C$ is the damping, and $K$ is the stiffness.
The dimension $N$ is referred to as the number of Degrees of Freedom (DoF).

$M$, $C$, and $K$ are usually real, symmetric, positive-definite matrices, so
we will impose these hypotheses as well.
This can be justified using the lagrangian formulation for a system of oscillators, and then
approximating the dynamics to first order, in which case
$M$ and $K$ are jacobian matrices;
however, see Section 2.8 in {% cite ewins00 %} for non-symmetric examples.
$C$ is usually hard to model, and oftentimes conditions on it are imposed
for mathematical convenience, but not for physical motivation.

### Resonances

A resonance is a function $e^{i\omega t}\phi$, for $\omega > 0$ and $\phi \in \C^N$,
that solves the homogenous system \eqref{eq:basic_vibration} with $C = 0$.
The number $\omega$ is referred to as a natural frequency, and
$\phi$ as a mode shape of the undamped system.
The system \eqref{eq:basic_vibration} always has a full basis of $N$ resonances.

To find the resonances, notice that by the symmetry of $M$ there is an orthonormal matrix $U$ such that $U^t M U = \diag(m_1, \ldots, m_N)$.
If we rescale it as $\Psi := U\diag(1 / \sqrt{m_1}, \ldots, 1/\sqrt{m_N})$,
then $\Psi^t M \Psi = I$.

Now, let us write $y := \Psi^{-1} x$ and multiply \eqref{eq:basic_vibration} by $\Psi^t$ to get

$$
\begin{equation*}
\ddot{y} + A\dot{y} + By = 0,
\end{equation*}
$$

where $A := \Psi^t C \Psi$ and $B := \Psi^t K \Psi$.
Since we are assuming that $A = 0$ (undamped system), we must find solutions of the system

$$
\begin{equation*}
    (\omega^2 - B)y = 0,
\end{equation*}
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
\begin{equation*}
    \ddot{q}_k + 2\omega_k\zeta_k \dot{q}_k + \omega_k^2q_k = 0,
\end{equation*}
$$

where $\zeta_k$ is called the damping ratio.
The mode shapes in the original coordinate system are again $\phi_k := \Psi \eta_k$, which are real vectors and mass-normalized.
In this case, it is said that the damping is proportional, and
it is a common and convenient assumption about the matrix $C$.

When is the damping proportional?
Two symmetric matrices are mutually diagonalizable if and only if
$[A, B] := AB - AB = 0$; see Theorem A in the Appendix.
In terms of $C$ and $K$, this is $C\Psi\Psi^t K = K\Psi\Psi^t C$.
Since $M^{-1} = \Psi \Psi^t$, then proportional damping is equivalent to

$$
\begin{equation}
    (M^{-1}C)(M^{-1}K) = (M^{-1}K)(M^{-1}C);
    \label{mab:eq:commutativity}
\end{equation}
$$

see Section 2.5 in {% cite ewins00 %} for further discussion.

If the matrix $A$ commutes with the symmetric matrix $B$, and
all the eigenvalues (natural frequencies) of $B$ are distinct, then $A = \sum_{j=0}^n c_jB^j$ for
some $n$ and some coefficients $c_j$; see Theorem B in the Appendix.
Again, in terms of $C$ and $K$ we end up with the known formula

$$
\begin{equation*}
    (M^{-1}C) = \sum_{j=0}^n c_j(M^{-1}K)^j.
\end{equation*}
$$

A more detailed account of these results can be found in {% cite cuaghey65 %}.

### Non-proportional damping

In the general case of \eqref{eq:basic_vibration},
we can still look for solutions of the form $e^{zt}\phi$,
where $z \in \C$ are complex frequencies and $\phi \in \C^N$ are mode shapes.
If we replace $e^{zt}\phi$ in \eqref{eq:basic_vibration} we get

$$
\begin{equation}
    (z^2M + zC + K)\phi = 0.\label{eq:undamped_quad}
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
    \label{eq:def_Omega}
\end{equation}
$$

In fact, if $z$ and $\phi$ satisfy \eqref{eq:undamped_quad}, then $(\phi, z\phi)^t$ is an eigenvector of $\Omega$ with eigenvalue $z$.
It is not hard to see that every eigenvector has this form.

Since the system matrices are real, then for each eigenvector $(\phi, z\phi)^t$
the vector $(\bar{\phi}, \bar{z}\bar{\phi})^t$ is also an eigenvector.
Hence, for definiteness, we add to the complex frequencies $z$ the condition $\im(z) > 0$, because
the other eigenvector is obtained by conjugation;
under our hypotheses about the system (*i.e.* symmetric, positive-definite matrices), $\im(z) \neq 0$.
We will assume that the mode shapes $\\{\phi_k\\}_k$ form a basis of $\C^N$,
which is true for a generic system --- *i.e.*, it is false for set of systems $(K, C, M)$ of measure zero.

In general, if $T$ is a matrix and $U = (u_1, \ldots, u_N)$ is a basis of eigenvectors,
then $TU = U\Lambda$, where $\Lambda := \diag(\lambda_1,\ldots,\lambda_N)$ are the eigenvalues.
The matrix $U$ is well-defined up to normalization. In fact,
if $d$ is a diagonal matrix, then $Ud$ is also a basis of eigenvectors.

Let us group the mode shapes and eigenvalues into the matrices $\Phi := (\phi_1, \ldots, \phi_N)$ and $Z := \diag(z_1, \ldots, z_N)$.
In this way,

$$
\begin{equation*}
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
        Z & \\
         & \bar{Z}
    \end{bmatrix},
\end{equation*}
$$

or taking inverse at both sides

$$
\begin{equation*}
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
        Z & \\
         & \bar{Z}
    \end{bmatrix}.
\end{equation*}
$$

It is useful to get a more transparent expression for the inverse matrix appearing in this formula.

Considering again the general case $TU = U\Lambda$,
we can deduce that $U^{-1}T = \Lambda U^{-1}$, and then
transposing we get $T^tU^{-t} = U^{-t}\Lambda$.
This means that the columns of $U^{-t}$ form a basis of eigenvectors of $T^t$.
Hence, if we solve the spectral problem for $A^t$ and find $V$ such that $T^tV = V\Lambda$,
then we could conclude that $U^{-t} = Vd$, where $d$ is a diagonal matrix, and
we would get our desired expression for the inverse of $U$, that is, $U^{-1} = dV^t$.

If we apply the above arguments to $T = \Omega$,
then we must solve the spectral problem for

$$
\begin{equation*}
    \Omega^t =
    \begin{bmatrix}
        0 & -KM^{-1} \\
        I & -CM^{-1}
    \end{bmatrix}.
\end{equation*}
$$

It is not hard to see that the eigenvectors $v$ have the form

$$
\begin{equation*}
    v =
    \begin{bmatrix}
        -K\phi \\ z M\phi
    \end{bmatrix},
\end{equation*}
$$

where $\phi$ satisfies \eqref{eq:undamped_quad}.

Therefore, a matrix of eigenvectors is

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
    \label{mab:eq:inverse}
\end{equation}
$$

so we only remains to fix the diagonal matrix $d$.
For that, notice that

$$
\begin{align*}
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
\end{align*}
$$

Incidentally, this expression also provides us with important orthogonality relations
for the mode shapes.
From the non-diagonal terms we get

$$
\begin{equation*}
    z_iz_j\phi_i^tM\phi_j - \phi_i^tK\phi_j = 0,
    \qquad\text{when } z_i \neq z_j;
\end{equation*}
$$

see Section 2.7.1 in {% cite ewins00 %}.
On the other hand, from the diagonal terms we get

$$
\begin{equation*}
    z_k^2\phi^t_k M \phi_k - \phi^t_k K \phi_k = 1/d_k.
\end{equation*}
$$

To set a value for $d_k$, we need to specify a normalization for the mode shapes.
When the damping is proportional,
the mass-normalization of the mode shapes implies $z_k^2 - \omega^2_k = 1/d_k$.
Since $z_k = -\omega_k\zeta_k + i\omega_k\sqrt{1 - \zeta_k^2}$,
then

$$
\begin{equation*}
    1/d_k = z_k^2 - \rvert z_k\lvert^2
    = 2iz_k\im(z_k).
\end{equation*}
$$

Hence, in general, we say that *the mode shapes are mass-normalized if*

$$
\begin{equation*}
    z_k^2\phi^t_k M \phi_k - \phi^t_k K \phi_k = 2iz_k\im(z_k).
\end{equation*}
$$

Now, we can write down the inverse in \eqref{mab:eq:inverse} as

$$
\begin{equation}
    \begin{bmatrix}
    \Phi & \bar{\Phi} \\
    \Phi Z & \bar{\Phi}\bar{Z}
    \end{bmatrix}^{-1}
    =
    \frac{i}{2}
    \begin{bmatrix}
        \im(Z)^{-1} & \\
         & \im(Z)^{-1}
    \end{bmatrix}
    \begin{bmatrix}
        -Z^{-1}\Phi^t & -\Phi^t \\
        \bar{Z}^{-1}\bar{\Phi}^t & \bar{\Phi}^t
    \end{bmatrix}
    \begin{bmatrix}
        -K & \\
         & M
    \end{bmatrix}
    \label{mab:eq:final_inverse}
\end{equation}
$$

### Real Mode Shapes

For a general system, the mode shapes are complex, but
we have seen that for undamped systems ($C = 0$)
it is possible to choose the mode shapes in $\R^N$.
We will show that, conversely, *if all the mode shapes are real, then $C = 0$,*
so when $C \neq 0$ at least one mode shape must be complex.

To see that real mode shapes imply $C = 0$,
we notice that for real mode shapes we can find another expression for the inverse \eqref{mab:eq:final_inverse} more directly as

$$
\begin{equation*}
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
    \end{bmatrix}.
\end{equation*}
$$

If we compare this expression with \eqref{mab:eq:final_inverse}, we see that

$$
\begin{equation*}
    \begin{bmatrix}
    \bar{Z} \Phi^{-1} & -\Phi^{-1} \\
    -Z \Phi^{-1} & \Phi^{-1}
    \end{bmatrix}
    =
    \begin{bmatrix}
        -Z^{-1}\Phi^t & -\Phi^t \\
        \bar{Z}^{-1}\bar{\Phi}^t & \bar{\Phi}^t
    \end{bmatrix}
    \begin{bmatrix}
        -K & \\
         & M
    \end{bmatrix},
\end{equation*}
$$

which implies

$$
\begin{equation*}
    \Phi^t M \Phi = I
    \qquad\text{and}\qquad
    \Phi^t K \Phi = \rvert Z \lvert^2.
\end{equation*}
$$

Plugging in these formulae in \eqref{eq:undamped_quad} we see as well that $\Phi^t C\Phi = -2\re(Z)$.
Therefore,

$$
\begin{equation*}
    M^{-1}K = \Phi \lvert Z\rvert^2 \Phi^{-1}
    \qquad\text{and}\qquad
    M^{-1}C = -2\Phi \re(Z)\Phi^{-1}.
\end{equation*}
$$

These two matrices commute as in \eqref{mab:eq:commutativity}, so
the damping is proportional.
Hence, proportional damping is equivalent to all mode shapes being real.

## Appendix

We sketch proofs of some results of linear algebra we used above. 

**Theorem A**

Let $A$ and $B$ be two diagonalizable matrices.
Then, $[A, B] = 0$ if and only if
there exists a common basis that diagonalizes both matrices.

 
*Proof*

It is not hard to see that mutually diagonalizable matrices commute.

Conversely, let $\\{\lambda_k\\}_k$ be the distinct eigenvalues of $A$, and $V_k$ the corresponding subspaces of eigenvectors.
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

If the eigenvalues of one of the matrices are all distinct, then
it implies that the other matrix is diagonalizable as well.

**Theorem B**

Let $A$ be a diagonalizable matrix with all eigenvalues distinct.
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
