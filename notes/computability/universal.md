---
layout: math
title: The universal function
nav_order: 8
mathjax: true
parent: Computability
---

# The Universal Function

From this point onwards let us fix a particular variable `x` wrt which our
[definition of
computability](https://uob-coms20007.github.io/reference/computability/functions.html#computes)
is stated.

We define a function

$$ 
  \begin{aligned}
    & U : \mathbf{Stmt} \times \mathbb{N} ⇀ \mathbb{N} \\
    & U(P, n) = ⟦ P ⟧_\texttt{x}(n)
  \end{aligned}
$$

We often call $U$ the __universal function__, in the sense that it is able to
produce the behaviour of every computable function. If a function $f :
\mathbb{N} ⇀ \mathbb{N}$ is computable, then by definition it is equal to $⟦ P
⟧_{\texttt{x}}$ for some While program $P$. It follows that 

$$
  f(n) \simeq ⟦ P ⟧_\texttt{x}(n) \simeq U(P,n)
$$

for all $n \in \mathbb{N}$.

$U$ is partial because these computable functions $⟦ P ⟧_\texttt{x}$ are
partial themselves.

The following is a celebrated result in computability theory:

**Theorem (Kleene)**. The universal function is computable.

Let's unpack what this means. First things first: in order to speak of
computability, we have to encode the domain as natural numbers. The domain
itself is the product of two sets, $\textbf{Stmt}$ and $\mathbb{N}$. The first
can be encoded in the natural numbers through the Gödel numbering $\ulcorner -
\urcorner : \textbf{Stmt} \to \mathbb{N}$. The second is already the natural
numbers. By an exercise in one of the sheets we can then construct a bijection
as the composite function

$$
  \mathbf{Stmt} \times \mathbb{N}
    \xrightarrow{\ulcorner - \urcorner \times \text{id}_\mathbb{N}}
  \mathbb{N} \times \mathbb{N}
    \xrightarrow{\langle -, - \rangle}
  \mathbb{N}
$$

where if $f : A \to B$ and $g : C \to D$ we define 

$$
  \begin{aligned}
    & f \times g : A \times C \to B \times D \\ 
    & (f \times g)(a, c) = (f(a), g(c))
  \end{aligned}
$$

It is not very difficult to prove that if $f$ and $g$ are bijections, then so is
$f \times g$. Moreover, we have previously shown that the composite of two
bijections is a bijection, so the composite function $\langle -, - \rangle \circ
(\ulcorner - \urcorner \times \text{id}_\mathbb{N})$ is a bijection.

It follows that we can reflect this function into the natural numbers using
the Gödel numbering and pairing. 

The theorem says that resultant function is computable!

In lieu of a proof, let us informally sketch what the program that computes this
does.

1. Upon receiving $i$ as an argument, it decodes it as a pair $(e, n) =
   \textsf{split}(i)$.
2. It decodes the first component $e = \ulcorner S \urcorner \in \mathbb{N}$
   into (the AST of) a While program $S$.
3. It simulates the running of the program $S$ on input $n$.

This last step consists of constructing the configuration $\langle S,
[\texttt{x} \mapsto n] \rangle$, followed by computing the (unique) next
configuration of the operational semantics of While. This is repeated until
the simulation reaches a halting configuration $\langle \texttt{skip},
[\texttt{x} \mapsto m] \rangle$. At this point the program halts, returning
$m$ in its output.

That description may seem familiar: it corresponds to what we would nowadays
call an __interpreter__ for a programming language. An interpreter is a
program that loads the source code of another program, and computes the
intended behaviour of that program. One way to write an interpreter is to
follow the operational semantics of a language, as described above. Usually
interpreters do not translate the source code, but merely produce the desired
behaviour directly. The most famous language that is interpreted (and not
compiled) is perhaps
[Python](https://en.wikipedia.org/wiki/Python_(programming_language)).

This theorem of Stephen Kleene states that it is possible to write an
interpreter. Curiously, it was proven long before any actual computers
existed!

The proof of the theorem proceeds by actually constructing the interpreter
itself as a While program. We will not do so, because it is awfully tedious:
it involves unpairing a number of inputs, and pattern-matching on the next
instruction (if the program is of the form $S_1; S_2$, then do the first
instruction of $S_1$, and so on). As While only supports natural numbers, all
this needs to be done numerically, by computing the reflections these
functions! Evidently, that is a formidable task. 

However, it should not be very hard for you to see how to write such an
interpreter in Haskell, using the powerful features of algebraic data types
and pattern-matching. 

For the purposes of this unit we are willing to accept Kleene's theorem at
face value, without an explicit construction. We will also be reasonably
relaxed about the explicit construction of such programs from this point
onwards, and accept
[handwaving](https://en.wikipedia.org/wiki/Hand-waving#In_mathematics_(and_formal_logic,_philosophy,_theoretical_science))
instead.