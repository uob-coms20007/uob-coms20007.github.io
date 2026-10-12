---
layout: math
mathjax: true
parent: "Slides"
title: 41. Go Ambiguity
nav_order: 41
---

$$
  \begin{array}{rcl}
  \nt{SimpleStmt} &\Coloneqq& \nt{EmptyStmt} \mid \nt{ExpressionStmt} \mid \nt{SendStmt} \mid \nt{IncDecStmt} \mid \nt{Assignment} \mid \nt{ShortVarDecl}\\[2mm]
  \nt{ExpressionStmt} &\Coloneqq& \nt{Expression}\\[2mm]
  \nt{SendStmt} &\Coloneqq& \nt{Channel}\ \tm{\leftarrow}\ \nt{Expression}\\[2mm]
  \nt{Channel} &\Coloneqq& \nt{Expression}
  \end{array}
$$