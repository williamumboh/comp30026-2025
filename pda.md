(sec-PDA)=

# Pushdown Automata \[DRAFT\]

Intuitively, a pushdown automata is a finite automata but with a stack.
The stack is unbounded which gives the pushdown automata unbounded
memory. However, the pushdown automata has limited access to the stack:
it can only read and remove the top element of the stack (a pop
operation) and write a new element at the top of the stack (a push
operation).

:::::: {image} ./pda.png
:width: 400px
::::::

Nevertheless, it can do simple counting which lets it recognize the
language $\{0^n1^n \mid n \geq 0\}$.

## Pushdown automata for $0^n1^n$

Let us first see what a deterministic pushdown automata consists of and
what it can do. It consists of a set of states and its transition
function $\delta$ defines rules for state transitions with the following
template:

***If** we are currently in state $q$, the next input symbol is $a$ and
the top symbol of the stack is $b$, **then** go to state $r$ and either
leave the stack alone, or apply a push or pop operation to the stack.*

## Definitions

::: {prf:definition} Pushdown automata

:::
