---
layout: math
mathjax: true
parent: "Slides"
title: 27. Interpreter AST
nav_order: 27
---

```ocaml
type sexp =
  | Atom of string
  | Bool of bool
  | Num of int
  | Ident of string
  | Lambda of string list * sexp
  | Call of primop * sexp list
  | App of sexp * sexp list

type primop =
  | Plus
  | Minus
  | Times
  | Divide
  | Eq
  | Less
  | If
  | And
  | Or
  | Not
```
