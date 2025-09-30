---
layout: math
mathjax: true
parent: "Slides"
title: 23. Parsing Table
nav_order: 23
---

<div class="defn" markdown="1">
We define the __parsing table__, usually $T$, for a given grammar as a 2d array in which each entry $T[X,a]$ is a set of production rules from the grammar, such that some rule $X \Coloneqq \beta$ is in the set $T[X,a]$ just if, either:
  
  1. $a \in \first(\beta)$
  2. or, $\nullable(\beta)$ and $a \in \follow(X)$
</div>

{: .defn }
A grammar whose parsing table contains at most one rule in each cell is called LL(1).