# Modeling Computation

## What is computation?

At its most basic form, "to compute" means to take an input, process it
in a **finite** number of steps, and produce an output:

Input -\> Algorithm -\> Output

## Computational problem

A *computational problem* is a set of input-output pairs, specifying the
correct output for each input. In particular, a computational problem
can be represented as a predicate $P(x,y)$ that indicates, for every
input $x$ and output $y$, whether $y$ is the correct output for input
$y$.

Let $A$ be an algorithm and $A(x)$ be its output on input $x$.
Informally, we say that algorithm $A$ *solves* problem $P$ if for
**every** input $x$, $A(x)$ is the correct output to input $x$. We can
express this formally using predicate logic: $∀x~P(x,A(x))$.

## Model of computation

We are interested not just whether a problem can be solved in finite
number of steps, but whether it can be solved efficiently. But what does
"efficient" mean? Algorithms use resources which can range from time,
space (memory), randomness, quantum entanglement, input access, and many
others.[^1] Thus, "efficient" means that there is a constraint on the
*type* and *quantity* of resources that the algorithm can use.

Informally, a *model of computation* defines constraints on resources. A
problem is *solvable* in a model of computation there exists an
algorithm that solves the problem and obeys the resource constraints on
**every** input. For example, sorting is solvable in polynomial-time as
there are algorithms (e.g. mergesort) that correctly sorts every input
array in time polynomial in the input size. On the other hand, we only
know how to solve integer factoring in polynomial time using quantum
entanglement.

Note that to show that $P$ is solvable, it suffices to show that there
is some algorithm that satisfies the desired properties. But showing
that $P$ is not solvable requires us to argue that **every** algorithm
either does not correctly solve the problem on every input, or does not
satisfy the resource constraint on every input.

We are interested in studying the power and limitations of various
models of computation:

1.  Are there problems that cannot be solved even with unlimited
    resources?[^2]
2.  What problems can and cannot be solved under various resource
    constraints?
3.  Tradeoffs between different resources[^3]

(str-lang-dec)=

## Strings, languages and decision problems

In the rest of the semester, we are going to consider problems in which
the input is some string (most commonly, bit strings; more to come
later), and the goal is to correctly output YES/NO, i.e. to correctly
decide if some fixed (fixed means independent of the input string)
predicate is satisfied by the input string or not. These are called
*decision* problems.

:::{note} Examples of decision problems

- Is the length of the input string even?
- Does the input string have more 1s than 0s?
- Is the input formula satisfiable?

:::

:::{important} Important - Empty strings

We allow the input to be empty, i.e. the input string is the empty
string. We use $\epsilon$ to denote the empty string.

:::

We can compactly represent the problem by the set of strings for which
the correct answer is YES. We call the set of strings a *language*. In
particular, a language $L$ is a set of strings that satisfy some
predicate.

Let $A$ be an algorithm. We say that $A$ *accepts* a string $x$ if
$A(x) = \text{YES}$ and *rejects* otherwise. The language of strings
accepted by $A$ is denoted by $L(A)$. We say that $A$ *recognizes* a
language $L$ if $L(A) = L$.

::: {aside}

While it may seem restrictive to only consider decision problems, every
other problem can be "reduced" to a decision version. For example, the
problem of adding two numbers $x$ and $y$ has a corresponding decision
version: Is the $i$-th bit of $x+y$ equal to $1$? One can then solve the
decision version several times to compute $x+y$.

:::

# Add figure for computation

[^1]: Even [time
    travel](https://www.scottaaronson.com/papers/ctchalt.pdf) is a
    [computational
    resource](https://www.youtube.com/watch?v=fUGjv44_X4Q)!

[^2]: Note that the algorithm is still required to output in finite
    steps.

[^3]: Intuitively, this is similar to an "exchange rate" between
    different resources.
