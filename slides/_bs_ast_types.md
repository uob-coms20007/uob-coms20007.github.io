---
layout: math
mathjax: true
parent: "Slides"
title: 45. BS AST Types
nav_order: 45
---

```ocaml
(** A [form] is either a top-level definition or an expression to be evaluated .*)
type form =
  | Define of string * sexp
  | Expr of sexp

(** A [sexp] is an expression to be evaluated. *)
and sexp =
  | Bool of bool
  | Num of int
  | Ident of string
  | Lambda of string list * sexp
  | Call of primop * sexp list
  | App of sexp * sexp list

(** A [prog] is just a list of [form]. *)
type prog = form list

```