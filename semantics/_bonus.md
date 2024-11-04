---
layout: math
title: Hoare Logic (Bonus Content)
nav_order: 6
mathjax: true
parent: Semantics
---

## Notice: This content is _not_ assessed, so feel free to skip if you are revising.

# Hoare Logic

Hopefully at this point you're convinced that semantics allows us to reason about programs in a non-trivial way, but you're potentially equally convinced that this is annoyingly fiddly.
Fortunately, program verification is rarely done by hand in this way.
It is much more common to consider a _program logic_. 
A program logic is a custom reasoning (or proof) system that is not aimed at proving arbitrary mathematical formulas, but specific properties about programs.
They can nicely obscure some of the final points of induction proofs.

> ### Aside 
> Sometimes people refer to program logics such as the one we will discuss below as _axiomatic semantics_ as they could in theory be taken as the behaviour of the program whereby meaning is given as the collection of statements that are true of the program, but this is a bit suspect...

The logic we will consider is called _Hoare Logic_ named after the famous computer scientist [https://en.wikipedia.org/wiki/Tony_Hoare](Tony Hoare).
As with operational semantics, Hoare logic is an inductively defined relation consisting of triples:

$$
  \{ b_1 \}\ S\ \{ b_2 \}
$$

where $b_1,\, b_2 \in \mathcal{B}$ are Boolean expressions and $S \in \mathcal{S}$ is a statement.

The meaning of such a triple is that, if for _any_ initial state $\sigma \in \mathsf{State}$ and _any_ final state $\sigma' \in \mathsf{State}$ such that $\llbracket b_1 \rrbracket_\mathcal{B}(\sigma) = \top$ and $S,\, \sigma \Downarow \sigma'$, we have that $\llbracket b_2 \rrbracket_\mathcal{B}(\sigma') = \top$.
Intuitively, this says that if $b_1$ is true before we execute the program, then $b_2$ will be true after we execute the program.

We can use these judgements (referred to as _Hoare triples_) to assert that a program behaves correctly.
For example, if $S$ was an implementation of our exponent function, then we might wish to derive a triple $\{ x = x0,\, y = y0,\, z = z0 \}\ S\ \{ z } 