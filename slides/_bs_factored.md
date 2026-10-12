---
layout: math
mathjax: true
parent: "Slides"
title: 40. BS Factored
nav_order: 40
---

$$
  \begin{array}{rcl}
  \nt{SExpr} &\Coloneqq& \tm{literal}\\[2mm] 
                &\mid& \tm{ident}\\[2mm]
                &\mid& (\ R\\[4mm]
  \nt{R} &\Coloneqq& \nt{SExpr}\ \nt{SExpr}^*\ )\\[2mm]
                &\mid& \tm{primop}\ \nt{SExpr}^*\ )\\[2mm]
                &\mid& \tm{lambda}\ \tm{(}\ \tm{ident}^*\ \tm{)}\ \nt{SExpr}\ )
  \end{array}
$$