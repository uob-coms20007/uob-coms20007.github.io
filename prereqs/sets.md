---
layout: math
title: Sets
mathjax: true
nav_order: 2
parent: Prerequisites
---

# Basic Set Theory

You should ensure you properly understand the meaning of the following notation:
* The empty set $\emptyset$
* Set extension $\\{x,y,z,\ldots\\}$
* Set comprehension $\\{ x \in S \mid P(x) \\}$
* Binary union $X \cup Y$
* Binary intersection $X \cap Y$
* Difference $X - Y$
* Complement $X^c$
* Subset inclusion $X \subseteq Y$
* Set cardinality (number of elements) $$\lvert X \rvert$$
* Power set $\mathcal{P}(X)$

# Predicates

A __predicate__ on a set $A$ is a subset $U \subseteq A$.

We may think of a predicate $U$ as consisting of a set of (interesting,
desirable, pertinent) elements of a bigger set $A$.

## Example

We define the predicate $E \subseteq \mathbb{N}$ to be the set

$$
  E = \{ n \in \mathbb{N} \mid \text{ $n$ is even } \}
$$

consisting of all even numbers.

# Relations

Let $A$ and $B$ be sets.

The __cartesian product__ of $A$ and $B$ is the set consisting of pairs $(a,
b)$ of one element from $A$ and one element from $B$:
$$
  A \times B = \{ (a, b) \mid a \in A \land b \in B \}
$$

A __relation__ from $A$ to $B$ is a subset $R \subseteq A \times B$ of the
cartesian product.