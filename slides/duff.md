---
layout: math
mathjax: true
parent: "Slides"
title: 10. Duff's Device
nav_order: 10
---

```c
  switch (x) 
    if (0) 
      case 0: x=2; 
    else 
      case 1: x=3;
```