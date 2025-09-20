---
layout: math
mathjax: true
parent: "Slides"
title: 19. Lexer State
nav_order: 19
---

* `init s` - will load the string `s` into `input`, set `input_len` accordingly and set `idx` to `0`.
* `is_more ()` - returns true if the input is not yet fully consumed and false o/w
* `peek ()` - will return the next unmatched character without consuming it
* `eat c` - attempts to match the next unmatched character to its argument, if it matches then it advances the index and otherwise raises an exception.