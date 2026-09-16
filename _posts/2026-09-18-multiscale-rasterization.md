---
layout: post
title: "Multiscale rasterization: exploring multi-agentic ai"
author: "Felipe Ponce-Vanegas"
categories: general
tags: [ai, coding]
---

Programming with a single agent seems to be, at the pace technology is advancing, the Stone Age.
A few days ago I decided to explore multi-agentic ai with a few agents ---
which, by the way, also seems outdated comparing with multi-agentic swarms.
Any way, I found many tools for different levels of users.

<p style="text-align:center;">
  <img src="/assets/img/multiagentic_tools.svg" alt="Multiagentic tools" width="90%">
</p>
<!-- <p style="text-align:center;"><em><small>
Caption text here.
</small></em></p> -->

I tried [Pydantic](https://pydantic.dev/docs/ai/overview/) with a small program where two agents played Hangman and another agent served as an arbiter.
Even though I had fun doing it, it was also too much work for daily use in my job, so
I decided a more user-friendly approach with VSCode + Openrouter.

VSCode with Copilot offers an intuitive interface for the management of agents,
which simplifies the integration into the codebase.
The small pool of models in Copilot can be expanded using [OpenRouter](https://openrouter.ai/), or
any other model manager like [OpenCode](https://opencode.ai/go).
I used Openrouter because it is easy to integrate into VSCode;
you only have to look for *Other Models* and then add models using the OpenRouter api-key.

<p style="text-align:center;">
  <img src="/assets/img/multiagentic_more_models.png" alt="Multiagentic more models" width="60%">
</p>

To test the multi-agentic capabilities,
I decided to implement a 2D version of the algorithm in {% cite daum12 %},
which is an algorithm for multiscale rasterization.
The main objective of the project was to see if the agents could implement it from scratch, that is,
from reading the paper to implement a working code (excluding edge-cases).

I wanted the code to be fast, so I chose C++ as language, but
with a Python wrapper for the user; by the way,
I have never implemented a wrapper, and my C++ skills are very rusty nowadays.

Before diving into the project,
some basic concepts of agentic ai are:
  - Agents (of course);
  - Instructions: a prompt with the role of each agent;
  - Skills: baggages of tools and data the agents can use without exhausting the context;
  - Tools: deterministic (non-stochastic) methods the agents can employ;
  - MCP servers: tools the agents can connect to;
  - Hooks: they endow a deterministic behavior to agents by intercepting them at key points.



## References

{% bibliography --cited %}
