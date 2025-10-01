# Preface

The last few weeks has given you a good foundation in rigorous
reasoning. We have also seen glimpses of computability when we discussed
the resolution and unification method. We are now going to apply that
foundation towards understanding computation, and equip you with the
skills and knowledge to study not just the computability issues arising
from logic but also questions such as:

1.  Can LLMs do everything?[^1]
2.  How do we design secure cryptographic protocols?
3.  How do we process massive data streams such as social media posts?

These questions are really about computational models and their power.
When designing cryptographic protocols, we need to precisely define the
computational model of the adversary that we want to be secure against.
For example, does the adversary have access to quantum computers? When
dealing with massive data streams, the traditional computational model
is insufficient as it assumes that we can store the entire input. Thus,
the [streaming](https://en.wikipedia.org/wiki/Streaming_algorithm) model
of computation was developed to capture this setting. There are also
plenty of other questions about economic markets, biology, physics, that
have been fruitfully studied using the ["computational
lens"](https://www.ias.edu/ideas/2015/computational-lens), by framing
them in terms of computational models.

In the rest of this subject, we will delve into the field of theoretical
computer science and learn how to *rigorously reason* about the nature
and limits of computation. In a sense, we will learn the "laws of
computation", analogous to "laws of physics" in physics. In particular,
we are going to build towards understanding the limits of computation,
i.e. impossiblity results. Just as the theory of relativity says that
nothing can go faster than the speed of light, we will learn that there
are some problems that cannot be solved, no matter the amount of
resources you throw at it.

[^1]: Check out this workshop on the power and limits of transformers
    <https://simons.berkeley.edu/workshops/transformers-computational-model>
