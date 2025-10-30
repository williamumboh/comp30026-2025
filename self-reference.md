(sec-self-reference)=

# Undecidabilty and Unrecognizability by Self-Reference

The proof of the undecidability of $A_{TM}$ given in
@sec-diagonalization is different from the typical proof, which uses
self-reference to obtain a contradiction, and can be hard to understand
at first. In this section, we give proofs using self-reference as
self-reference is a key concept in computation and is essential for
proofs of undecidability using diagonalization.

Intuitively, the idea is similar to how self-reference can lead to
logical paradoxes such as the [liar
paradox](https://en.wikipedia.org/wiki/Liar_paradox) and the [barber
paradox](https://en.wikipedia.org/wiki/Barber_paradox) (also in Homework
problem P6.1). In one version of the liar paradox, there are two types
of people: truth-tellers who always tell true statements, and liars who
always tell false statements. Suppose you meet a person who declares "I
am a liar". If the statement is true, then the person is lying and so
the statement is false. On the other hand, if the statement is false,
then the person is telling the truth and so the statement is true. Since
the statement can either be true or false, and both possibilities lead
to contradictions.

## Unrecognizability by Self-Reference

::: {prf:theorem}

There exists a language $L_S$ that is not recognizable.

:::

:::::: {prf:proof}

As before, fix an encoding of Turing machines $M$ into strings
$\langle M \rangle$ and let $M_i$ be the $i$-th Turing machine in string
order of its encoding.

The language $L_S$ is defined as follows:

- if $w$ is not a valid encoding of a Turing machine, $w$ is not in
  $L_S$
- if $w$ is a valid encoding of some Turing machine $M_i$ (i.e.
  $w = \langle M_i \rangle$), then $w$ is in $L_S$ if and only if $w$ is
  not accepted by $M_i$ (i.e. $M_i$ either rejects or runs forever on
  $w = \langle M_i \rangle$)

Equivalently, $L_S$ consists of the encodings of Turing machines $M$
that do not accept their own encodings:
$$L_S = \{ \langle M \rangle \mid \langle M \rangle \notin L(M)\}.$$

::: {figure} ./unrecognizable-self-reference.png

Illustration of the construction of $L_S$.

:::

We now use proof by contradiction to show that there is no recognizer
for $L_S$.

Suppose, towards a contradiction, that there is a TM $R$ that recognizes
$L_S$. Thus, $R$ accepts $\langle M \rangle$ if and only if $M$ accepts
its own encoding $\langle M \rangle$.

What happens if we run $R$ on its own encoding $\langle R \rangle$?
There are two possibilities:

1.  either $R$ accepts its own encoding $\langle R \rangle$
2.  or $R$ does not accept $\langle R \rangle$.

We now show that both of these cases lead to contradictions, just like
in the paradox example above.

1.  If $R$ accepts $\langle R \rangle$, then $\langle R \rangle$ is not
    in $L_S$ by definition of $L_S$. But since $R$ recognizes $L_S$ and
    $\langle R \rangle$ is not in $L_S$, we get that $R$ does not accept
    $\langle R \rangle$, a contradiction.
2.  If $R$ does not accept $\langle R \rangle$, then $\langle R \rangle$
    is in $L_S$ by definition of $L_S$. But since $R$ recognizes $L_S$
    and $\langle R \rangle$ is in $L_S$, we get that $R$ accepts
    $\langle R \rangle$, a contradiction.

Since both possibilities lead to contradictions, and all reasoning steps
are valid except for the assumption that $L_S$ is recognizable, we
conclude that $L_S$ is in fact not recognizable.

::::::

## Undecidabilty of $A_{TM}$ by Self-Reference

::: {prf:theorem}

$A_{TM}$ is undecidable.

:::

::: {prf:proof}

Suppose, towards a contradiction, that there is a TM $H$ that decides
$A_{TM}$. Thus, $H$ accepts $\langle M, w \rangle$ if and only if $M$
accepts $w$.

We now construct a TM $D$ using $H$ that takes as input encodings of
Turing machines and behaves as follows:

    On input <M>:
      Run H on <M, <M>>
      Accept if H rejects
      Reject if H accepts

Since $H$ is a decider, we can indeed run $H$ on
$\langle M, \langle M \rangle \rangle$ so $D$ is also a decider.

Observe that for every Turing machine $M$, $D$ accepts
$\langle M \rangle$ if and only if $M$ does not accept
$\langle M \rangle$. So, if we run $D$ on its own encoding
$\langle D \rangle$, we get that $D$ accepts $\langle D \rangle$ if and
only if $D$ does not accept ⟨ D ⟩\$!

Since assuming that $A_{TM}$ is decidable leads to a contradiction, we
get that $A_{TM}$ is in fact undecidable.

:::

## Optional: Fun with Self-Reference

There are lots of fun things you can do with self-reference:

- quines are programs that print their own source code. This is how
  viruses replicate.
- creating a backdoored login program where the

Check out for more examples.
