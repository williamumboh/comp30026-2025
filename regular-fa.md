# Operations on Languages

When designing an algorithm for a problem, it is often helpful to
decompose the problem into simpler sub-problems. We can do something
similar when designing algorithms for languages as well. In particular,
we will now consider operations on languages that allow us to combine
languages, similar to how propositional connectives
($\neg, \vee, \wedge$) let us combine propositions.

These operations are:

- Complement
- Intersection
- Union
- Concatenation
- Kleene star (aka Kleene closure)

The last 3 are called *regular operations*.

Given an alphabet $\Sigma$, we define $\Sigma^*$ to be the set of all
strings. For example, when $\Sigma = \{0,1\}$, then
$\Sigma^* = \{\epsilon, 0, 1, 00, 01, 10, 11, \ldots \}$. A language $L$
over the alphabet $\Sigma$ is a subset of $\Sigma^*$.

Given a set $S$, we use the notation $x \in S$ to mean $x$ is in $S$ and
$x \notin S$ to mean $x$ is not in $S$.

::: {prf:definition} Complement of language $L$

The *complement* of $L$, denoted by $L^c$, is defined to be the set of
strings $z$ not in $L$, i.e. $z \in L^c$ iff $z \notin L$. Equivalently,
$L^c = \Sigma^* \setminus L$.

:::

::: {prf:definition} Intersection of languages $L_1$ and $L_2$

The *intersection* of $L_1$ and $L_2$ is defined to be the set of
strings $z$ such that in $z \in L_1$ **and** $z \in L_2$,
i.e.\~$L_1 \cap L_2$.

:::

::: {prf:definition} Union of languages $L_1$ and $L_2$

The *union* of $L_1$ and $L_2$ is defined to be the set of strings $z$
such that $z \in L_1$ **or** $z \in L_2$, i.e.\~$L_1 \cup L_2$.

:::

Given two strings $x$ and $y$, we denote their concatenation by $xy$.

::: {prf:definition} Concatenation of languages $L_1$ and $L_2$

The *concatenation* of $L_1$ and $L_2$, denoted by $L_1 \circ L_2$, is
defined to be the set of strings $z$ such that there exists $x \in L_1$
and $y \in L_2$ such that $z = xy$.

:::

:::::: {note}

It can be helpful to think of the following equivalent pseudocode to
generate $L_1 \circ L_2$:

::: {python}

L = {} for x in L~1~: for y in L~2~: add xy to L

:::

::::::

::: {prf:definition} Kleene star of language $L$

The *Kleene star* (also called *Kleene closure*) of $L$ is defined to be
the set of strings $z$ such that there exists $x \in L_1$ and
$y \in L_2$ such that $z = xy$.

:::
