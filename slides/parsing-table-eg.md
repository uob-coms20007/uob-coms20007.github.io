---
layout: math
mathjax: true
parent: "Slides"
title: 23. Table Eg
nav_order: 23
---

$$
  \begin{array}{|c|c|}\hline
    \text{Nonterminal} & \text{Follow} \\\hline
    B & ),\,\andop,\,\orop \\\hline
  \end{array}
\qquad
  \begin{array}{|c|c|}\hline
    \text{Rule RHS} & \text{Nullable?} & \text{First} \\\hline
    \tt & \times & \tt \\
    \ff & \times & \ff \\
    (B) & \times & ( \\
    B \andop B & \times & \tt,\,\ff,\,( \\  
    B \orop B & \times & \tt,\,\ff,\,( \\\hline
  \end{array}
$$

<div class="defn" markdown="1">
We define the __parsing table__, usually $T$, for a given grammar as a 2d array in which each entry $T[X,a]$ is a set of production rules from the grammar, such that some rule $X \Coloneqq \beta$ is in the set $T[X,a]$ just if, either:
  
  1. $a \in \first(\beta)$
  2. or, $\nullable(\beta)$ and $a \in \follow(X)$
</div>
