(sec-high-level)=

# High Level Framework for Automata

For the most part, we have described finite-state and pushdown automata
using state transition diagrams. While state transition diagrams are
great for analyzing automata, in my opinion, they are not as helpful
that for designing automata (especially for beginners) and the notation
for state transitions can be quite cryptic for pushdown automata.

We are now going to see a more explicit way of describing automata that
is more akin to programming.[^1] The hope is that it makes automata
clearer by allowing students to draw on their previous programming
experience. Along the way, we will also discuss how to analyze the
correctness of an automata.

## Deterministic Finite Automata

:::::: {figure width=400px} ./fa-schematic.png

Schematic diagram of a finite automata. The finite control box
represents the various states and state transitions of the finite
automata.

::::::

We model a DFA as a program whose input is presented on a *tape*. In
particular, the tape is subdivided into *tape cells* with one cell per
input sybmol. The program also controls a "tape head": it can read the
input symbol currently under the tape head and move the tape head to the
right by one cell.

:::::: {aside}

::: {figure label=program-oddones}

``` python
Start:
  If '1':
    Move Right
    Goto OddOnes
  If '0':
    Move Right
    Goto EvenOnes
  If 'End':
    Reject

OddOnes:
  If '1':
    Move Right
    Goto EvenOnes
  If '0':
    Move Right
    Goto OddOnes
  If 'End':
    Accept

EvenOnes:
  If '1':
    Move Right
    Goto OddOnes
  If '0':
    Move Right
    Goto EvenOnes
  If 'End':
    Reject
```

A program corresponding to a DFA that accepts a string if and only if it
has an odd number of 1s.

:::

::::::

The program consists of *code blocks*. A code block starts with its
*label* followed by exactly 3 `If` statements. The block labeled `Start`
is special: this is where execution starts.

The 3 `If` statements correspond to the cases that the input symbol
under tape head is equal to '1', '0', or and the case that there is no
more input. Each of the first two If cases has exactly two statements:
`Move Right` which moves the tape head to the right by one cell, and a
`Goto <label>` statement which causes the execution to jump to the start
of the corresponding block. The third `If` case (when the input has
ended) has exactly one statement and is either `Accept` or `Reject`.

::: {note}

The syntax for `If` statements is `If <symbol>` which is short for "If
input symbol under tape head is equal to `<symbol>`".

:::

The equivalence between the usual definition of finite automata and the
above is straightforward:

- blocks correspond to states
- the first two `If` statements of a block correspond to the transitions
  out of the corresponding state
- the last correspond to whether the state is accepting or not

## Best Practices for Designing DFA

::: {tip} How to design DFA

1.  Think of the information you want to encode about the string seen so
    far.
2.  Write down the labels for the blocks/states. Use descriptive labels
    that correspond to the information being encoded by each
    block/state.
3.  Design the Gotos/transitions in such a way that the desired
    invariant holds true.
4.  If not possible, go to step 1 and rethink the encoding.

:::

The crux in designing a DFA is figuring out how to **encode information
about the string we have seen so far using a finite number of
blocks/states**.

Consider the task of determining whether a string has an odd number of
1s. It suffices to keep track of whether the number of 1s we have seen
so far is odd or even. Thus, @program-oddones has blocks `OddOnes` and
`EvenOnes`. The idea is that the following holds true throughout the
execution of the program[^2]:

- it enters the `OddOnes` block if and only if the number of 1s seen so
  far is odd
- it enters either the `Start` or the `EvenOnes` block if and only if
  the number of 1s seen so far is even.

It is not too hard to convince oneself that they hold true because[^3]:

- At the start, we go to the `EvenOnes` block if the first input symbol
  is 0 and the `OddOnes` block if it is 1
- Suppose we have already seen an even number of ones. That means we are
  in the `EvenOnes` block. If the next input symbol is 0, we go to the
  `EvenOnes` block, and if it is 1, we go to the `OddOnes` block
- Suppose we have already seen an odd number of ones. That means we are
  in the `OddOnes` block. If the next input symbol is 0, we go to the
  `OddOnes` block, and if it is 1, we go to the `EvenOnes` block

::: {note} 2 states

It is not too hard to see that in @program-oddones, the Start and
EvenOnes blocks can be merged into a single one. The choice to have 3
states is so that the labels are as descriptive as possible.

:::

::: {prf:example} Thinking process

Consider the task of determining whether the input string contains '01'.
Here's my thinking process.

As a first attempt, the information to encode about the string seen so
far is:

- Have we seen at least one 0 so far?
- If we have seen at least one 0 so far, is the last input symbol a 1?
- Have we seen '01' so far?

This gives us labels `AllOnes`, `Exists0`, `Exists0Last1` and `Seen01`
and it is now straightforward to fill in the rest of the program.

    Start:
      If '1':
        Move Right
        Goto AllOnes
      If '0':
        Move Right
        Goto Exists0
      If 'End':
        Reject

    AllOnes:
      If '1':
        Move Right
        Goto AllOnes
      If '0':
        Move Right
        Goto Exists0
      If 'End':
        Reject

    Exists0:
      If '1':
        Move Right
        Goto Exists0Last1
      If '0':
        Move Right
        Goto Exists0
      If 'End':
        Reject

    Exists0Last1:
      If '1':
        Move Right
        Goto Seen01
      If '0':
        Move Right
        Goto Seen01
      If 'End':
        Accept

    Seen01:
      If '1':
        Move Right
        Goto Seen01
      If '0':
        Move Right
        Goto Seen01
      If 'End':
        Accept

