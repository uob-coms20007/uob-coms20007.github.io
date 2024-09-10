---
layout: math
mathjax: true
parent: "Slides"
title: 14. Correct Instances
nav_order: 14
---

## Correct Instantiation

<br/><br/>

$$\begin{prooftree}\AxiomC{$a \matches a$}\AxiomC{$b^* \matches bbb$}\LeftLabel{$\rlnm{Concat}$}\BinaryInfC{$ab^* \matches abbb$}\end{prooftree}$$ 

<br/><br/>

$$\begin{prooftree}\AxiomC{$a+b \matches b$}\AxiomC{$a+b \matches a$}\LeftLabel{$\rlnm{Concat}$}\BinaryInfC{$(a+b)(a+b) \matches ba$}\end{prooftree}$$

<br/><br/>

$$\begin{prooftree}\AxiomC{$abc \matches abc$}\AxiomC{$(b + \epsilon) \matches \epsilon$}\LeftLabel{$\rlnm{Concat}$}\BinaryInfC{$(abc)(b + \epsilon) \matches abcb$}\end{prooftree}$$ 