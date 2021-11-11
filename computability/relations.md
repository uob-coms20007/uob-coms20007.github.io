---
layout: math
title: Decidable predicates
nav_order: 2
mathjax: true
parent: Computability
---

# Characteristic Functions

Let $U \subseteq \mathbb{N}$ be a predicate on the natural numbers.

The __characteristic function__ of $X$ is the function
$$
  \begin{gathered}
  \chi_U : \mathbb{N} \to \mathbb{N}
  \chi_U(n) =
    \begin{cases}
      1 & \text{ if $n \in X}
      0 & \text{ otherwise}
    \end{cases}
  \end{gathered}
$$

A number either is or isn't a member of the predicate. Thus, its
characteristic function is always _total_: it is defined for all natural
numbers.

# Decidable predicates

A predicate $U \subseteq \mathbb{N}$ is __decidable__ just if its
characteristic function $\chi_U : \mathbb{N} \to \mathbb{N}$ is computable.
(That is: if there exists a `while` program that computes it.)

## Examples

1. The predicate
   $$
     E = \{ n \in \mathbb{N} \mid n \text{ is even} \}
   $$
   is decidable. It is decided (with respect to the variable $\texttt{x}$) by
   the program
   ```
    while (x >= 0) do
      x := x - 2
    if (x = 0) then x := 1 else x := 0 
   ```