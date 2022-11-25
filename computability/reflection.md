---
layout: math
title: Reflections
nav_order: 6
mathjax: true
parent: Computability
---

# Reflections: computing under representation

The technology of bijections and section-retraction pairs allow us to define
computability for sets $S$ other than $\mathbb{N}$.

## Reflecting under bijections

Let $i : A \xrightarrow{\cong} \mathbb{N}$ be a bijection with inverse
$i^{-1}$.

We may think of $i$ as uniquely representing all the elements of set $A$ as
natural numbers.

Let $f : A ⇀ A$ be a partial function. We cannot directly speak to $f$'s computability, as it is a partial function on
the set $A$, not on the set $\mathbb{N}$. However, the bijection $i$ allows us to _transport_ the function to the set $\mathbb{N}$!


**Definition.** The __reflection__ of $f$ *under* $i$ is the function 

$$
  \begin{aligned}
  & \tilde{f} : \mathbb{N} ⇀ \mathbb{N} \\
  & \tilde{f}(n) = i(f(i^{-1}(n)))
  \end{aligned}
$$

Written in point-free style, we define $\tilde{f} = i \circ f \circ i^{-1}$.

$$\require{AMScd}
\begin{CD}
A @>{f}>> A\\
@VVV @VVV \\
\mathbb{N} @>{\tilde{f}}>> \mathbb{N};
\end{CD}$$

The reflection $\tilde{f}$ computes $f$ "under" the encoding $i$. Thus,
instead of acting on elements of $A$, it acts on their encoded forms.

We can then say that, in a sense, the function $f : A ⇀ A$ is computable just
if its reflection $\tilde{f} : \mathbb{N} ⇀ \mathbb{N}$ is.

This obviates the need to define what it means to be computable for sets that
are not the natural numbers (e.g. integers).