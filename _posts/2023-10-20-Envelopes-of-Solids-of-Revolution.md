---
layout: post
title: "Envelopes of Solids of Revolution"
author: "Felipe Ponce-Vanegas"
categories: general
---

Flank milling is a manufacturing process which
works by removal of material from the workpiece with a rotary cutter.
Since the cutter rotates much faster than the feed rate of the tool, then
the cutter can be thought of as a solid of revolution,
and the envelope swept by this solid becomes, up to several subtleties,
the final surface of the workpiece.

Many books on differential geometry discuss the subject of envelopes;
see for example {% cite kreyszig91 --label section --locator 85 %}.
In the paper {% cite ponce23 %} we discuss the effect of the regularity
of the profile and the trajectory of the tool on the smoothness of the envelope.

Since the profile and the trajectory of a tool is designed using splines,
then the loss of smoothness at the knots of the spline makes it relevant
to understand the consequences of the knots on the smoothness of the envelope.
In our paper we illustrate different phenomena using pictures of envelopes generated under several circumstances,
and it is the objective of this post to show how to produce one of these pictures, Figure 10 in our paper.

Figure 8 was rendered using <a href="http://www.povray.org/" target="_blank"> POV-Ray </a>,
and the parameterization of the different geometrical objects was done with Matlab.
The code has been uploaded to GitHub, and from now on
I will refer to the files as found there.

## Matlab Code

The directory *src* contains the basic program to generate envelopes.
There are three files, one for each class: **Tool**, **RigidMotion** and **Envelope**.
All these classes are used in *Example/C1_Envelope.m* for Figure 8.

### The Tool class

First, we define in *C1_Envelope.m* the profile of the tool using <a href="https://www.mathworks.com/help/curvefit/spmak.html" target="_blank"> spmak</a>:

    L = 0.8;
    pfl = spmak([-L/2 -L/2 -L/2 0 L/2 L/2 L/2], ...
        [0.1 0.1 0.25 0.25]);

This is a B-spline of order 3 representing a tool of length *L*,
so the profile of the tool is $C^1$, and $0$ is the point where smoothness is lost.
To create an instance of **Tool** we write:

    tl = Tool(pfl);

The class **Tool** has the method *plot* which you can use as `tl.plot()` to see the profile of the tool,
or you can use `tl.plot(k)` to see the $k$-derivative; for example,
with `tl.plot(2)` you can see the discontinuity at $0$.

Type in the Matlab Command Window `tl.plot(povray=true)`, which creates the file *tool.txt*.
This file saves the information of the profile of the tool, and
writes the instructions POV-Ray needs to render the tool using
<a href="https://www.povray.org/documentation/view/3.7.1/60/" target="_blank"> lathe</a>.

### The RigidMotion class

We assume that the tool is a solid of revolution around the axis $x_3$.
We can describe the motion of the tool with a rigid motion $E$ as

$$E(t)\bold{y} := R(t)\bold{x} + \bold{a}(t),$$

where $R \in SO(3)$ is a rotation, $\bold{a}$ is a translation, and $t$ is time;
here, $t$ does not refer to the physical time but to an advance parameter.
Nothing is lost if we assume that $a_2 = t$, because
we can always use a rigid motion and a time re-parameterization to enforce this condition.

In general, to specify a rotation we need 3 parameters, but
because of the symmetry of the tool this number goes down to 2.
For the **RigidMotion** class we chose the parameterization

$$\begin{gather}
R(\theta,\varphi) = U(\theta)\,V(\varphi), \\ \\
U(\theta) = \begin{pmatrix}
1   & 0             & 0             \\
0   & \cos(\theta)  & \sin(\theta)  \\
0   & -\sin(\theta) & \cos(\theta)
\end{pmatrix}
\quad \text{and} \quad
V(\varphi) = \begin{pmatrix}
\cos(\varphi)   & 0  & \sin(\varphi)   \\
0               & 1  & 0               \\
-\sin(\varphi)  & 0  & \cos(\varphi)
\end{pmatrix},
\end{gather}$$

where $\theta \in [-\pi, \pi)$, and $\varphi \in [-\pi/2, \pi/2)$.
Hence, the motion of the tool is completely determined by $\theta$, $\varphi$, and $\bold{a}$.

<p style="text-align:center;">
<img
    src="/assets/img/rotation_pmt.png"
    alt="rotation"
    width="300">
</p>
<p style="text-align:center;"><em><small>
A rotation of a solid of revolution can be seen as a vector in the unit sphere.
</small></em></p>

For Figure 8 we used the trajectory

    theta = spmak([0 1 2 2 2], 0.25*pi*[-0.4 0.3]);
    phi = spmak([0 1 2 2 2], 0.25*pi*[-0.5 -0.1]);
    a1 = spmak([0 1 2 2 2], [0.5 -0.2]);
    a2 = spmak([0 2 2], 2);
    a3 = spmak([0 1 2 2 2], [-0.6 -0.8]);

To create an instance of **RigidMotion** we write:

    rm = RigidMotion(theta, phi, a1, a2, a3);

The **RigidMotion** has the method *exp_povray* which creates a file named *rotation_time.txt*.
This file can be read by POV-Ray to move an object according to the rigid motion.
In the Command Window type `rm.exp_povray(2)` so that we can place the tool at the end of the envelope at time $t = 2$.

### The Envelope class

## POV-Ray Code

## References

{% bibliography --cited %}
