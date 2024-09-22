---
layout: math
title: Operational Semantics
nav_order: 3
mathjax: true
parent: Semantics
---

# Operational Semantics

Operational semantics is an alternative approach to denotational semantics that emphasis the steps during the execution of the program.
We will provide an operational semantics for While programs.

Recall the grammar of programs (i.e. statements) is given as follows:

<div class="defn" markdown="1">
A __statement__ is an element of the following grammar:

$$
  \begin{array}{rcl}
    S &\longrightarrow& \mathsf{skip} \mid x \leftarrow A \mid S ; S \mid \mathsf{if}\ B\ \mathsf{then}\ S\ \mathsf{else}\ S \mid \mathsf{while}\ B\ \mathsf{do}\ S
  \end{array}
  $$

where $A$ stands for any arithmetic expression and $B$ stands for any Boolean expression.
</div>

As with denotational semantics, we will work with the abstract syntax tree of expressions rather than the string that produced them.
Therefore, we needn't consider parentheses or curly braces as part of this grammar.

## Big-Step Semantics

The type of operational semantics that we will employ is sometimes called _big-step_ operational semantics.
This framework involves defining a relation $S,\, \sigma_1 \Downarrow \sigma_2$ which says that the program $S$ in the initial state $\sigma_1$ executes leaving a final state $\sigma_2$.
The name big-step semantics refers to the fact that this relation doesn't expose any intermediary states - only the initial and final of the computation.

If you squint, it may seem that this is awfully similar to the denotational setup except we're using a relation instead of a function.
The difference lies in the way these relations/functions are defined.
Recall that the denotational semantics was given by _recursion_ over the expression.
For the operational semantics, we will define the relation _inductively_.

## Inductive Inference Systems

Already in this course, we have met some inductive defined sets.
In particular, the natural numbers are inductively defined and the set of arithmetic/Boolean expressions are inductively defined.
We can think of inductive definitions as  