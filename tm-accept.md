# Turing Machine Acceptance Problem

The TM Acceptance problem is as follows: given an encoding
$\langle M \rangle$ of a Turing machine $M$ and an input string $w$,
does $M$ accept $w$?

Equivalently, we define the language $A_{TM}$ to consist of strings
$\langle M, w \rangle$ such that $M$ is a Turing machine and $M$ accepts
$w$.

::: {prf:theorem label=thm-tm-accept-recognizable}

$A_{TM}$ is recognizable.

:::

::: {prf:proof}

The following Turing machine $U$ recognizes $A_{TM}$:

On input $\langle M, w \rangle$: Simulate $M$ on input $w$ Accept if $M$
halts and accepts Reject if $M$ halts and rejects

The simulation can be done in a similar fashion as for DFAs. Details
omitted.

To prove that it recognizes $A_{TM}$, we need to show that $U$ accepts
$\langle M, w \rangle$ if and only if $M$ accepts $w$. There are three
possible cases when $M$ is run on $w$:

1.  $M$ halts and accepts
2.  $M$ halts and rejects
3.  $M$ runs forever

By definition, if $M$ accepts, then $U$ accepts. In the other cases, $U$
either rejects or runs forever. Thus, $U$ accepts $\langle M, w \rangle$
if and only if $M$ accepts $w$, as desired.

:::

::::: {caution}

It is tempting to claim that the following TM decides $A_{TM}$.

On input $\langle M, w\rangle$:

1.  Simulate $M$ on input $w$
2.  Accept if $M$ halts and accepts
3.  Reject if $M$ halts and rejects
4.  Reject if $M$ runs forever

The issue is that "Reject if M runs forever" is not a valid TM action.

::: {figure width=300px} ./titanic-meme.jpg

An actual run of the TM.

:::

:::::

## Universal Turing machines

The fact that we can design a Turing machine that takes as input an
encoding of another Turing machine and simulate it is referred to as the
"universal" nature of Turing machines. In contrast, finite automata and
pushdown automata do not have this feature: there is no universal finite
automaton (pushdown automaton, resp.) that can take an encoding of a
finite automaton (pushdown automaton, resp.) and simulate it.

Universality also has huge practical importance. For example, your
mobile phone is not a single-purpose device: by loading different apps,
it can take on other functionality. Without universality, [you need one
device to browse the Internet, a separate one to play music, and a
separate one to make
calls](https://www.youtube.com/watch?v=OLenSrOsWLc&t=61s).

## Decider for TM Acceptance too good to be true

It turns out that TM Acceptance is undecidable. Before we show that,
let's build some intuition first by showing that a decider for TM
acceptance is too good to be true: using such a decider, we can turn any
recognizer into a decider.

::: {prf:theorem}

Suppose that $A_{TM}$ is decidable. Then, for every language $L$, if $L$
is recognizable, then $L$ is decidable.

:::

::: {prf:proof}

Let $M$ be a recognizer of $L$ and let $U$ be a decider for $A_{TM}$.
Now we are going to define a TM $D$ that decides $L$.

On input $w$:

1.  Run $U$ on $\langle M,w \rangle$
2.  Accept if $U$ accepts
3.  Reject if $U$ rejects

Since $U$ is a decider, it always accepts or rejects, it never runs
forever. Thus, $D$ also always halts. Moreover, for every string $w$,
$D$ accepts it if and only if $M$ accepts $w$. Therefore,
$L(D) = L(M) = L$ and so $D$ decides $L$.

:::
