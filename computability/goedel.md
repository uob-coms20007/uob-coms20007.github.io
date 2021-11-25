---
layout: math
title: Encodings
nav_order: 7
mathjax: true
parent: Computability
---


# Gödel numbering

We have now shown how to put the following things in bijection with the
natural numbers:
[integers](https://uob-coms20007.github.io/reference/computability/bijections.html#bijection-between-naturals-and-integers),
[pairs of
naturals](https://uob-coms20007.github.io/reference/computability/encodings.html#pairing-function),
[lists of
naturals](https://uob-coms20007.github.io/reference/computability/encodings.html#encoding-lists),
and [binary trees of
naturals](https://uob-coms20007.github.io/reference/computability/encodings.html#encoding-binary-trees).

But in the previous part of the course we saw that While programs themselves
may be represented as an [Abstract Syntax Tree
(AST)](https://uob-coms20007.github.io/reference/while/abstract-syntax.html).
So it is not a stretch of the imagination to imagine that we may encode While
ASTs as natural numbers.

As While's chief data type is that of integers, an encoding of this sort
means that _we will be able to compute functions that modify or act on While
programs within While itself_. This sounds somewhat paradoxical, but it
isn't. You will explore the way this is done in next week's Haskell-based
problem sheet.

Encodings of "systems" (logical systems, programming languages, etc.) in
themselves are often called __Gödel numberings__. They were first proposed by
the great logician [Kurt
Gödel](https://en.wikipedia.org/wiki/Kurt_G%C3%B6del) in the proof of his celebrated
[incompleteness
theorems](https://en.wikipedia.org/wiki/G%C3%B6del%27s_incompleteness_theorems).

Gödel numberings allow us to prove various __impossibility results__ about
logical systems and/or programming languages. All these proofs proceed
through constructions that seem quasi-paradoxical (but, again, are not). The
apparent paradox often implies that the programming language has some
limitation, i.e. is unable to achieve some task (compute a function, decide a
predicate, etc.). We will see such a result next week.

There is a [significant amount of
lore](https://en.wikipedia.org/wiki/G%C3%B6del,_Escher,_Bach) surrounding
Gödel numberings. However, this lore somewhat overstates their importance.
When decoded intuitively, most impossibility results amount to statements  "a
Parliament cannot grant itself amnesty by its own vote: it must recruit some
external authority larger than itself."


