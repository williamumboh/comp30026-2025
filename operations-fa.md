# Operations on Languages

When designing an algorithm for a problem, it is often helpful to
decompose the problem into simpler sub-problems. We can do something
similar when designing algorithms for languages as well. In particular,
we will now consider operations on languages that allow us to combine
languages, similar to how propositional connectives
($\neg, \vee, \wedge$) let us combine propositions.

These operations are:

1.  complement
2.  intersection
3.  union
4.  concatenation
5.  Kleene star (aka Kleene closure)

The last 3 are called *regular operations*.

Given an alphabet $\Sigma$, we define $\Sigma^*$ to be the set of all
strings. For example, when $\Sigma = \{0,1\}$, then
$\Sigma^* = \{\epsilon, 0, 1, 00, 01, 10, 11, \ldots \}$. A language $L$
over the alphabet $\Sigma$ is a subset of $\Sigma^*$.

::: {prf:definition} Complement of language $L$

The *complement* of $L$ (denoted by $L^c$) is defined to be the set of
strings $z$ not in $L$, i.e. $z \in L^c$ iff $z \notin L$. Equivalently,
$L^c = \Sigma^* \setminus L$.

:::

::: {prf:definition} Intersection of languages $A$ and $B$

The *intersection* of $A$ and $B$ is defined to be the set of strings
$z$ such that $z \in A$ **and** $z \in B$, i.e. $A \cap B$.

:::

::: {prf:definition} Union of languages $A$ and $B$

The *union* of $A$ and $B$ is defined to be the set of strings $z$ such
that $z \in A$ **or** $z \in B$, i.e $A \cup B$.

:::

# Concatenation

Given two strings $x$ and $y$, we denote their concatenation by $xy$.

::: {prf:definition} Concatenation of languages $A$ and $B$

The *concatenation* of $A$ and $B$ (denoted by $A \circ B$), is defined
to be the set of strings $z$ such that there exists $x \in A$ and
$y \in B$ such that $z = xy$.

:::

::: {prf:example}

Consider the languages $A = \{00, 0110\}$ and
$B = \{0, 10, 110, 1110\}$. Then,

$$A \circ B = \{000, 0010, 00110, 001110, 01100, 011010, 0110110, 01101110\}$$

:::

::: {prf:definition} $L^k$

We use the notation $L^k$ to mean $L$ concatenated with itself $k$
times, i.e.
$L^k = \underbrace{L \circ L \circ \cdots \circ L}_{k\ \text{times}}$.

:::

::: {tip}

To get a good feel for mathematical definitions of a set (such as a
language obtained by concatenating two languages), it is helpful to
attempt to derive alternative equivalent definitions.

:::

## Alternative Definition Attempt 1

Let's first try to design an algorithm that constructs $A \circ B$. To
be more precise, we want an algorithm that prints the strings in
$A \circ B$, one by one.

If $A$ and $B$ are finite, it is easy to do it using the following
algorithm:

::: {code}

for x in A:
  for y in B:
    print x+y

:::

Note that the pseudocode uses the Python notation for string
concatenation.

What about when $A \circ B$ is infinite (e.g. if $B = \Sigma^*$)? In
this case, we say that the algorithm *succeeds* if every string in
$A \circ B$ is *eventually* printed by the algorithm.

Taking a closer look, we realize that when $B$ is finite, the algorithm
succeeds even when $A$ is infinite. Unfortunately, it is unclear at this
point how to modify the algorithm to be successful if both $A$ and $B$
are infinite. We leave this as an advanced exercise for now.

::: {exercise .dropdown} Advanced exercise

Given a language $L$, an algorithm $M$ is said to *enumerate* $L$ if
every string in $L$ is eventually printed by $M$.

Suppose that there are algorithms $M_A$ and $M_B$ that enumerate $A$ and
$B$, respectively. Design an algorithm that enumerates $A \circ B$. Your
algorithm should work even when both $A$ and $B$ are infinite.

:::

## Alternative Definition Attempt 2

Let's instead try to build on cases of $A \circ B$ that are easier to
understand.

Suppose $A$ consists of a single string $a$, i.e. $A = \{a\}$. Then,
$A \circ B$ is easy to describe: it is the set of strings that are
obtained by appending $a$ with a string in $B$.

We can then define $A \circ B$ iteratively as follows: $A \circ B$ is
the union of the sets $\{a\} \circ B$ over all $a \in A$.[^1]

::: {exercise}

Argue that

- $L \circ \emptyset = \emptyset$
- $L \circ \{\epsilon\} = L$
- $A \circ B \supseteq A$ if the empty string $\epsilon \in B$

:::

# Kleene star

::: {prf:definition} Kleene star of language $L$

The *Kleene star* (also called *Kleene closure*) of $L$ is defined to be
the set of strings consisting of:

- the empty string and
- $L^k$ for every positive integer $k$

:::

::: {important}

For every language $L$ (including the empty set), the empty string
$\epsilon$ is always in $L^*$.

:::

::: {caution}

The definition of Kleene star is quite tricky. Spend some time trying
out examples and tinkering with the definition.

:::

Consider the following algorithm that, roughly speaking, generates
$L^*$, given a subroutine concat" that computes the concatenation of two
languages:

::: {code}

k = 0
initialise Lstar to contain only the empty string
while(true):
  for j = 1 up to k:
    Lstar = concat(Lstar, L)

:::

::: {prf:example}

Consider the languages $A = \{01\}$ and $B = \{01, 10\}$. Then,

$$A^* = \{\epsilon, 01, 0101, 010101, 01010101,\ldots\}$$

$$B^* = \{\epsilon, 01, 0101, 0110, 1001, 1010,\ldots\}$$

:::

::: {warning}

$\{\epsilon\}^* = \{\epsilon\}$ and also $\emptyset^* = \{\epsilon\}$.

:::

## Closure

::: {prf:theorem}

Regular languages are closed under complement, intersection, union,
concatenation, and Kleene star:

- if $L$ is regular, then $L^c$ and $L^*$ is also regular
- if $A$ and $B$ are regular, then so are $A \cap B$, $A \cup B$ and
  $A \circ B$.

Moreover,

- given finite automaton for $L$, we can compute a finite automaton for
  $L^c$ and $L^*$
- given finite automata for $A$ and $B$, we can compute a finite
  automata for $A \cap B$, $A \cup B$ and $A \circ B$.

:::

The proof follows by taking the finite automata for $L$, $A$ and $B$,
and modifying them appropriately.

For complement, this is easy: let $M$ be the finite automaton for $L$.
Then, the finite automaton $N$ for $L^c$ is obtained by swapping accept
and reject states in $M$. The others seem difficult. We will need the
power of non-determinism to deal with them.

[^1]: If you are comfortable with set notation,
    $A \circ B = \bigcup_{a \in A} (\{a\} \circ B)$.
