# Finite Automata

We begin our study of computation with a simple model of computation
called *finite automata*.

::: {aside}

Singular = "automaton", plural = "automata"

:::

The most important feature of a finite automaton is that it has a
**fixed number of states** that it can be in. Intuitively, this
corresponds to an algorithm that is only allowed to use a constant
amount of memory, regardless of input size; think of a program with only
a constant number of variables. In particular, it cannot allocate new
memory in response to the input.

This restriction makes sense in highly-constrained settings such as
low-power sensors, where one has to deal with processing a steady stream
of data input over long periods of time.

::: {tip} What does it feel like to be a finite automaton?

The definition of finite automata can seem very abstract and
unrelatable. I personally find it helpful to place myself in the shoes
of a finite automaton to understand the definition, and later, to design
finite automata, as well as to understand their limitations.

Fortunately, there is a wonderful movie by Christopher Nolan called
Memento about anterograde amnesia, which is a condition in which one
cannot make new memories. See the trailer
[here](https://www.youtube.com/watch?v=HDWylEQSwFo).

:::
