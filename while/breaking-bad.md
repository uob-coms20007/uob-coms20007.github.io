---
layout: math
title: Liveness and Safety
nav_order: 3
mathjax: true
parent: The While Language
---

# Liveness, Safety and Breaking Bad

The While language is powerful (you will see exactly how powerful with Alex).
The While language is also simple and boring. These are good properties to have
when trying to reason about programming languages and about programmes written
in them.

We've done a bit of reasoning about the language itself in the last problem
sheet. We're now going to reason a bit about programmes.

The most interesting concrete thing that having formal semantics for the
language enables us to do is to reason about the _safety_ and _correctness_ of
programmes.

Safety, the topic of this lecture, is the most basic property we can consider:
given a programme, we want to identify the initial states in which that
programme has a meaning. (More formally, given $s \in \mathcal{S}$, find
$\phi \subseteq \mathsf{State}$ such that,
for any $\sigma \in \phi$, there is $\sigma' \in \mathsf{State}$ and
$\left\langle s, \sigma \right\rangle \Downarrow \sigma'$.)[^safetynote]

## Safety for simple While programmes

Let's consider the following programme, which we decide to call
$\texttt{euclid}$ because we are people of culture.

$$
\begin{aligned}
& \texttt{q \(\leftarrow\) 0;}\\
& \texttt{r \(\leftarrow\) a;}\\
& \texttt{while (r <= b)}\\
& \left\lfloor~~\begin{aligned}
  & \texttt{r \(\leftarrow\) r - b;}\\
  & \texttt{q \(\leftarrow\) q + 1}
  \end{aligned}\right.
\end{aligned}
$$

There is an obvious predicate $\phi$ such that any initial state in $\phi$ will
lead to a terminating execution of $\texttt{euclid}$: $\phi = \emptyset$. It is
not a very interesting example, however.

Much less obviously, there also exists at least one initial state that does not
lead to a terminating execution of $\texttt{euclid}$. For example, here are two:

$$
\left[
\begin{align*}
\texttt{a} & \mapsto 42\\
\texttt{b} & \mapsto 0
\end{align*}
\right]
$$

$$
\left[
\begin{align*}
\texttt{a} & \mapsto 97\\
\texttt{b} & \mapsto -72
\end{align*}
\right]
$$

How can we convince ourselves that executing $\texttt{euclid}$ in these two
states does indeed not terminate? How can we then delimit (or approximate) the
set of states that do lead to terminating executions of $\texttt{euclid}$? If
we approximate this set of states, in which direction should we do so? (For
example, our first approximation above—a coarse underapproximation, was not
particularly useful at first glance. But would the equally coarse
overapproximation $\phi = \mathsf{State}$ be more useful?)

TBC

## Ways in which While programmes cannot go wrong

As mentioned, the While language is designed to be the most boring interesting
language.[^joke] As such there are very many ways in which real programmes in real
programming languages go wrong that we don't consider here.

- Python programmes can go wrong when you add an integer to a string, or when
  you call a method an object does not implement. Some programming languages
  catch such ways of "going wrong" using _type systems_, which you can learn
  about in Types and Lambda Calculus next year. While programme cannot go wrong
  in this way because their _syntax_ separates boolean from arithmetic
  expressions, and enforces that only arithmetic values get stored in the
  state.

- Programmes written in most programming languages "go wrong" when you divide
  by 0. While programmes cannot go wrong in this way because we don't have
  division operations.

- Programmes written in most imperative languages "go wrong" when they read
  from an invalid memory location. While programmes cannot go wrong in this way
  because all (syntactically valid) variable names can always be read from or
  written to.

- Programmes written in C (and most other languages) can "go wrong" if a signed
  integer arithmetic operation overflows the type's capacity. While programmes
  cannot go wrong in this way because they operate in some magical state that
  can store mathematical integers.

- Operating systems may force programmes who misuse memory to go wrong
  (independently of the language's semantics and whether a particular programme
  has a meaning).

The manner of the going wrong varies from "noisily fails" (arithmetic
exceptions on division by 0) to "quietly does something you do not want the
programme to do, like output a cryptographic secret to the network" (buffer
overflows, ...). Understanding what it means for a programme in your chosen
language to go wrong, and what happens in each case is _important_. Using
(static analysis) tools to help you check that a programme is not going to go
wrong is part of modern software engineering practice in large agile companies.

It is also worth noting that in some settings, non-termination does not count
as "going wrong": it is entirely OK to want the software that drives Air
Traffic Control in the UK to continue working as long as the computer it runs
on is powered and functional; it is also OK to want a web server to stay up.
But the While language does not have any features allowing it to affect the
world around it other than by terminating in a particular state. A While
programme that does not terminate is just a very inefficient heater.

## Breaking Bad: Making the While language fun

We are a bit touched, so we extend the While language so it can go wrong in
interesting ways. In particular, in the following, we add two new kinds of
binary arithmetic operators: quotient and modulo. We define their semantics to
"go wrong" when their second argument evaluates to 0. In our case, we choose to
make it so "going wrong" simply means "gets stuck". We will explore what that
means in practice in the problem sheet.

TBC

[^safetynote]: It is worth noting that, because the only way While programmes can go
    wrong is by not terminating, what we call safety here is, in fact called
    _liveness_ in the general programme verification literature.

[^joke]: Does that, then, make it more interesting than some other random
    interesting language? This is not as deep a question as it may seem.
