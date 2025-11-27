---
layout: math
title: Encoding first-order data
nav_order: 2
mathjax: true
parent: Computability
---

# Bijections 

Bijections can be used to put sets into exact correspondence with each other.

## Definitions

* A function $f : A \to B$ is __injective__ (or 1-1) just if for any $a_1, a_2
  \in A$ we have that $f(a_1) = f(a_2)$ implies $a_1 = a_2$. We sometimes write
  $f : A \rightarrowtail B$ whenever $f$ is an injection.

* A function $f : A \to B$ is __surjective__ just if for any $b \in B$ there
  exists $a \in A$ such that $f(a) = b$. We sometimes write $f : A
  \twoheadrightarrow B$ whenever $f$ is a surjection.

* A function $f : A \to B$ is a __bijection__ just if it is _both_ injective and
  surjective.

## Isomorphism

Let $f : A \to B$ be a function.

$f$ is an __isomorphism__ just if it has an __inverse__. That is, if there exists a function $f^{-1} : B \to A$
such that
* for all $a \in A$ we have $f^{-1}(f(a)) = a$
* for all $b \in B$ we have $f(f^{-1}(b)) = b$

Both equations are necessary!

Using [function
composition](https://uob-coms20007.github.io/notes/prereqs/functions.html#composition-of-functions)
and the [identity
function](https://uob-coms20007.github.io/notes/prereqs/functions.html#identity-function),
they may be re-written in a so-called _point-free_ style as
* $f^{-1} \circ f = \textsf{id}_A$
* $f \circ f^{-1} = \textsf{id}_B$

In the problem sheet you will prove the following result.

*Claim.* A function $f : A \to B$ is a bijection if and only if it is an
isomorphism.

Thus, a bijection $f : A \to B$ is a device for putting the sets $A$ and $B$
into perfect correspondence: it maps every $a \in A$ to a uniquely associated
$b \in B$. 
## Integers vs. natural numbers

In light of this, the following result is a bit surprising.

Define

$$
\begin{aligned}
  &\beta : \mathbb{Z} \to \mathbb{N} \\
  &\beta(x) = \begin{cases}
    2x    & \text{ if $x \geq 0$} \\
    -2x-1 & \text{ if $x < 0 $}
  \end{cases}
\end{aligned}
$$

$\beta$ is a __bijection between naturals and integers__. It maps 
* positive numbers to even numbers
* negative numbers to odd numbers

*Claim.* The function $\beta$ is a bijection.

_Proof._ 

1. $\beta$ is injective. Let $k_1, k_2 \in \mathbb{Z}$, and suppose $f(k_1) = f(k_2)$.
   There are four cases to consider:
   * *$k_1, k_2$ both positive.* Then $f(k_1) = f(k_2)$ is exactly the statement that $2k_1 = 2k_2$, whence $k_1 = k_2$.
   * *$k_1, k_2$ both negative.* Then $f(k_1) = f(k_2)$ is exactly the statement that $-2k_1 - 1 = -2k_2 - 1$, whence $k_1 = k_2$.
   * *$k_1$ positive, $k_2$ negative.* Then $f(k_1) = f(k_2)$ is exactly the statement that $2k_1 = -2k_2 - 1$. From this we infer $k_1 + k_2 = -1/2$, which cannot happen as both are whole numbers. Thus, this case cannot occur in practice, so there is nothing to prove.
   * *$k_1$ negative, $k_2$ positive.* Similar to the last case.

2. $\beta$ is surjective. Let $n \in \mathbb{N}$. We consider the parity of $n$.
   * If $n$ is even, write it as $n = 2n'$. Then $f(n') = 2n' = n$.
   * If $n$ is odd, write it as $n = 2n' + 1$. Then $f(-n' - 1) = -2(-n' - 1) - 1 = 2n' + 1$.

Hence $\beta$ is a bijection. ▣

We may also equivalently show that $\beta$ is a bijection by showing it's an
isomorphism, i.e. by constructing an inverse $\beta^{-1} : \mathbb{N} \to
\mathbb{Z}$. This inverse is

$$
  \beta^{-1}(n) =
    \begin{cases}
      n/2 & \text{ if $n$ is even} \\
      -(n+1)/2 & \text{ if $n$ is odd}
    \end{cases}
$$

We must then remember to show that 

$$
  \beta^{-1} \circ \beta = \text{id}_\mathbb{Z}
$$

and

$$

\beta \circ \beta^{-1} = \text{id}_\mathbb{N}

$$

# Encoding first-order data

The main way to encode a set $S$ of data as natural numbers is to construct a bijection $S \xrightarrow{\cong} \mathbb{N}$.

We have already seen how to construct a bijection $\beta : \mathbb{Z}
\xrightarrow{\cong} \mathbb{N}$, so that any integer may be encoded as a natural
number. We will now see how to encode other kinds of _first-order_ data (e.g.
numbers, lists, trees, etc.) as natural numbers.

## Encoding pairs of numbers

A __pairing function__ is a bijection $\mathbb{N} \times \mathbb{N}
\xrightarrow{\cong} \mathbb{N}$.

It is not immediately evident that a pairing function might exist: why should
it be possible to put $\mathbb{N}$ and $\mathbb{N} \times \mathbb{N}$ in
*exact* correspondence with each other? Does not the product set $\mathbb{N}
\times \mathbb{N}$ have somehow 'infinitely more' elements than $\mathbb{N}$?

Nevertheless, [a variety of pairing
functions](https://en.wikipedia.org/wiki/Pairing_function) exists.

* The [Cantor pairing
  function](https://en.wikipedia.org/wiki/Pairing_function#Cantor_pairing_function)
  scans the 'plane'
  [diagonally](https://en.wikipedia.org/wiki/Pairing_function#/media/File:Cantor's_Pairing_Function.svg).

* [Other pairing functions](https://en.wikipedia.org/wiki/Pairing_function#/media/File:Diagonal_argument.svg) cover the plane in a [boustrophedon](https://en.wikipedia.org/wiki/Boustrophedon) pattern (that of an ox plowing a field)

* ... and there are [many other variants](https://mathworld.wolfram.com/PairingFunction.html).

From this point onwards we assume that we have a fixed pairing function

$$
  \langle -, - \rangle : \mathbb{N} \times \mathbb{N} \xrightarrow{\cong} \mathbb{N}
$$

Moreover, we write

$$
  \textsf{split} : \mathbb{N} \xrightarrow{\cong} \mathbb{N} \times \mathbb{N}
$$

for its inverse.

## Encoding lists

Once we have a pairing function $\langle -, - \rangle$ and its inverse
$\textsf{split}$ we can use them to encode all sorts of other data.

For example, we may encode lists of natural numbers as numbers. The set of all
lists of natural numbers, $\mathbb{N}^\ast$, is defined by the following rules.

$$
  \frac{}{[] \in \mathbb{N}^\ast}
  \quad
  \frac{
    n \in \mathbb{N} \quad ns \in \mathbb{N}^\ast
  }{
    (n : ns) \in \mathbb{N}^\ast
  }
$$

To encode such a list as a number we define a function $\phi_\ast :
\mathbb{N}^\ast \to \mathbb{N}$ by induction:

$$
  \begin{aligned}
    \phi_\ast([])   &= 0 \\
    \phi_\ast(n:ns) &= 1 + \langle n, \phi_\ast(ns) \rangle
  \end{aligned}
$$

This function is a bijection. They key to writing down the inverse is to
notice that all operations used in its definition ($\phi$, $+1$) are
invertible. We define $\phi_\ast^{-1} : \mathbb{N} \to \mathbb{N}^\ast$ by

$$
  \begin{aligned}
    \phi_\ast^{-1}(n) = \begin{cases}
      []                     & \text{ if $n = 0$} \\
      x : \phi_\ast^{-1}(m) & \text{ if $n > 0$ and $(x, m) = \textsf{split}(n - 1)$}
    \end{cases}
  \end{aligned}
$$

Other algebraic data types (e.g. binary trees) may be encoded as natural numbers
by following the same pattern.

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
