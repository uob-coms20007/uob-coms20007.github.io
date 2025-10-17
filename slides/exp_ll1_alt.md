---
layout: math
mathjax: true
parent: "Slides"
title: 49. Exp LL(1) Alt
nav_order: 49
---

$$
  \begin{array}{rcl}
    T &\longrightarrow& F\ T' \\
    T' &\longrightarrow& \tm{+}\ F\ T' \mid \epsilon \\ 
    F &\longrightarrow& L\ F' \\
    F' &\longrightarrow& \tm{*}\ L\ F' \mid \epsilon \\
    L &\longrightarrow& \tm{num} \mid (T)
  \end{array}
$$