---
layout: math
parent: Transition Systems
mathjax: true
title: Invariants
---

# Invariants

Fix a transition system $(C,\,\tr)$ and an "initial" configuration $c_0$.  

A property $P(c)$ of configurations is an __invariant__ just if $P$ is true for all the configurations reachable from $c_0$.

A property $P(c)$ of configurations is an __inductive invariant__ just if:
  * $P(c_0)$
  * and, for all $c,\,d \in C$: $P(c)$ and $c \tr d$ together imply $P(d)$
