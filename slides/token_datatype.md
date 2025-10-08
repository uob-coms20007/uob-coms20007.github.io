---
layout: math
mathjax: true
parent: "Slides"
title: 33. Token Datatype
nav_order: 33
---

```ocaml
(** [token] is an enumeration of all possible tokens produced by the lexer *)
type token =
  | TkLit of literal
  | TkIdent of string
  | TkLParen
  | TkRParen
  | TkDefine
  | TkLambda
  | TkPrimOp of primop
  | TkEnd
```