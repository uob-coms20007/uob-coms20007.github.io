---
layout: math
title: Computable functions
nav_order: 1
mathjax: true
parent: Computability
---

# Computable Functions

Let $\texttt{x}$ be a variable of the `while` language.

We write $[\texttt{x} \mapsto n]$ for the state that maps the
variable $\texttt{x}$ to the number $n \in \mathbb{N}$, and every other variable to $0$.

A `while` program $\texttt{S}$ **computes** a partial function  
$f : \mathbb{N} ⇀ \mathbb{N}$ (with respect to $\texttt{x}$) just if $f(m)
\simeq n$ exactly when $\langle S, [\texttt{x} \mapsto m] \rangle
\Rightarrow^\ast \langle \texttt{skip}, [\texttt{x} \mapsto n] \rangle$.

The function computed by a while program $\texttt{S}$ must be partial, because
$\texttt{S}$ might decide to loop on certain inputs.

A function $f : \mathbb{N} ⇀ \mathbb{N}$ is __computable__ just if there is a
program $S$ that computes $f$ with respect to the variable $\texttt{x}$. We
often abbreviate "with respect to" to _wrt_.

Given a `while` program $S$ we write
$$
  ⟦ S ⟧_{\texttt{x}} : \mathbb{N} ⇀ \mathbb{N}
$$
for the function computed by $S$ with respect to variable $\texttt{x}$.

## Examples

1. The function

  $$
  \begin{aligned}
  & f : \mathbb{N} \to \mathbb{N} \\
  & f(x) = x+1
  \end{aligned}
  $$
  
  is computable. We have that $f = ⟦ S ⟧_{\texttt{x}}$, where $S$ is the program
  ```
    x := x + 1
  ```

2. The factorial function $(\cdot)! : \mathbb{N} \to \mathbb{N}$ is computable.
   We have that $(\cdot)! = ⟦ Q ⟧_{\texttt{n}}$, where $Q$ is the program
  ```
  r := 1
  while (1 <= n) {
    r := r * n
    n := n - 1
  }
  n := r
  ```

3. The integer-division-by-2 function $\textbf{div-2} : \mathbb{N} \to
   \mathbb{N}$, which performs Euclidean division by 2 and returns the
   quotient, is computable. It is computed wrt `n` by the program
   ```
   q := 0; r := n;
   while (! r < 2) {
     q := q + 1; r := r - 2   // Loop invariant: 
   }
   q := 0; r := 0; n := q
   ```