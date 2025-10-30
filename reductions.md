# Reductions

Previously, we used diagonalization to show undecidability. We now turn
to another technique called *reduction*.

::: {prf:definition} Reduction

A language $A$ *reduces to* a language $B$ (also written as $A \leq B$)
if and only if $B$ is decidable implies that $A$ is decidable. A
*reduction* is a construction of a decider for $A$ given a decider for
$B$.

:::

::: {figure} ./schematic-reduction.png

Schematic diagram of reductions

:::

Reductions can be used to obtain deciders for other problems. For
example, we obtained a decider for NFA Acceptance by reducing it to DFA
Acceptance, and a decider for DFA Equivalence by reducing it to DFA
Emptiness.

Reductions can also be used to prove other problems are undecidable.
This is because the statement "$B$ is decidable implies that $A$ is
decidable" is logically equivalent to the statement "$A$ is undecidable
implies that $B$ is undecidable". We have seen such a reduction in the
first proof of the undecidability of $A_{TM}$ in
@sec-tm-accept-reduction where we used a decider for $A_{TM}$ to
construct a decider for $L_D$.

Thus, by showing that $A \leq B$, we get that:

- if $B$ is decidable, then $A$ is decidable
- if $A$ is undecidable, then $B$ is undecidable

In summary, $A \leq B$ means that $A$ is as easy as (or no harder than)
$B$.

## Halting Problem

The Halting Problem is defined as follows: given an encoding of a Turing
machine $M$ and a string $w$, decide if $M$ halts on $w$. The
corresponding language $HALT_{TM}$ consists of $\langle M, w \rangle$
such that $M$ halts on $w$.

::: {prf:theorem}

$A_{TM} \leq HALT_{TM}$.

:::

::: {prf:proof}

To show that $A_{TM} \leq HALT_{TM}$, we need to construct a decider for
$A_{TM}$ given a decider for $HALT_{TM}$.

Let $H$ be a decider for $HALT_{TM}$. Consider the following TM $S$ for
$A_{TM}$.

    On input <M,w>:
      Run H on <M,w>
      If H rejects, reject
      Else:
        Run M on w
        Accept if M accepts
        Reject if M rejects

First, we argue that $S$ is a decider. Since $H$ is a decider for
$HALT_{TM}$, the first step always terminates. Moreover, we only run $M$
on $w$ if $H$ accepts $\langle M, w\rangle$, i.e. if $M$ halts on $w$.
Thus, the simulation of $M$ on $w$ always halts as well. Therefore, $S$
is indeed a decider.

Next, we argue that $S$ is correct. Observe that if $M$ does not halt on
$w$, then $M$ does not accept $w$. In this case, $S$ correctly rejects.
If $M$ halts on $w$, $S$ accepts if and only if $M$ accepts $w$.
Therefore, $S$ is correct.

We conclude that $S$ is indeed a decider for $A_{TM}$ and so
$A_{TM} \leq HALT_{TM}$.

:::

Since $A_{TM}$ is undecidable, we conclude that $HALT_{TM}$ is
undecidable.

::: {prf:theorem}

$HALT_{TM}$ is undecidable.

:::

## TM Emptiness

The TM Emptiness Problem is defined as follows: given an encoding of a
Turing machine $M$, decide if $L(M) = \emptyset$, i.e. if $M$ does not
accept any string. The corresponding language $EMPTY_{TM}$ consists of
$\langle M, w \rangle$ such that $M$ halts on $w$.

::: {prf:theorem}

$A_{TM} \leq EMPTY_{TM}$.

:::

::: {prf:proof}

To show that $A_{TM} \leq EMPTY_{TM}$, we need to construct a decider
for $A_{TM}$ given a decider for $EMPTY_{TM}$.

Let $H$ be a decider for $EMPTY_{TM}$. Consider the following TM $S$ for
$A_{TM}$. The reduction is trickier than the one for $HALT_{TM}$ and
involves constructing a TM that we then run $H$ on.

    On input <M,w>:
      Construct TM M_w that behaves as follows:
         On input x:
            Run M on w
            Accept if M accepts
            Reject if M rejects
            Run forever if M runs forever
      Run H on <M_w>
      Accept if H rejects
      Reject if H accepts

First, observe that whether or not $M_w$ accepts $x$ is independent of
$x$, it only depends on whether $M$ accepts $w$. Thus, $M_w$ accepts
every string if $M$ accepts $w$ and $M_w$ accepts no string otherwise.
Therefore, $L(M_w) = \emptyset$ if and only if $M$ does not accept $w$.
So, $S$ is correct for $A_{TM}$. The fact that it is a decider follows
easily from the fact that $H$ is a decider and thus running $H$ on
$\langle M_w \rangle$ always halts.

We conclude that $S$ is indeed a decider for $A_{TM}$ and so
$A_{TM} \leq EMPTY_{TM}$.

:::

Since $A_{TM}$ is undecidable, we conclude that $EMPTY_{TM}$ is
undecidable.

::: {prf:theorem}

$EMPTY_{TM}$ is undecidable.

:::

## TM Equivalence

The TM Equivalence Problem is defined as follows: given an encoding of
two Turing machines $M_1$ and $M_2$, decide if $L(M_1) = L(M_2)$, i.e.
for every string $w$, $M_1$ accepts $w$ if and only if $M_2$ accepts
$w$. The corresponding language $EQ_{TM}$ consists of
$\langle M_1, M_2 \rangle$ such that $L(M_1) = L(M_2)$.

::: {prf:theorem}

$EMPTY_{TM} \leq EQ_{TM}$.

:::

::: {prf:proof}

To show that $EMPTY_{TM} \leq EQ_{TM}$, we need to construct a decider
for $EMPTY_{TM}$ given a decider for $EQ_{TM}$.

Let $H$ be a decider for $EQ_{TM}$. Consider the following TM $S$ for
$EMPTY_{TM}$.

    On input <M>:
      Construct TM N that rejects every string
      Run H on <M, N>
      Accept if H accepts
      Reject if H rejects

Since $N$ rejects every string, we get that $L(N) = \emptyset$ and so
$S$ accepts $\langle M \rangle$ if and only if
$L(M) = L(N) = \emptyset$. So, $S$ is correct for $EMPTY_{TM}$. The fact
that it is a decider follows easily from the fact that $H$ is a decider
and thus running $H$ on $\langle M, N \rangle$ always halts.

We conclude that $S$ is indeed a decider for $EMPTY_{TM}$ and so
$EMPTY_{TM} \leq EQ_{TM}$.

:::

Since $EMPTY_{TM}$ is undecidable, we conclude that $EQ_{TM}$ is
undecidable.

::: {prf:theorem}

$EQ_{TM}$ is undecidable.

:::
