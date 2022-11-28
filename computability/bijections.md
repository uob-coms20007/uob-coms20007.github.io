---
layout: math
title: Bijections etc.
nav_order: 3
mathjax: true
parent: Computability
---

# Bijections etc.

Bijections can be used to put sets into exact correspondence with each other.

## Definitions

* A function $f : A \to B$ is __injective__ (or 1-1) just if for any $a_1, a_2
  \in A$ we have that $f(a_1) = f(a_2)$ implies $a_1 = a_2$. We sometimes write
  $f : A \rightarrowtail B$ whenever $f$ is an injection.

* A function $f : A \to B$ is __surjective__ just if for any $b \in B$ there
  exists $a \in A$ such that $f(a) = b$. We sometimes write $f : A
  \twoheadrightarrow B$ whenever $f$ is a surjection.

* A function $f : A \to B$ is a __bijection__ just if it is _both_ injective and
  surjective.

## Isomorphism

Let $f : A \to B$ be a function.

$f$ is an __isomorphism__ just if it has an __inverse__. That is, if there exists a function $f^{-1} : B \to A$
such that
* for all $a \in A$ we have $f^{-1}(f(a)) = a$
* for all $b \in B$ we have $f(f^{-1}(b)) = b$

Both equations are necessary!

Using [function
composition](https://uob-coms20007.github.io/reference/prereqs/functions.html#composition)
and the [identity
function](https://uob-coms20007.github.io/reference/prereqs/functions.html#identity-function),
they may be re-written in a so-called _point-free_ style as
* $f^{-1} \circ f = \textsf{id}_A$
* $f \circ f^{-1} = \textsf{id}_B$

In the problem sheet you will prove the following result.

*Claim.* A function $f : A \to B$ is a bijection if and only if it is an
isomorphism.

Thus, a bijection $f : A \to B$ is a device for putting the sets $A$ and $B$
into perfect correspondence: it maps every $a \in A$ to a uniquely associated
$b \in B$. 
## Integers vs. natural numbers

In light of this, the following result is a bit surprising.

Define

$$
\begin{aligned}
  &\beta : \mathbb{Z} \to \mathbb{N} \\
  &\beta(x) = \begin{cases}
    2x    & \text{ if $x \geq 0$} \\
    -2x-1 & \text{ if $x < 0 $}
  \end{cases}
\end{aligned}
$$

$\beta$ is a __bijection between naturals and integers__. It maps 
* positive numbers to even numbers
* negative numbers to odd numbers

*Claim.* The function $\beta$ is a bijection.

_Proof._ 

1. $\beta$ is injective. Let $k_1, k_2 \in \mathbb{Z}$, and suppose $f(k_1) = f(k_2)$.
   There are four cases to consider:
   * *$k_1, k_2$ both positive.* Then $f(k_1) = f(k_2)$ is exactly the statement that $2k_1 = 2k_2$, whence $k_1 = k_2$.
   * *$k_1, k_2$ both negative.* Then $f(k_1) = f(k_2)$ is exactly the statement that $-2k_1 - 1 = -2k_2 - 1$, whence $k_1 = k_2$.
   * *$k_1$ positive, $k_2$ negative.* Then $f(k_1) = f(k_2)$ is exactly the statement that $2k_1 = -2k_2 - 1$. From this we infer $k_1 + k_2 = -1/2$, which cannot happen as both are whole numbers. Thus, this case cannot occur in practice, so there is nothing to prove.
   * *$k_1$ negative, $k_2$ positive.* Similar to the last case.

2. $\beta$ is surjective. Let $n \in \mathbb{N}$. We consider the parity of $n$.
   * If $n$ is even, write it as $n = 2n'$. Then $f(n') = 2n' = n$.
   * If $n$ is odd, write it as $n = 2n' + 1$. Then $f(-n' - 1) = -2(-n' - 1) - 1 = 2n' + 1$.

Hence $\beta$ is a bijection. ▣

We may also equivalently show that $\beta$ is a bijection by showing it's an
isomorphism, i.e. by constructing an inverse $\beta^{-1} : \mathbb{N} \to
\mathbb{Z}$. This inverse is

$$
  \beta^{-1}(n) =
    \begin{cases}
      n/2 & \text{ if $n$ is even} \\
      -(n+1)/2 & \text{ if $n$ is odd}
    \end{cases}
$$

We must then remember to show that 

$$
  \beta^{-1} \circ \beta = \text{id}_\mathbb{Z}
$$

and

$$

\beta \circ \beta^{-1} = \text{id}_\mathbb{N}

$$