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
variable $\texttt{x}$ to the number $n \in \mathbb{N}$. 

A `while` program $S$ **computes** a partial function  
$f : \mathbb{N} ⇀ \mathbb{N}$ (with respect to $\texttt{x}$) just if $f(m)
\simeq n$ exactly when $\langle S, [\texttt{x} \mapsto m] \rangle
\Rightarrow^\ast \langle \texttt{skip}, \sigma' \rangle$ for some $\sigma'$
with $\sigma'(\texttt{x}) = n$.

The function computed by a while program $S$ must be partial, because $S$
might decide to loop on certain inputs.

A function $f : \mathbb{N} ⇀ \mathbb{N}$ is __computable__ just if there is
a program $S$ that computes $f$ with respect to the variable $\texttt{x}$.

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