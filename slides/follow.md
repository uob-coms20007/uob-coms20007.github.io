---
layout: math
mathjax: true
parent: "Slides"
title: 19. Follow(X)
nav_order: 19
---

<div class="defn" markdown=1>
$\follow(X)$ is the set of terminal symbols that can appear immediately following non-terminal $X$ in a derivable sentential form, i.e.

$$
  \follow(X) = \{ a \in \Sigma \mid \exists \beta\ \gamma.\: S \to^* \beta X a \gamma \}
$$
</div>