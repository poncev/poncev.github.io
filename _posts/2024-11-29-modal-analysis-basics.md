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
The dimension of the equation $N$ is referred to the Degrees of Freedom (DoF).

$M$ and $K$ are usually real, symmetric, positive-definite matrices.
This can be justified using the lagrangian formulation of a system of oscillators and
approximating the dynamics to first order, in which case
$M$ and $K$ are jacobian matrices;
however, see Section 2.8 in {% cite ewins00 %}.
$C$ is usually hard to model, and we will impose some conditions on it
for mathematical convenience, but not physically motivated.

A resonance is a function $e^{i\omega t}\phi$ that solves the homogenous system \eqref{eq:basic_vibration} with $C = 0$,
where $\omega > 0$ and $\phi \in \C^N$ is a constant vector.
The system \eqref{eq:basic_vibration} always has a full basis of resonances.
To see this, notice that by the symmetry of $K$ there is an orthonormal matrix $U$ such that $U^t M U = \diag(m_1, \ldots, m_N)$.
If we rescale it as $\Psi := U\diag(1 / \sqrt{m_1}, \ldots, 1/\sqrt{m_N})$,
then $\Psi^t M \Psi = I$.

If we write $y := \Psi^{-1} x$ and multiply \eqref{eq:basic_vibration} by $\Psi^t$ we get

$$
\begin{equation}
\ddot{y} + A\dot{y} + By = 0,
\end{equation}
$$

where $A := \Psi^t C \Psi$ and $B := \Psi^t K \Psi$.
Since we are assuming that $A = 0$, we have to find solutions of the system

$$
\begin{equation}
    (\omega^2 - B)y = 0,
\end{equation}
$$

but by the symmetry of $B$ we can find an orthogonal basis of real vectors $\\{y_k\\}_{k=1,\ldots, N}$ with eigenvalues $\\{\omega_k^2 \\}$.
In the original system of coordinates, the eigenvalues are $\phi_k := \Psi y_k$,
and they satisfy $\phi_l^t M \phi_k = \delta _{kl}$, where
$\delta _{kl}$ is the Kronecker delta;
it is said that the basis $\\{\phi_k\\}$ is mass-normalized.

If the symmetric matrices $A$ and $B$ are mutually diagonalizable, that is,
if there is an orthonormal matrix $Q$ such that $Q^tAQ$ and $Q^t BQ$ are diagonal, then writing $q := Q^ty$ we can further decouple the system into the $N$ independent equations

$$
\begin{equation}
    \ddot{q}_k + 2\omega_k\zeta_k \dot{q}_k + \omega_k^2q_k = 0,
\end{equation}
$$

where $\zeta_k$ is called the damping ration.
In this case, it is said that the damping is proportional, and
it is a common and convenient assumption about the matrix $C$.

Two symmetric matrices are mutually diagonalizable if and only if
$[A, B] := AB - AB = 0$; see, *e.g.*, Chapter 8.2.4 of {% cite linearAlgebra59 %}.
In terms of $C$ and $K$, this is $C\Psi\Psi^t K = K\Psi\Psi^t C$.
Since $M^{-1} = \Psi \Psi^t$, we can re-write the last expression in a most common form

$$
\begin{equation}
    (M^{-1}C)(M^{-1}K) = (M^{-1}K)(M^{-1}C).
\end{equation}
$$

If the matrix $A$ commutes with the symmetric matrix $B$, and
all the eigenvalues of $B$ are distinct, then $A = \sum_{j=0}^n c_jB^j$ for
some $n$ and some coefficients $c_j$; see, *e.g.*, Chapter 8.2.2 of {% cite linearAlgebra59 %}.
Again, in terms of $C$ and $K$ we end up with the known formula

$$
\begin{equation}
    (M^{-1}C) = \sum_{j=0}^n c_j(M^{-1}K)^j.
\end{equation}
$$

A more detailed account of these results can be found in {% cite cuaghey65 %}.



## References

{% bibliography --cited %}
