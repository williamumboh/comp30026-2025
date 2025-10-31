(sec-tm-closure)=

# Closure Properties

We now consider closure properties of recognizable and decidable
languages.

## Closure Properties for Decidable Languages

Closure properties for decidable languages are relatively
straightforward since we can easily simulate deciders as they are
guaranteed to always halt.

::: {prf:theorem}

If $L$ is decidable, then so is $L^c$ (recall that $L^c$ consists of the
strings that are not in $L$).

:::

::: {prf:proof}

If $L$ is decidable, then there is a decider $M$ for $L$. Then, we can
construct a decider $N$ for $L^c$ that simulates $M$ and does the
opposite:

On input $w$: Run $M$ on $w$ Accept if $M$ rejects Reject if $M$ accepts

Since $M$ is a decider it always halts and either accepts or rejects.
Since $N$ accepts if and only if $M$ rejects, we get that $N$ decides
$L^c$.

(Alternatively, we can construct $N$ by swapping the accept and reject
states of $M$.)

:::

::: {prf:theorem}

If $L_1$ and $L_2$ are decidable, then so is $L_1 \cup L_2$ and
$L_1 \cap L_2$.

:::

::: {prf:proof}

Similarly, the proof is by simulation. Let $M_1$ and $M_2$ be the
deciders for $L_1$ and $L_2$, respectively. The decider for
$L_1 \cup L_2$ runs $M_1$ and $M_2$ on $w$ and accepts if at least one
of them accepts. The decider for $L_1 \cap L_2$ runs $M_1$ and $M_2$ on
$w$ and accepts if both accept. Since $M_1$ and $M_2$ always halt, their
simulations always halt as well.

:::

## Closure Properties for Recognizable Languages

::: {prf:theorem}

If $L$ and $L^c$ are recognizable, then $L$ is decidable.

:::

::: {prf:proof}

Let $M_1$ and $M_2$ be the recognizers for $L$ and $L^c$, respectively.

Consider an input string $w$. We know that either $w$ is in $L$ or is in
$L^c$. If $w$ is in $L$, then $M_1$ will eventually halt and accept; if
$w$ is in $L^c$, then $M_2$ will eventually halt and accept.

A first try is to first simulate $M_1$ on $w$ and then $M_2$ on $w$.
However, since $M_1$ is only a recognizer, it may not halt on $w$, and
so the simulation may never end. The same problem remains if we first
simulate $M_1$ then $M_2$.

What we will do is to instead simulate $M_1$ and $M_2$ in a round-robin
fashion. Here's a description for the decider for $L$:

On input $w$:

1.  Compute the initial configurations of $M_1$ and $M_2$ on $w$
2.  While neither of the current configuration of $M_1$ and $M_2$ has
    halted:
    - Compute the next configurations of $M_1$ and $M_2$ by applying
      their respective transition functions to their current
      configurations
3.  Accept when $M_1$ accepts or $M_2$ rejects
4.  Reject when $M_1$ rejects or $M_2$ accepts

Since we have already argued earlier that either $M_1$ or $M_2$ will
eventually halt on $w$, the while loop will eventually terminate.

:::

A consequence of this is that we get that $A^c_{TM}$ is not
recognizable. In particular, $A^c_{TM}$ is the language consisting of
the encoding of a Turing machine $M$ and a string $w$ such that $M$ does
not accept $w$.

This is interesting as it tells us that there are natural problems of
practical interest that are not recognizable.

::: {prf:theorem}

$A^c_{TM}$ is not recognizable.

:::

::: {prf:proof}

Since $A_{TM}$ is recognizable but not decidable, we get that $A^c_{TM}$
is not recognizable.

:::
