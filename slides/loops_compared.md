---
layout: math
mathjax: true
parent: "Slides"
title: 44. Loops Compared
nav_order: 44
---

<table>
  <tr>
    <td>
<div  markdown=1>
```c
  while (true) { 
    x = x * n; 
    y = y - 1; 
  } 
```
</div>

    </td>
    <td>
<div markdown=1>
```python
  while true:
    x = x * n
    y = y - 1

```
</div>
    </td>
  </tr>
  <tr>
    <td>
<div markdown=1>
  ```ocaml
    while true do 
      x := !x * n
      y := !y - 1
    done 
  ```
</div>
    </td>
    <td>
<div markdown=1>
  ```ada
    while true loop
      x := x * n;
      y := y - 1;
    end loop
  ```
</div>
    </td>
  </tr>
</table>