After writing it down, I realize that the `Exists0Last1` and `Seen01`
blocks can be merged into a single block: If there exists a 0 and the
last symbol is 1, then clearly we have seen 01.

:::

## Pushdown Automata (DPDA)

:::::: {figure width=400px} ./pda.png

Schematic diagram of a pushdown automata.

::::::

A PDA is the same as a FA with a stack. In particular, it has two new
instructions: `push <symbol>` and `pop`. Moreover, its `If` statements
can also depend on the top symbol of the stack. To make it more
explicit, we write `If input = <symbol> and stack = <symbol>:`.

Technically, we need to push the \$ symbol onto the stack at the start
to let us check if we have reached the bottom of the stack. However, for
simplicity of description, we hide this detail here.

::: {prf:example label=pda-0n1n} PDA for $\{0^n1^n \mid n \geq 0\}$

The following is a description of a PDA for $\{0^n1^n \mid n \geq 0\}$.
The idea is that the program enters the Zeros block if and only if the
string seen so far is all 0s, and then enters the Ones block if and only
if the string seen so far is 0s followed by 1s.

    Start:
      If input = '1':
        Move Right
        Goto Invalid
      If input = '0':
        Move Right
        Push '0'
        Goto Zeroes
      If input = 'End':
        Accept

    Zeros:
      If input = '1':
        Move Right
        Pop
        Goto Ones
      If input = '0':
        Move Right
        Push '0'
        Goto Zeros
      If input = 'End':
        Reject

    Ones:
      If stack is empty:
        Move Right
        Goto Equal
      If input = '1' and stack = '0':
        Move Right
        Pop
        Goto Ones
      If input = '0':
        Move Right
        Goto Invalid
      If input = 'End' and stack is not empty:
        Reject

    Equal:
      If input = '1':
        Move Right
        Goto Invalid
      If input = '0':
        Move Right
        Goto Invalid
      If input = 'End':
        Accept

    Invalid:
      If input = '1':
        Move Right
        Goto Invalid
      If input = '0':
        Move Right
        Goto Invalid
      If input = 'End':
        Reject

:::

## Nondeterminism

To model nondeterminism, we use a `Fork` statement, one per
non-deterministic branch. For example, suppose if the next input symbol
is 1, we want to go to `Block1` and `Block2`, then we write:

      If input = '1':
        Fork:
          Move Right
          Goto Block1
        Fork:
          Move Right
          Goto Block2
    ...

::: {important}

When the PDA makes a non-deterministic branch, it creates another copy
of itself **and** its stack, and the two copies then proceed
**independently** of each other.

:::

::: {note} Conveniences

For convenience, we treat `input` as a variable and allow statements
such as `Push input` to mean that we push the input symbol to the stack.
We also use `If input = stack` to mean "if input symbol equals symbol at
top of stack".

:::

::: {prf:example label=pda-even-pal} PDA for even-length palindromes

The following is a description of a PDA for the language of even-length
palindromes
$$\{ww^R \mid w \text{ is a (possibly empty) binary string}\}.$$

The idea is to push the first half of the input to the stack and use the
fact that popping the stack gives us the reverse of the first half. The
issue is that the PDA does not know the input length and so does not
when it has reached the end of the first half. To get around this, we
non-deterministically guess when we have reached the end of the first
half.

The key observation is that the string is an even-length palindrome if
and only if there exists a guess such that the reverse of the guessed
first half equals the second half.

    Start:
      If input != 'End':
        Move Right
        Push input
        Goto FirstHalf
      If input = 'End':
        Accept

    FirstHalf:
      If input != 'End'
        Fork:
          Move Right
          Push input
          Goto FirstHalf
        Fork:
          Move Right
          Push input
          Goto SecondHalf
      If input = 'End':
        Reject

    SecondHalf:
      If stack is empty:
        Goto Palindrome
      If input = stack:
        Move Right
        Pop
        Goto SecondHalf
      If input != 'End' and input != stack:
        Move Right
        Goto Invalid
      If input = 'End'
        Reject

    Palindrome:
      If input != 'End':
        Move Right
        Goto Invalid
      If input = 'End':
        Accept

    Invalid:
      If input != 'End':
        Move Right
        Goto Invalid
      If input = 'End':
        Reject

[^1]: This is inspired by the notation used in [Stanford CS
    103](https://web.stanford.edu/class/archive/cs/cs103/cs103.1246/lectures/20/Lecture%20Slides.pdf)
    which in turn is inspired by Emil Post's *Formulation 1* and Hao
    Wang's *Basic Machine B*.

[^2]: These are also called *invariants*.

[^3]: This is an induction argument.
