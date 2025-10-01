---
author: William Umboh
date: "\\[2025-08-21 Thu 13:58\\]"
title: Preamble
---

The last few weeks has given you a good foundation in rigorous
reasoning. We are now going to apply that foundation towards
understanding computation, and equip you with the skills and knowledge
to study questions such as:

- Can LLMs do everything?
- How do we design secure systems in banking, communications, etc?

These questions are actually about computational models and their
power.[^1] For example, "can LLMs solve everything" can be refined to
"are there computational problems that LLMs cannot solve?" Given that
LLMs are resource-hungry, we are particularly interested in knowing what
problems LLMs can *efficiently* solve.[^2] This is also true in
cryptography where the goal is to design *computationally secure*
systems. For example, an instant messaging platform is computationally
secure if users can efficiently communicate with each other but
attackers cannot efficiently decrypt the communications. There is also a
strong desire to develop post-quantum cryptography where we want to be
secure against attackers with quantum computers.

In the rest of this subject, we will delve into the field of theoretical
computer science and learn how to *rigorously reason* about the nature
and limits of computation. In a sense, we will learn the "laws of
computation", analogous to "laws of physics" in physics.

In particular, we are going to build towards understanding the limits of
computation, i.e. impossiblity results. Just as the theory of relativity
says that nothing can go faster than the speed of light, we will learn
that there are some problems that cannot be solved, no matter the amount
of resources you throw at it.

[^1]: There are also plenty of questions and many others, can be
    fruitfully studied using the ["computational
    lens"](https://www.ias.edu/ideas/2015/computational-lens), by
    framing them in terms of computational models. See for example
    <https://www.ias.edu/ideas/2015/computational-lens>

[^2]: Check out this theoretical computer science workshop on the power
    and limits of transformers
    <https://simons.berkeley.edu/workshops/transformers-computational-model>
