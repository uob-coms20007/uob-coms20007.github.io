---
layout: math
mathjax: true
parent: "Slides"
title: 6. BExp Grammar
nav_order: 6
---

$$
  \begin{array}{rrcl}
  (1) & B &\longrightarrow& F \\
  (2) & B &\longrightarrow& F \orop B \\ 
  (3) & F &\longrightarrow& L \\
  (4) & F &\longrightarrow& L \andop F\\
  (5) & L &\longrightarrow& \tt \\ 
  (6) & L &\longrightarrow& \ff\\
  (7) & L &\longrightarrow& (B)
  \end{array}
$$