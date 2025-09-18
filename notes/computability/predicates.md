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
      1 & \text{ if $n \in U$} \\
      0 & \text{ if $n \not\in U$}
    \end{cases}
  \end{aligned}
$$

A number either is or isn't a member of the predicate. Thus, its
characteristic function is always _total_: it is defined for all natural
numbers.

# Decidable predicates

A predicate $U \subseteq \mathbb{N}$ is __decidable__ just if its
characteristic function $\chi_U : \mathbb{N} \to \mathbb{N}$ is computable.

The `while` program that computes the characteristic function $\chi_U$ of a
predicate $U \subseteq \mathbb{N}$ is called a _decision procedure_.

Any predicate for which there is no decision procedure is called
__undecidable__.

## Examples

1. The predicate
   $$
     E = \{ n \in \mathbb{N} \mid n \text{ is even} \}
   $$
   is decidable. It is decided with respect to the variable $\texttt{x}$ by
   the program
   ```
    while (x >= 2) do {
      x <- x - 2
    }
    x <- 1 - x
   ```
   The invariant maintained by this loop is that the parity of $x$ (i.e. whether
   it is even or odd) is the same as it was before the loop started. At the end
   of the loop the variable $\texttt{x}$ is $< 2$, which is to say either $0$ or
   $1$. If it is $0$, the initial value had an even parity, and if it is $1$,
   the initial value had an odd parity. We have to remember to flip this value
   in order to return the correct result (i.e. $1$ if it is even, and $0$ if it
   is odd).

# Semi-decidable predicates

Let $U \subseteq \mathbb{N}$ be a predicate on the natural numbers.

The __semi-characteristic function__ of $U$ is the partial function

$$
  \begin{aligned}
  &\xi_U : \mathbb{N} ⇀ \mathbb{N} \\
  &\xi_U(n)
    \begin{cases}
      \simeq 1 & \text{ if $n \in U$} \\
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

A predicate $U \subseteq \mathbb{N}$ is __semi-decidable__ just if its
semi-characteristic function $\xi_U$ is computable.

## Examples

2.  The predicate
    $$
      U = \{ k \in \mathbb{N}^+ \mid \text{ the Collatz sequence starting at $k$ eventually reaches 1 } \}
    $$
    is semi-decidable.

    The ___Collatz sequence___ starting at $k \in \mathbb{N}^+$ is the
    sequence of numbers $(a_n)_{n \in \mathbb{N}}$ defined by

    $$
    \begin{aligned}
      a_0 &= k \\
      a_{n+1} &= \begin{cases}
        a_n/2      & \text{ if $a_n$ is even} \\
        (3a_n+1)/2 & \text{ if $a_n$ is odd}
      \end{cases}
    \end{aligned}
    $$

    The [Collatz
    conjecture](https://en.wikipedia.org/wiki/Collatz_conjecture) is a
    famously unsolved problem of mathematics. It states that the Collatz
    sequence $a_0, a_1, \dots$ starting at any $a_0 \in \mathbb{N}^+$ will
    eventually reach the number $1$. 

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
      q <- 0; r <- n;
      while (r >= 2) {
        q <- q + 1; r <- r - 2      // Loop invariant: n = q * 2 + r & r > 0
      }

      // Depending on the parity compute the next element in the sequence.
      if (r = 0) then {
        // The next element is a_n / 2.
        n <- q
      }
      else {
        // The next element is (3 * a_n + 1) / 2.
        // Compute 3 * a_n + 1, and then divide it by 2.
        q <- 0; r <- 3 * n + 1;
        while (r >= 2) do {
          q <- q + 1; r <- r - 2
        }
        n <- q
      }
    }
    // If we get here it must be that n = 1. Zero out all other variables.
    q <- 0; r <- 0
    ```
    Suppose we run this program starting with $n > 0$. If the Collatz sequence eventually reaches $1$, the `while` loop will terminate (with the value $1$ in `n`). Otherwise, the loop will run forever.

    What will happen if we start with $n = 0$?