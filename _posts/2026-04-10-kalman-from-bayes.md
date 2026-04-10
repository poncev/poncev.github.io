---
layout: post
title: "Kalman filter from Bayes filter"
author: "Felipe Ponce-Vanegas"
categories: general
tags: [bayesian statistics]
---

An important problem in engineering is to estimate the evolution of a hidden (latent) variable,
such as the trajectory of a rocket, from noisy observations.
If the dynamics of the latent variable is known, and
also the relationship between the latent variable and the observation, also called emission, then
Bayes filter is a robust method for estimating the latent variable.

## Bayes filter

For discrete times, we denote the latent variable by $x_k \in \R^n$ at time $k \in \Z$, and
the observation by $y_k \in \R^m$.
We can formulate the filtering problem as computing the conditional probability

$$
\begin{equation}
    \label{eq:filter_problem}
    p(x_k\mid y_{1:k}) = \text{the probability of } x_k \text{ given the historical } y_{1:k},
\end{equation}
$$

where $y_{1:k}$ is a shorthand for $y_1, \ldots, y_k$.
By <a href="https://en.wikipedia.org/wiki/Bayes%27_theorem" target="_blank"> Bayes's theorem</a>, \eqref{eq:filter_problem} gets into

$$
\begin{align}
    p(x_k\mid y_{1:k}) &= \frac{1}{p(y_k\mid y_{1:k-1})}\,p(y_k\mid x_k, y_{1:k-1})p(x_k\mid y_{1:k-1}) \notag \\
    &\propto p(y_k\mid x_k, y_{1:k-1})\,p(x_k\mid y_{1:k-1}).
\end{align}
$$

For fixed observations $y_k$, the terms $p(y_k\mid y_{1:k-1})$ are constant that at the end can be swept under the rug as a normalization constant.

Now we introduce an important hypothesis:
*at each time, the observation only depends on the latent variable at that time*.
This hypothesis simplifies $p(y_k\mid x_k, y_{1:k-1})$ to $p(y_k\mid x_k)$.
Hence,

$$
\begin{equation}
    p(x_k\mid y_{1:k}) \propto p(y_k\mid x_k)\,p(x_k\mid y_{1:k-1}).
\end{equation}
$$

To compute $p(x_k\mid y_{1:k-1})$ we use the integral representation

$$
\begin{equation}
    \label{eq:half-simplification}
    p(x_k\mid y_{1:k-1}) = \int p(x_k \mid x_{k-1}, y_{1:k-1})\,p(x_{k-1} \mid y_{1:k-1})\,dx_{k-1}.
\end{equation}
$$

Here we introduce the second fundamental hypothesis:
*at each time, the latent variable only depends on the previous state*.
This is known as the causal Markov condition, and
it allows us to rewrite $p(x_k \mid x_{k-1}, y_{1:k-1})$ in \eqref{eq:half-simplification} as $p(x_k \mid x_{k-1})$, so
the target conditional probability in \eqref{eq:filter_problem} is

$$
\begin{equation}
    \label{eq:bayes_result}
    p(x_k\mid y_{1:k}) \propto p(y_k\mid x_k) \int p(x_k \mid x_{k-1})\,p(x_{k-1} \mid y_{1:k-1})\,dx_{k-1}.
\end{equation}
$$

The important conclusion here is that the central quantity $p(x_k\mid y_{1:k})$ can be expressed iteratively in terms of $p(x_{k-1}\mid y_{1:k-1})$, as long as
we know the dynamics of the latent variable $p(x_k \mid x_{k-1})$, and
the emission $p(y_k\mid x_k)$.

## Kalman filter

Suppose that the dynamics of the latent variable and emission obey the following model

$$
\begin{align}
x_k &= F_{k-1}x_{k-1} + w_{k-1} \\
y_k &= H_k x_k + v_k,
\end{align}
$$

