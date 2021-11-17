---
layout: math
title: Decidable predicates
nav_order: 2
mathjax: true
parent: Computability
---

# Characteristic Functions

Let $U \subseteq \mathbb{N}$ be a predicate on the natural numbers.

The __characteristic function__ of $U$ is the function

$$
  \begin{aligned}
  &\chi_U : \mathbb{N} \to \mathbb{N} \\
  &\chi_U(n) =
    \begin{cases}
      1 & \text{ if $n \in X$} \\
      0 & \text{ otherwise}
    \end{cases}
  \end{aligned}
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
    while (x > 0) do
      x := x - 2
    if (x = 0) then x := 1 else x := 0 
   ```

# Semi-decidable predicates

Let $U \subseteq \mathbb{N}$ be a predicate on the natural numbers.

The __semi-characteristic function__ of $U$ is the partial function

$$
  \begin{aligned}
  &\xi_U : \mathbb{N} ⇀ \mathbb{N} \\
  &\xi_U(n)
    \begin{cases}
      \simeq 1 & \text{ if $n \in X$} \\
      \uparrow & \text{ otherwise}
    \end{cases}
  \end{aligned}
$$

The characteristic function $\chi_U$ of $U$ is always total: it always
returns either $0$ or $1$.

In contrast, the semi-characteristic function $\xi_U$ is _partial_. If $n \in
U$ the semi-characteristic function returns $1$. Otherwise, it is undefined.

The idea is that semi-characteritic functions are somewhat easier to compute
than characteristic functions. A program that computes a characteristic
function must halt with a specific answer (yes or no) on all valid inputs.
However, a program that computes the semi-characteristic function must only
halt for 'yes' inputs. Otherwise, it is allowed to continue computing
forever.

## Examples

2.  The predicate
    $$
      U = \{ k \in \mathbb{N} \mid \text{ the Collatz sequence starting at $k$ eventually reaches 1 } \}
    $$
    is semi-decidable.

    The ___Collatz sequence___ starting at $k \in \mathbb{N}^+$ is the
    sequence of numbers $(a_n)_{n \in \mathbb{N}}$ defined by

    $$
    \begin{aligned}
      a_0 &:= k \\
      a_{n+1} &:= \begin{cases}
        n/2      & \text{ if $a_n$ is even} \\
        (3n+1)/2 & \text{ if $a_n$ is odd}
      \end{cases}
    \end{aligned}
    $$

    The [Collatz
    conjecture](https://en.wikipedia.org/wiki/Collatz_conjecture) is a
    famously unsolved problem of mathematics. It states that the Collatz
    sequence $a_0, a_1, \dots$ starting at any $a_0 \in \mathbb{N}^+$ will
    eventually reach the number $1$. As a matter of convention we suppose $0
    \not\in E$.

    If the Collatz conjecture is true, then every number is in the predicate
    $U$, i.e. $U = \mathbb{N}$. Thus, if $U$ were proven to be decidable,
    then that would reveal a lot of interesting things for this unresolved
    conjecture!

    However, we can show that $U$ is semi-decidable. Its semi-characteristic
    function $\xi_U$ is computed by the following program (wrt `n`), which
    calculates every number in the sequence until it reaches $1$.
    ```
    while (! (n = 1)) {
      // Divide n by 2, putting the quotient in q and the remainder in r.
      q := 0; r := n;
      while (! (r < 2)) {
        q := q + 1; r := r - 2      // Loop invariant: n = q * 2 + r & r > 0
      }

      // Depending on the parity compute the next element in the sequence.
      if (r = 0) then n := q else n := 3 * n + 1
    }
    ```
    If when starting from $n$ the sequence eventually reaches $1$, the
    `while` loop will terminate (with the value $1$ in `n`). Otherwise, the
    loop will run forever.