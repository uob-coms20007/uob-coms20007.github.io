---
layout: math
title: Computable functions
nav_order: 1
mathjax: true
parent: Computability
---

# Computable Functions

Let $\texttt{x}$ be a variable of the While language.

We write $[\texttt{x} \mapsto n]$ for the state that maps the
variable $\texttt{x}$ to the number $n \in \mathbb{N}$. 

A while program $S$ **computes** a partial function  
$f : \mathbb{N} \to \mathbb{N}$  (with respect to \texttt{x}) just if $f(x)
\simeq y$ if and only if $\langle S, [\texttt{x} \mapsto n_i] \rangle
\Rightarrow^\ast \langle \texttt{skip}, \sigma' \rangle$ for some state
$\sigma'$ with $\sigma'(\texttt{x}) = y$.

The function computed by a while program $S$ must be partial, because $S$
might decide to loop on certain inputs.

Given a while program $S$ we write
$$
  \llbracket S \rrbracket_{\texttt{x}} : \mathbb{N} ⇀ \mathbb{N}
$$
for the function computed by $S$ with respect to variable $\texttt{x}$.

A function $f : \mathbb{N} \to \mathbb{N}$ is __computable__ just if there is
a program $S$ that computes $f$ with respect to the variable \texttt{x}.

## Examples

1. The function
   $$
   f : \mathbb{N} \to \mathbb{N} \\
   f(x) = x+1
   $$
   is computable. We have that $f = \llbracket S \rrbracket_{\texttt{x}}$, where $S$ is the program
   ```
     x := x + 1
   ```