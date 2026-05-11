---
layout: post
title: "Indar is finally out! A Python package for the analysis of vibrations"
author: "Felipe Ponce-Vanegas"
categories: general
tags: [modal analysis,Python package]
---

This project started because I wanted to model machining processes such as turning,
where estimating the structural parameters of the system — tool holder, workpiece, fixture, and so on — is essential.
These structures are usually modeled as linear spring systems,
whose dynamics can be [solved analytically]({% post_url 2024-11-18-modal-analysis-dynamics %}) and used to fit experimental data.

There are already excellent programs for Experimental Modal Analysis (EMA),
such as [Simcenter Testlab](https://www.siemens.com/en-gb/products/simcenter/physical-testing/testlab/)
or the [Structural Dynamics Toolbox](https://www.sdtools.com/), so
why develop another one? Initially, I simply wanted to learn the subject in depth.
But once I started implementing the algorithms myself,
sharing the work with the community felt like a natural next step.
That eventually led me to publish the project in BCAM’s GitLab [repository](https://gitlab.bcamath.org/fponce/indar).

For a long time, I referred to the package simply as *resonance*,
but when the time came to publish it, I needed a memorable name that was not already taken on PyPI.
After a long conversation with ChatGPT and discussions with several local people in the Basque Country,
I decided to name it *Indar*, which means *force* in Basque.
More precisely, the published package name is *bcam-indar*, since I use *bcam* as the namespace.

While learning and implementing EMA algorithms,
I also developed several ideas that I incorporated into the code.
I plan to publish a companion paper describing these contributions and discussing possible improvements
to existing EMA methodologies in more detail.

At the moment, there is still much work to do in terms of code quality, testing, and documentation.
Still, I hope the project eventually grows into something useful for the wider community.
