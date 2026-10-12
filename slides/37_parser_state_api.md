---
layout: math
mathjax: true
parent: "Slides"
title: 37. Parser State
nav_order: 37
---

* `peek ()` returns the next unconsumed token
* `drop ()` consumes the next unconsumed token (i.e. discards it)
* `eat tk` consumes the next unconsumed token if it is exactly `tk` and otherwise fails
* `eat_ident tk` consumes the next unconsumed token if it is an identifier token and returns the lexeme, otherwise fails
* `eat_var tk` consumes the next unconsumed token if it is a variable token and returns the lexeme, otherwise fails
