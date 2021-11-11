---
layout: math
title: Computable functions
nav_order: 1
mathjax: true
parent: Computability
---

# Computable Functions

Let $\texttt{x}_1$, ..., $\texttt{x}_n$ be variables of the While language.

We write $[\vec{\texttt{x}_i} \mapsto n_i]$ for the state that maps the
variable $\texttt{x}_i$ to the number $n_i \in \mathbb{Z}$. 

A function $f : \mathbb{Z}^m \to \mathbb{Z}$ is **while-computable** (with
respect to variables `x_1`, \dots, `x_n`) just when there is a while program
$S$ such that $f(n_1, \dots, n_m) \simeq z$ if and only if $\langle S,
[\vec{\texttt{x}_i} \mapsto n_i] \rangle \Rightarrow^\ast \langle
\texttt{skip}, \sigma' \rangle$ for some state $\sigma'$ with
$\sigma'(\texttt{x}_1) = z$.