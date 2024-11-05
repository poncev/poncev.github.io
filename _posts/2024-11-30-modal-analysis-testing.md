---
layout: post
title: "Modal Analysis: testing"
author: "Felipe Ponce-Vanegas"
categories: general
tags: [modal analysis]
---

In this follow-up to a [post]({% post_url 2024-11-02-modal-analysis-basics %}) on basic concepts in modal analysis,
we see the theory behind experimental modal analysis.

Linear models for describing vibrating structures have the form

$$
\begin{equation}
\begin{aligned}
&M\ddot{x} + C\dot{x} + Kx = F, \\
&y = Ox
\end{aligned}\tag{1}\label{mat:eq:basic_vibrations}
\end{equation}
$$

where $x(t) \in \R^N$ is a vector function representing the displacement from equilibrium,
$F$ are external forces,
$M$ is the mass, $C$ is the damping, $K$ is the stiffness,
and $O$ is the observation matrix.
We omit noise terms to simplify the analysis.

During testing the state $x$ is measured (or rather, the acceleration $\ddot{x}$ in most cases), so
we need to solve the differential equation in \eqref{mat:eq:basic_vibrations} to
relate it with the data collected.
For that, it is more convenient to write it as

$$
\begin{align}
    \frac{d}{dt}
    \begin{bmatrix}
        x \\ \dot{x}
    \end{bmatrix}
    &=
    \Lambda
    \begin{bmatrix}
        x \\ \dot{x}
    \end{bmatrix}
    +
    \begin{bmatrix}
        0 \\ f
    \end{bmatrix} \\
    &:=
    \begin{bmatrix}
        0 & I \\
        -M^{-1}K & -M^{-1}C
    \end{bmatrix}
    \begin{bmatrix}
        x \\ \dot{x}
    \end{bmatrix}
    +
    \begin{bmatrix}
        0 \\ f
    \end{bmatrix}
    \tag{2}\label{mat:eq:rewriting_main}
\end{align}
$$

where $f := M^{-1}F$;
recall that the symbol ($:=$) is used to introduce a definition, in this case for $\Lambda$.

The general solution of \eqref{mat:eq:rewriting_main} is

$$
\begin{equation}
    \begin{bmatrix}
        x(t) \\ \dot{x}(t)
    \end{bmatrix}
    =
    U(t)
    \begin{bmatrix}
        x_0 \\ \dot{x}_0
    \end{bmatrix}
    +
    \int_0^t U(t - \tau)
    \begin{bmatrix}
        0 \\ f(\tau)
    \end{bmatrix}
    \,d\tau,
\end{equation}
$$

where $x_0$ is the initial position, $\dot{x}_0$ the initial velocity, and
$U$ is the solution operator.
If $(u, v)^t$ is any column vector, then $U(t)(u, v)^t$ is a solution of the homogeneous system ($f = 0$).

As we saw in our [previous post]({% post_url 2024-11-02-modal-analysis-basics %}),
there are $N$ complex frequencies $z_k \in \C$, with $\im(z_k) > 0$, and independent mode shapes $\phi_k \in \C^N$ such that
the functions $e^{z_k}\phi_k$ solve the homogeneous system.
