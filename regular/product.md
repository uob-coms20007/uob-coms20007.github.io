---
layout: obs
title: Product Construction
nav_order: 2
mathjax: true
parent: Finite State Automata
grand_parent: Reference
---

## The Product Construction

Suppose we have two DFA over the same alphabet:

$$
    \begin{array}{l}
      M_1 = (Q_1,\,\Sigma,\,\delta_1,\,q_1,\,F_1) \\
      M_2 = (Q_2,\,\Sigma,\,\delta_2,\,q_2,\,F_2) 
    \end{array}
$$

The __product automata__ are the family of DFA defined as $(Q,\,\Sigma,\,\delta,\,q_0,\,F)$, for some choice of $$F \subseteq Q$$,
where:
* $Q = Q_1 \times Q_2$
* $\delta((p_1,\,p_2),a) = \{(p_1',\,p_2')\}$ where $$\{p_1'\} = \delta_1(p_1,\,a)$$ and $$\{p_2'\} = \delta_2(p_2,\,a)$$.
* $q_0 = (q_1,\,q_2)$

Setting $$F := F_1 \times F_2$$ we obtain the __intersection automaton__ which we shall write as $$M_1 \otimes M_2$$.  This definition ensures that $$L(M_1 \otimes M_2) = L(M_1) \cap L(M_2)$$.

Setting $$F := (F_1 \times Q) \cup (Q \times F_2)$$ we obtain the __union automaton__ which we shall write as $$M_1 \oplus M_2$$.  This definition ensures that $$L(M_1 \oplus M_2) = L(M_1) \cup L(M_2)$$.
