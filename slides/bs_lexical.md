---
layout: math
mathjax: true
parent: "Slides"
title: 22. BS Lexical Struct
nav_order: 22
---

For _Brischeme_, the lexemes are the largest substrings not containing whitespace that fall into one of the following classifications (classifications given in bold):

  * The substrings `(`, `)` are the left (**lparen**) and right (**rpraren**) parentheses.
  * The substrings `+`, `-`, `*`, `/`, `<`, `=`, `not`, `and`, `or` are the **primops** (primitive operators).
  * The substring `define` is the keyword **define** and the substring `lambda` is the keyword **lambda**.
  * Any non-empty sequence of digits 0-9, and the substrings `#t` and `#f` are **literals** (number literals and Boolean literals respectively). 
  * Any non-empty substring, not falling into one of the above classes, that: 
      - begins with a lowercase letter of the English alphabet
      - proceeds with letters that are either lower or upper case letters of the English alphabet, the underscore, an exclamation mark, a question mark or digits
  
      is an **ident** (identifier).