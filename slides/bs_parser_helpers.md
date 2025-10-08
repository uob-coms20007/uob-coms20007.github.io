---
layout: math
mathjax: true
parent: "Slides"
title: 37. Parsing Helpers
nav_order: 37
---

```ocaml
peek : unit -> token

eat : token -> unit
eat_lit : unit -> sexp
eat_ident : unit -> string
eat_primop : unit -> primop
```