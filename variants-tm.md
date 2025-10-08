# Church-Turing Thesis

In the early 1900s, mathematicians wanted to formalize what it means for
a computational problem to "effectively computable". One of the
computational problems they were interested in is determining whether a
formula in first-order logic is satisfiable. Besides the Turing machine,
other models include $λ$-calculus, rewriting systems, and others.
Although the models look very different, they all turn out to be
equivalent to one another in terms of computational power.

This led to the formulation of the Church-Turing Thesis which states
that every reasonable model of unrestricted computation is equivalent to
the Turing machine. In particular, it means that a problem can be solved
using \<insert favorite programming language\> if and only if it can be
solved by a Turing machine.

::: {note} Thesis vs Theorem

The Church-Turing Thesis is not a formal statement due to the phrase
"every reasonable model of unrestricted computation" and thus cannot be
proved or disproved. Therefore, it is not a theorem. However, there has
been a mountain evidence agreeing with the Thesis.

:::

In the next two sections, we will see that while we can define Turing
machines with extra capabilities, they are no more powerful than the
standard Turing machine. This provides some justification that Turing
machines is a model of general-purpose computation.

## Multi-Tape Turing machines

::: {prf:definition} Multi-Tape Turing Machine

A *multi-tape* Turing machine $M$ is one with an additional fixed number
$k$ of tapes and tape heads. The additional tapes are called *work
tapes*. Initially, all work tapes are blank and their tape heads are at
the start of the work tapes. The transition function of $M$ depends on
the current state and the symbols under all tape heads, and in a single
step, writes to every tape and moves every tape head simultaneously.

:::

::: {prf:theorem}

A language $A$ is Turing-recognizable if and only if it is recognized by
a multi-tape Turing machine.

:::

::: {prf:proof} Proof Sketch

If $A$ is Turing-recognizable, then there is a multi-tape Turing machine
that recognizes it. This is because we can simulate the usual
single-tape TM with a TM that has multiple tapes.

Suppose that there is a multi-tape TM $M$ that recognizes $A$. We are
now going to construct a single-tape TM $S$ that simulates $M$ on every
input. The idea is to simulate multiple tapes with a single tape.

At a high level, at each point in time, the contents of the tapes of $M$
are concatenated together on the tape of $S$, separated by a special
symbol '#'. Moreover, to keep track of the tape heads, $S$ uses a
"marked" version of each tape symbol of $M$. In particular, for each
tape symbol of $M$, there is a marked version $\dot x$.

To simulate a transition of $M$, the TM $S$ scans the tape to determine
the symbols under the tape heads of $M$, and then scans the tape to
update it (the contents of the tape as well as the tape head locations)
according to the transition function of $M$. The TM $S$ shifts the tape
contents to create more room, if needed.

The TM $S$ halts and accepts/rejects when $M$ does.

:::

## Nondeterministic Turing Machines

A *nondeterministic* Turing machine (NTM) is one where the transition
function is allowed to return more than one outcome and as a result, a
single configuration can yield more than one configuration. Thus, the
NTM can be in multiple configurations at once.

(sec-ntm-process)= Let $N$ be a NTM and $w$ be an input string. We use
$\mathcal{C}$ to denote the set of configurations it is in currently.
$N$ processes $w$ as follows:

1.  Initially, $\mathcal{C}$ consists of the start configuration $q_0w$
2.  For each configuration $C$ in $\mathcal{C}$ that is neither
    $q_{acc}$ or $q_{rej}$, replace $C$ with the set of configurations
    that $C$ yields. More precisely, we replace the configuration that
    entered $C$ earliest.
3.  The NTM accepts the string if $\mathcal{C}$ eventually contains
    $q_{acc}$

Similar to NFAs, we can visualise the execution of the NTM as a
computation tree where each node of the tree is a configuration. The
root node of the tree is the initial configuration, and there is an edge
from a node representing configuration $C$ to a node representing
configuration $C'$ if and only if $C'$ is one of the configurations that
$C$ yields.

::: {prf:theorem}

A language $A$ is Turing-recognizable if and only if it is recognized by
a nondeterministic Turing machine.

:::

::: {prf:proof} Proof Sketch

If $A$ is Turing-recognizable, then there is a nondeterministic Turing
machine that recognizes it since a deterministic TM can be simulated by
a nondeterministic one. (In fact, the definition of nondeterministic TMs
also capture deterministic TMs.)

Now, we show that if a nondeterministic TM $N$ recognizes $A$, then
there is a TM $M$ that recognizes $A$ as well. We will do this by
creating a TM $M$ that simulates $N$'s computation tree.

At a high level, $M$ keeps track of the set of configurations
$\mathcal{C}$ that $N$ is in at any point in time. This can be done by
concatenating the configurations using the [notation](#sec-tm-config) of
the previous section, separated by a special symbol \#.

To apply the transition function, it scans through the list of
configurations from left to right, and applies the transition function
to each configuration. Whenever a new configuration needs to be added,
it adds it to the end.

:::

## Lecture Prep
