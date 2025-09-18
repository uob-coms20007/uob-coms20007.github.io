---
layout: math
title: The Halting Problem
nav_order: 10
mathjax: true
parent: Computability
---

# The Halting Problem

We will now prove a theorem due to [Alan Turing](https://en.wikipedia.org/wiki/Alan_Turing).

Suppose that we want to solve the following problem. Given
1. the source code of a program
2. an input for the program

we seek to determine

- whether the program will terminate on that input, or go into an infinite loop.

An infinite loop might arise in many circumstances. For example, an
[off-by-one error](https://en.wikipedia.org/wiki/Off-by-one_error) might
cause a while loop to fail to terminate. Alternatively confusing a $\leq$
with a $\ge$ might lead to wrong code that runs forever. We would ideally
like a tool - a debugger, or perhaps a fancier [static
analyzer](https://en.wikipedia.org/wiki/Static_program_analysis) - that can
tell us at compile-time that we have made a serious mistake of the sort.

Turing proved that this is impossible.

*Theorem.* The predicate

$$
  \textsf{HALT} = \{ \langle \ulcorner S \urcorner, n \rangle \mid ⟦ S ⟧_\texttt{x}(n) \downarrow \}
$$

is undecidable.

Let us unpack this statement. First, it refers to a predicate, i.e. a set of
'good' numbers. It says that this predicate is undecidable: no While program
can decide whether a particular number belongs to this set or not. By the
[Church-Turing thesis](https://uob-coms20007.github.io/reference/computability/church-turing.html#Church-Turing-thesis),
no algorithm written in any language can decide it either.

The 'good' numbers in this set arise as pairs of the form $\langle e, n
\rangle$. The first component must of the form $e = \ulcorner S \urcorner$, i.e.
the encoding of a While-program $S$ under the Gödel numbering $\ulcorner -
\urcorner$. The second component $n$ can be any number.

Disregarding all encodings for a second, the theorem says that: if we are given
the source code $S$ of a program and an input $n$, then we cannot
algorithmically tell if running $S$ with initial store $[\texttt{x} \mapsto n]$
will ever terminate with $\langle \texttt{skip}, [\texttt{x} \mapsto m] \rangle$
for some $m \in \mathbb{N}$. Thus, it is impossible to decide the _termination_
of a program (i.e. the absence of infinite loops) just by looking at its
source code and input!

This is known as the **Halting Problem.** No digital computer can ever solve
it. (And neither can quantum computers, by the way.)

We will prove that the set $\textsf{HALT}$ is undecidable. The proof method
will be through a technique known as _diagonalisation_.

*Proof.* Suppose that we can decide $\textsf{HALT}$. We will derive a
contradiction from this assumption.

That we can decide $\textsf{HALT}$ means that its characteristic function is
computable; suppose that it is computed by a While program $\texttt{H}$ wrt
`x`.

We then write the following program $\texttt{D}$:

```
  x := ⟨x, x⟩;
  H;
  if (x = 1) then {
    while (true) do skip
  }
```

The first line, namely `x := ⟨x, x⟩`, is a shorthand for a piece of code that
replaces the value of `x` with the value of the pair of `x` and `x`. It is one
of our implicit assumptions here that this computing this 'diagonal' is
possible.

By the structure of $\texttt{D}$, we have for any While program $\texttt{S}$ that

$$
  \begin{aligned}
    ⟦ \texttt{D} ⟧_\texttt{x}(\ulcorner \texttt{S} \urcorner) \uparrow
      &⟺\ ⟦ \texttt{H} ⟧_\texttt{x}(\langle \ulcorner \texttt{S} \urcorner, \ulcorner \texttt{S} \urcorner \rangle) \simeq 1 \\
      &⟺\  \langle \ulcorner \texttt{S} \urcorner, \ulcorner \texttt{S} \urcorner \rangle \in \textsf{HALT} \\
      &⟺\ ⟦ \texttt{S} ⟧_\texttt{x}(\ulcorner \texttt{S} \urcorner) \downarrow
  \end{aligned}
$$

That is to say: the program $\texttt{D}$ will _loop forever_ exactly when you
give it the source code of a program $\texttt{S}$ which is going to _terminate_
when fed its own source code as input. The program $\texttt{D}$ works this out
by using the program $\texttt{H}$, which we have assumed decides the Halting
Problem, as a subroutine.

[Make sure you understand this last paragraph well before moving on!]

Consider running the program $\texttt{D}$, giving it its own source code as input (!). Then the above equivalence becomes

$$
  ⟦ \texttt{D} ⟧_\texttt{x}(\ulcorner \texttt{D} \urcorner) \uparrow\
    ⟺\
  ⟦ \texttt{D} ⟧_\texttt{x}(\ulcorner \texttt{D} \urcorner) \downarrow
$$

which is an evident contradiction. 

We reached this contradiction by assuming that $\textsf{HALT}$ was decidable.
So it cannot be. ▣


# Semidecidability of the Halting Problem

Turing's result shows that $\textsf{HALT}$ is not decidable.

However, it is not very difficult to show that

*Theorem.* $\textsf{HALT}$ is [semi-decidable](https://uob-coms20007.github.io/reference/computability/predicates.html#semi-decidable).

_Proof._ The semi-characterstic function of $\textsf{HALT}$ is computed by a program that works as follows.

On input $x$,
1. Decode $x = \langle \ulcorner \texttt{S} \urcorner, n \rangle$.
2. Simulate the running of program $\texttt{S}$ on input $n$.
3. If that simulation terminates in a state of the form $[\texttt{x} \mapsto m]$, return $m$. ▣

In the above proof we do not write an explicit program for the
semi-characteristic function. Instead, we merely describe its function. We
argue that we could elaborate this description into an actual program due to two factors:

* the fact that the [universal
  function](https://uob-coms20007.github.io/reference/computability/universal.html)
  is computable, so the idea of "simulating" a program on an input is not
  unrealistic; and
* the fact that everything else we do is vaguely realistic in terms of
  programming, so by the [Church-Turing
  thesis](https://uob-coms20007.github.io/reference/computability/church-turing.html)
  must be achievable through a While program.

From now on our proofs will often be of that form.