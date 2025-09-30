---
layout: math
mathjax: true
parent: "Slides"
title: 25. BExp Alt Table
nav_order: 25
---

$$
  \begin{array}{lcl}
    B &\Coloneqq& A\ B'\\
    B' &\Coloneqq& \andop A\ B' \mid \mathord{\orop}\ A\ B' \mid \epsilon \\
    A &\Coloneqq& \tt \mid \ff \mid (B)
  \end{array}
$$

<br/>

$$
  \begin{array}{|c|c|c|c|c|}\hline
    \text{NT} & \tt & \ff & ( & ) & \andop & \orop \\\hline
    B & B \Coloneqq AB' & B \Coloneqq AB' & B \Coloneqq AB' & & & \\
    B' & & & & B' \Coloneqq \epsilon & B' \Coloneqq \mathord{\andop}AB' & B' \Coloneqq \mathord{\orop}AB'\\
    A & A \Coloneqq \tt & A \Coloneqq \ff & A \Coloneqq (B) & & & \\\hline
  \end{array}
$$