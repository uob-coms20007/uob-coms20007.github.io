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
\Downarrow [\texttt{x} \mapsto n]$.

The function computed by a while program $\texttt{S}$ must be partial, because
$\texttt{S}$ might decide to loop on certain inputs.

A function $f : \mathbb{N} ⇀ \mathbb{N}$ is __computable__ just if there is a
program $S$ that computes $f$ with respect to the variable $\texttt{x}$. We
often abbreviate the phrase "with respect to" to "_wrt_."

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
     x <- x + 1
   ```

2. The factorial function $(\cdot)! : \mathbb{N} \to \mathbb{N}$ is
   computable. We have that $(\cdot)! = ⟦ Q ⟧_{\texttt{n}}$, where $Q$ is the
   program
   ```
   r <- 1;
   while (n >= 1) {
     r <- r * n;
     n <- n - 1
   }
   // Zero out the auxiliary variable.
   r <- 0;
   // Put the desired result in n.
   n <- r
   ```

3. The integer-division-by-2 function $\textbf{div2} : \mathbb{N} ⇀
   \mathbb{N}$, which performs Euclidean division by 2 and returns the
   quotient, is computable. It is computed wrt `n` by the program
   ```
   q <- 0; r <- n;
   while (r >= 2) {
     q <- q + 1; r <- r - 2    // Loop invariant: n = q * 2 + r & r >= 0
   }
   // Zero out all auxiliary variables.
   q <- 0; r <- 0;
   // Put the result in n.
   n <- q
   ```

4. The partial function 

   $$
   \begin{aligned}
   & g : \mathbb{N} ⇀ \mathbb{N} \\
   & g(x) \begin{cases}
      \simeq x + 1 & \text{ if $x < 2$} \\
      \uparrow     & \text{ otherwise}
     \end{cases}
   \end{aligned}
   $$

   is computable. It is computed wrt `x` by the following program.
   ```
   if (x <= 1) then
     x <- x + 1
   else
     while (true) do { skip }
   ```