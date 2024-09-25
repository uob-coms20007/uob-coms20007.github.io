---
layout: math
mathjax: true
parent: "Slides"
title: 12. CFG Defn
nav_order: 12
---

<div class="defn" markdown="1">
A __Context Free Grammar (CFG)__ consists of four components:
* An alphabet of __terminal__ symbols, which we shall usually write as $\Sigma$ (capital letter sigma)
* A finite, non-empty set of __non-terminal__ symbols, disjoint from the terminals, which we shall usually write as $\mathcal{N}$ (caligraphic letter N)
* A finite set of __production rules__, which we shall usually write as $\mathcal{R}$ (caligraphic letter R)
* A designated non-terminal from $\mathcal{N}$, called the _start symbol_, which we will usually write as $S$.
</div>

{: .defn }
A __sentential form__ is just a sequence of terminals (from $\Sigma$) and nonterminals (from $\mathcal{N}$).

<div class="defn" markdown="1">
The __production rules__ $\mathcal{R}$ of a CFG consisting of components $$(\Sigma,\mathcal{N},\mathcal{R},S)$$ each have shape:

$$
  X \longrightarrow \alpha
$$

where $X$ is a nonterminal from $\mathcal{N}$ and $\alpha$ is a sentenial form over $\Sigma$ and $\mathcal{N}$.
</div>