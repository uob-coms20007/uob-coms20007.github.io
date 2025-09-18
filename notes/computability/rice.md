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

Rice's theorem says that **telling what a computer program does from looking
at its source code is either trivial or impossible**.

We can state this in a more mathematical way. Let 

$$
\mathcal{F} \subseteq \mathcal{PR}
$$

be some set of computable functions. Recall that

$$
  \mathcal{PR} = \{ f : \mathbb{N} ⇀ \mathbb{N} \mid \text{$f = ⟦ P ⟧_{\texttt{x}}$ for some While program $P$ } \}
$$

is the set of _all_ computable functions. $\mathcal{F}$ picks out only some of
them, which we will regard as the "good" computable functions.

We would like to be able to tell whether a computable function $f$ is "good",
i.e. in $\mathcal{F}$, just by looking at its source code. 

### Example 1

Suppose that we want to decide whether a program returns $0$ when
given input $0$. This amounts to asking whether the function it computes is
in the set

$$
  \mathcal{F}_0 = \{\ f : \mathbb{N} ⇀ \mathbb{N} \in \mathcal{PR} \mid f(0) \simeq 0 \}
$$

of computable functions that return $0$ on input $0$. ▣

### Example 2

Suppose that we want to decide whether a program terminates on all
inputs. This amounts to asking whether the function it computes is in the set

$$
  \mathcal{T} = \{\ f : \mathbb{N} ⇀ \mathbb{N} \in \mathcal{PR} \mid \text{$f$ is total, i.e. } \forall n. f(n) \downarrow \}
$$

of computable functions that are also total. ▣

### Reflecting into the natural numbers

Given any predicate $\mathcal{F} \subseteq \mathbb{N} ⇀ \mathbb{N}$ on
functions, we can _reflect_ it on the natural numbers under the Gödel numbering.
That is, we can define the set

$$
  F = \{ \ulcorner S \urcorner \mid ⟦ S ⟧_\texttt{x} \in \mathcal{F} \}
$$

of (Gödel numbers of) programs which compute only "good" functions.

### Example 1

Reflecting $\mathcal{F}_0$, we obtain the set

$$
  F_0 = \{ \ulcorner S \urcorner \mid ⟦ S ⟧_\texttt{x}(0) \simeq 0 \}
$$

which contains the (Gödel numbers of) programs which output $0$ on input $0$. ▣

### Example 2

Reflecting $\mathcal{T}$, we obtain the set

$$
  T = \{ \ulcorner S \urcorner \mid ⟦ S ⟧_\texttt{x}(0) \text{ is a total function } \}
$$

which contains the (Gödel numbers of) programs which terminate on any input. ▣

## Statement of the theorem

We then have the

**Theorem (Rice).** For any $\mathcal{F}$ of computable functions that is not
trivial (either the empty set, or the set of all computable functions), the
reflected set $F$ is undecidable.

The proof of Rice's theorem is by reduction.

## Intuition

Recall the notion of _static analysis_ from our discussion of the [Halting
Problem](https://uob-coms20007.github.io/reference/computability/halting.html#the-halting-problem).

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