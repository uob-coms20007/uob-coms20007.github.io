---
layout: math
mathjax: true
parent: "Slides"
title: 17. Proof Tree Ex 2
nav_order: 17
---

A proof tree for the statement "$(0+1)(0+1) \matches 10$":

\\[
  \begin{prooftree}
    \AxiomC{}
    \LeftLabel{$\rlnm{Char}$}
    \UnaryInfC{$1 \matches 1$}
    \LeftLabel{$\rlnm{ChoiceR}$}
    \UnaryInfC{$0+1 \matches 1$}
    \AxiomC{}
    \LeftLabel{$\rlnm{Char}$}
    \UnaryInfC{$0 \matches 0$}
    \LeftLabel{$\rlnm{ChoiceL}$}
    \UnaryInfC{$0+1 \matches 0$}
    \LeftLabel{$\rlnm{Concat}$}
    \BinaryInfC{$(0+1)(0+1) \matches 10$}
  \end{prooftree}
\\]