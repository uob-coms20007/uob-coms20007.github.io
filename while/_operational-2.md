---
layout: math
title: Operational Semantics (II)
nav_order: 4
mathjax: true
parent: Semantics

---
# Operational Semantics (II)

## Conditional Statements

Let's now turn to the rules for conditional statements.
The behaviour of an if statement naturally depends on the value of its branching condition.
If the condition is true, the first statement will be executed; otherwise, the second statement will be executed.
Correspondingly, there are two inference rules that cover each case.

$$
  \begin{array}{cc}
    \dfrac
    {s_1,\, \sigma \Downarrow \sigma'}
    {\mathsf{if}\ e\ \mathsf{then}\ s_1\ \mathsf{else}\ s_2,\, \sigma \Downarrow \sigma'}
    \llbracket e \rrbracket_B(\sigma) = \top

    &
    \dfrac
    {s_2,\, \sigma \Downarrow \sigma'}
    {\mathsf{if}\ e\ \mathsf{then}\ s_1\ \mathsf{else}\ s_2,\, \sigma \Downarrow \sigma'}
    \llbracket e \rrbracket_B(\sigma) = \bot
  \end{array}
$$

To the left of each of these inference rules is a condition known as the _side condition_.
The side condition is a caveat to the rule that restricts the instances in which it applies.
For example, the first side should be read as: for any Boolean expression $e$, statements $s_1$ and $s_2$, and states $\sigma$ and $\sigma'$ _such that_ $\llbracket e \rrbracket_B(\sigma) = \top$, if $s_1,\, \sigma \Downarrow \sigma'$ then $\mathsf{if}\ e\ \mathsf{then}\ s_1\ \mathsf{else}\ s_2,\, \sigma \Downarrow \sigma'$.
This means that we can use this rule to derive the final state if the branch condition $x \leq 2$ and the initial state were $[x \mapsto 0]$ for example.

The side conditions are treated differently from premises as they constrain the instances a metavariable may take, rather than relating to other instances of the inductively defined relation.

## While Statement

Finally, let's look at while statements.
As with conditionals, the behaviour of a while statement $\mathsf{while}\ e\ \mathsf{do}\ s$ is characterised by two cases:

  - If the branch condition $e$ is not true, then the statement does nothing.

  - If, on the other hand, the branch condition $e$ is true, then the statement executes the sub-statement $s$ before repeating the process.

Correspondingly, there are two inference rules:

$$
  \begin{array}{cc}
    \dfrac
    {}
    {\mathsf{while}\ e\ \mathsf{do}\ s, \sigma \Downarrow \sigma}
    \llbracket e \rrbracket_B(\sigma) = \bot

    &
    \dfrac
    {s,\, \sigma \Downarrow \sigma'
      \quad \mathsf{while}\ e\ \mathsf{do},\, \sigma' \Downarrow \sigma''}
    {\mathsf{while}\ e\ \mathsf{do}\ s,\, \sigma \Downarrow \sigma''}
    \llbracket e \rrbracket_B(\sigma) = \top
  \end{array}
$$

The side condition on the first rule ensures that it is only applicable when the branch condition evaluates to false in the initial state.
Additionally, this rule does not change the 

Conversely, the side condition on the second rule ensures that it is only applicable when the branch condition evaluates to true in the initial state.

## A Worked Example