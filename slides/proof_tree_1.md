---
layout: math
mathjax: true
parent: "Slides"
title: 16. Proof Tree Ex 1
nav_order: 16
---

A proof tree for the statement "$$b^* \matches bb$$":

\\[
  \begin{prooftree}
    \AxiomC{}
    \LeftLabel{$\rlnm{Char}$}
    \UnaryInfC{$b \matches b$}
    \AxiomC{}
    \LeftLabel{$\rlnm{Char}$}
    \UnaryInfC{$b \matches b$}
    \AxiomC{}
    \LeftLabel{$\rlnm{StarB}$}
    \UnaryInfC{$b^* \matches \epsilon$}
    \LeftLabel{$\rlnm{StarS}$}
    \BinaryInfC{$b^* \matches b$}
    \LeftLabel{$\rlnm{StarS}$}
    \BinaryInfC{$b^* \matches bb$}
  \end{prooftree}  
\\]