where $F_k$ and $H_k$ are matrices, and
the random effects are mutually independent normals $w_k \sim \mathcal{N}(0, Q_k)$ and $v_k \sim \mathcal{N}(0, R_k)$,
where $Q_k$ and $R_k$ are the covariance matrices.
These two equations provide us with the missing quantities at the end of the previous section $p(x_k \mid x_{k-1})$ and $p(y_k\mid x_k)$, respectively.

To compute $p(x_k \mid y_{1:k})$ in \eqref{eq:filter_problem},
we will exploit the fact that a normal distribution is fully determined by its mean and covariance matrix.
Inductively, let us assume that $p(x_{k-1} \mid y_{1:k-1})$ is normal, with mean $\hat{x}\_{k-1}$ and covariance $P_{k-1}$.

We expand the integral in \eqref{eq:bayes_result} as

$$
\begin{multline}
    \label{eq:big_integral}
    \int \frac{1}{\lvert Q_{k-1}\rvert^{1/2}}\exp\Big(-\frac{1}{2}w_{k-1}^TQ_{k-1}^{-1}w_{k-1}\Big) \cdot \\
    \frac{1}{\lvert P_{k-1}\rvert^{1/2}}\exp\Big(-\frac{1}{2}(x_{k-1}-\hat{x}_{k-1})^TP_{k-1}^{-1}(x_{k-1}-\hat{x}_{k-1})\Big)\,dx_{k-1},
\end{multline}
$$

where $\lvert Q_{k-1}\rvert$ is the determinant.
Using the new variables $\Delta x_{k-1} := x_{k-1} - \hat{x}\_{k-1}$ and
$\eta := x_{k} - F_{k-1}\hat{x}_{k-1}$
Working out the exponent, we find that the above integral transforms into

$$
\begin{multline}
    \frac{\exp(-\frac{1}{2}(\eta^TQ_{k-1}^{-1}\eta - \mu^TW_{k-1}^{-1}\mu))}{(\lvert Q_{k-1}\rvert \lvert P_{k-1}\rvert)^{1/2}}\cdot \\
    \int \exp\Big(-\frac{1}{2}(\Delta x_{k-1} - \mu)^TW_{k-1}^{-1}(\Delta x_{k-1} - \mu)\Big)\,d\Delta x_{k-1},
\end{multline}
$$

where

$$
\begin{gather}
    W_{k-1}^{-1} := F_{k-1}^TQ_{k-1}^{-1}F_{k-1} + P^{-1}_{k-1},
    \text{ and} \\
    \mu := W_{k-1}F_{k-1}^TQ_{k-1}^{-1}\eta
\end{gather}
$$

The exponent outside the integral is

$$
\begin{equation}
    \eta^TQ_{k-1}^{-1}\eta - \mu^TW_{k-1}^{-1}\mu
    = \eta^T (Q^{-1}_{k-1} - Q^{-1}_{k-1}F_{k-1}W_{k-1}F_{k-1}^TQ^{-1}_{k-1})\eta.
\end{equation}
$$

Hence, the integral in \eqref{eq:big_integral} is proportional to

$$
\begin{equation}
    \exp\Big(-\frac{1}{2}\eta^T(Q^{-1}_{k-1} - Q^{-1}_{k-1}F_{k-1}W_{k-1}F_{k-1}^TQ^{-1}_{k-1})\eta\Big)
\end{equation}
$$


Recall we are interested in \eqref{eq:bayes_result}, and
the all this work was to get

$$
\begin{equation}
    p(x_k\mid y_{1:k}) \propto p(y_k\mid x_k) \exp\Big[-\frac{1}{2}\eta^TA\eta\Big].
\end{equation}
$$

Expanding now $p(y_k\mid x_k)$, we have that

$$
\begin{equation}
    p(x_k\mid y_{1:k}) \propto \exp\Big[-\frac{1}{2}\big(v_k^TR_k^{-1}v_k + \eta^TA\eta\big)\Big].
\end{equation}
$$
