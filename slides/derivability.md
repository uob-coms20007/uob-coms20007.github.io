---
layout: math
mathjax: true
parent: "Slides"
title: 14. Derivability Defn
nav_order: 14
---

<div class="defn" markdown="1">
A **derivation sequence** is a non-empty sequence of sentential forms $\alpha_1$, $\alpha_2$, ... $\alpha_{k-1}$, $\alpha_k$ in which consecutive elements of the sequence are derivation steps:

$$
  \alpha_1 \to \alpha_2 \to \cdots{} \to \alpha_{k-1} \to \alpha_k
$$
</div>

<br/>

<div class="defn" markdown="1">
A sentential form $\beta$ is **derivable** from $\alpha$, written $\alpha \to^* \beta$ just if there is a derivation sequence starting with $\alpha$ and ending with $\beta$.
</div>

<br/>

<div class="defn" markdown="1">
The **language of a grammar $G$**, written $L(G)$, is the set of all strings $w$ consisting only of terminal symbols and which are derivable from the start symbol, i.e. $$\{\, w \mid S \to^* w \,\}$$.
</div>