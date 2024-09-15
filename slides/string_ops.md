---
layout: math
mathjax: true
parent: "Slides"
title: 4. 
nav_order: 4
---

## String Operations

{: .defn }
A string $w$ is said to be a __substring__ of a string $v$ just if $w$ appears consecutively in $v$.

{: .defn }
The __length__ of a string $w$, written $\|w\|$, is just the number of characters in the string.  That is, if $x = a_1\cdots{}a_k$ then $\|x\| = k$.

{: .defn }
Given strings $x$ and $y$, we write $xy$ for the string obtained by __concatenating__ $y$ to the end of $x$.  That is, if $x = a_1\cdots{}a_k$ and $y = b_1 \cdots{} b_m$ then $xy = a_1\cdots{}a_k b_1 \cdots{} b_m$.  We write $w^k$ for the __$k$-fold concatenation__ of $w$ with itself, i.e. the word $\underbrace{ww\cdots{}w}_{\text{$k$-times}}$.