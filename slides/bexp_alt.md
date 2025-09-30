---
layout: math
mathjax: true
parent: "Slides"
title: 16. BExp Alt CFG
nav_order: 16
---

$$
  \begin{array}{lcl}
    B &\Coloneqq& A\ B'\\
    B' &\Coloneqq& \andop A\ B' \mid \mathord{\orop}\ A\ B' \mid \epsilon \\
    A &\Coloneqq& \tt \mid \ff \mid (B)
  \end{array}
$$