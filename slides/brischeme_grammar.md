---
layout: math
mathjax: true
parent: "Slides"
title: 28. BS Grammar
nav_order: 28
---

$$
      \begin{array}{rcl}
        \nt{Prog} &::=& \nt{Form}^*\\[1mm]
        \nt{Form} &::=& \nt{SExpr} \mid (\ \tm{define}\ \nt{Ident}\ \nt{SExpr}\ )\\[1mm]
        \nt{SExpr} &::=& \nt{Num} \mid (\ \nt{Ident}\ \nt{SExpr}^*\ ) \mid \ldots \\[1mm]
        \nt{Ident} &::=& \ldots\\[1mm]
        \nt{Num} &::=& \ldots
      \end{array}
$$

