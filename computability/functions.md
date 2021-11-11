---
layout: math
title: Computable functions
nav_order: 1
mathjax: true
parent: Computability
---

# Computable Functions

A function $f : \mathbb{Z}^m \to \mathbb{Z}$ is **while-computable** (with
respect to variables `x_1`, \dots, `x_n`) just when there is a while program
$S$ such that $f(n_1, \dots, n_m) \simeq z$ if and only if the program

```
x_1 := $n_1$; ...; x_n := $n_m$;
S
```

halts with the value of `x_1` being $z$.