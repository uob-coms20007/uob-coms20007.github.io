---
layout: math
mathjax: true
parent: "Slides"
title: 22. BExp Alt Grammar
nav_order: 22
---

$$
  \begin{array}{lcl}
    D &\longrightarrow& C\ D'\ $ \\
    D' &\longrightarrow& \orop C\ D' \mid \epsilon \\
    C &\longrightarrow& A\ C' \\
    C' &\longrightarrow& \andop A\ C' \mid \epsilon \\
    A &\longrightarrow& \tt \mid \ff \mid \tm{id} \mid (D)
  \end{array}
$$