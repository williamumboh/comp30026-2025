(sec-intro-tm)=

# Introduction to Turing Machines \[DRAFT\]

## Turing Machines, Informally

Recall the view of finite automata as a finite-state machine whose input
is on a tape and controls a tape head (@sec-high-level).

:::::: {figure width=400px} ./fa-schematic.png

Schematic diagram of a finite automata. The finite control box
represents the various states and state transitions of the finite
automata.

::::::

The finite automata can:

- move its tape head to the right by one cell
- read the symbol under the tape head
- the tape contains only its input
- accepts or rejects once it has reached the end of the tape

A Turing machine (TM) on the other hand can in addition:

- move the tape head to the left by one cell
- write a symbol to the tape cell under the tape head (overwriting
  whatever is in the tape cell)
- has an infinite tape (the input appears first and is followed by an
  infinite number of blanks, denoted by the blank symbol "˽")
- can accept or reject at any time (not only at the end of the input)

::: {prf:example} Informal example

Consider the canonical noncontext-free language
$\{0^n1^n2^n \mid n \geq 0\}$.

Let us first assume that the TM has the ability to "cross off" tape
cells. Here's a high-level description of a TM that recognizes the
language:

1.  Scan right until ˽ (this is the end of the input) while checking
    that the input is of the form $0^*1^*2^*$. If not, reject.
2.  Return tape head to left end
3.  Scan right, "crossing off" the first $0$ that is not crossed off
    yet, the first $1$ that is not crossed off yet, and the first $2$
    that is not crossed off yet
4.  At the end of the scan:
    - If we successfully cross off one $0$, one $1$, one $2$, repeat
      line 3
    - Else if every input symbol is crossed off, accept
    - Else, reject

The crossing off operation can be implemented by using the symbols {strike}`0`, {strike}`1`, {strike}`2` to mean the crossed off versions of 0, 1, and 2.

:::

## Formal Definition

::: {prf:definition} Turing Machine (TM)

A Turing machine $M$ consists of:

1.  Finite input alphabet $Σ$
2.  Finite tape alphabet $\Gamma$ (contains $\Sigma$)
3.  Set of states $Q$
4.  Initial state $q₀$
5.  State transition function $\delta$ which takes as input the current
    state and the symbol under the tape head, and outputs the next
    state, the symbol to write under the tape head and whether to move
    the tape head to the left or right. Mathematically,
    $\delta : Q × \Gamma → Q \times \Gamma \times \{L,R\}$.
6.  Accepting state $q_{acc}$ and rejecting state $q_{rej}$

:::

The transition function can also be represented using a state transition
diagram as was done for finite and pushdown automata. The transition
$\delta(q,a) = (r,b,d)$ is represented by
$q \xrightarrow{a \rightarrow b, d} r$. For transitions that do not
overwrite the symbol under the tape head with a different symbol, i.e.
$\delta(q,a) = (r,a,d)$, we use $q \xrightarrow{a \rightarrow d} r$.

Let $M$ be a Turing machine. Consider an input string
$w = v_1 v_2 \cdots v_n$ where each $v_i$ is a symbol of the alphabet.
The TM processes $w$ as follows:

1.  Initially, the first $n$ tape cells of the tape contain the symbols
    of $w$, with the remaining cells containing the blank symbol ˽
2.  Start at the start state and with the tape head at the start (i.e.
    left end) of the tape
3.  Repeat the following:
    - Let $q$ be the current state and $x$ be the symbol under the tape
      head
    - If $q = q_{acc}$, $M$ halts and accepts
    - If $q = q_{rej}$, $M$ halts and rejects
    - Else:
      - let $(r,y,d) = \delta(q,x)$
      - write $x$ to the cell under the tape head, move tape head in
        direction $d$, and move to state $r$

::: {important} Outcomes of Turing machine computation

A subtle but important feature of Turing machines that finite and
pushdown automata do not possess is that a Turing machine can
potentially loop forever on an input, never entering $q_{acc}$ or
$q_{rej}$.

:::

::: {prf:definition} Halting

A TM $M$ is said to *halt* on input $w$ if it $M$ eventually enters
$q_{acc}$ or $q_{rej}$. A TM $M$ is said to be a *decider* if it halts
on every input.

:::

::: {prf:definition} Turing machine acceptance and language

A string $w$ is said to be *accepted* by a TM $M$ if $M$ eventually
enters $q_{acc}$ and is said to be *rejected* if $M$ eventually enters
the $q_{rej}$ state.

The language of a TM $M$ is the set of strings accepted by $M$. We use
$L(M)$ to mean the language of $M$.

:::

::: {prf:definition} Recognizers and deciders

A language $A$ is *Turing-recognizable*[^1] if there exists a TM $M$
such that $A = L(M)$, and *Turing-decidable* if there exists a TM $M$
such that $A = L(M)$ **and** $M$ is a decider. We say that $A$ is
*recognized* by $M$ and *decided* by $M$, respectively.

:::

(sec-tm-config)=

### Configurations

We now explain in more detail exactly what a single step of computation
in a Turing machine looks like.

::: {prf:definition} TM Configuration

The *configuration* of a Turing machine $M$ on an input string $w$ is a
snapshot of its execution at a point in time. In particular, the
configuration consists of:

- the current state
- the current state of the tape (i.e. the contents of the tape)
- the current location of the tape head

We use the following notation for a configuration: we write $xqy$ to
mean that the current state is $q$, the contents of the tape is $xy$ and
the tape head is on the first symbol of $y$.

:::

Let $C$ and $C'$ be two configurations. We say that $C$ *yields* $C'$
iff applying $\delta$ to $C$ results in $C'$ and write
$C \Rightarrow C'$.

::: {prf:example}

Let $C$ be the configuration in which the tape has the string $ababb$
followed by an infinite sequence of blanks, the current state is $q$ and
the tape head is on the third symbol $a$. Then, we write $C = abqabb$.

If $\delta(q,a) = (r,b,R)$, then $C$ yields the configuration
$C' = abbrbb$.

:::

The initial configuration of $M$ on $w$ is $q_0w$: the state is the
initial state, the tape contains the input $w$ and the tape head is at
the start of the tape.

With these definitions, it is now clear that:

- $M$ halts and accepts on input $w$ if after applying the transition
  function to the initial configuration a finite number of times, we
  obtain a configuration whose state is $q_{acc}$.
- $M$ halts and rejects on input $w$ if after applying the transition
  function to the initial configuration a finite number of times, we
  obtain a configuration whose state is $q_{rej}$.
- $M$ never halts if it is not possible to obtain a configuration whose
  state is either $q_{acc}$ or $q_{rej}$ after applying the transition
  function to the initial configuration a finite number of times.

::: {prf:example} while loop

Here is an example of a TM that never halts on any input. The TM simply
moves the tape head to the right forever.

:::::: {figure width=100px} ./tm-non-halting.png

:::

[^1]: Also called *semi-decidable*.
