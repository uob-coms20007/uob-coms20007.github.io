---
layout: math
mathjax: true
parent: "Slides"
title: 42. Inlined Ambiguity
nav_order: 42
---

$$
\begin{array}{rcl}
\nt{SimpleStmt} &\Coloneqq& \nt{EmptyStmt}\\[2mm] 
&\mid& \nt{Expression} \\[2mm]
&\mid& \nt{Expression}\ \tm{\leftarrow}\ \nt{Expression} \\[2mm]
&\mid& \nt{IncDecStmt} \\[2mm]
&\mid& \nt{Assignment} \\[2mm]
&\mid& \nt{ShortVarDecl}
\end{array}
$$