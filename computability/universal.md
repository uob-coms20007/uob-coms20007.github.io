---
layout: math
title: The universal function
nav_order: 8
mathjax: true
parent: Computability
---

From this point onwards let us fix a particular variable `x` wrt which our
[definition of
computability](https://uob-coms20007.github.io/reference/computability/functions.html#computes)
is stated.

We define a function

$$
  U : \mathbf{Stmt} \times \mathbb{N} ⇀ \mathbb{N}
  U(P, n) = ⟦ P ⟧_\texttt{x}(n)
$$

We often call $U$ the __universal function__, in the sense that it is able to
produce the behaviour of every computable function. If a function $f :
\mathbb{N} ⇀ \mathbb{N}$ is computable, then by definition $f = ⟦ P
⟧_\texttt{x}$. It follows that $f(n) \simeq ⟦ P ⟧_\texttt{x}(n) \simeq U(P,
n)$.

The following is a celebrated result in computability theory:

**Theorem (Kleene)**. The universal function is computable.

Let's unpack what this means. First things first: in order to speak of
computability, we have to encode the domain as natural numbers. The domain
itself is the product of two sets, $\textbf{Stmt}$ and $\mathbb{N}$. The
first can be encoded in the natural numbers through the Gödel numbering
$\gamma : \textbf{Stmt} \to \mathbb{N}$. The second is already the natural
numbers. We can then construct a bijection as the composite function

$$
  \mathbf{Stmt} \times \mathbb{N}
    \xrightarrow{\gamma \times \textsf{id}_\mathbb{N}}
  \mathbb{N} \times \mathbb{N}
    \xrightarrow{\phi}
  \mathbb{N}
$$

where if $f : A \to B$ and $g : C \to D$, the function $f \times g : A \times
C \to B \times D$ is defined by $(f \times g)(a, c) = (f(a), g(c))$. It is
not very difficult to prove that if $f$ and $g$ are bijections, then so is $f
\times g$. Moreover, we can prove that if $h : A \to B$ and $k : B \to C$ is
a bijection, then the composite $k \circ h$ is a bijection as well. Thus the
above composite $\phi \circ (\gamma \times \textsf{id}_\mathbb{N})$ is a
bijection.

It follows that we can reflect this function into the natural numbers using
the Gödel numbering and pairing. The theorem says that resultant function is
computable!

In lieu of a proof, let us sketch what this function does.

1. Upon receiving $n$ as an argument, it decodes it as a pair $n = \phi(e, n)$.
2. It decodes the first component $e \in \mathbb{N}$ into (the AST of) a While program $S = \gamma^{-1}(e)$.
3. It simulates the effect of the program $S$ on input $n$.

This last step consists of constructing the configuration $\langle S,
[\texttt{x} \mapsto n] \rangle$, followed by computing the next configuration
of the operational semantics of While. This is repeated until we reach the
simulation reaches a halting configuration $\langle \texttt{skip},
[\texttt{x} \mapsto m] \rangle$. At this point the program halts, returning
$m$ in its output.

That last bit may seem familiar: it is what we call an __interpreter__ for a
programming language. An interpreter is a program that loads the source code
of another program, and computes the intended behaviour of that program. One
way to write an interpreter is to follow the operational semantics of a
language, as described above: we construct the initial configuration, and
perform one step of computation at a time, until we are done. Usually
interpreters do not translate the source code, but merely produce the desired
behaviour directly. The most famous language that is interpreted (and not
compiled) is perhaps
[Python](https://en.wikipedia.org/wiki/Python_(programming_language).

This theorem of Stephen Kleene states that it is possible to write an
interpreter. Curiously, it was proven long before any actual computers
existed.

The proof of the theorem is by constructing the interpreter itself as a While
program. We will not do so, because the exact construction awfully tedious:
it involves unpairing a number of inputs, and pattern-matching on the next
instruction ("if the program is of the form $S_1; S_2$, then do the first
instruction of $S_1$, and so on). Using the very limited expressive features
of While - where everything has to be encoded as a natural number - means
that it would be a challenge to fully write out such an interpreter. However,
it should not be very hard for you to see how to write such an interpreter in
Haskell! Hence, we are willing to accept Kleene's theorem at face value,
without extensive proof.