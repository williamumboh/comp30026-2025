# Nondeterministic Finite Automata

The finite automata we have seen so far are *deterministic* finite
automata (DFA): given the current state and the next input symbol, there
is exactly one state that a finite automaton can move to. In this
section, we introduce *nondeterministic* finite automata (NFA), which
have two additional features:

1.  An NFA's state transition function takes as input the current state
    and the next input symbol, and is allowed to return, not just one
    state, but a subset of states.
2.  An NFA is also allowed to make transitions without consuming the
    next input symbol. These are called $ε$-transitions as it is as if
    the next input symbol is the empty string.

We will introduce these two features one by one.

# Transition to more than one state

::: {prf:definition} NFA without epsilon-transitiions

A *nondeterministic* finite automaton (NFA) $N$ consists of:

1.  Finite input alphabet $Σ$
2.  Set of states $Q$
3.  Initial state $q₀$
4.  State transition function $\delta : Q × \Sigma → 2^Q$ [^1]
5.  Subset of accepting states $F$

:::

::: {important}

A key difference with DFAs is that at any time, the NFA can be in
several states at once. Thus, its current "state" is actually a set of
states.

:::

Let $N$ be an NFA without $ε$-transitions. Consider a string
$w = v_1 v_2 \cdots v_n$ where each $v_i$ is an input symbol from
$\Sigma$. The NFA processes $w$ as follows. We use $S_i$ to denote the
set of states the NFA is in after processing $v_1\cdots v_i$.

1.  Start at the start state: $S_0 = \{q_0\}$.
2.  For each input symbol $v_i$ (starting from $v_1$): define $S_i$ to
    consist of states $r$ such that there exists $q \in S_{i-1}$ and
    $r = \delta(q,v_i)$.
3.  After all input symbols have been processed, it accepts the string
    if $S_n$ contains an accepting state, and rejects it otherwise.

We can also view the NFA as having several possible state transition
sequences of the form

::: {math}
q_0 \xrightarrow{v_1} r_1 \xrightarrow{v_2} r_2 \xrightarrow{v_3} \ldots \xrightarrow{v_n} r_n
:::

where $r_i \in \delta(r_{i-1}, v_i)$ for each $i$ ranging from $1$ to
$n$. Note that since $\delta(r_{i-1},v_i)$ can be a set of more than one
state, there can be several possible state transition sequences, and so
there may be several resulting states. We denote by $Q(w)$ the set of
resulting states.

::: {prf:definition} NFA acceptance
:label: NFA-accept-def

An NFA $N$ *accepts* a string $w$ iff $Q(w)$ contains an accepting state and *rejects* $w$ if $q(w)$ is not an accepting state. The set of strings accepted by the NFA is denoted by $L(N)$. We say that $N$ *recognizes* a language $A$ iff the set of strings accepted by $N$ is exactly $A$, i.e. $A = L(N)$.

:::

The high level interpretation is as follows: Suppose the NFA $N$ is at
state $q$, the next input symbol is $0$, and $\delta(q,0) = S$ for some
set of states $S$. Then, $N$ **simultaneously** transitions to all of
them, in parallel. Each of these parallel "threads" then process the
input independently of each other. Metaphorically, the universe is
"split" into $k$ parallel universes, where $k$ is the size of $S$. The
NFA accepts the string iff it ends up in an accepting state in one of
the universes.

## Computation tree

Similar to DFAs, we can define a computation path to be a sequence of
state transitions followed by $N$ while processing $w$. We say that the
path is *accepting* path if it ends in an accepting state and
*rejecting* otherwise. Thus, an NFA accepts a string $w$ iff there
exists an accepting computation path.

Observe that with DFAs, we have a single computation path for each input
string. NFAs on the other hand, can have multiple computation paths.
Consider the following NFA.

::::::: {image} ./nfa.png
:width: 300px
:::::::

On input 110, it has 3 possible state transition sequences, each
corresponding to a computation path.

::: {math}
q_0 \xrightarrow{1} q_1 \xrightarrow{1} q_3 \xrightarrow{0} q_3
:::

::: {math}
q_0 \xrightarrow{1} q_0 \xrightarrow{1} q_1 \xrightarrow{0} q_2
:::

::: {math}
q_0 \xrightarrow{1} q_0 \xrightarrow{1} q_0 \xrightarrow{0} q_3
:::

We can in turn represent these paths using a tree, which we call a
*computation tree*.

:::::: {image} ./nfa-tree.png
:width: 200px
::::::

In this example, the string 110 is accepted by the NFA as there is one
computation path (the one in the middle) ending in an accepting state.

# epsilon-transitions

An NFA with $ε$-transitions is one where instead of consuming the next
input symbol and processing it using the transition function, the NFA
can instead choose to follow an $ε$-transition which is a transition
that does not consume the next input symbol.

::: {prf:definition} Nondeterministic finite automaton (NFA)

A *nondeterministic* finite automaton (NFA) $N$ consists of:

1.  Finite input alphabet $Σ$
2.  Set of states $Q$
3.  Initial state $q₀$
4.  State transition function
    $\delta : Q × (\Sigma \cup \{\epsilon\}) → 2^Q$
5.  Subset of accepting states $F$

:::

Thus, in the state transition diagram, there may be edges that are
labeled with $\epsilon$ instead of a symbol from $\Sigma$. Suppose the
NFA $N$ is at state $q$, the next input symbol is $0$, and
$\delta(q,\epsilon) = S$ for some set of states $S$. Then, $N$ can
immediately transition simultaneously to the states in $S$.

Let $N$ be an NFA. Consider a string $w = v_1 v_2 \cdots v_n$ where each
$v_i \in \Sigma \cup \{\epsilon\}$. A state $q$ is a *resulting state*
of $N$ after processing $w$ iff there exists a sequence of state
transitions:

::: {math}
r_0 \xrightarrow{v_1} r_1 \xrightarrow{v_2} r_2 \xrightarrow{v_3} \ldots \xrightarrow{v_n} r_n
:::

where $r_0 = q_0$, and $r_i \in \delta(r_{i-1}, v_i)$. Note that there
may be several resulting states. We denote by $Q(w)$ the set of
resulting states.

The definition of acceptance is the same as for NFAs without
$ε$-transitions (@NFA-accept-def).

## Computation tree

Consider the following NFA.

::::::: {image} ./nfa-eps.png
:width: 300px
:::::::

On input 10, it has 3 possible state transition sequences, each again
corresponding to a computation path.

::: {math}
q_0 \xrightarrow{\epsilon} q_1 \xrightarrow{1} q_3 \xrightarrow{0} q_3
:::

::: {math}
q_0 \xrightarrow{1} q_0 \xrightarrow{\epsilon} q_1 \xrightarrow{0} q_2
:::

::: {math}
q_0 \xrightarrow{1} q_0 \xrightarrow{0} q_3
:::

::: {note}

Observe that due to $ε$-transitions, the computation paths can be of
different lengths. Also observe that for each computation path, if we
concatenate the input symbols along the path, we get the input string.
For example, on the first path, we get $\epsilon 10 = 10$.

:::

Here is its computation tree. Observe that for every root-to-leaf path,
concatenating the symbols on the path gives us 10.

:::::: {image} ./nfa-eps-tree.png
:width: 200px
::::::

In this example, the string 10 is accepted by the NFA as there is one
computation path (the one in the middle) ending in an accepting state.

[^1]: See @app-set for details on the notation $2^Q$.
