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

Let $U, V \subseteq \mathbb{N}$ be predicates, and let $f : \mathbb{N} \to
\mathbb{N}$. The function $f$ is a __many-one reduction__ from $U$ to $V$ just if

1. it is computable, and 
2. it is also the case that

$$
  n \in U\ ⟺ f(n) \in V
$$

We may sometimes write $f : U ≲ V$ to mean that $f$ is a reduction from $U$ to $V$.

## Intuition

A many-one reduction $f : U ≲ V$ _reduces_ the problem of deciding $U$ to the
problem of deciding $V$.

Suppose I have a number $n$, and I want to determine whether $n \in U$.
Instead of doing that, I can compute $f(n)$. this is always defined because
$f$ is total, and it's always possible to compute because $f$ is a computable
function. Then, I can ask whether $f(n) \in V$ instead. Because the definition of $f$ involves a logical equivalence, I can deduce the following facts:
* if $f(n) \in V$, then I can tell that $n \in U$
* if $f(n) \not\in V$, then I can tell that $n \not\in U$ 

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

The strategy is now clear: if for a predicate $V \subseteq \mathbb{N}$ I can
find a reduction $f : \textsf{HALT} ≲ V$, then I have shown that $V$ is more
difficult than $\textsf{HALT}$. In other words: if I could 'solve' $V$, then
I could also 'solve' $\textsf{HALT}$. But the latter cannot happen, so

## Example
neither can the former.

For example, we can show the undecidability of the set

$$
  \textsf{ALL} = \{ \ulcorner S \urcorner \mid \forall n \in \mathbb{N}.\  ⟦ S ⟧_\texttt{x}(n) \downarrow \}
$$

of (the Gödel numbers of) programs which _never_ go into an infinite loop.

To construct the reduction $g : \textsf{HALT} ≲ \textsf{ALL}$, recall the notion
of a [code
transformation](https://uob-coms20007.github.io/reference/computability/goedel.html#code-transformation).
Define a function

$$
  g : \textbf{Stmt} \times \mathbb{N} \to \textbf{Stmt}
$$

where $g(S, n)$ is a While program $G_{S, n}$ that performs the
following steps: on input $m$,
1. Ignore the input.
2. Simulate $S$ on input $n$ (i.e. run the interpreter/universal function, giving it both $S$ and $n$, which are now hardcoded).
3. If that simulation ever finishes, set `x <- 0` to output $0$.

$g$ is a function that takes some source code ($S$) and a number ($n$) as input,
and returns another program as output. The program $g(S, n)$ it outputs is a
program that first simulates the running of $S$ on $n$, and - if and when that
finishes - simply returns 0 as its final result. We know that we can write such
a program by adapting the code of the [universal
function](https://uob-coms20007.github.io/reference/computability/universal.html#universal-function).

The reduction we seek is the reflection $\tilde{g} : \mathbb{N} \to \mathbb{N}$
of $g$. Hence, $\tilde{g}$ is a function maps the natural number/encoding
$\langle \ulcorner S \urcorner, n \rangle$ to the natural number/encoding
$\ulcorner G_{S, n} \urcorner$.

Is this $\tilde{g}$ computable? It is indeed! It is not a stretch of the
imagination to think of a bash or Python script that assembles the source code
$G_{S, n}$ from the source code $S$ and the number $n$.

Finally, $\tilde{g}$ is a reduction $\textsf{HALT} ≲ \textsf{ALL}$ because it is the
case that

$$
  \langle \ulcorner S \urcorner, n \rangle \in \textsf{HALT}
    ⟺
  \ulcorner G_{S, n} \urcorner \in \textsf{ALL}
$$

Indeed, the program $G_{S, n}$ always simulates the running of $S$ on input
$n$ before stopping with its own input as output. Thus, 
* If $\langle \ulcorner S \urcorner, n \rangle \in \textsf{HALT}$, then $S$
  halts on input $n$. Then, the program $G_{S, n}$ always halts, returning $0$.
  Consequently, the program $G_{S, n}$ is an expensive and roundabout way of
  computing the identity function. The identity function is always defined on
  all inputs, so $\ulcorner G_{S, n} \urcorner \in \textsf{ALL}$.
* If $\langle \ulcorner S \urcorner, n \rangle \not\in \textsf{HALT}$, then $S$
  does _not_ halt on input $n$. Then, the program $G_{S, n}$ also never halts:
  the simulation of $S$ on $n$ runs forever! Hence, $G_{S, n}$ computes a
  function that is undefined for any input. Hence $\ulcorner G_{S, n} \urcorner
  \not\in \textsf{ALL}$.

These two points prove the aforementioned equivalence.

Thus $\textsf{HALT}$ is "easier" than $\textsf{ALL}$. Hence $\textsf{ALL}$ is not decidable.

### Intuition

Showing the reduction above required some heavy symbols and some non-trivial
reasoning. Here is an intuitive explanation of what is going on:

1. Suppose that someone challenges you to solve the halting problem: they
   provide a particular $S$ and $n$ (encoded as natural numbers), and ask you if
   they are an instance of the halting problem (i.e. if $\langle \ulcorner S
   \urcorner, n \rangle \in \textsf{HALT}$.)

2. Given those two pieces of data (encoded as numbers), we were able to write a
   program $G_{S, n}$ whose encoding $\ulcorner G_{S, n} \urcorner$ was in
   $\textsf{ALL}$ exactly when $\langle \ulcorner S \urcorner, n \rangle$ was in
   $\mathsf{HALT}$. This program $G_{S, n}$ had $S$ and $n$ hard-coded into its
   source code (in the data section of the binary, as a constant, etc.) Morever,
   writing down this program $G_{S, n}$ was quite mechanical, and could be done
   by a computer.
   
3. This shows that every question about halting can be transformed to a question
   about $\textsf{ALL}$. Moreover, this transformation did not require thinking
   or ingenuity, and could be done automatically by a computer program
   ("metaprogramming", i.e. writing programs that input and output other
   programs).

4. Hence, if we could solve $\textsf{ALL}$, we could solve $\textsf{HALT}$.

5. We cannot solve $\textsf{HALT}$.

6. Therefore we cannot solve $\textsf{ALL}$ either.