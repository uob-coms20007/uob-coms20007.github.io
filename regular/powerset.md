---
layout: obs
title: Powerset Construction
nav_order: 3
mathjax: true
parent: Finite State Automata
grand_parent: Reference
---

## Powerset Construction

Suppose $N = (Q_N,\,\Sigma,\,\delta_N,\,q_N,\,F_N)$ is an NFA.  Then we construct a DFA written $\mathsf{Pow}(N)$, called the __powerset automaton__, as follows:

$$
    (Q,\,\Sigma,\,\delta,\,q,\,F)
$$

where:
* $Q = \mathcal{P}(Q_N)$
* \$$\delta(R,a) = \{\bigcup_{q \in R} \delta_N(q,a)\}$$
* \$$q = \{q_N\}$$
* $F = \\{R \in Q \mid R \cap F_N \neq \emptyset\\}$

This guarantees that $L(\mathsf{Pow}(N)) = L(N)$ (but the former is a DFA whereas the latter is an NFA).