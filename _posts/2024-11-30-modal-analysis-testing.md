---
layout: post
title: "Modal Analysis: testing"
author: "Felipe Ponce-Vanegas"
categories: general
tags: [modal analysis]
---

In this follow up of a [previous post]({% post_url 2024-11-02-modal-analysis-basics %}),
we 

$$
\begin{equation}
M\ddot{x} + C\dot{x} + Kx = F,\tag{1}\label{eq:basic_vib}
\end{equation}
$$
s
where $x(t) \in \R^N$ is a vector function representing the displacement from equilibrium,
$F$ are external forces,
$M$ is the mass, $C$ is the damping, and $K$ is the stiffness.
The dimension $N$ is referred to as the number of Degrees of Freedom (DoF).
See [Name of Link]