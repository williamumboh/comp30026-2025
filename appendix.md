---
numbering:
  false
---

# Mathematical Background (Notation and terms)

(app-set)=

## Set notation

In the following, let $S$ and $T$ be sets.

- $\emptyset$ is the empty set
- $x \in S$ means $x$ is in $S$; and $x \notin S$ means $x$ is not in
  $S$.
- $S \setminus T$ is the set of elements in $S$ but not in $T$
- $S \cap T$ is the set of elements in $S$ **and** $T$, and is called
  the *intersection* of $S$ and $T$
- $S \cup T$ is the set of elements in $S$ **or** $T$, and is called the
  *union* of $S$ and $T$
- $S \subseteq S$ means $S$ is a subset of $T$, i.e. every element of
  $S$ is an element of $T$
- $S \supseteq T$ means $S$ is a superset of $T$, i.e. every element of
  $S$ is an element of $T$
- $2^S$ is the set containing every subset of $S$ (including $\emptyset$
  and $S$), and is called the *powerset* of $S$. Sometimes, it is
  denoted by $\mathcal{P}(S)$.
  - For example, when $S$ is the set $\{a,b\}$, then the elements of
    $2^S$ are $\emptyset$, $\{a\}$, $\{b\}$ and $\{a,b\}$.
- $\{x \in S \mid P(x)\}$ means the set of elements in $S$ that
  satisfies the predicate $P$. This is called the *set-builder
  notation*.
  - For example, if $S$ is a set of numbers, then
    $\{x \in S \mid x \text{ is even}\}$ is the set of even numbers in
    $S$.
  - Sometimes, : is used in place of $\mid$. For example,
    $\{x \in S : x \text{ is even}\}$

For a more in-depth and beginner-friendly discussion see [Guide to
Elements and
Subsets](https://web.stanford.edu/class/archive/cs/cs103/cs103.1246/resources/Guide%20to%20Elements%20and%20Subsets.pdf).

## Theorems and Lemmas

Usually, a *theorem* is an important result that we want to show, and a
*lemma* is a smaller result that is not necessarily interesting by
itself, but is useful to bigger results. Often, a theorem is proved by
proving a sequence of smaller lemmas. In a way, lemmas are similar to
the role of subroutines in programming.

See also [CS103 Guide to
Proofs](https://web.stanford.edu/class/cs103/guide_to_proofs_on_discrete_structures#writing-longer-proofs)
and Section 1.3.2 of [Introduction to Theoretical Computer
Science](https://introtcs.org/public/lec_00_1_math_background.html).
