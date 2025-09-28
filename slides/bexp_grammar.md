---
layout: math
mathjax: true
parent: "Slides"
title: 6. BExp Grammar
nav_order: 6
---

$$
  \begin{array}{rrcl}
  (1) & B &\Coloneqq& F \\
  (2) & B &\Coloneqq& F \orop B \\ 
  (3) & F &\Coloneqq& L \\
  (4) & F &\Coloneqq& L \andop F\\
  (5) & L &\Coloneqq& \tt \\ 
  (6) & L &\Coloneqq& \ff\\
  (7) & L &\Coloneqq& (B)
  \end{array}
$$