---
layout: math
mathjax: true
parent: "Slides"
title: 24. Parse Table
nav_order: 24
---

$$
  \nullable_s(\alpha) \quad\mathit{ iff }\quad \alpha \to^* \epsilon
$$

$$
  \first_s(\alpha) = \{ a \in \Sigma \mid \alpha \to^* a\beta\}
$$

<div class="defn" markdown="1">
We define the __parsing table__, usually $T$, for a given grammar as a 2d array in which each entry $T[X,a]$ is a set of production rules from the grammar, such that some rule $X \longrightarrow \beta$ is in the set $T[X,a]$ just if, either:
  
  1. $a \in \first_s(\beta)$
  2. or, $\nullable_s(\beta)$ and $a \in \follow(X)$
</div>