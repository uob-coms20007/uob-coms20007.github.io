---
layout: math
mathjax: true
parent: "Slides"
title: 25. NFF for BExpLL
nav_order: 25
---

| Nonterminal | Nullable | First | Follow |
| S | | true, false, id, ( | | 
| D | | true, false, id, ( | \$, ) | 
| D' | $\checkmark{}$ | $\orop$ | \$, ) |
| C | | true, false, id, ( | $\orop$, \$, ) |
| C' | $\checkmark{}$ | $\andop$ | $\orop$, \$, ) |
| A | | true, false, id, ( | $\andop$, $\orop$, \$, ) | 