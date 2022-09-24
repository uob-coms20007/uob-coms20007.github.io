---
layout: math
title: Transition Systems
nav_order: 2
mathjax: true
has_children: true
has_toc: false
---

# Transition Systems  

A __transition system__, typically $T$, comprises two pieces of data:
  * A set $C$ of elements that we call *configurations*
  * A relation $\mathord{\tr} \subseteq C \times C$ that we call the *transition relation*

We will typically use $c$, $d$ and so on to stand for arbitrary configurations and we write the transition relation infix, writing e.g. $c \tr d$ rather than $(c,d) \in \mathord{\tr}$.  When $c \tr d$ we say that the system can make a transition (or *step*) from $c$ to $d$.

Fix a transition system and two of its configurations $c$ and $d$.  
  * We say that $d$ is a __successor__ of $c$ just if $c \tr d$.
  * We say that a configuration $c$ is __terminal__ just if it has no successor.

A __deterministic transition system__ is one in which  each configuration has at most one successor.  A transition system that is not deterministic can be said to be *nondeterministic* (for emphasis).

## Traces and Reachability

Fix a transition system.  

A __trace__ is a non-empty, finite or infinite sequence of configurations $c_0,\,c_1,\,c_2,\ldots$ in which each is the successor of the previous:

$$
c_0 \tr c_1 \tr c_2 \cdots
$$

A __complete trace__ is a trace that is either infinite or, otherwise, it ends in a terminal configuration.

Given configurations $c$ and $d$, we say that $d$ is __reachable__ from $c$ (or that $c$ __transitions to__ $d$), written $c \trs d$, just if there is a finite trace whose first configuration is $c$ and whose last configuration is $d$.  NOTE: this definition allows that every configuration can transition to itself via a trace consisting of just that configuration.

We write $c \tr^{+} d$ just if there exists a finite trace of some length $\geq 2$ whose first configuration is $c$ and whose last configuration is $d$.

We write $c \tr^{k} d$ just if there exists a finite trace of length $k+1$ whose first configuration is $c$ and whose last configuration is $d$.