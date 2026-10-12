---
layout: math
mathjax: true
parent: "Slides"
title: 30. Lexer State API
nav_order: 30
---

* `peek ()` returns `Some c` when `c` is the current character of the input, and `None` otherwise.
* `drop ()` discards the current character from the input.
* `emit tk` adds the token `tk` to the end of the output token sequence.