---
layout: post
title: "The Miracle of Data-Driven Modelling"
author: "Felipe Ponce-Vanegas"
categories: general
---

# The Miracle of Data-Driven Modelling

A typical problem in machine learning is to fit a collection of labelled data points
$(\bold{x}^1, y^1), \ldots, (\bold{x}^p, y^p)$ using some model function
$$y = f(\bold{x}).$$
Each point $\bold{x} = (x_1, \ldots, x_N)$ is a vector in $\R^N$, where $N$ could easily be about millions.
For example, a black and white picture with a regular camera may has $1920\times 1080$ pixels,
so we can think of each photo as a vector $\bold{x}\in\R^N$, where $N = 1920\times 1080 = 2\,073\,600$
and the value of each variable $x_i$ is in a gray scale $0 \le x_i \le 1$, where
0 represents white and 1 black.

Sometimes people think that many numbers equals many data because of
a lack of clarity about their objectives, so
they fail to understand what constitutes a single instance.
If the objective is to tell whether there is a cat in a picture,
hundreds of photos with a cheap camera is much better than
two photos with a camera with thousands of pixels.
In fact, too many variables with a handful of instances jeopardize the effectiveness of learning from data.

If our data $\bold{x}$ are contained in the unit cube (or rather, hyper-cube) $[0,1]^N\subset\R^N$,
a naive approach to learning from data would be to sample uniformly the unit cube and
then to fit the labels $y$ with our model function $f$.
However, if $N$ is slightly big, then the naive approach is hopeless; let's see why.

To sample $[0,1]^N$ uniformly
we divide $[0,1]^N$ into smaller cubes of side-length $l$ and sample the center $\bold{x}^{\textrm{center}}$ of the cubes.
The size of $l$ depends on the resolution we wish for our algorithm.
If we define the Pythagorean distance function
$$\lVert \bold{x}^1 - \bold{x}^2\rVert^2 := \sum_{i=1}^N \,\lvert x_i^1 - x_i^2\rvert^2,$$
then we may be satisfied accepting two points $\bold{x}^1$ and $\bold{x}^2$ as equal if $\lVert \bold{x}^1 - \bold{x}^2\rVert < \delta$ for some small number $\delta$ which we call resolution.

In a cube of side-length $l$,
the distance between a point and the center of the cube is at most
$$\lVert \bold{x}^{\textrm{center}} - \bold{x}\rVert \le \sqrt{N}(l/2),$$
and this bound cannot be improved.
To put it in context, suppose that our data points belong to $\R^{10^6}$ and that each variable $x_i$ is measured in millimeters,
then in a cube of side-length $l = 1$ mm there exist pairs of points at 1 m of distance apart!
This phenomenon is a manifestation of the curse of dimensionality and this works against us;
other interesting phenomenon is <a href="https://en.wikipedia.org/wiki/Concentration_of_measure" target="_blank"> concentration of measure </a>.

If we set a resolution of $\delta$,
then the side-length of the cubes should be $l \approx \delta / \sqrt{N}$,
which is very small for large dimensions $N$.
Now, the number of cubes in our partition of $[0,1]^N$ is
$$M = l^N \approx (N/\delta^2)^{N/2}.$$
If $N = 10^6$ then $M$ is huge, and it scales
terribly fast.
Even giving up resolution and taking $\delta = \delta_0\sqrt{N}$ large does not solve the problem;
in these terms, we are replacing the Pythagorean distance by the root mean square, which, for example,
is not good at detecting transients in a signal.
How can we possibly learn from data when we need so much?

The answer is that real life data usually come with some structure, as
information theory has taught us since long ago.
Pictures we find interesting are not just a bunch of random pixels,
but they are make up of uniform regions where color does not change much;
hence, uniform sampling usually misrepresents the process
of sampling data in real life problems.
Data points may concentrate around low-dimensional manifolds, or
create clusters that allow us to ease the computational burden when training algorithms;
see also <a href="https://en.wikipedia.org/wiki/Feature_extraction" target="_blank"> feature extraction </a>.
Then, one of the challenges in Data Science is to be able to detect and exploit patterns in information.

## Other references

1. Mallat, S. (2016). Understanding deep convolutional networks, *Phil. Trans. R. Soc. A.* **374**: 20150203. [link][1]
2. Hastie, T., Tibshirani, R., & Friedman, J. (2009). *The elements of statistical learning: Data mining, inference, and prediction*, 2nd ed. Springer.

[1]: https://doi.org/10.1098/rsta.2015.020