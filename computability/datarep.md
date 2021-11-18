---
layout: math
title: Data representation
nav_order: 3
mathjax: true
parent: Computability
---

# Bijections

A function $f : A \to B$ is __injective__ just if for any $a_1, a_2 \in A$ we have
$$
  f(a_1) = f(a_2) \quad\Longrightarrow a_1 = a_2
$$

A function $f : A \to B$ is __surjective__ just if for any $b \in B$ there exists $a \in A$ such that $f(a) = b$.

A function $f : A \to B$ is a __bijection__ just if it is injective and surjective.


# Integers vs. natural numbers

Define

$$
\begin{aligned}
  &\beta : \mathbb{Z} \to \mathbb{N} \\
  &\beta(x) = \begin{cases}
    2x    & \text{ if $x \geq 0$} \\
    -2x-1 & \text{ otherwise}
  \end{cases}
\end{aligned}
$$

$\beta$ maps 
* positive numbers to even numbers
* negative numbers to odd numbers

**Claim.** The function $\beta$ is a bijection.

_Proof._ 

1. $\beta$ is injective. Let $k_1, k_2 \in \mathbb{Z}$, and suppose $f(k_1) = f(k_2)$.
   There are four cases to consider:
   * **$k_1, k_2$ positive.** Then $f(k_1) = f(k_2)$ is exactly the statement that $2k_1 = 2k_2$, whence $k_1 = k_2$.
   * **$k_1, k_2$ negative.** Then $f(k_1) = f(k_2)$ is exactly the statement that $-2k_1 - 1 = -2k_2 - 1$, whence $k_1 = k_2$.
   * **$k_1$ positive, $k_2$ negative.** Then $f(k_1) = f(k_2)$ is exactly the statement that $2k_1 = -2k_2 - 1$. From this we infer $k_1 + k_2 = -1/2$, which cannot happen as both are whole numbers.
   * **$k_1$ negative, $k_2$ positive.** Similar to the last case.

2. $\beta$ is surjective. Let $n \in \mathbb{N}$. We consider the parity of $n$.
   * If $n$ is even, write it as $n = 2n'$. Then $f(n') = 2n' = n$.
   * If $n$ is odd, write it as $n = 2n' + 1$. Then $f(-n' - 2) = -2(-n' - 2) - 1 = 2n' + 1$.

Hence $\beta$ is a bijection. ▣
