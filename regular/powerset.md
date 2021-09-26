---
layout: math
title: Powerset Construction
nav_order: 4
mathjax: true
parent: Finite State Automata
---

## Subset Construction

The __epsilon closure__ $E(P)$ of a set of states $P$ is defined by:
$$
  E(P) = \{ q \mid \exists p \in P.\: (p,\epsilon) \trs (q,\epsilon) \}
$$

Suppose $N = (Q_N,\,\Sigma,\,\delta_N,\,q_N,\,F_N)$ is an NFA.  Then we construct a DFA written $\mathsf{Pow}(N)$, called the __powerset automaton__, as follows:

$$
    (Q,\,\Sigma,\,\delta,\,Q_0,\,F)
$$

where:
* $Q = \mathcal{P}(Q_N)$
* \$$\delta(R,a) = \{\bigcup_{q \in R} E(\delta_N(q,a))\}$$
* \$$Q_0 = E(\{q_N\})$$
* $F = \\{R \in Q \mid R \cap F_N \neq \emptyset\\}$

This guarantees that $L(\mathsf{Pow}(N)) = L(N)$ (but the former is a DFA whereas the latter is an NFA).