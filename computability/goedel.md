---
layout: math
title: Gödel numbering
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
isn't.

Encodings of "systems" (logical systems, programming languages, etc.) in
themselves are often called __Gödel numberings__. They were first proposed by
the great logician [Kurt
Gödel](https://en.wikipedia.org/wiki/Kurt_G%C3%B6del) in the proof of his celebrated
[incompleteness
theorems](https://en.wikipedia.org/wiki/G%C3%B6del%27s_incompleteness_theorems).

Let $\textbf{Stmt}$ be the set of [ASTs of
While](https://uob-coms20007.github.io/reference/while/abstract-syntax.html).
For the rest of this unit we assume that we have a Gödel numbering

$$
  \gamma : \textbf{Stmt} \xrightarrow{\cong} \mathbb{N}
$$

which encodes While programs as natural numbers.

# Uses and abuses of Gödel numberings

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

# Code transformations


It may not be entirely clear for what sort of thing one might use a Gödel
numbering. The answer is that they can be used to compute _code
transformations_.

Let $\textbf{Stmt}$ be the set of all While statements represented as ASTs
(i.e. the mathematical counterpart of the relevant Haskell data type used to
represent While ASTs).

A __code transformation__ is a function $f : \textbf{Stmt} \to \textbf{Stmt}$.

Code transformations may seem close to Haskell's higher-order functions, in
that they take programs as arguments, and return programs (cf.
functions-as-data). However, they are strictly more powerful than that.
Essentially, the only thing a Haskell program with a higher-order argument 
`f :: a -> b` can do is call it at some inputs (see e.g. the definition of
`map`, which calls `f` on every element of a list).

Instead, code transformations are significantly more powerful, as they are
able to arbitrarily construct and deconstruct pieces of syntax. For example,
we can have a code transformation $h : \textbf{Stmt} \to \textbf{Stmt}$ that
is defined by

$$
  h(S) = \begin{cases}
    S; x := 0 & \text{ if $x$ occurs in $S$} \\
    S         & \text{ otherwise}
  \end{cases}
$$

Or we can even define $h' : \textbf{Stmt} \to \textbf{Stmt}$ by

$$
  h(S) = \begin{cases}
    S                 & \text{ if there is a while loop in $S$ } \\
    \textbf{skip}     & \text{ otherwise}
  \end{cases}
$$

This program transformation depends on the particular _syntactic_
characteristics of $S$ (i.e. whether or not it ever uses the variable $x$).
Modulo the different programming paradigms (functional vs. imperative), no
Haskell code can "examine" its higher-order inputs and tell whether they are
using some character or another.

The raison d'être of Gödel numberings is so that we can discuss the
computability of such code transformations. That is, we want to
- write code transformations that map While programs to other While programs,
- use the Gödel numbering to reflect them into functions $\mathbb{N} \to \mathbb{N}$, and
- compute these reflected functions in While itself.

Why we might want to do this last thing will become evident early next week.