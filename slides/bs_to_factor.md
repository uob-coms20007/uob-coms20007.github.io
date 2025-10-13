---
layout: math
mathjax: true
parent: "Slides"
title: 39. BS Overlapping
nav_order: 39
---

$$
  \begin{array}{rcl}
    \nt{Prog} &\Coloneqq& \nt{Form}^*\\[4mm]
    \nt{Form} &\Coloneqq& \nt{SExpr}\\[2mm]
              &\mid& (\ \tm{define}\ \tm{ident}\ \nt{SExpr}\ )\\[4mm]
    \nt{SExpr} &\Coloneqq& \tm{literal}\\[2mm] 
                &\mid& \tm{ident}\\[2mm]
                &\mid& (\ \nt{SExpr}\ \nt{SExpr}^*\ )\\[2mm]
                &\mid& (\ \tm{primop}\ \nt{SExpr}^*\ )\\[2mm]
                &\mid& (\ \tm{lambda}\ \tm{(}\ \tm{ident}^*\ \tm{)}\ \nt{SExpr}\ )
  \end{array}
$$

