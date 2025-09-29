---
layout: math
mathjax: true
parent: "Slides"
title: 22. First(α)
nav_order: 22
---

$$
  \begin{array}{|c|c|}\hline
    \text{Nonterminal} & \text{Nullable?} & \text{First} & \text{Follow} \\\hline
    B & \times & \tt,\,\ff,\,( & ),\,\andop,\,\orop \\\hline
  \end{array}
$$

<div class="defn" markdown=1>
$$
  \first(\alpha) =
    \begin{cases}
      \emptyset & \text{if $\alpha = \epsilon$}\\
      \{a\} & \text{if $\alpha$ is of shape $a\beta$}\\
      \first(X) & \text{if $\alpha$ is of shape $X\beta$ and $\neg \nullable(X)$}\\
      \first(X) \cup \first(\beta) & \text{if $\alpha$ is of shape $X\beta$ and $\nullable(X)$}
    \end{cases}
$$
</div>