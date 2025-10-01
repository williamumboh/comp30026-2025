(sec-nonreg)=

# Proving Nonregularity

::: {warning} Challenging material ahead

This section will require several passes and lots of hands-on practice.
Please ensure that you budget extra time.

:::

In this section, we get a first glimpse at proving impossibility
results. In particular, we will discuss methods for proving a given
language $L$ is nonregular, i.e. that there is no possible deterministic
finite automata that can recognise $L$.

::: {exercise} Check your understanding

We have also seen nondeterministic finite automata and regular
expressions. Based on what we have already seen, do we need to prove
separately that there is no nondeterministic finite automata that
recognises $L$?

:::

We begin with the fooling set technique, and then we show how to use
closure properties to leverage the fact that some other language is
already known to be nonregular.

::: {attention .dropdown} Fooling sets vs Pumping Lemma

Previous offerings of this subject and the majority of resources out
there use a different approach based on the so-called Pumping Lemma
instead of the fooling set technique. However, past experience and
student feedback has shown that it is highly challenging for students
who are not accustomed to proofs. Courses that assume similar levels of
mathematical background (e.g. [Stanford CS
103](https://web.stanford.edu/class/archive/cs/cs103/cs103.1246/)) use
the fooling set technique as it's a more direct and less abstract. In my
opinion, the fooling set technique also has the benefit that it is
easier to provide intermediate stepping stones, and thus partial marks,
in assessments.

:::

::: {seealso}

The lecture notes are based[^1] on the exposition in:

- Sections 1.1 and 1.2 of [A Note on Proving Non-Regularity via Fooling
  Sets](https://chekuri.cs.illinois.edu/teaching/fooling-sets.pdf) by
  Chandra Chekuri (UIUC). This is a beautifully written note that
  provides a lot of intuition behind the approach.[^2]
- [Stanford CS103
  slides](https://web.stanford.edu/class/archive/cs/cs103/cs103.1246/lectures/18/)
- Section 3.8 of [Lecture Notes on Finite-State
  Machines](http://jeffe.cs.illinois.edu/teaching/algorithms/models/03-automata.pdf)
  by Jeff Erickson (UIUC)

:::

(sec-nonreg-fooling)=

## Technique 1: Fooling Set Technique

At an intuitive level, to prove a language $L$ is not regular, we need
to exploit the fact that a finite automata has a fixed amount of memory,
independent of the length of the input string. The fooling set technique
allows us to prove lower bounds on the amount of memory that is needed
to recognize $L$.

In more detail, the technique lets us prove **lower bounds on the number
of states a DFA needs to recognize \$L\$**, i.e. it allows us to prove
statements of the form "no DFA with fewer than 5 states can recognize
$L$". Since a DFA has a fixed number of states, to show that no DFA can
recognize $L$, we will prove that **for every non-negative integer $k$,
no DFA with fewer than $k$ states can recognize $L$.**

(sec-nonreg-meaning)=

### What does it mean to prove nonregularity?

Recall that a language $L$ is regular if and only if there is a DFA $M$
that recognizes it.

Recall also that a DFA $M$ recognizes a language $L$ if and only if for
every input $x$:

- if $x$ is in $L$, then $M$ accepts $x$, and
- if $x$ is not in $L$, then $M$ rejects $x$.

Thus, we get the following definition of nonrecognition.

::: {prf:definition} Finite automata nonrecognition

We say that $M$ *does not recognize* $L$ iff there exists an input
string $x$ such that:

- $x$ is in $L$ and $M$ rejects $x$, or
- $x$ is not in $L$ and $M$ accepts $x$.

Such an input string $x$ is called a *counterexample* to $M$, as it
proves that $M$ does not recognize $L$.

:::

This then leads to the following definition of nonregularity.

::: {prf:definition} Nonregular languages

A language $L$ is *nonregular* iff there is counterexample to every DFA
$M$, i.e. for every DFA $M$, there exists an input string $x$ such that
either:

- $x$ is in $L$ and $M$ rejects $x$, or
- $x$ is not in $L$ and $M$ accepts $x$.

:::

### Memorylessness of DFAs, formalised

Let $M$ be a DFA. Recall the definition of the state transition function
$\delta$: given current state $q$ and after processing the next input
symbol $a$, the DFA $M$ moves to state $\delta(q,a)$ that the transition
function $\delta$ tells us the next state given its current state and
the next input symbol. Recall also that the *resulting state* of an
input string $w$ is the state that $M$ ends up in after processing $w$,
and is denoted by $q(w)$.

We now define the extended transition function $\delta^*$ which tells us
the resulting state after processing a string starting from a given
state. See the following for a formal definition.

::: {prf:definition} Extended transition function $\delta^*$

Let $M$ be a deterministic finite automaton with

1.  Finite input alphabet $Σ$
2.  Set of states $Q$
3.  Initial state $q₀$
4.  State transition function $\delta : Q × \Sigma → Q$
5.  Subset of accepting states $F$

The *extended transition function*
$\delta^* : Q \times \Sigma^* \rightarrow Q$ is defined recursively as
follows[^3]:
$$δ^*(q,w) =
\begin{cases}
q  & \text{if $w = \epsilon$}\\
\delta(r,a) & \text{if $w = xa$ for some $a \in \Sigma$ and $r = \delta^*(q,x)$}
\end{cases}$$

:::

::: {note} Iterative definition of $\delta^*$

We remark that $\delta^*(q,w)$ can be viewed as an iterated application
of $\delta$. To see this, let $n$ be the length of $w$ and $w_i$ be the
$i$-th symbol of $w$. Unfolding the recurrence in the recursive
definition of $\delta^*$ gives
$$\delta^*(q,w) = \delta(\delta(\cdots \delta(\delta(q, w_1), w_2)\cdots), w_{n-1}), w_n)$$
For example, $\delta^*(q,01) = \delta(\delta(q,0),1)$.

:::

::: {note} Diagrammatic representation of $\delta^*$

As can be seen from the above note, the notation for iterated
application of $\delta^*$ can get unwieldy. It will be more convenient
to represent the expression $\delta^*(q,w) = r$ using the following
diagram:
$$q \xrightarrow{w} r$$

:::

::: {important} Key takeaway from the definition of $\delta^*$

Observe that $\delta^*(q,z)$ depends only on the state $q$, the string
$z$ and the state transition function $\delta$. Intuitively, this means
that after the finite automata processes a prefix of the input string,
the remaining state transitions and more importantly, whether the input
string is accepted or not, depends **only** on the **current state** and
the **remaining input string, i.e. the suffix**.[^4][^5]

:::

Next, we obtain some important consequences of the definition of
$\delta^*$. Let $w$ be an input string. It immediately follows from the
definition that the resulting state $q(w)$ is equal to
$\delta^*(q_0,w)$.

The below observation also immediately follows from the definition but
will be important later on.

::: {prf:observation} Prefix-Suffix Decomposition
:label: obs-nonreg-decomp

Let $w$ be an input string, and suppose we decompose $w$ into a prefix
$x$ and a suffix $y$, i.e. $w = xy$, and let $r = \delta^*(q_0,x)$, i.e.
$r$ is the resulting state of the prefix $x$. Then,
$\delta^*(q_0, w) = \delta^*(r,y)$.[^6]

In other words, the computation path of $M$ on $w$ is the computation
path on the prefix $x$ starting from the initial state $q_0$ followed by
the computation path on the suffix $y$ starting from where the first
path ended, i.e. the resulting state of the prefix. We represent this
using the following diagram:

$$q_0 \xrightarrow{x} r \xrightarrow{y} q(w)$$

:::

:::::: {prf:example} Examples of @obs-nonreg-decomp

Consider the following finite automata.

::: {image} ./path-automaton.png
:width: 300px
:::

Here are some examples of @obs-nonreg-decomp:

1.  Input string $01011$ with prefix $010$ and suffix $11$, we get
    $$  \delta^*(q_0, 010) = q_2 \text{ and } \delta^*(q_2, 11) = q_1.
      $$ Equivalently,
    $$  q_0 \xrightarrow{010} q_2 \xrightarrow{11} q_1
      $$
2.  Input string $111$ with prefix $1$ and suffix $11$, we get
    $$  \delta^*(q_0, 1) = q_2 \text{ and } \delta^*(q_2,11) = q_1.
      $$ Equivalently,
    $$  q_0 \xrightarrow{1} q_2 \xrightarrow{11} q_1
      $$
3.  Input string 0011 with prefix $00$ and suffix $11$, we get
    $$  \delta^*(q_0, 00) = q_2 \text{ and } \delta^*(q_2,11) = q_1.
      $$ Equivalently,
    $$  q_0 \xrightarrow{00} q_2 \xrightarrow{11} q_1
      $$

::::::

Looking at the above examples more closely, we see a pattern: if two
strings $x$ and $y$ have the same resulting state, then appending the
same string $z$ to both $x$ and $y$ yield strings $xz$ and $yz$ with the
same resulting state. Moreover, it does not matter what $z$ is, i.e. the
statement holds true for every $z$.

In fact, this is not a coincidence and holds for all DFA. We state it
more formally in the following lemma.[^7]

::: {prf:lemma} Memorylessness Lemma
:label: lem-dist1

Let $M$ be a DFA, and let $x$ and $y$ be strings.

If $x$ and $y$ have the same resulting state, then, for every string $z$, the strings $xz$ and $yz$ also have the same resulting state (i.e. $q(xz) = q(yz)$), and as a consequence: $M$ either accepts both $xz$ and $yz$, or rejects both of them.

Equivalently, if there exists a string $z$ such that $M$ accepts exactly one of $xz$ and $yz$ and rejects the other, then $x$ and $y$ do not have the same resulting state (i.e. $q(x) \neq q(y)$).

:::

### Warm up: Ruling out single-state DFAs

@lem-dist1 is central to the fooling set technique. Before we proceed
with the fooling set technique, let's see how analyzing resulting states
is useful. In particular, we will prove the following characterization
of the languages recognized by DFAs with a single state. The
characterization allows us to rule out single-state DFAs for some
languages $L$: no DFA with 1 state can recongize $L$ if $L$ is not the
empty language or the language of all strings.

::: {prf:lemma} Characterization of languages of single-state DFA
:label: lem-nonreg-single

A single-state DFA either accepts every string or rejects every string.
Consequently, if $L$ is neither the empty language nor the language of
all strings, then $L$ cannot be recognized by a single-state DFA.

:::

::: {prf:proof}
:enumerated: false

Let $M$ be a single-state DFA. Since $M$ has only one state, the resulting state of every string is the initial state. If the initial state is accepting, then the resulting state of every string is an accepting state and so $M$ accepts every string. If the initial state is not accepting, then $M$ rejects every string.

:::

### Distinguishable pairs and distinguishing suffixes

@lem-dist1 captures the essence of the memorylessness of finite
automata. To use it to show that certain DFA cannot recognize a language
$L$, we will need the following definition.

::: {prf:definition} Distinguishable pairs and distinguishing suffixes
:label: def-nonreg-suffix

Let $L$ be a language. A pair of strings $x$ and $y$ is a
*distinguishable pair* with respect to $L$ iff there exists a string $z$
such that exactly one of $xz$ and $yz$ is in $L$ and the other is not.
The string $z$ is said to be a *distinguishing suffix* of the pair
$x,y$.

:::

We also sometimes say that $x$ and $y$ are distinguishable with respect
to $L$, and when the language $L$ is clear from context, we also say $x$
and $y$ are distinguishable.

Let us take a look at some examples and exercises to consolidate our
understanding of the definition.

### Examples of distinguishable pairs and distinguishing suffixes

::: {prf:example}

Let $L$ be the language of even-length bit strings:
$$\epsilon, 00, 01, 10, 11, 0000, 0001, 0010, 0011$$

The strings $\epsilon$ and 0 are distinguishable with distinguishing
suffix $\epsilon$:

- $\epsilon\epsilon = \epsilon$ which is in $L$
- $0\epsilon = 0$ which is not in $L$.

:::

::: {exercise}

Let $L$ be the language of bit strings ending in 10. Give a pair of
distinguishable strings for $L$ along with their distinguishing suffix.

:::

::: {exercise}

Let $L$ be the language of bit strings whose length is divisible by 5.
Give a pair of distinguishable strings for $L$ along with their
distinguishing suffix.

:::

### Distinguishable strings have different resulting states

Next, we connect the notion of distinguishable strings and @lem-dist1 to
show that every DFA that recognizes $L$ must satisfy a certain
condition.

::: {prf:lemma}
:label: lem-dist2
Let $L$ be a language. For every pair of strings $x$ and $y$ that are distinguishable with respect to $L$, and every DFA $M$ that recognizes $L$, the strings $x$ and $y$ cannot have the same resulting state in $M$.

:::

::: {prf:proof}
:enumerated: false

Let $x$ and $y$ be distinguishable with respect to $L$, and $z$ be their
distinguishing suffix. Let $M$ be a DFA that recognizes $L$.

By definition of distinguishing suffixes, exactly one of $xz$ and $yz$
is in $L$ and the other is not in $L$. Since $M$ recognizes $L$, it also
accepts exactly one of $xz$ and $yz$ and rejects the other. Therefore,
$x$ and $y$ must have different resulting states in $M$, as otherwise,
@lem-dist1 implies that $M$ either accepts both $xz$ and $yz$, or
rejects both of them.

:::

An immediate consequence of @lem-dist2 is that any language with at
least one distinguishable pair (such as the ones in the example and
exercises above) cannot be recognized by a single-state DFA. Indeed,
there are only 2 languages without any distinguishable pair: the empty
language and the language of all strings. Observe that this gives a
different proof of the characterization of the languages accepted by
single-state DFA (@lem-nonreg-single).

### Fooling sets

Next, we show how to use @lem-dist2 to rule out DFAs with more than 1
state.

::: {prf:definition} Fooling Set
:label: def-nonreg-fooling

A set of strings $F$ is a *fooling set* for a language $L$ iff every
pair of distinct strings $x \neq y$ in $F$ is distinguishable with
respect to $L$.

:::

::: {tip}

If you find the following proof hard to follow, the next [subsection](#sec-nonreg-explicit) has more explicit proofs that you may find easier to follow.

:::

::: {prf:lemma} Fooling Set Lemma
:label: lem-nonreg-fool

Let $L$ be a language and $F$ be a fooling set for $L$. If $F$ has size
$k$, then every DFA recognizing $L$ has at least $k$ states.

Moreover, if for every $k$, there exists a fooling set $F_k$ of size
**at least** $k$, then $L$ is not regular.

:::

::: {prf:proof}
:enumerated: false

We begin by proving the first part of the lemma. Let $M$ be a DFA with
fewer than $k$ states and $F$ be a fooling set for $L$ of size $k$.
Then, by the [pigeonhole
principle](https://en.wikipedia.org/wiki/Pigeonhole_principle), there
exist two strings $x$ and $y$ in $F$ with the same resulting state.
Since every pair of strings in $F$ are distinguishable, the strings $x$
and $y$ are distinguishable. Thus, @lem-dist2 implies that $M$ does not
recognize $L$.

Next, we prove the second part of the lemma. Suppose that for every $k$,
there exists a fooling set $F_k$ for $L$ of size at least $k$. The first
part of the lemma implies that for every $k$, there is no DFA with fewer
than $k$ states can recognize $L$. Since a DFA has a fixed, finite
number of states, this means that no DFA can recognize $L$.

:::

(sec-nonreg-explicit)=

### More explicit proofs of Fooling Set Lemma

Here's an alternative proof of the first part of the lemma that is more
explicit. See also the last page of the [Week 8 Lecture 2
slides](./w8l2-slides.pdf) for a visual illustration of the argument for
$k = 3$.

::: {prf:lemma} First part of Fooling Set Lemma (restated)

Let $L$ be a language and $F$ be a fooling set for $L$. If $F$ has size
$k$, then every DFA recognizing $L$ has at least $k$ states.

:::

::: {prf:proof}
:enumerated: false

Let $F = \{w_1, \ldots, w_k\}$ be a fooling set for $L$ of size $k$.
Suppose $M$ is a DFA that recognizes $L$. For each string $w_i$ in $F$,
define $r_i = q(w_i)$. Thus, $r_1, \ldots, r_k$ are the $k$ resulting
states of the $k$ strings $F$. We now use the fact that every pair of
string in $F$ is distinguishable to show that these $k$ states must all
be distinct (i.e. no two of them are the same) and thus, $M$ has at
least $k$ states.

Consider strings $w_i$ and $w_j$ and their respective resulting states
$r_i$ and $r_j$. Since $F$ is a fooling set, $w_i$ and $w_j$ are
distinguishable. Let $z$ be their distinguishing suffix and suppose
$w_iz$ is in $L$ and $w_jz$ is not in $L$.

Since $M$ recognizes $L$, we get that $M$ accepts $w_iz$ but not $w_jz$,
and so the two strings cannot have the same resulting state, i.e.
$q(w_iz) \neq q(w_jz)$. The [Memorylessness Lemma](#lem-dist1) then
tells us that $q(w_i) \neq q(w_j)$.

Applying the same argument to every pair of strings in $F$ and their
resulting states, we get that $r_i \neq r_j$ for every
$1 \leq i < j \leq k$. Since no 2 of the $k$ states $r_1, \ldots r_k$
are the same, $M$ must have at least $k$ states.

:::

Next, we give a more direct proof of the second part of the Fooling Set
Lemma, without relying on the first part.

::: {prf:lemma} Second part of Fooling Set Lemma (restated)
:label: lem-fooling-explicit

Let $L$ be a language. If for every $k$, there exists a fooling set
$F_k$ of size at least $k$ then $L$ is not regular.

:::

::: {prf:proof} 
:enumerated: false

As discussed [here](#sec-nonreg-meaning), to show $L$ is not regular, we
need to show that for every DFA $M$, there is a counterexample string
$x$ for $M$:

- $x$ is in $L$ and $M$ rejects $x$, or
- $x$ is not in $L$ and $M$ accepts $x$.

Let $M$ be a DFA. Suppose $M$ has $k$ states. We now use the fooling set
$F_{k+1}$ of size at least $k+1$ to find a counterexample string for
$M$. Let $w_1, \ldots, w_{k+1}$ be $k+1$ distinct strings from
$F_{k+1}$, and let $r_1, \ldots, r_{k+1}$ be their resulting states.

By the Pigeonhole Principle, two of these resulting states must be the
same. Let $w_i$ and $w_j$ be the two strings such that
$q(w_i) = q(w_j)$.

On the one hand, by [definition of fooling sets](#def-nonreg-fooling),
every pair of strings in $F$ has a distinguishing suffix. Thus, there
exists a distinguishing suffix $z^*$ for $w_i$ and $w_j$. By [definition
of distinguishing suffix](#def-nonreg-suffix), we have two cases:

1.  $w_iz^*$ is in $L$ and $w_jz^*$ is not in $L$, or
2.  $w_iz^*$ is not in $L$ and $w_jz^*$ is in $L$

On the other hand, the [Memorylessness Lemma](#lem-dist1) tells us that
since $q(w_i) = q(w_j)$, for every string $z$, we have
$q(w_iz) = q(w_jz)$. In particular, we have $q(w_iz^*) = q(w_jz^*)$.
Since $w_iz^*$ and $w_jz^*$ have the same resulting states, we also have
two cases:

1.  $M$ accepts both $w_iz^*$ and $w_jz^*$
2.  $M$ rejects both $w_iz^*$ and $w_jz^*$

In all of these cases, $M$ makes a mistake on either $w_iz^*$ or
$w_jz^*$, i.e. one of them is a counterexample and so $M$ does not
recognize $L$.

Applying the above argument to every DFA, we get that there is no DFA
that recognizes $L$. Therefore, $L$ is not regular.

:::

::: {tip}

Since the proof outlines a method to find a counterexample, you may find
it helpful to run through the argument in the proof on a specific
nonregular language $L$ and a specific DFA $M$. You should be able to
get a counterexample for that specific DFA. For example, you can take
any of the example DFAs, and the language $L = \{0^n1^n \mid n \geq 0\}$
and its fooling set given below.

:::

### Examples of fooling sets

::: {prf:example}

Let $L$ be the language of strings ending in 10. Then,
$F = \{\epsilon, 1, 10\}$ is a fooling set of size 3. Here are the
distinguishing suffixes for each pair of string in $F$:

- $(\epsilon, 1)$ has distinguishing suffix 0.
- $(\epsilon, 10)$ has distinguishing suffix $\epsilon$.
- $(1, 10)$ has distinguishing suffix 0.

We conclude that $L$ requires at least 3 states.

:::

::: {exercise}
:label: ex-equal01
Let $L = \{0^n1^n \mid n \geq 0\}$. Show that for every $k \geq 1$, there exists a fooling set $F_k$ of size at least $k$.

:::

::: {exercise}

Let $L$ be the language consisting of bit strings of the form $ww$ for
some bit string $w$. In other words, every string in $L$ is some string
$w$ concatenated with itself. So, $L$ contains
$$\epsilon, 00, 11, 1010, 0101, 1111, 101101, \ldots$$

Show that for every $k \geq 1$, there exists a fooling set $F_k$ of size
at least $k$.

:::

### Strategies for constructing fooling sets

There is no sure-fire way of constructing fooling sets for a language
$L$. Here are some general heuristics to try. Each of these were used
for the examples and exercises above.

1.  Construct fooling set using prefixes of strings in $L$.
2.  To construct $F_k$, consider strings for which it seems a counter
    that can count up to at least $k$ is needed to distinguish between
    them.
3.  Often, it is possible to construct a fooling set $F_k$ such that for
    every string $x \in F_k$, there is a string $z$ such that $z$
    distinguishes $x$ from the other strings in $F_k$, i.e. either $xz$
    is in $L$ but $yz$ is not in $L$ for every other $y$ in $F_k$, or
    vice versa.

::: {seealso}

- Section 1.2 of [A Note on Proving Non-Regularity via Fooling
  Sets](https://chekuri.cs.illinois.edu/teaching/fooling-sets.pdf)
- The boxed text at the top of page 17, just before Section 3.9, of
  [Lecture Notes on Finite-State
  Machines](http://jeffe.cs.illinois.edu/teaching/algorithms/models/03-automata.pdf)

:::

::: {tip}

We only need to show that for every $k$, there exists a fooling set of
size **at least** $k$. We do **not** need to show that there exists a
fooling set of size **exactly** $k$ for every $k$.

:::

::: {caution} **Every pair** must be distinguishable

A common pitfall is constructing a set of strings in which not every
pair of strings is distinguishable. The proof needs every pair to be
distinguishable as the pigeonhole principle only tells us that there is
*some* pair $x$ and $y$ with the same resulting states[^8], and our
proof needs to work no matter what pair it is.

For example, it is easy to design a 2-state finite automata that
recognizes the language of even-length bit strings. On the other hand,
consider the set of strings $S$ consisting of the empty string
$\epsilon$ as well as all odd-length strings. Pairing $\epsilon$ with
any odd-length string $x$ gives a distinguishing pair with
distinguishing suffix $0$. Thus, $S$ contains infinitely many
distinguishable pairs but does not imply that the language is not
regular.

:::

### More examples of fooling sets

::: {exercise}

Let $L$ be the language consisting of bit strings where 0 and 1 occur
the same number of times:
$$\epsilon, 01, 10, 1001, 0110, 000111, 010101, 101010, \ldots$$

Show that for every $k \geq 1$, there exists a fooling set $F_k$ of size
at least $k$.

:::

::: {exercise}

A *palindrome* is a string that is the same as its reverse. Let $L$ be
the language of palindromes over the usual Latin alphabet consisting of
letters from a to z. Examples of palindromes in $L$ include: abba,
civic, deed, kayak, level, radar, tacocat[^9]

Show that for every $k \geq 1$, there exists a fooling set $F(k)$ of
size at least $k$.

:::

### Fooling sets and minimization

The fooling set technique can also be used to prove that a DFA $M$ is
minimal for a language $L$: if there exists a fooling set $F$ for $L$ of
size exactly equal to the number of states of $M$, then $M$ is minimal.
This is interesting as the fooling set $F$ is a **witness** to the
minimality of $M$.

::: {exercise}

Let $L$ be the language of even-length bit strings. Give a DFA $M$ that
recognizes $L$ and a fooling set $F$ of size equal to the number of
states of $M$.

:::

## Technique 2: Closure Properties

Now that we have several languages that we have shown to be nonregular,
we switch to a different technique that can often result in a shorter
proof but can be trickier to apply.

Recall that regular languages are closed under several operations such
as:

1.  complement
2.  intersection

These operations let us take regular languages and create new regular
languages:

1.  if $L$ is regular, then $L^c$ is also regular.
2.  if $A$ and $B$ are regular, then so is $A \cap B$.

We can also use them to prove nonregularity as follows:

1.  if $L^c$ is not regular, then $L$ cannot be regular.
2.  if $A$ is regular and $A \cap B$ is not regular, then $B$ cannot be
    regular.

### Example applications

::: {prf:example}

Let $L$ be the language of bit strings where 0 and 1 do not occur the
same number of times. Since $L^c$ is the language of bit strings where 0
and 1 occur the same number of times, and we have already shown $L^c$ is
not regular in @ex-equal01, we get that $L$ is not regular.

:::

::: {prf:example}

Let $B$ be the language of bit strings where 0 and 1 occur the same
number of times.

We have already shown using that $B$ is not regular in @ex-equal01 via
fooling sets. We now give a shorter proof using the fact that the
language $L = \{0^n1^n \mid n \geq 0\}$ is not regular. Observe that
$L = A \cap B$ where $A = 0^*1^*$, i.e. the language of strings in which
0s can only appear to the right of 1, and vice versa. Since $A$ is
regular but $A \cap B$ is not, then $B$ cannot be regular.

:::

### Other operations

In general, one can use any operation that the regular languages are
closed under. Other operations include:

1.  union
2.  concatenation
3.  Kleene star (aka Kleene closure)

## Fooling sets vs closure properties

Here are the benefits and drawbacks to the two techniques:

1.  If a language $L$ is not regular, one can always prove its
    nonregularity using fooling sets.[^10] This is not always the case
    for the closure property technique.[^11]
2.  Even if a language can be proved nonregular using closure
    properties, it can be quite tricky to find the right languages and
    operations.
3.  Proofs via closure properties tend to be much shorter.

One can also combine the two. For example, suppose you want to show $B$
is nonregular but are having difficulty finding fooling sets for $B$.
Then, you can try to find a regular language $A$ such that it is easier
to find fooling sets for $A \cap B$. In other words, closure of
intersection lets you *reduce* the task of finding fooling sets for $B$
to finding fooling sets for $A \cap L$. More generality, it lets you
reduce the task of proving nonregularity of $B$ to proving nonregularity
of $A \cap B$.

[^1]: One main difference between these and my lecture notes is that I
    have tried to avoid proof by contradiction as much as possible as
    students who have not encountered them before find them very
    confusing, and I have tried to minimize use of mathematical notation
    and jargon.

[^2]: Chandra also discusses the Pumping Lemma approach and the
    differences between the two approaches.

[^3]: For the mathematically inclined, we can express the second case
    more succinctly as $\delta^*(q,w) = \delta(\delta^*(q,x),a)$.

[^4]: For a more concrete example, think back to the tally counter from
    @sec-tally. The counter does not remember how many times it has been
    reset. Pressing the increment counter 5 times when it is in state
    0000 always results in 0005.

[^5]: If you are familiar with Markov chains, this should remind you of
    a similar [property of Markov
    chains](https://en.wikipedia.org/wiki/Markov_property).

[^6]: For the mathematically inclined, we can express this more
    succinctly as $\delta^*(q_0,w) = \delta^*(\delta^*(q,x),y)$.

[^7]: The name of the lemma is something I came up with, not a standard
    name used by others.

[^8]: The pair can depend on the exact specification of $M$.

[^9]: What is a tacocat? It's a 🐈 in a 🌮, of course! Here's
    [mine](./tacocat.jpg).

[^10]: In fact, the Myhill-Nerode Theorem says that $L$ is not regular
    if and only if for every $k$, there is a fooling set of size at
    least $k$.

[^11]: One might say the fooling set technique is *fool-proof*.
