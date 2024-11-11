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
They can nicely circumnavigate some of the finer points of induction proofs.

> ### Aside 
> Sometimes people refer to program logics such as the one we will discuss below as _axiomatic semantics_ as they could in theory be taken as the behaviour of the program whereby meaning is given as the collection of statements that are true of the program, but this is a bit suspect...

The logic we will consider is called _Hoare Logic_ named after the famous computer scientist [https://en.wikipedia.org/wiki/Tony_Hoare](Tony Hoare).
As with operational semantics, Hoare logic is an inductively defined relation consisting of triples:

$$
  \{ b_1 \}\ S\ \{ b_2 \}
$$

where $b_1,\, b_2 \in \mathcal{B}$ are Boolean expressions, referred to as the pre- and post-condition respectively, and $S \in \mathcal{S}$ is a statement.

The meaning of such a triple is that, if for _any_ initial state $\sigma \in \mathsf{State}$ and _any_ final state $\sigma' \in \mathsf{State}$ such that $\llbracket b_1 \rrbracket_\mathcal{B}(\sigma) = \top$ and $S,\, \sigma \Downarow \sigma'$, we have that $\llbracket b_2 \rrbracket_\mathcal{B}(\sigma') = \top$.
Intuitively, this says that if $b_1$ (the pre-condition) is true before we execute the program, then $b_2$ (the post-condition) will be true after we execute the program.

We can use these judgements (referred to as _Hoare triples_) to assert that a program behaves correctly.
For example, if $S$ was an implementation of our exponent function, then we might wish to derive a triple $\{ x = x0 && y = y0 \}\ S\ \{ z = x0^y0 \}$.
Notice that the post-condition doesn't specify anything about the variables $x$ and $y$; this simplification is available as we are not specifying initial and final states but _properties_ that are true of states.
Additionally, these triples can be reused as they apply universally allow us to perform program verification in a _compositional_ manner.

The inference rules for Hoare logic are given bellow:

$$
  \begin{array}{cc}
    \dfrac
    {}
    {\{ b \}\ S\ \{ b \}}

    &
    \dfrac
    {}
    {\{ b[x \mapsto e] \}\ x \leftarrow e\ \{ b \}}
    \\[10pt]

    \multicolumn{2}{c}{
      \dfrac
      {
        {\{ b_1 && b \}\ S_1\ \{ b_2 \}}
        \quad


        {\{ b_1 && !b \}\ S_2\ \{ b_2 \}}
      }
      {\{ b_1 \}\ \mathsf{if}\ b\ \mathsf{then}\ S_1\ \mathsf{else}\ S_2\ \{ b_2 \}}
    }
    \\[10pt]

    \multicolumn{2}{c}{
      \dfrac
      {
        {\{ b && e \}\ S\ \{ b \}}
      }
      {\{ b \}\ \mathsf{while}\ e\ \mathsf{do}\ S\ \{ b && !e \}}
    }
  \end{array}
$$

The most interest of these rules is the rule for $\mathsf{while}$ statements as it allows us to show a universal property of program execution without any inductive reasoning.
This rule relies on the notion of a _loop invariant_ which is a property that is preserved by any number of iterations of the loop.