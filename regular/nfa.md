---
layout: obs
title: Nondeterministic Finite State Automata
nav_order: 1
mathjax: true
parent: Finite State Automata
grand_parent: Reference
---

## Finite State Automata

A __(Nondeterministic) Finite State Automaton (NFA)__ (without epsilon) is a 5-tuple of shape:

$$
    (Q,\,\Sigma,\,\delta,\,q_0,\,F)
$$

whose components are restricted and named as follows:
* $Q$ is any *finite* set, called the __states__
* $\Sigma$ is any alphabet
* $\delta : Q \times \Sigma \to \mathcal{P}(Q)$ is called the __transition function__
* $q_0 \in Q$ is called the __start state__ (equivalently: __initial state__)
* $F \subseteq Q$ is called the __accept states__

Note, we use a simpler kind of NFA to those defined in Sipser because we have no need for "epsilon transitions".

### Language Represented

Let $M = (Q,\,\Sigma,\,\delta,\,q_0,\,F)$ be an NFA and let $w = w_1 \cdots{} w_n$ be a word over the alphabet $\Sigma$.

We say that $M$ __accepts__ $w$ just if there is a sequence of states $r_0,r_1,\ldots,r_n$ in $Q$ satisfying the following properties:
* $r_0 = q_0$
* $r_{i+1} \in \delta(r_i,\,w_{i+1})$
* $r_n \in F$

The __language__ of $M$ is the set $L(M) = \\{w \mid \text{$M$ accepts $w$}\\}$. 

### Determinism

An NFA is called __deterministic__ just if, at each state there is exactly one outgoing transition for each letter of the alphabet, i.e. for all $$q \in Q$$ and all $a \in \Sigma$, $$\vert \delta(q,a) \vert = 1$$.  If an automaton is deterministic, we will often refer to it as a __deterministic finite state automaton (DFA)__.