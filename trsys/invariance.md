---
layout: math
parent: Transition Systems
mathjax: true
title: Invariants
---

# Invariants

Fix a transition system $(C,\,\tr)$ and a set of "initial" configurations $I \subseteq C$.  

A set $P$ of configurations is an __invariant__ just if $P$ contains all the configurations reachable from $c_0$, i.e. 
\[
  \{ d \mid \exists c \in I.\: c \trs d \} \subseteq P
\]

A set $P$ of configurations is an __inductive invariant__ just if:
  * $I \subseteq P$
  * and, for all $c,\,d \in C$: if $c \in P$ and $c \tr d$ then $d \in P$ too.
