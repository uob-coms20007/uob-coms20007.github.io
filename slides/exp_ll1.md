---
layout: math
mathjax: true
parent: "Slides"
title: 48. Exp LL(1)
nav_order: 48
---

$$
  \begin{array}{rcl}
    A &\Coloneqq& L\ R\\
    R &\Coloneqq& \tm{+}\ A\ R \mid \tm{*}\ A\ R \mid \epsilon\\
    L &\Coloneqq& \tm{num} \mid (\ A\ )
  \end{array}
$$

```ocaml
  and parse_a() : exp =
    match peek () with
    | TkNum _ | TkLParen -> 
        let e = parse_l () in
        let es = parse_r () in
        ???
```