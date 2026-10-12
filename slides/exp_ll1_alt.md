---
layout: math
mathjax: true
parent: "Slides"
title: 49. Exp LL(1) Alt
nav_order: 49
---

$$
  \begin{array}{rcl}
    T &::=& F\ T' \\
    T' &::=& \tm{+}\ F\ T' \mid \epsilon \\ 
    F &::=& L\ F' \\
    F' &::=& \tm{*}\ L\ F' \mid \epsilon \\
    L &::=& \tm{num} \mid (T)
  \end{array}
$$