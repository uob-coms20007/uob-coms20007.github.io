---
layout: math
mathjax: true
parent: "Slides"
title: 23. Grammar Structure
nav_order: 23
---

<div class="defn" markdown=1>
$$
  \nullable(X) \;\text{iff}\; X \to^* \epsilon
$$
</div>

<div class="defn" markdown=1>
$$
  \first(X) = \{ a \in \Sigma \mid \exists \beta.\, X \to^* a\beta \}
$$ 
</div>

<div class="defn" markdown=1>
$$
  \follow(X) = \{ a \in \Sigma \mid \exists \alpha \beta.\: S \to^* \alpha X a \beta \}
$$
</div>