---
layout: math
mathjax: true
parent: "Slides"
title: 30. Associativity
nav_order: 30
---

  The __associativity__ of an operator is the name given to the convention used when inserting implicit parentheses.  
  
  * An operator $\oplus$ is said to be __left associative__ if a chain $u \oplus v \oplus w$ is considered syntactically identical to $(u \oplus v) \oplus w$, that is, having the same AST.  
  * An operator $\oplus$ is said to be __right associative__ if a chain $u \oplus v \oplus w$ is considered syntactically identical to $u \oplus (v \oplus w)$, that is, having the same AST.