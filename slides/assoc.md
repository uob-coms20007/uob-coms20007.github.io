---
layout: math
mathjax: true
parent: "Slides"
title: 46. Assoc Left/Right
nav_order: 46
---

<div class="defn" markdown=1>
  The __associativity__ of an operator is the name given to a convention used when inserting implicit parentheses (building abstract syntax trees).  An operator $\oplus$ is said to be __left associative__ if a chain $u \oplus v \oplus w$ should be considered syntactically identical to $(u \oplus v) \oplus w$, that is, having the same abstract syntax tree.  An operator $\oplus$ is said to be __right associative__ if a chain $u \oplus v \oplus w$ is considered syntactically identical to $u \oplus (v \oplus w)$.
</div>