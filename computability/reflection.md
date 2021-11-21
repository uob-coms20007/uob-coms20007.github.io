---
layout: math
title: Reflections
nav_order: 4
mathjax: true
parent: Computability
---

Let $i : A \xrightarrow{\cong} \mathbb{N}$ be a bijection. We may think of
$i$ as uniquely representing all the elements of set $A$ as natural numbers.

Let $f : A ⇀ A$ be a partial function. The __reflection__ of $f$ *under* $i$
is the function 

$$
  \begin{aligned}
  & \tilde{f} : \mathbb{N} ⇀ \mathbb{N} \\
  & \tilde{f}(n) = i(f(i^{-1}(n)))
  \end{aligned}
$$

Written in point-free style, we define $\tilde{f} = i \circ f \circ i^{-1}$.

The reflection $\tilde{f}$ computes $f$ "under" the encoding $i$. Thus,
instead of acting on elements of $A$, it acts on their encoded forms.

We can then say that, in a sense, the function $f : A ⇀ A$ is computable just
if its reflection $f : \mathbb{N} ⇀ \mathbb{N}$ is.

This obviates the need to define what it means to be computable for sets that
are not the natural numbers (e.g. integers).