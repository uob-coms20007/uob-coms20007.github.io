---
layout: math
title: The universal function
nav_order: 8
mathjax: true
parent: Computability
---

From this point onwards let us fix a particular variable `x` wrt which our
[definition of
computability](https://uob-coms20007.github.io/reference/computability/functions.html#computes)
is stated.

We define a function

$$
  \eta : \mathbb{N} \to (\mathbb{N} ⇀ \mathbb{N})
  \eta(n) = ⟦ \gamma^{-1}(n) ⟧_\texttt{x}
$$

That is: upon receiving $n$, $\eta$ treats it as a program and decodes it
using the Gödel numbering $\gamma$. Then, it returns the function that it
computes. 