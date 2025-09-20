---
layout: math
mathjax: true
parent: "Slides"
title: 15. Derivations
nav_order: 15
---

<div class="defn" markdown="1">
The __one-step derivation relation__ is a binary relation on sentential forms with two sentential forms $\alpha$ and $\beta$ related, written $$\alpha \to \beta$$, just if $\alpha$ is of shape $\alpha_1 X \alpha_2$ and there is a production rule $X \longrightarrow \gamma$ and $\beta$ is exactly $\alpha_1 \gamma \alpha_2$.  

We write $\alpha \to^* \beta$, and say __$\beta$ is derivable from $\alpha$__ (or __$\alpha$ derives $\beta$__) just if $\beta$ can be derived from $\alpha$ in any (finite) number of steps, including zero steps.

Finally, we say that a word $w$ over $\Sigma$ is in the __language of a grammar__ $(\Sigma,\mathcal{N},\mathcal{R},S)$ just if $S \to^* w$.
</div>