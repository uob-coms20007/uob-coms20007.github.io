---
layout: math
mathjax: true
parent: "Slides"
title: 21. Nullable(α)
nav_order: 21
---

$$
  \begin{array}{|c|c|}\hline
    \text{Nonterminal} & \text{Nullable?} & \text{First} & \text{Follow} \\\hline
    B & \times & \tt,\,\ff,\,( & ),\,\andop,\,\orop \\\hline
  \end{array}
$$

<div class="defn" markdown=1>
$$
  \nullable(\alpha) =
    \begin{cases}
      \checkmark{} & \text{if $\alpha = \epsilon$}\\
      \times & \text{if $\alpha$ is of shape $a\beta$}\\
      \nullable(X) \wedge \nullable(\beta) & \text{if $\alpha$ is of shape $X\beta$}
    \end{cases}
$$
</div>