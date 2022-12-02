---
layout: math
title: Reductions
nav_order: 11
mathjax: true
parent: Computability
---

# Reductions

Establishing that the [Halting
Problem](https://uob-coms20007.github.io/reference/computability/halting.html)
was undecidable was a laborious task. It required an explicit proof using a
diagonal method, and lots of careful reasoning.

We can sometimes prove that a predicate is undecidable by another method: we
show that _if it were decidable_, then $\textsf{HALT}$ would be decidable as
well. This is a _proof by reduction_.

## Definition

Let $U, W \subseteq \mathbb{N}$ be predicates, and let $f : \mathbb{N} \to
\mathbb{N}$. The function $f$ is a __many-one reduction__ from $U$ to $W$ just if
it is computable, and it is also the case that

$$
  n \in U\ ⟺ f(n) \in W
$$

We may write $f : U ≲ V$ (read "$f$ is a reduction from $U$ to $V$").

## Intuition

A many-one reduction $f : U ≲ V$ _transforms_ the problem of deciding $U$ to
the problem of deciding $V$.

Suppose I have a number $n$, and I want to determine whether $n \in U$.
Instead of doing that, I can compute $f(n)$; this is always defined because
$f$ is total, and it's always possible to compute because $f$ is a computable
function. Then, I can ask whether $f(n) \in V$ instead. Because the definition of $f$ involves a logical equivalence, I can deduce the following facts:
* if $f(n) \in V$, then I can tell that $n \in U$
* if $f(n) \not\in V$, then I can tell that $n \not\in U$ neither

Thus: instead of deciding whether $n \in U$, I can compute $f(n)$ and ask
whether $f(n) \in V$ instead.

This proves the 

*Lemma.* If $f : U ≲ V$ and $V$ is decidable, then so is $U$.

This explains the notation $U ≲ V$. If I can decide $V$, then I can decide
$U$. In other words, $U$ is an easier, "smaller" problem.


From this I can infer the
[contrapositive](https://en.wikipedia.org/wiki/Contraposition) of this
statement:

*Lemma.* If $f : U ≲ V$ and $U$ is undecidable, then so is $V$.

Thus, if $U$ is difficult, then the harder, "bigger" problem $V$ is also
difficult.

## Example

The strategy is now clear: if for a predicate $V \subseteq \mathbb{N}$ I can
find a reduction $f : \textsf{HALT} ≲ V$, then I have shown that $V$ is more
difficult than $\textsf{HALT}$. In other words: if I could 'solve' $V$, then
I could also 'solve' $\textsf{HALT}$. But the latter cannot happen, so
neither can the former.

For example, we can show the undecidability of the set

$$
  V = \{ \ulcorner S \urcorner \mid \forall n \in \mathbb{N}.\  ⟦ S ⟧_\texttt{x}(n) \downarrow \}
$$

of (the Gödel numbers of) programs which _never_ go into an infinite loop.

To construct the reduction $g : \textsf{HALT} ≲ V$, recall the notion of a
[code
transformation](https://uob-coms20007.github.io/reference/computability/goedel.html#code-transformation). Define a function

$$
  g : \textbf{Stmt} \times \mathbb{N} \to \textbf{Stmt}
$$

where $g(S, n)$ outputs a While program $G_{S, n}$ that performs the
following steps: on input $m$,
1. Simulate $S$ on input $n$.
2. If that simulation ever finishes, set `x := m`.

All that $g$ does is take some source code ($S$) and a number ($n$) as input,
and then write another program as output. The program $g(S, n)$ it outputs is
a program that first simulates the running of $S$ on $n$, and - if and when
that finishes - simply outputs its input. We know that we can write such a
program by adapting the code of the [universal
function](https://uob-coms20007.github.io/reference/computability/universal.html#universal-function).

The reduction we seek is the reflection $\tilde{g} : \mathbb{N} \to \mathbb{N}$
of $g$. Hence, $\tilde{g}$ is a function maps the natural number/encoding
$\langle \ulcorner S \urcorner, n \rangle$ to the natural number/encoding
$\ulcorner G_{S, n} \urcorner$.

Is this $\tilde{g}$ computable? It is indeed! It is not a stretch of the
imagination to think of a bash or Python script that assembles its source
code from the source code $S$.

Finally, $\tilde{g}$ is a reduction $\textsf{HALT} ≲ V$ because it is the
case that

$$
  \langle \ulcorner S \urcorner, n \rangle \in \textsf{HALT}
    ⟺
  \ulcorner G_{S, n} \urcorner \in V
$$

Indeed, the program $G_{S, n}$ always simulates the running of $S$ on input
$n$ before stopping with its own input as output. Thus, 
* If $\langle \ulcorner S \urcorner, n \rangle \in \textsf{HALT}$, then $S$
  halts on input $n$. Then, the program $G_{S, n}$ always halts, returning its
  own input as output. Consequently, the program $G_{S, n}$ is an expensive and
  roundabout way of computing the identity function. The identity function is
  always defined on all inputs, so $\ulcorner G_{S, n} \urcorner \in V$.
* If $\langle \ulcorner S \urcorner, n \rangle \not\in \textsf{HALT}$, then $S$
  does _not_ halt on input $n$. Then, the program $G_{S, n}$ also never halts:
  the simulation of $S$ on $n$ never ends! Thus, the program $G_{S, n}$ computes
  a function that is nowhere-defined. Hence $\ulcorner G_{S, n} \urcorner
  \not\in V$.

These two points prove the aforementioned equivalence.

Thus $\textsf{HALT}$ is "easier" than $V$. Hence $V$ is not decidable.