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

*Warning*: since this post is based on {% cite ponce23 %},
I will use the notation and concepts in it without further notice.

Figure 8 was rendered using <a href="http://www.povray.org/" target="_blank"> POV-Ray </a>,
and the parameterization of the different geometrical objects was done with Matlab.
The code has been uploaded to BCAM's GitLab, and from now on
I will refer to the files as found there.

*Note:* The code was tested only in Ubuntu.

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

Now we create an instance of **RigidMotion**:

    rm = RigidMotion(theta, phi, a1, a2, a3);

**RigidMotion** has the method *exp_povray* which creates a file named *rotation_*[time]*.txt*.
This file can be read by POV-Ray to move an object according to the rigid motion.
In the Command Window type `rm.exp_povray(2)` so that we can place the tool at the end of the envelope at time $t = 2$.

### The Envelope class

This is the more complex class, and I explain here only what is strictly necessary;
at the end of this post I give more details.
To instantiate the class we need a tool and a rigid motion:

    e = Envelope(tl, rm);

Since the envelope tends to be singular along the edge of regression (cite paper),
we want to rule out its presence by verifying that $\partial_t^2 G$ does not change sign.
For that, type in the Command Window `e.plot_Gtt(2^6 + 1, 2^6 + 1);` and inspect the plot;
the arguments of *plot_Gtt* are the number of subdivisions of the time interval and tool length, respectively.

To see the envelope swept by `tl` subject to the motion `rm`,
type in the Command Window

    e.plot_envelope(2^5 + 1, 2^5 + 1);
    axis equal;

Since our goal is to visualize the envelope in POV-Ray,
let us type `e.plot_envelope(2^5 + 1, 2^5 + 1, povray=true);` to generate a file called *envelope.txt*,
which saves the points of the envelope and the normal vectors to each of them so that
POV-Ray can read them as <a href="https://www.povray.org/documentation/view/3.6.1/295/" target="_blank"> smooth triangles</a> in a <a href="https://www.povray.org/documentation/view/3.6.1/292/" target="_blank"> mesh</a>.

We can also plot the characteristic curves and streamlines on the envelope.
For that, type `hold on` to hold the plot of the envelope, and
then type `e.plot_characteristic(1, 2^7 + 1);` and `e.plot_time_curves(0, 2^7 + 1);`.
We can see now the characteristic curve at time $t = 1$, and
the streamline at tool height $l = 0$;
both methods also have a *povray* keyword argument to generate files which POV-Ray can read.
We emphasize these curves because smoothness is lost along them.

## POV-Ray Code

The goal of this section is to generate the envelope defined in the previous section:

<p style="text-align:center;">
<img
    src="/assets/img/C1_Regular.png"
    alt="rotation"
    width="200">
</p>
<p style="text-align:center;"><em><small>
Envelope with the tool (green) depicted at the end time.
</small></em></p>

Basic information about the placement of the camera and the light source in POV-Ray can be found
<a href="https://www.povray.org/documentation/3.7.0/t2_2.html#t2_2" target="_blank"> here</a>.
Recall that in the previous section we generated the files:

 - *tool.txt*
 - *envelope.txt*
 - *rotation_2.000.txt*
 - *char_100_whole.txt*
 - *time_curve_whole.txt*

You can see in the file *Example/C1_Regular.pov* that
to give a shiny look to the tool and the envelope we defined the
<a href="https://www.povray.org/documentation/view/3.6.0/79/" target="_blank"> finish</a>:

    #declare MyFinish = finish { 
      ambient .6
      diffuse .6
      phong 0.4
      phong_size 40
    }

Following the C++ tradition, we declared the objects before using them.
We declare the tool as a lathe object:

    #declare Tool = union {
      lathe {
        bezier_spline
        #include "tool.txt"
        texture{
          pigment { color rgbf<94/255,119/255,3/255, 0> }
          finish { MyFinish }
        }
        sturm
      }
    }

We needed the `sturm` keyword to avoid oddities in the tool.

The envelope is declared as a mesh:

    #declare Envelope = mesh {
      #include "envelope.txt"
    }

If you inspect *envelope.txt*, you will see the macro *smooth_quad*, which
is defined in *Example/MyMacros.inc*.
This macro merges two smooth triangles into a "smooth square".

We include the following code at the end of *C1_Regular.pov*
to place the tool and the envelope in the picture:

    object {
      Envelope
      texture {
        pigment { color rgb<106/255,125/255,142/255> }
        finish { MyFinish }
      }
    }

    object {
      Tool
      #include "rotation_2.000.txt"
    }

The characteristic curve at time $t=1$ and the streamline at tool height $l=0$
are included with the following code:

    #include "char_100_whole.txt"
    curve(-0.4, 0.4, 8e-3, 1e-3, <255/255,0/255,0/255>)

    #include "time_curve_whole.txt"
    curve(0, 2, 8e-3, 1e-3, <255/255,0/255,0/255>)

The macro `curve` is defined in *MyMacros.inc*.

To render the image, open a terminal in the directory *Example* and type:

    povray C1_Regular.pov MySettings.ini

## About the Envelope class

## References

{% bibliography --cited %}
