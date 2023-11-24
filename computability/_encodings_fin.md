---
layout: math
title: Encoding finite data (BONUS)
nav_order: 6
mathjax: true
parent: Computability
---
# Encoding finite data


**This section is not to be taught or examined in 2022/23.**

For some sets of data $S$ it is impossible to find a bijection $S
\xrightarrow{\cong} \mathbb{N}$ that encodes $S$ as natural numbers.

This may happen for two reasons:
* _$S$ is too big._ For example, it is a [well-known
  theorem](https://en.wikipedia.org/wiki/Cantor's_theorem) that there cannot be
  a surjection $\mathcal{P}(\mathbb{N}) \twoheadrightarrow \mathbb{N}$ from the
  set of all subsets of natural numbers to the set of natural numbers.

* _$S$ is too small_. For example, it is easy to show that there cannot be a
  bijection $\mathbb{B} \xrightarrow{\cong} \mathbb{N}$.
  
In the first case there is little we can do (though some researchers disagree,
and [have worked out many interesting methods](https://en.wikipedia.org/wiki/Computable_number), 
some of them [even involving Haskell](https://math.andrej.com/2008/11/21/a-haskell-monad-for-infinite-search-in-finite-time/)).

In the second case, we have to use something that is _less than a bijection_.

This is the notion of sections and retractions.

## Sections and retractions

A function $s : A \to B$ is a __section__ just when there exists a
corresponding __retraction__, i.e. a function $r : B \to A$ such that for all
$a \in A$ it is the case that $r(s(a)) = a$.

In point-free style, this is written as $r \circ s = \textsf{id}_A$.

We sometimes call $(s, r)$ a __section-retraction pair__.

A retraction is _not_ an inverse to the section. Rather, it is a 'one-sided
inverse.' In terms of data representation, it says that we can encode every
$a \in A$ as an element $s(a)$ of $B$. We can then 'recover' the original $a
\in A$ by computing the element $r(s(a)) = a$.

## Example: Booleans vs. natural numbers

We may encode the set $\mathbb{B} = \{ \top, \bot \}$ of boolean values as
natural numbers through a section-retraction pair.

We define

$$
  \begin{aligned}
    & s : \mathbb{B} \to \mathbb{N} \\
    & s(\top) = 0 \\
    & s(\bot) = 1
  \end{aligned}
$$

We can recover the original Boolean value through the retraction

$$
  \begin{aligned}
    & r : \mathbb{N} \to \mathbb{B} \\
    & s(n) = \begin{cases}
      \top & \text{ if $n = 0$} \\
      \bot & \text{ otherwise}
    \end{cases}
  \end{aligned}
$$

We have $r(s(b)) = b$ for all $b \in \mathbb{B}$.

Many natural numbers ($2, 3, 4, \ldots$) are not in the image of the section
$s : \mathbb{B} \to \mathbb{N}$. Thus, they do not naturally arise as the
representation of some Boolean value: only $0$ and $1$ do. Thus, the
retraction $r : \mathbb{N} \to \mathbb{B}$ makes an 'executive decision' to
map all other numbers to falsity. (This choice is arbitrary, and we could have
chosen to map them to truth.)

Incidentally, this is the mathematical way to represent the implicit meaning
of [Unix/Linux/POSIX exit
codes](https://tldp.org/LDP/abs/html/exit-status.html): $0$ represents a
successful exit (true), while any other number signals an error (false).

This situation cannot be improved. While $\mathbb{N}$ is infinite, the set of
Boolean values $\mathbb{B}$ is finite. Thus, there cannot be a bijection (=
perfect correspondence) between them.

However, the section-retraction pair given above shows that we can encode
Boolean values as natural numbers in a 'lossless' way.

## Reflecting under section-retraction pairs

**This section is not to be taught or examined in 2022/23.**

It is worth noticing that the definition of a reflection does not require the
full power of a bijection. It is in fact sufficient to have a section-retraction
pair $(s : A \to \mathbb{N}, r : \mathbb{N} \to A)$.

*Definition.* The __reflection__ of $f$ *under* $(s, r)$ is the function 

$$
  \begin{aligned}
  & \tilde{f} : \mathbb{N} ⇀ \mathbb{N} \\
  & \tilde{f}(n) = s(f(r(n)))
  \end{aligned}
$$

Written in point-free style, we define $\tilde{f} = s \circ f \circ r$.

$$
\require{amscd}
\begin{CD}
  A @>{f}>> A\\
  @ArAA @VVsV \\
  \mathbb{N} @>{\tilde{f}}>> \mathbb{N}
\end{CD}
$$

Unlike the case of bijections, this can be used to encode computability on
finite data types.