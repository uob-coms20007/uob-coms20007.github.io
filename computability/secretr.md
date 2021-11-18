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
natural numbers. We define

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

We have $r(s(b)) = b$ for all $b \in \mathbb{B}$.

Many natural numbers ($2, 3, 4, \ldots$) are not in the image of the section
$s : \mathbb{B} \to \mathbb{N}$. Thus, they do not naturally arise as the
representation of some Boolean value: only $0$ and $1$ do. Thus, the
retraction $r : \mathbb{N} \to \mathbb{B}$ makes an 'executive decision' to
map all other numbers to falsity. (This choice is arbitrary, and we could have
chosen to map them to truth.)

Incidentally, this is the mathematical way to represent the implicit meaning
of [Unix/Linux/POSIX exit
codes](https://tldp.org/LDP/abs/html/exit-status.html): $0$ represents a
successful exit (true), while any other number signals an error (false).

This situation cannot be improved. While $\mathbb{N}$ is infinite, the set of
Boolean values $\mathbb{B}$ is finite. Thus, there cannot be a bijection (=
perfect correspondence) between them.

However, the section-retraction pair given above shows that we can encode
Boolean values as natural numbers in a 'lossless' way.