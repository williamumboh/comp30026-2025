# Undecidabilty and Unrecognizability by Diagonalization

As mentioned earlier, it turns out that $A_{TM}$ is undecidable so there
are undecidable problems. Worse than that, there actually are problems
that are not even recognizable!

We begin by showing that there exists a language that is not
recognizable. The idea is to show that there are more languages than
there are Turing machines and thus there is a language that is not
recognized by any Turing machine. But the set of languages is infinite
and so is the set of Turing machines, so first we need a way to compare
the sizes of infinite sets.

To compare the sizes of finite sets, we can simply count the number of
elements in both sets and then compare the counts. We cannot do this for
infinite sets. However, observe that another way to show that two finite
sets $A$ and $B$ have the same size is by showing that we can match each
element of $A$ to a unique element of $B$. If the sets have different
sizes, there is no such matching.

::: {prf:example}

Consider the set $A = \{1,2,3\}$ and $B = \{a,b,c\}$. Then, we can match
them as follows:

- 1 $\rightarrow$ a
- 2 $\rightarrow$ b
- 3 $\rightarrow$ c

:::

Fortunately, this idea of matching extends to infinite sets. Formally,
the matching is given by a function $f : A \rightarrow B$ that maps
elements in $A$ to elements in $B$. The function $f$ is a *1-1
correspondence* if it satisfies:

1.  (Injectivity) No two elements in $A$ get mapped to the same element
    in $B$. Formally, for every $x,y \in A$, we have $f(x) \neq f(y)$.
2.  (Surjectivity) Every element in $B$ is mapped to by some element in
    $A$. Formally, for every $z \in B$, there is a $x \in A$ such that
    $f(x) = z$.

We say that two sets $A$ and $B$ have the same size iff there is a 1-1
correspondence $f: A \rightarrow B$. Moreover, if set $A$ has the same
size as the set of natural numbers $\mathcal{N} = \{1, 2, 3, \ldots\}$,
then we say that $A$ is *countable*.

## Countable sets

::: {prf:theorem}

The set of Boolean strings $\{0,1\}^*$ is countable.

:::

::: {prf:proof}

Sort the set of Boolean strings in string order: $\epsilon$, 0, 1, 00,
01, 10, 11, 000, \ldots Define the 1-1 correspondence
$f : \mathcal{N} \rightarrow \{0,1\}^*$ as follows: $f(i)$ is the $i$-th
string in string order.

:::

::: {prf:theorem}

The set of Turing machines is countable.

:::

::: {prf:proof}

Fix an encoding of Turing machines into strings. We can then sort the
Turing machines according to string order of their encodings and define
the 1-1 correspondence $f$ as follows: $f(i)$ is the $i$-th Turing
machine in the above order.

:::

## Diagonalization for Reals

To show that there are more languages than Turing machines, we will use
a technique called *diagonalization*. First, we present the technique in
a simpler setting and use it to show that the set of real numbers
$\mathcal{R}$ is uncountable. Thus the set of reals is larger than the
set of natural numbers.

::: {prf:theorem}

$\mathcal{R}$ is uncountable.

:::

::::: {prf:proof}

We will show that every function
$f : \mathcal{N} \rightarrow \mathcal{R}$ misses some real number, i.e.
is not surjective: there exists $x \in \mathcal{R}$ such that for every
$n \in \mathcal{N}$, $f(n) \neq x$.

Consider a function $f : \mathcal{N} \rightarrow \mathcal{R}$. Define
the real number $x$ as follows: for every $i \geq 1$, $x$ differs from
$f(i)$ in the $i$-th decimal place.

::: {figure} ./diagonalization-reals.png

Illustration of construction of $x$ given $f$. It is called
"diagonalization" because $x$ is defined by the diagonal of the table.

:::

This is a valid definition of a real number. By definition,
$f(n) \neq x$ for every $n \geq 1$ and so $f$ is not a 1-1
correspondence.

Since this argument holds for every function
$f : \mathcal{N} \rightarrow \mathcal{R}$, we get that there is no 1-1
correspondence between $\mathcal{N}$ and $\mathcal{R}$ and so
$\mathcal{R}$ is uncountable.

:::::

## Diagonalization for Languages

::: {prf:theorem}

There exists a language $L_D$ that is not recognizable.

:::

::::: {prf:proof}

Fix an encoding of Turing machines into strings. Let $M_i$ be the $i$-th
Turing machine in string order of its encoding, and $w_i$ be the $i$-th
Boolean string in string order.

Define the language $L_D = \{w_i \mid w_i \notin L(M_i)\}$, i.e. $L_D$
disagrees with $L(M_i)$ on $w_i$ and is not recognized by $M_i$.

::: {figure} ./diagonalization-languages.png

Illustration of construction of $L_D$. In this diagram, "in" means the
string is in the language of the corresponding row, and "out" means it
is not in the language.

:::

This is a valid definition of a language. Since every Turing machine has
an encoding into a finite-length string, every Turing machine is $M_i$
for some $i$. Since $L_D$ is not recognized by any $M_i$, it is not
recognizable.

:::::

## Undecidability of TM Acceptance

We can now show that $A_{TM}$ is undecidable by showing that a decider
for $A_{TM}$ gives a decider for $L_D$ which is impossible since $L_D$
is not even recognizable.

::: {prf:theorem}

$A_{TM}$ is undecidable.

:::

::: {prf:proof}

We first show that if there exists a decider $U$ for $A_{TM}$, then we
can construct a TM $D$ that decides $L_D$.

    On input w:
      Compute i such that w = w_i
      Run U on <M_i, w>
      Accept if U rejects
      Reject if U accepts

Since $U$ is a decider, it always accepts or rejects, it never runs
forever. Thus, $D$ also always halts. Moreover, for every string $w_i$,
$D$ accepts it if and only if $M_i$ does not accept $w_i$. Therefore,
$L(D) = L_D$ and so $D$ decides $L_D$.

Since $L_D$ is not recognizable and thus not decidable, we conclude that
$A_{TM}$ is not decidable.

:::
