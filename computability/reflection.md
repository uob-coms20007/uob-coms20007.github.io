---
layout: math
title: Reflections
nav_order: 5
mathjax: true
parent: Computability
---

# Reflections: computing under representation

The technology of bijections allow us to define computability for sets $S$
other than $\mathbb{N}$.

## Reflecting functions on a single set

First, let us suppose we are able to represent a set $A$ through the natural
numbers. In other words, let us suppose we have a bijection $i : A
\xrightarrow{\cong} \mathbb{N}$, with inverse $i^{-1} : \mathbb{N}
\xrightarrow{\cong} A$.

We may think of $i$ as uniquely representing all the elements of set $A$ as
natural numbers.

Let $f : A ⇀ A$ be a partial function. We cannot directly speak to $f$'s
computability, as it is a partial function on the set $A$. However, we only know
what computability means for functions of type $\mathbb{N} \rightharpoonup
\mathbb{N}$ only. Nevertheless, the bijection $i$ allows us to _transport_ the
function to the set $\mathbb{N}$!


*Definition.* The __reflection__ of $f : A \rightharpoonup A$ *under* $i$ is the
function 

$$
  \begin{aligned}
  & \tilde{f} : \mathbb{N} ⇀ \mathbb{N} \\
  & \tilde{f}(n) = i(f(i^{-1}(n)))
  \end{aligned}
$$

Written in point-free style, we define $\tilde{f} = i \circ f \circ i^{-1}$.
This definition can also be expressed as a [commutative
diagram](https://en.wikipedia.org/wiki/Commutative_diagram):

$$
\require{amscd}
\begin{CD}
  A @>{f}>> A\\
  @Ai^{-1}AA @VViV \\
  \mathbb{N} @>{\tilde{f}}>> \mathbb{N}
\end{CD}
$$

(with apologies for not being able to render the partial arrow $⇀$ in the diagram)

The reflection $\tilde{f}$ computes $f$ "under" the encoding $i$. Thus,
instead of acting on elements of $A$, it acts on their encoded forms.

In a sense, the function $f : A ⇀ A$ is computable just if its reflection
$\tilde{f} : \mathbb{N} ⇀ \mathbb{N}$ is.

## Reflecting general functions

In the previous section we dealt with the computability of functions that had a
set $A$ as both domain and codomain. How can this be generalised to cover any
old function $f : A \to B$ with any domain $A$ and any codomain $B$?

In this case, we will need two bijections, one for each set:

$$
\begin{aligned}
  &\phi : A \xrightarrow{\cong} \mathbb{N}
  &
  &\psi : B \xrightarrow{\cong} \mathbb{N}
\end{aligned}
$$

As before, $\phi$ enables us to encode elements $a \in A$ as numbers $\phi(a)
\in \mathbb{N}$, while being invertible.

Similarly, $\psi$ enables us to encode elements $b \in B$ as numbers $\psi(b)
\in \mathbb{N}$, while also being invertible.

We can then play the same trick as before, but now using each bijection to
appropriately decode and then encode elements of $A$ and $B$ as natural numbers.

*Definition.* The __reflection__ of $f : A \rightharpoonup B$ *under* $(\phi,
\psi)$ is the function 

$$
  \begin{aligned}
  & \tilde{f} : \mathbb{N} ⇀ \mathbb{N} \\
  & \tilde{f}(n) = \psi(f({\phi}^{-1}(n)))
  \end{aligned}
$$

Written in point-free style, we define $\tilde{f} = \psi \circ f \circ \phi^{-1}$.
This definition can also be expressed as a commutative
diagram:

$$
\require{amscd}
\begin{CD}
  A @>{f}>> B\\
  @A{\phi}^{-1}AA @VV{\psi}V \\
  \mathbb{N} @>{\tilde{f}}>> \mathbb{N}
\end{CD}
$$

(with apologies for not being able to render the partial arrow $⇀$ in the diagram)

This enables us to define what it means for a general function $f : A \to B$
between any sets to be computable, as long as these sets can be put in bijection
with the natural numbers.