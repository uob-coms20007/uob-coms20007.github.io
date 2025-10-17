---
layout: math
mathjax: true
parent: "Slides"
title: 47. Precedence
nav_order: 47
---

<div class="defn" markdown=1>
  The __precedence__ of an operator is a convention used when inserting implicit parentheses (building abstract syntax trees).  An operator $\otimes$ is said to be of _higher precedence_ than an operator $\oplus$ just if every chain $u \otimes v \oplus w$ is considered syntactically identical to (that is, having the same abstract syntax as) $(u \otimes v) \oplus w$; and $u \oplus v \otimes w$ is considered syntactically identical to $u \oplus (v \otimes w)$.
</div>