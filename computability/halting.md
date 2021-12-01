---
layout: math
title: The Halting Problem
nav_order: 10
mathjax: true
parent: Computability
---

We will now prove a theorem due to Alan Turing.

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
tell us at compile-term that we have made a serious mistake of the sort in
our program.

Turing proved that this is impossible.

*Theorem.* The predicate

$$
  \textsf{HALT} = \{ \phi(\gamma(S), n) \mid ⟦ S ⟧_\texttt{x}(n) \downarrow \}
$$

is undecidable.

Let us unpack this statement. First, it refers to a predicate, i.e. a set of
'good' numbers. It says that this predicate is undecidable: no While program
can decide whether a particular number belongs to this set or not. By the
[Church-Turing thesis](https://uob-coms20007.github.io/reference/computability/church-turing.html#Church-Turing-thesis),
no algorithm written in any language can decide it either.

The 'good' numbers in this set arise as pairs of the form $\phi(a, b)$. $a$
must of the form $\gamma(S)$, i.e. the encoding of a While-program $S$ under
the Gödel numbering $\gamma$. $b$ can be any number.

Disregarding all encodings for a second, the theorem says that: if we are
given the source code $S$ of a program and an input $n$, then we cannot
algorithmically tell if running $S$ with initial store $[\texttt{x} \mapsto
n]$ will ever terminate with $\langle \texttt{skip}, \sigma' \rangle$. Thus,
it is impossible to decide the _termination_ of a program (i.e. the absence
of infinite loops) just by looking at its source code and input!

This is known as the **Halting Problem.** No digital computer can ever solve
it. (And neither can quantum computers.)

We will prove that the set $\textsf{HALT}$ is undecidable. The proof method
will be diagonalisation once more.

*Proof.* Suppose that we can decide $\textsf{HALT}$. We will derive a
contradiction from this assumption.

That we can decide $\textsf{HALT}$ means that its characteristic function is
computable; suppose that it is computed by a While program $\texttt{H}$ wrt
`x`.

We then write the following program $\texttt{D}$:

```
  x := ϕ(x, x);
  H;
  if (x = 1) then {
    while (true) do skip
  }
```

The first line, namely `x := ϕ(x, x)`, is a piece of code that replaces the
value of `x` with the value of the pair of `x` and `x`. It is one of our
implicit assumptions here that this computing this 'diagonal' is possible.

By the structure of $\texttt{D}$, we have for any While program $\texttt{S}$ that

$$
  \begin{aligned}
    ⟦ \texttt{D} ⟧_\texttt{x}(\gamma(\texttt{S})) \uparrow
      &⟺\ ⟦ \texttt{H} ⟧_\texttt{x}(ϕ(\gamma(\texttt{S}), \gamma(\texttt{S}))) \simeq 1 \\
      &⟺\  ϕ(\gamma(\texttt{S}), \gamma(\texttt{S}))\in \textsf{HALT} \\
      &⟺\ ⟦ \texttt{S} ⟧_\texttt{x}(\gamma(\texttt{S})) \downarrow
  \end{aligned}
$$

Consider running the program $\texttt{D}$, giving it its own source code as input (!). Then the above equivalence becomes

$$
  ⟦ \texttt{D} ⟧_\texttt{x}(\gamma(\texttt{D})) \uparrow
    ⟺
  ⟦ \texttt{D} ⟧_\texttt{x}(\gamma(\texttt{D})) \downarrow
$$

which is an evident contradiction. 

We reached this contradiction by assuming that $\textsf{HALT} was decidable.
So that cannot be. ▣
