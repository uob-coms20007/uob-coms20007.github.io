---
layout: math
title: Rice's theorem
nav_order: 12
mathjax: true
parent: Computability
---

# Rice's theorem

We will now prove another fundamental result of computability theory, which
is known as Rice's theorem.

Rice's theorem says that _telling what a computer program does from looking
at its source code is in general impossible_.

We can state this in a more mathematical way. Let $\mathcal{F} \subseteq
\mathbb{N} ⇀ \mathbb{N}$ be a predicate on _functions_. That is,
$\mathcal{F}$ is a set of "good" functions.

*Example.* Suppose that we want to decide whether a program returns $0$ when
given input $0$. This amounts to asking whether the function it computes is
in the set

$$
  \mathcal{F}_0 = \{\ f : \mathbb{N} ⇀ \mathbb{N} \mid f(0) \simeq 0 \}
$$

of "good" partial functions (i.e. functions that return $0$ on input $0$). ▣

Given any predicate $\mathcal{F} \subseteq \mathbb{N} ⇀ \mathbb{N}$ on
functions, we can _reflect_ it on the natural numbers under the Gödel
numbering. That is, we can define the set

$$
  F = \{ \gamma(S) \mid ⟦ S ⟧_\texttt{x} \in \mathcal{F} \}
$$

of (Gödel numbers of) programs which compute only "good" functions.

*Continuation of previous example.* Reflecting $\mathcal{F}_0$ into the natural numbers, we obtain the set

$$
  F_0 = \{ \gamma(S) \mid ⟦ S ⟧_\texttt{x}(0) \simeq 0 \}
$$

which contains the (Gödel numbers of) programs which output $0$ on input $0$. ▣

We then have the

**Theorem (Rice).** For any $\mathcal{F}$ that is not trivial (either empty
or all functions), the set $F$ is undecidable.

## Intuition

Recall the notion of _static analysis_ from our discussion of the [Halting Problem](https://uob-coms20007.github.io/reference/computability/halting.html#the-halting-problem).

A static analyzer is supposed to go through a program and detect bad
behaviours that might arise during runtime.

Thus, given a program $S$, a static analyzer is supposed to be able to tell
something about the function $⟦ S ⟧_\texttt{x}$ that it computes.

Rice's theorem states that it making a static analyzer is impossible. Asking
whether the function $⟦ S ⟧_\texttt{x}$ belongs to a class of desirable
functions $\mathcal{F}$ is an unsolvable problem (as long as $\mathcal{F}$ is
neither "no functions" nor "all functions", in which case the answer is
trivial).

The definition of decidability asks for _perfect_ static analysis: the set
$F$ is decidable if, upon seeing the source code $S$, we can tell with
certainty whether $S$ is good or $S$ is bad. Rice's theorem proves that we can never be that certain.

Then, how do we have static analyzers in practice? The reason is that the
static analyzers we use in real life are _imperfect_. Sometimes they fail to
catch all errors. Sometimes they report errors when there aren't any - i.e.
they are too _conservative_.

## Proof

The proof of Rice's theorem is non-examinable.

It is relatively easy to show by reduction from the Halting problem.