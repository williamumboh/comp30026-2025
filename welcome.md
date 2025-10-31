# Welcome!

Welcome to the website for the second half of COMP30026 Models of
Computation!

This website contains the companion notes to my lectures and other
resources. There is a lot of dense information, putting all of them up
on the slides can be visually overwhelming. Thus, I have designed my
slides to only contain information pertinent to the focus of the
lecture. You are welcome to open up the website on your laptop or phone
(it's actually mobile-friendly) to use as a reference during the
lecture.

::: {aside}

Tip: You can hover over a linked definition, lemma or theorem and have
it popup like [this](./hover-link-demo.mov).

:::

::: {caution}

The lecture notes and the lectures are meant to complement each other. Certain information is best presented in text form and others are best presented in a live delivery. Thus, there will be some information that is only presented in one of the two modes. Also note that some live demonstrations will not be captures on slides.

:::

# Lecture schedule

| Lecture | Topics Covered (Topic Numbers) |  |
|----|----|----|
| Week 6 Lecture 1 | Finite Automata ([2](./preamble/), [3](./prelim/), [4 - 4.1.5](./fa/)) | [Slides](./w6l1-slides.pdf) |
| Week 6 Lecture 2 | Finite Automata ([4.1.5-4.1.7](./intro-fa#finite-automata-in-general)), Operations on Languages ([4.2](./operations-fa)), Nondeterministic Finite Automata ([4.3](./nondet-fa)) | [Slides](./w6l2-slides.pdf) |
| Week 7 Lecture 1 (Cezary) | NFA Determinisation and Minimisation | [Cezary's Slides](https://canvas.lms.unimelb.edu.au/courses/215477/files/24683709?module_item_id=6927050) |
| Week 7 Lecture 2 (Cezary) | Regular Expressions | [Cezary's Slides](https://canvas.lms.unimelb.edu.au/courses/215477/files/24840376?module_item_id=6966082) |
| Week 8 Lecture 1 | @sec-nonreg | [Slides](./w8l1-slides.pdf) |
| Week 8 Lecture 2 | @sec-nonreg | [Slides](./w8l2-slides.pdf) |
| Week 9 Lecture 1 | [Finite Automata Summary](./fa-summary), @sec-CFL, @sec-CFG | [Sipser's slides](https://math.mit.edu/~sipser/18404/Lectures%20Fall%202020/Lecture%204%20final.pptx), [Sipser's lecture (up to 27:19)](https://youtu.be/m9eHViDPAJQ?si=LTxUewK89ifhyggV) (note Sipser does not cover Sections 5.1.4 and 5.1.5) |
| Week 9 Lecture 2 | @sec-PDA | [Cezary's Slides](https://canvas.lms.unimelb.edu.au/courses/215477/files/25005055?module_item_id=6985915) |
| Week 10 Lecture 1 | @sec-cfg-pda, @sec-cfl-summary, @sec-intro-tm | [Slides Part 1](https://canvas.lms.unimelb.edu.au/courses/215477/files/25195562?module_item_id=7013971), [Slides Part 2](https://canvas.lms.unimelb.edu.au/courses/215477/files/25104011?module_item_id=7001717) |
| Week 10 Lecture 2 | @sec-variants, @sec-alg-reg-cfl | [Slides](https://canvas.lms.unimelb.edu.au/courses/215477/files/25195591?module_item_id=7014019) |
| Week 11 Lecture 2 | @sec-diagonalization | [Slides](https://canvas.lms.unimelb.edu.au/courses/215477/files/25178539?module_item_id=7011087) |
| Week 11 Lecture 2 | @sec-self-reference | [Slides](https://canvas.lms.unimelb.edu.au/courses/215477/files/25183900?module_item_id=7011852) |
| Week 12 Lecture 1 | @sec-reductions, @sec-tm-closure | [Slides](https://canvas.lms.unimelb.edu.au/courses/215477/files/25244885?module_item_id=7018377) |
| Week 12 Lecture 2 | Wrap-Up |  |

# Resources

My materials and approach to teaching them are inspired by the following
amazing books and courses. They will also be the main resource for this
subject.

- [Introduction to Theoretical Computer
  Science](https://introtcs.org/public/index.html) by Boaz Barak
  (Harvard University)
- [CS 103: Mathematical Foundations of
  Computing](https://cs121.boazbarak.org) (Stanford University)
- [CS 251: Great Ideas in Theoretical Computer
  Science](https://s23.cs251.com/index.html) (CMU)
- [Notes on Models of
  Computation](http://jeffe.cs.illinois.edu/teaching/algorithms/#models)
  by Jeff Erickson[^1] (UIUC)

Introduction to the Theory of Computation by Michael Sipser is a great
textbook.

# Proofs

One of the biggest differences with the above resources is that writing
mathematical proofs about computation is not an intended learning
outcome for COMP30026. Instead, our goal is to teach the key ideas and
arguments behind these proofs. These can then be turned into
mathematical proofs once one has learned mathematical proofs (e.g.
[MAST20026 Real
Analysis](https://handbook.unimelb.edu.au/subjects/mast20026)). If you
are interested in self-learning how to write proofs, check out the
Resources section and lectures of CS 103 (this subject only assumes US
high school algebra as a
[prerequisite](https://web.stanford.edu/class/archive/cs/cs103/cs103.1246/prereqs).)
and Module 1 of CS 251.

# Tips for success

The
[Preface](https://introtcs.org/public/lec_00_0_preface.html#to-the-student)
of Introduction to Theoretical Computer Science has some great advice on
how to succeed.

There are two pieces of advice that I want to add to the above.

::: {tip}

**Examples** are essential to understanding definitions and theorems.

:::

Most of the time, the definitions and theorems are stated in very
general terms and thus can seem very abstract. I strongly encourage you
to think about some simple concrete examples of the objects in the
definitions and theorems. For example, later on, we will talk about sets
of bit strings; it is helpful to check your understanding by thinking
about whether the set contains the empty string? What is the smallest
string in the set? What is the smallest string that is *not* in the set?
Is the set empty? Does it contain every string?

::: {tip}

When you feel stuck, ask yourself: what is the **simplest** thing I do
not yet understand or understand how to do?

:::

For example, when working on problems, it is useful to try to find
simpler versions that you can tackle. We will provide scaffolding for
problems in Assignment 2 to show you how to break down problems into
simpler subproblems.

[^1]: Jeff also has a wonderful [Algorithms
    textbook](https://jeffe.cs.illinois.edu/teaching/algorithms/) that
    is free and available online.
