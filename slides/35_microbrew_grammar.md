---
layout: math
mathjax: true
parent: "Slides"
title: 35. Microbrew Grammar
nav_order: 35
---

$$
  \begin{array}{rcl}
    \nt{Cmd} &\Coloneqq& \tm{\$}\\[2mm]
    &\mid& \nt{Exp}\ \tm{\$}\\[2mm]
    &\mid& \tm{def}\ \tm{ident}\ \tm{(}\ \nt{ExpList}\ \tm{)}\ \tm{=}\ \nt{Exp}\ \tm{\$}\\[4mm]
    \nt{Exp} &\Coloneqq& \tm{var} \\[2mm]
    &\mid& \tm{ident}\ \tm{(}\ \nt{ExpList}\ \tm{)}\\[4mm]
    \nt{ExpList} &\Coloneqq& \epsilon\\[2mm]
    &\mid& \nt{Exp}\ \nt{ExpList'}\\[4mm]
    \nt{ExpList'} &\Coloneqq& \epsilon\\[2mm]
    &\mid& \tm{,}\ \nt{Exp}\ \nt{ExpList'}\\[4mm]
  \end{array}
$$