---
layout: math
title: Regular Languages
nav_order: 5
mathjax: true
parent: Finite State Automata
---

## Regular Languages

A language $A$ is said to be a __regular language__ just if it is recognised by some DFA, i.e. just if there is some DFA $M$ such that $L(M) = A$.

*Theorem*: The following statements regarding some language $A$ are equivalent:
  * $A$ is regular
  * there is a DFA that recognises $A$
  * there is an NFA that recognises $A$
  * there is a regular expression that denotes $A$

## Closure Properties

The regular languages are closed under various operations.  This means that, if you take some regular languages and apply the operations then the output is guaranteed to again be regular.

*Theorem* (__Regular Closure__): The regular languages are closed under intersection, union, complement, concatenation and Kleene star.
  * Given two regular languages $A$ and $B$, $A \cap B$ is regular.
  * Given two regular languages $A$ and $B$, $A \cup B$ is regular.
  * Given a regular language $A$, $A^c = \\{w \in \Sigma^* \mid w \notin A\\}$ is regular.
  * Given two regular languages $A$ and $B$, $A \cdot B = \\{wv \mid w \in A \wedge v \in B\\}$ is regular.
  * Given a regular language $A$, $A^*$ is regular.

## Limitations

*Theorem* (__Pumping Lemma__): If $A$ is regular, then there is a number $p > 0$ such that, for all $s \in A$ of length at least $p$, $s$ can be divided as $s = uvw$ and:
  1. $v$ is not empty
  2. the length of $uv$ is at most $p$
  3. for each $i \in \mathbb{N}$: $uv^iw \in A$

*Theorem* (__Irregularity of $b^nc^n$__): The language $\\{b^nc^n \mid n \in \mathbb{N}\\}$ is *not* regular.