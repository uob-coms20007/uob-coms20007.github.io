---
layout: math
title: Encoding data
nav_order: 4
mathjax: true
parent: Computability
---

# Encoding first-order data

The main way to encode a set $S$ of data as natural numbers is to construct a bijection $S \xrightarrow{\cong} \mathbb{N}$.

We have already seen how to construct a bijection $\beta : \mathbb{Z}
\xrightarrow{\cong} \mathbb{N}$, so that any integer may be encoded as a natural
number. We will now see how to encode other kinds of _first-order_ data (e.g.
numbers, lists, trees, etc.) as natural numbers.

## Encoding pairs of numbers

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

From this point onwards we assume that we have a fixed pairing function

$$
  \langle -, - \rangle : \mathbb{N} \times \mathbb{N} \xrightarrow{\cong} \mathbb{N}
$$

Moreover, we write

$$
  \textsf{split} : \mathbb{N} \xrightarrow{\cong} \mathbb{N} \times \mathbb{N}
$$

for its inverse.

## Encoding lists

Once we have a pairing function $\langle -, - \rangle$ and its inverse
$\textsf{split}$ we can use them to encode all sorts of other data.

For example, we may encode lists of natural numbers, i.e. elements of the set

$$
  \mathbb{N}^\ast = \{ [n_0, \dots, n_k] \mid k \geq 0, \forall i. n_i \in \mathbb{N} \}
$$

as natural numbers. To encode we define a function $\phi_\ast :
\mathbb{N}^\ast \to \mathbb{N}$ by induction:

$$
  \begin{aligned}
    \phi_\ast([])   &= 0 \\
    \phi_\ast(n:ns) &= 1 + \langle n, \phi_\ast(ns) \rangle
  \end{aligned}
$$

This function is a bijection. They key to writing down the inverse is to
notice that all operations used in its definition ($\phi$, $+1$) are
invertible. We define $\phi_\ast^{-1} : \mathbb{N} \to \mathbb{N}^\ast$ by

$$
  \begin{aligned}
    \phi_\ast^{-1}(n) = \begin{cases}
      []                     & \text{ if $n = 0$} \\
      x : \phi_\ast^{-1}(m) & \text{ if $n > 0$ and $(x, m) = \textsf{split}(n - 1)$}
    \end{cases}
  \end{aligned}
$$

Other algebraic data types (e.g. binary trees) may be encoded as natural numbers
by following the same pattern.