# Introduction to Finite Automata

Our plan is to start with a very concrete example of a finite automaton
in the form of tally counters, learn how to do computation with it, and
then generalize.

(sec-tally)=

## (Simple) tally counters

Consider the modest [tally
counters](https://en.wikipedia.org/wiki/Tally_counter) that you have
probably seen at entrances to bars, concerts, museums, and other venues
where they are used to keep track of how many people have entered and
how many have left.

:::{aside}

There are many online implementations of tally counters that you can use
as you read along.
[Here](https://www.rapidtables.com/tools/click-counter.html?c1=0) is an
example.

:::

::: {figure} https://upload.wikimedia.org/wikipedia/commons/c/c3/Tally_Counters.jpg
:width: 500px
Image of a tally counter. Credit: [Wikimedia Commons](https://commons.wikimedia.org/wiki/File:Tally_Counters.jpg)
:::

The tally counter has 3 components:

1.  A displayed number that is initially $0000$, and can go up to
    $9999$. We call this number the *count*.
2.  An "increment" button on top. Pressing the button increments the
    number by $1$ if the current count is less than $9999$, and it
    becomes $0000$ otherwise, i.e. it adds $1$ modulo $10000$.
3.  A "reset" dial on the right side. The dial can be used to reset the
    count to $0000$ by rotating it clockwise sufficiently many times.

Thus, we can view the tally counter as a device with 10000 states, whose
initial state is $0$, and that receives as input a sequence of
"increments" and "resets".[^1]

## (Even simpler) single-digit tally counter

To make the rest of our discussion manageable, in the remainder, we
focus on the state of the first digit (i.e. the rightmost digit). This
is equivalent to a **single-digit tally counter** that operates as
follows:

1.  It has 10 states, corresponding to the numbers $0, ..., 9$.
2.  Its initial state is $0$.
3.  Let $i$ be the current state.
    - Incrementing changes its state to $i \mod 10$.
    - Resetting changes its state to $0$.

We can succinctly represent the single-digit tally counter using a
*state transition table* and a *state diagram*.

State transition table:

| Current state | Reset | Increment |
|---------------|-------|-----------|
| (Initial) 0   | 0     | 1         |
| 1             | 0     | 2         |
| 2             | 0     | 3         |
| ⋮             | ⋮     | ⋮         |
| 9             | 0     | 0         |

State diagram:

::: {figure} ./automaton.png

Dashed edges correspond to transitions due to "resets" and solid ones correspond to "increments".
:::

## Turning counters into computational devices

Let's now turn the single-digit tally counter into a computational
device that receives its input bit string as a sequence of bits, one bit
at a time. There are two tasks we need to turn it into a finite
automaton:

1.  Specify how the device accepts/rejects strings.[^2] In general, this
    is done by indicating some states as *accepting states* and defining
    an input string to be accepted if the device ends up in an accepting
    state at the end of the string.
2.  Specify how it transitions between states in response to each
    incoming bit of the input. In particular, we need to specify a state
    transition table.

Let us keep things simple and suppose that the string is accepted if the
final state of the tally counter is 0. Consider the following simple
examples, and let's see what the set of accepted strings are in each of
these examples.

::: {exercise}
:label: ex-1
When we receive an input bit, we increment the tally counter regardless of whether its 0 or 1.

:::

Let's take a look at its state transition table and diagram

| Current state          | 0   | 1   |
|------------------------|-----|-----|
| (Initial, Accepting) 0 | 1   | 1   |
| 1                      | 2   | 2   |
| 2                      | 3   | 3   |
| ⋮                      | ⋮   | ⋮   |
| 9                      | 0   | 0   |

::: {figure} ./ex-1-fa.png

We use double circle to denote that 0 is an accepting state.

:::

::: {attention} Stop and think

Stop and think on your own what the set of accepted strings looks like before revealing the answers. Drawing the state transition diagram can be helpful. 

:::


::: {solution} ex-1
:class: dropdown
:label: sol-1
Strings whose length is a non-negative multiple of 10.

:::

::: {exercise}
:label: ex-2
When we receive an input bit, we increment the tally counter if the bit is 1 and do nothing if it is 0.

:::

::: {solution} ex-2
:class: dropdown
:label: sol-2
Strings in which the number of 1s is a non-negative multiple of 10.

:::

::: {exercise}
:label: ex-3
When we receive an input bit, we increment the tally counter if the bit is 1 and reset if it is 0.

:::

For this one, it can be helpful to take a look at this [Ed lesson](https://edstem.org/au/courses/23763/lessons/79201/slides/622878) which simulates the finite automaton.

::: {solution} ex-3
:class: dropdown
:label: sol-3
It is easy to see that every string ending with 0 is accepted. But these are not the only ones. Every string whose number of 1s is a non-negative multiple of 10 and has no 0s are is also accepted. In general, a string is accepted iff the number of 1s since the last-occuring 0 is a non-negative multiple of 10 (if there is no 0, then since the start of the string).

:::

In general, from any state, we can go to any other state by using a
sequence of "increments".

## Finite automata for bit strings

Recall the state transition table for @ex-1 the first example above.

| Current state | 0   | 1   |
|---------------|-----|-----|
| (Initial) 0   | 1   | 1   |
| 1             | 2   | 2   |
| 2             | 3   | 3   |
| ⋮             | ⋮   | ⋮   |
| 9             | 0   | 0   |

In general, we can define a finite automaton for bit strings using a
state transition table with 3 columns:

- Each row represents a state of the finite automaton
- Column 1: name of the state
- Column 2: which state to transition to if next bit is 0
- Column 3: which state to transition to if next bit is 1

::: {important}

State transition diagrams are a great way to analyze finite automata.
However, when designing a finite automaton, it is more helpful to first
think of what the finite automaton needs to remember about the string so
far. Once you have figured this out, the states and the transitions
between them natural.

:::

Here's the formal definition of finite automata for bit strings.

::: {prf:definition} Finite automata for bit strings

A finite automaton $M$ for bit strings consists:

1.  Set of states $Q$
2.  Initial state $q₀$
3.  State transition function $\delta : Q × \{0,1\} → Q$
4.  Subset of accepting states $F$

:::

::: {aside}

We use $\delta$ because $\delta$ typically means "difference" or
"change" in calculus.

:::

## Finite automata in general

In general, we can have strings with other "alphabets", depending on the
application.

::: {prf:definition} Alphabet

An *alphabet* is a finite set of "symbols" and is denoted by $\Sigma$.

:::

::: {prf:example} Examples of alphabets

- {0,1}
- {a,b}
- {"reset", "increment"}
- {⬆️,➡️,⬇️,⬅️}
- {👶,🧑,👴,🧟}

:::

::: {prf:definition} Finite automata

A finite automaton $M$ consists of:

1.  Finite input alphabet $Σ$
2.  Set of states $Q$
3.  Initial state $q₀$
4.  State transition function $\delta : Q × \Sigma → Q$
5.  Subset of accepting states $F$

:::

Let $M$ be a finite automaton. Consider an input string
$w = v_1 v_2 \cdots v_n$ where each $v_i$ is a symbol of the alphabet.
The finite automaton processes $w$ as follows:

1.  Start at the start state
2.  For each input symbol $v_i$ (starting from $v_1$), follow the
    corresponding transition: if the current state is $q$, then go to
    state $\delta(q,v_i)$.
3.  After all input symbols have been processed, it accepts the string
    if the final state (also called the resulting state) is an accepting
    state, and rejects it otherwise.

In particular, it goes through the following sequence of state
transitions:

::: {math}
:label: transition-eq
q_0 \xrightarrow{v_1} r_1 \xrightarrow{v_2} r_2 \xrightarrow{v_3} \ldots \xrightarrow{v_n} r_n
:::

where we use the notation $r_{i-1} \xrightarrow{v_{i-1}} r_i$ to mean
that $r_i = \delta(r_{i-1}, v_i)$, i.e. the state after processing the
$i$-th symbol of the input string $w$.

The final state $r_n$ is called the *resulting state* of $w$. We define
the notation $q(w)$ to mean the resulting state of $w$.

::: {prf:definition} Finite automata acceptance

A finite automaton $M$ *accepts* a string $w$ if $q(w)$ is an accepting
state and *rejects* $w$ if $q(w)$ is not an accepting state. The set of
strings accepted by the finite automaton is denoted by $L(M)$.

:::

::: {prf:definition} Finite automata recognizing languages

Let $A$ be a language. We say that $M$ *recognizes* a language $A$ iff
the set of strings accepted by $M$ is exactly $A$, i.e. $A = L(M)$;
equivalently, for every input $x$:

- if $x$ is in $A$, then $M$ accepts $x$, and
- if $x$ is not in $A$, then $M$ rejects $x$.

:::

::: {caution}

When we say $M$ recognizes $A$, the equality $A = L(M)$ is important: if
there exists a string accepted by $M$ but is not in $A$, or if there
exists a string in $A$ but is not accepted by $M$, then $M$ does not
recognize $A$.

:::

::: {prf:definition} Regular languages

A language $L$ is *regular* iff there is a finite automaton that
recognizes it.

:::

::: {prf:example}

Let $M_1, M_2, M_3$ be the finite automata described by @ex-1, @ex-2,
@ex-3, respectively. Let $A_1, A_2, A_3$ be the languages in @sol-1,
@sol-2, @sol-3, respectively.

We have $A_1 = L(M_1), A_2 = L(M_2), A_3 = L(M_3)$. Thus, $A_1,A_2,A_3$
are regular languages.

:::

## Computation path

The state transition diagram representation gives a nice graphical
representation of the sequence of state transitions that a finite
automaton $M$ goes through while processing a string $w$. In particular,
we can represent the sequence
$$r_0 \rightarrow r_1 \rightarrow \ldots \rightarrow r_n$$
as a path in the state transition diagram that starts from $r_0$ (the
initial state), goes to $r_1$, then $r_2$, and so on. Note that the path
can repeat vertices.[^3]

We refer to this path as the *computation path* of $M$ on input $w$.

:::::: {prf:example}

Consider the following finite automata.

::: {image} ./path-automaton.png
:width: 300px
:::

Consider input string 01011. The sequence of state transitions are

::: {math}
q_0 \xrightarrow{0} q_1 \xrightarrow{1} q_1 \xrightarrow{0} q_2 \xrightarrow{1} q_3 \xrightarrow{1} q_1
:::

The computation path on input string 01011 is as follows. The labels on
the edges refer to their position on the path, i.e. the edge labeled 3
from $q_2$ to $q_3$ means it's the 3rd edge.

::: {image} ./path.png
:width: 300px
:::

::::::

## Key features of a finite automaton

1.  Constant memory
    - Memory = states and number of states is independent of input
      length
    - (Memorylessness) Behavior depends only on current state and next
      symbol, e.g. it does not remember how many times it has visited
      the current state before
2.  Receive the input as a stream
    - (It ain't over till it's over) Does not know when the input is
      going to end

::: {important}

When it receives the last symbol, it does not know that it is the last
symbol. The state it is in after processing the last symbol determines
whether it accepts or rejects the entire string. No additional
post-processing is allowed.

:::

Observe that the tally counter has the above properties. Clearly, its
memory is fixed and cannot allocate new memory.

Analogous to a program that has two phases:

1.  (Initialisation phase) Before seeing the input, it declares the set
    of variables, and the set of possible values they can take. This
    includes a variable called IsAccept which can be either True or
    False.
2.  (Input processing phase) While the stream is not empty, get next
    symbol and update variables. No new variables can be declared.
    Variables restricted to values declared in Init phase.
3.  (Final phase) When stream is empty, return IsAccept

::: {tip}

Take a look at the [Ed
lesson](https://edstem.org/au/courses/23763/lessons/79201/slides/622878)
that simulates the finite automata of @ex-3. I also recommend running
the code step-by-step through a tool that also lets you see that
contents of each variable in each step. For example,
<https://pythontutor.com>.

:::

[^1]: A more direct representation of the tally counter is as a finite
    automaton that receives a sequence of "increments" and "rotations"
    where the latter corresponds to a single dial rotation instead of
    several rotations that resets the count. However, the [exact
    effect](https://en.wikipedia.org/wiki/Tally_counter#Description) of
    a single dial rotation is too complicated for the purposes of our
    discussion.

[^2]: As discussed previously in @str-lang-dec, we are focusing on
    decision problems and so we need to specify a way for the device to
    indicate whether it accepts an input string or not.

[^3]: If you are familiar with graph theory, the path is not necessarily
    a simple path.
