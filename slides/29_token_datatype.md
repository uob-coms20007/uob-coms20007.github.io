---
layout: math
mathjax: true
parent: "Slides"
title: 29. Token Datatype
nav_order: 29
---

```ocaml
type token =
  | TkIdent of string
  | TkVar of string
  | TkLParen
  | TkRParen
  | TkDefine
  | TkComma
  | TkEquals
```