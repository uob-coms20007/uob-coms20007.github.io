---
layout: math
title: Product Construction
nav_order: 3
mathjax: true
parent: Finite State Automata
---

## The Product Construction

Suppose we have two DFA over the same alphabet:

$$
    \begin{array}{l}
      M_1 = (Q_1,\,\Sigma,\,\delta_1,\,q_1,\,F_1) \\
      M_2 = (Q_2,\,\Sigma,\,\delta_2,\,q_2,\,F_2) 
    \end{array}
$$

The __product automaton__ is the DFA defined as $(Q,\,\Sigma,\,\delta,\,q_0,\,F)$,
where:
* $Q = Q_1 \times Q_2$
* $\delta((p_1,\,p_2),a) = (p_1',\,p_2')$ where $p_1' = \delta_1(p_1,\,a)$ and $p_2' = \delta_2(p_2,\,a)$.
* $q_0 = (q_1,\,q_2)$
* $F = \\{(p,q) \in Q \mid p \in F_1 \wedge q \in F_2\\}$
