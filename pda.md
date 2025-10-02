(sec-PDA)=

# Pushdown Automata

::: {note}

The following contains some intuitive explanation of pushdown automata.
Please see Cezary's slides and lecture recording, and also Sipser's
slides on LMS for the full details.

:::

Intuitively, a pushdown automata is a finite automata but with a stack.
The stack is unbounded which gives the pushdown automata unbounded
memory. However, the pushdown automata has limited access to the stack:
it can only read and remove the top element of the stack (a pop
operation) and write a new element at the top of the stack (a push
operation).

:::::: {figure width=400px} ./pda.png

Schematic diagram of a pushdown automata.

::::::

Nevertheless, it can do simple counting which lets it recognize the
language $\{0^n1^n \mid n \geq 0\}$.

## Pushdown automata for $0^n1^n$

Let us first see what a deterministic pushdown automata (DPDA) consists
of and what it can do. It consists of a set of states (with one initial
state and some are marked as accepting) and its transition function
defines rules for state transitions with the following template:

***If** we are currently in state $q$, the next input symbol is $a$ and
the top symbol of the stack is $b$, **then** go to state $r$ and either
leave the stack alone, or apply a push or pop operation to the stack.*

We also allow the DPDA to make $ε$-transitions without consuming the
next input symbol.

The DPDA processes an input string $w = v_1v_2\cdots v_n$ as follows:

1.  Start at the initial state
2.  For each input symbol $v_i$ (starting from $v_1$), follow the
    applicable transition
3.  After all input symbols have been processed, accept the string if
    the final state is an accepting state and reject otherwise.

::: {note} Technicality: bottom of stack

Technically, we do not know when the stack is empty. But we can easily
fix this by first pushing a special "bottom-of-the-stack" marker
(usually we use the \$ symbol) onto the stack, before doing anything
else. Then, if we see the \$ symbol, we know that the stack is empty.

We also assume that applying the pop operation to the bottom of the
stack does not do anything.

:::

::: {note} Incomplete descriptions

We note that the slides use 4 states. This is because it is assuming
implicitly that if the automaton has to take an unspecified state
transition, then the string is rejected. These are called *incomplete*
automata.

For all assessments, you must use *complete* automata that specifies all
transitions.

:::

Unlike finite automata, it turns out that nondeterministic pushdown
automata are strictly more powerful and can recognize languages not
recognizable by deterministic pushdown automata. So, in the rest of this
section, we will stick with nondeterministic pushdown automata.
