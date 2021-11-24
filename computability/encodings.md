---
layout: math
title: Encodings
nav_order: 6
mathjax: true
parent: Computability
---

# Encoding pairs

A __pairing function__ is a bijection $\mathbb{N} \times \mathbb{N}
\xrightarrow{\cong} \mathbb{N}$.

It is not immediately evident that a pairing function might exist: why should
it be possible to put $\mathbb{N}$ and $\mathbb{N} \times \mathbb{N}$ in
*exact* correspondence with each other? Does not the product set $\mathbb{N}
\times \mathbb{N}$ have somehow 'infinitely more' elements than $\mathbb{N}$?

Nevertheless, [a variety of pairing
functions](https://en.wikipedia.org/wiki/Pairing_function) exists.

* The [Cantor pairing
  function](https://en.wikipedia.org/wiki/Pairing_function#Cantor_pairing_function)
  scans the 'plane'
  [diagonally](https://en.wikipedia.org/wiki/Pairing_function#/media/File:Cantor's_Pairing_Function.svg).

* [Other pairing functions](https://en.wikipedia.org/wiki/Pairing_function#/media/File:Diagonal_argument.svg) cover the plane in a [boustrophedon](https://en.wikipedia.org/wiki/Boustrophedon) pattern (that of an ox plowing a field)

* ... and there are [many other variants](https://mathworld.wolfram.com/PairingFunction.html).

# Encoding lists

Once we have a pairing function $\phi : \mathbb{N} \times \mathbb{N} \to
\mathbb{N}$ and its inverse $\phi^{-1} : \mathbb{N} \to \mathbb{N} \times
\mathbb{N}$ then we can use them to encode all sorts of other data.

For example, we may encode lists of natural numbers, i.e. elements of the set

$$
  \mathbb{N}^\ast = \{ [n_1, \dots, n_k] \mid k \geq 0, \forall i. n_i \in \mathbb{N} \}
$$

as natural numbers. To encode we define a function $\phi_ast :
\mathbb{N}^\ast \to \mathbb{N}$ by induction:

$$
  \begin{aligned}
    \phi_\ast([])   &= 0 \\
    \phi_\ast(n:ns) &= 1 + \phi(n, \phi_ast(ns))
  \end{aligned}
$$

This function is a bijection. They key to writing down the inverse is to
notice that all operations are reversible. We define $\phil_\ast^{-1} :
\mathbb{N} \to \mathbb{N}^\ast$ by

$$
  \begin{aligned}
    \phi_\ast^{-1}(n) = \begin{cases}
      []                     & \text{ if $n = 0$} \\
      x : \phi_\ast^{-1}(xs) & \text{ if $n > 0$ and (x, xs) = \phi^{-1}(n)$}
  \end{aligned}

# Encoding binary trees

