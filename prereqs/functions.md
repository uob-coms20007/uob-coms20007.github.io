---
layout: math
title: Functions
mathjax: true
nav_order: 3
parent: Prerequisites
---

# Relations

Let $A$ and $B$ be sets.

The __cartesian product__ of $A$ and $B$ is the set consisting of pairs $(a,
b)$ of one element from $A$ and one element from $B$:
$$
  A \times B = \{ (a, b) \mid a \in A \land b \in B \}
$$

A __relation__ from $A$ to $B$ is a subset $R \subseteq A \times B$ of the
cartesian product.


# Functions

A __function__ $f : A \to B$ from $A$ to $B$ is a relation $f$ from $A$ to $B$ that
has two special properties:
  * it is __total__: for every $a \in A$ there exists _at least one_ $b \in B$ such that $(a, b) \in f$
  * it is __single-valued__: for every $a \in A$ there exists _at most one_ $b \in B$ such that $(a, b) \in f$

Putting these together, $f$ is a function just if for every possible input $a
\in A$ there exists a _unique_ output $b \in B$ such that $(a, b) \in f$. We
write $f(a)$ for this unique $b \in B$.


# Partial functions

Computer programs sometimes fail to halt. In order to model that possibility
we sometimes relax the totality condition in the definition of a function.

A __partial function__ $f : A ⇀ B$ from $A$ to $B$ is a relation from $A$ to
$B$ that is _single-valued_, in that for every $a \in A$ there is at most one
$b \in B$ such that $(a, b) \in f$.

Thus, given some $x \in A$ the partial function $f : A ⇀ B$ might be
__undefined__ at that particular input; we write $f(x) \uparrow$ to denote
that.

Whenever $f$ is defined at $x \in A$ and is equal to $y \in B$, we write
$f(x) \simeq y$. As $f$ is single-valued, there is at most one such $y$.

## Kleene equality

In general, the notation $e \simeq e'$, which is called __Kleene equality__,
means one of two things:
* both the expressions $e$ and $e'$ are undefined, or
* both $e$ and $e'$ have a well-defined value, and their value is equal

For example, we can say that
$$
\frac{1}{0} \simeq \frac{2}{0}
$$
because both expressions are undefined (division by zero is not
well-defined---division is a partial function!).

However,
$$
\frac{1}{2} \not\simeq \frac{2}{2}
$$
as the value of both sides is well-defined, but unequal.

# Identity function

For every set $A$ there exists an __identity function__ which maps every
element to itself:

$$
\begin{aligned}
  & \textsf{id}_A : A \to A
  & \textsf{id}_A(x) = x
\end{aligned}
$$

# Composition of functions

Given two functions $f : A \to B$ and $g : B \to C$, their __composition__ $g
\circ f : A \to B$ is defined by

$$
  (g \circ f)(x) = g(f(x))
$$