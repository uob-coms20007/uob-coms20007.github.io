---
layout: math
mathjax: true
parent: "Slides"
title: 5. String Ops
nav_order: 5
---

## String Operations

{: .defn }
The __length__ of a string $w$, written $\|w\|$, is just the number of characters in the string.  That is, if $x = a_1\cdots{}a_k$ then $\|x\| = k$.

{: .defn }
Given strings $x$ and $y$, we write $xy$ for the string obtained by __concatenating__ $y$ to the end of $x$.  That is, if $x = a_1\cdots{}a_k$ and $y = b_1 \cdots{} b_m$ then $xy = a_1\cdots{}a_k b_1 \cdots{} b_m$.  We write $w^k$ for the __$k$-fold concatenation__ of $w$ with itself, i.e. the word $\underbrace{ww\cdots{}w}_{\text{$k$-times}}$.

{: .defn }
A string $w$ is said to be a __substring__ of a string $v$ just if $w$ appears consecutively in $v$.