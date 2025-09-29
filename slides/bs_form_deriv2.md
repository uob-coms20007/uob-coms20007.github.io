---
layout: math
mathjax: true
parent: "Slides"
title: 20. BS Deriv 2
nav_order: 20
---

$$
  \begin{array}{rll}
    \nt{Form} &\to& (\ \tm{define}\ \nt{Ident}\ \nt{SExpr}\ ) \\
              &\to& (\ \tm{define}\ \nt{Ident}\ \nt{Num}\ ) \\
              &\to^*& (\ \tm{define}\ \tm{x}\ \tm{3}\ )\\[8mm]
    \nt{Form} &\to& \nt{SExpr} \\
              &\to& (\ \nt{Ident}\ \nt{SExpr}^*\ ) \\
              &\to& (\ \nt{Ident}\ \nt{SExpr}\ \nt{SExpr}\ \nt{SExpr}\ \nt{SExpr}\ ) \\
              &\to& (\ \nt{Ident}\ \nt{Ident}\ \nt{Ident}\ \nt{Ident}\ \nt{Num}\ ) \\
              &\to^*& (\ \tm{def}\ \tm{i}\ \tm{ne}\ \tm{x}\ \tm{3}\ )
  \end{array}
$$