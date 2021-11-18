---
layout: math
title: Sections and retractions
nav_order: 4
mathjax: true
parent: Computability
---

# Sections and retractions

A function $s : A \to B$ is a __section__ just when there exists a
corresponding __retraction__, i.e. a function $r : B \to A$ such that for all
$a \in A$ it is the case that $r(s(a)) = a$.

In point-free style, this is written as $r \circ s = \textsf{id}_A$.

We sometimes call $(s, r)$ a __section-retraction pair__.

A retraction is _not_ an inverse to the section. Rather, it is a 'one-sided
inverse.' In terms of data representation, it says that we can encode every
$a \in A$ as an element $s(a)$ of $B$. We can then 'recover' the original $a
\in A$ by computing the element $r(s(a)) = a$.

# Booleans vs. natural numbers

We may encode the set $\mathbb{B} = \{ \top, \bot \}$ of boolean values as
natural numbers. We pick a Unix-like representation, where

$$
  \begin{aligned}
    & s : \mathbb{B} \to \mathbb{N} \\
    & s(\top) = 0 \\
    & s(\bot) = 1
  \end{aligned}
$$

We can recover the original Boolean value through the retraction

$$
  \begin{aligned}
    & r : \mathbb{N} \to \mathbb{B} \\
    & s(n) = \begin{cases}
      \top & \text{ if $n = 0$} \\
      \bot & \text{ otherwise}
    \end{cases}
  \end{aligned}
$$

We have $s(r(b)) = b$ for all $b \in \mathbb{N}$.

While $\mathbb{N}$ is infinite, $\mathbb{B}$ is finite. We cannot expect a
bijection (= perfect correspondence) between them.

However, the section-retraction pair given above shows that we can encode
Boolean values as natural numbers in a 'lossless' way.