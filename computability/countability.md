---
layout: math
title: Countability
nav_order: 9
mathjax: true
parent: Computability
---

We can show that there exist non-computable functions without constructing
one explicitly. The method will be slightly indirect, but it will give us the
desired result nonetheless.

We call a set $S$ __countable__ whenever there is a bijection $S
\xrightarrow{\cong} \mathbb{N}$ with the natural numbers.

In the previous lectures we showed that a number of sets are countable:
- $\mathbb{N} \times \mathbb{N}$ is countable
- $\mathbb{N}^\ast$ is countable

In fact, both of these constructions adapt to arbitrary sets. Exercise: show
that if $A$ and $B$ are countable, then so are $A \times B$ and $A^\ast$.

In some sense we identified the following three characteristics of sets data
values:
- being first-order
- being "encodable" in While
- being countable

The key property of countable sets is that we can __enumerate__  - or, more
prosaically, _list - their elements. That is, if $f : S \xrightarrow{\cong}
\mathbb{N}$, then we can apply the inverse $f^{-1} : \mathbb{N} \to S$ to
each and every natural number to produce an infinite list

$$
  f^{-1}(0),
  f^{-1}(1),
  f^{-1}(2),
  \ldots
$$

The natural numbers come one after another, and create an infinite list. The same is true of any countable set: if $S$ is countable we can speak of:
- the first element of $S$ ($f^{-1}(0)$)
- the second element of $S$ ($f^{-1}(1)$)
and so on.

# An uncountable set

The definition would not be interesting if every set was countable. Indeed,
there is a standard construction that does _not_ preserve countability.

Recall that the __power set__ $\mathcal{P}(A)$ of a set $A$ is defined to be the set

$$
  \mathcal{P}(A) = \{ X \mid X \subseteq A \}
$$ 

consisting of all possible subsets of $A$ (including the empty set
$\emptyset$ and the entire set $A$ itself).

If we apply this construction to $\mathbb{N}$ we obtain the set

$$
  \mathcal{P}(\mathbb{N}) = \{ U \mid U \subseteq \mathbb{N} \}
$$

We recognise this set from the previous lectures: it is the set of all
[predicates](https://uob-coms20007.github.io/reference/prereqs/sets.html#predicate)
on the set of natural numbers.

We will prove the following classic result:

*Theorem.* There is no surjection $\mathbb{N} \to \mathcal{P}(\mathbb{N})$.

As any bijection is surjective, this implies that there is no bijection
$\mathbb{N} \xrightarrow{\cong} \mathcal{P}(\mathbb{N})$.

_Proof._ We prove the theorem through the method of
[diagonalisation](https://en.wikipedia.org/wiki/Cantor%27s_diagonal_argument).

Suppose there is a surjection $f : \mathbb{N} \to \mathcal{P}(\mathbb{N})$. 
Defining $U_i = f(i)$ we obtain the infinite list of predicates

$$
  U_0, U_1, U_2, \ldots
$$

We define a predicate $W$ that differs from every single predicate in that
infinite list. As a result, it cannot be that $W$ is in the image of $f$.
Thus, the assumption that $f$ was surjective was dodgy to begin with.

The predicate $W$ is defined by

$$
  W = \{ i \in \mathbb{N} \mid i \not\in U_i \}
$$

Thus if $i$ is in the $i$th predicate $U_i$, it is _not_ in $W$. So $W$ is
different from every $U_i$ in the aforementioned list. In symbols, if $W =
U_j$ for some $j$, then for any number $k$ we have

$$
  j \in U_j
  ⟺ j \in W 
  ⟺ j \not\in U_j
$$

This is obviously nonsense.

Thus, our original assumption that there was such a surjection cannot be true.  ▣


# Countability and computability

Define a function

$$
  \begin{aligned}
    & C : \mathbb{N} \to \mathcal{P}(\mathbb{N}) \\
    & C(i) = \begin{cases}
      U         & \text{ if $\chi_U = ⟦ \gamma^{-1}(i) ⟧_{\texttt{x}}$ for some predicate $U$ } \\
      \emptyset & \text{ otherwise }
    \end{cases}
  \end{aligned}
$$

This is a perfectly well-defined mathematical function. (It need not be computable though!)

We claim that $U$ is decidable if and only if it is in the image of $C$, i.e.
if $C(i) = U$ for some $i \in \mathbb{N}$.

- Suppose $U$ is decidable. Then its characteristic function $\chi_U$ is
  computable, i.e. it is equal to $⟦ S ⟧_{\texttt{x}}$ for some program $S$.
  Use the Goedel numbering to write $S = \gamma^{-1}(i)$ for some $i \in
  \mathbb{N}$. Then we see that it is the case that $C(i) = U$.

- Conversely, suppose $C(i) = U$. If $U = \emptyset$ then that is readily
  decidable (why?). Otherwise,  it must be the case that 
  $\chi_U = ⟦ \gamma^{-1}(i) ⟧_{\texttt{x}}$ . As a result, $U$ is decided by the program $\gamma^{-1}(i)$.

Thus, if $C$ is surjective, it must be the case that all predicates are decidable!

But we just proved that there is no surjection $\mathbb{N} \to \mathcal{P}(\mathbb{N})$.

Hence it cannot be that all predicates are decidable: there must be at least
one that isn't. Hence, its characteristic function will not be computable.