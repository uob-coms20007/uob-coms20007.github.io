---
layout: math
title: Reasoning with Derivations
nav_order: 4
mathjax: true
parent: Semantics
---

<!-- # Proving Programs Correct

One of the most important applications of semantics is program verification, i.e. proving that a program is correct.
This goes above and beyond what is possible with testing as it covers all possible behaviour using rigorous mathematical techniques.

## Termination

First, we shall consider trying to show that a program terminates on all inputs.
Termination is a property that cannot be tested as we cannot know whether a program is about to terminate or will never terminate.
However, we can prove it terminates using the operational semantics.
In this context, the concept of termination is formalised as the existence of a final state - if it does not terminate, there will be no final state and vice versa.

Consider the following program, for example, that computes $y^x$:

```
z <- 1;
while 0 <= x do
  z <- z * y;
  x <- x + 1
```

We wish to prove that, for any initial state $\sigma \in \mathsf{State}$ with $\sigma(x) \geq 0$, there exists some final state $\sigma' \in \mathsf{State}$ such that $S_1,\, \sigma \Downarrow \sigma'$ where $S_1$ is the above program.
In other words, we must proof construct a derivation of the judgement $S,\, \sigma \Downarrow \sigma'$ for some $\sigma'$.
Initially, we can apply the rule for composition:

$$
  \dfrac
  {z \leftarrow 1,\, \sigma \Downarrow \sigma[z \mapsto 1]}
  {S_2,\, \sigma[z \mapsto 1] \Downarrow \sigma'}
  {S_1,\, \sigma \Downarrow \sigma'}
$$

where $S_2$ is the while loop: $\mathsf{while}\ 0 \leq x\ \mathsf{do}\ z \leftarrow z * y;\; x \leftarrow x + 1$.

However, the structure of the rest of this derivation will depend on the value of $x$ as that will dictate how many times the loop will be executed.
To construct the rest of the derivation, therefore, we will need to use an inductive argument.

Remember that the induction principle applies when trying to prove a statement of the form $\forall x \in X.\, P(x)$ where $X$ is an inductively defined set.
To apply the induction principle here, we will technically be proving the following statement:

$$
  \forall n \in \mathbb{N}.\, \forall \sigma \in \mathsf{State}.\, \textrm{if}\ \sigma(x) = n,\, \textrm{then}\  \exists \sigma' \in \mathsf{State}.\, S_2,\, \sigma \Downarrow \sigma'
$$

However, can simplify this process by thinking of it as induction on $\sigma(x)$ itself.
This gives us the following proof obligations:

 * Given that $\sigma(x) = 0$ show that there exists some $\sigma' \in \mathsf{State}.\, S,\, \sigma \Downarrow \sigma'$

 * Given that $\sigma(x) = n + 1$ and that, for any $\sigma'$ such that $\sigma(x) = n$, there exists some $\sigma'' \in \mathsf{State}.\, S,\, \sigma' \Downarrow \sigma''$ show that there exists some $\sigma'' \in \mathsf{State}.\, S,\, \sigma \Downarrow \sigma''$ --> -->

# Inversion Principle

A core concept derived from our the denotation semantics was that of semantic equivalence, i.e. when two expressions denote the same function.
A similar concept can be developed for While programs using the operational semantics.

<div class="defn" markdown="1">
  A statement $S_1 \in \mathcal{S}$ is said to be __semantically equivalent__ to another statement $S_2 \in \mathcal{S}$ if for any two states $\sigma,\, \sigma' \in \mathsf{State}$:
  $$
    S_1,\, \sigma \Downarrow \sigma' \Leftrightarrow S_2,\, \sigma \Downarrow \sigma'
  $$ 
</div>

Intuitively, two statements are semantically equivalent if when executed in the same start state they reach the same end state (or neither terminate).
To prove two statements are semantically equivalent, therefore, we consider _all_ possible behaviour they may exhibit.

Let us consider some arbitrary statements $S \in\mathcal{S}$ and another program $\mathsf{if}\ x \leq 2\ \mathsf{then}\ S\ \mathsf{else}\ S$.
We might wish to prove that these two programs are the semantically equivalent.
To do so, we must prove that:

  1 If $S,\, \sigma \Downarrow \sigma'$ for some states $\sigma,\, \sigma' \in \mathsf{State}$, then $\mathsf{if}\ x \leq 2\ \mathsf{then}\ S\ \mathsf{else}\ S,\, \sigma \Downarrow \sigma'$;

  * And, if $\mathsf{if}\ x \leq 2\ \mathsf{then}\ S\ \mathsf{else}\ S,\, \sigma \Downarrow \sigma'$ for some states $\sigma,\, \sigma' \in \mathsf{State}$, then $S,\, \sigma \Downarrow \sigma'$.

The first of this proof obligations is relatively straightforward.
Assuming $S,\, \sigma \Downarrow \sigma'$ for some states $\sigma,\, \sigma' \in \mathsf{State}$, we need only consider whether $\llbracket x \leq 2 \rrbracket_\mathcal{A}(\sigma)$ is true or not and this will allow us to construct a derivation for the judgement $\mathsf{if}\ x \leq 2\ \mathsf{then}\ S\ \mathsf{else}\ S,\, \sigma \Downarrow \sigma'$ as required.

The converse direction, however, is a little bit more subtle.
We start with the assumption that $\mathsf{if}\ x \leq 2\ \mathsf{then}\ S\ \mathsf{else}\ S,\, \sigma \Downarrow \sigma'$ for some states $\sigma,\, \sigma' \in \mathsf{State}$ and must show that $S,\, \sigma \Downarrow \sigma'$.
However, we now appear to be stuck as we don't have any information about the behaviour of $S$ itself and, therefore, it is unclear how we would construct a derivation of $S,\, \sigma \Downarrow \sigma'$.

To make progress with this proof, we need to analyse our assumptions more closely. 
As the operational semantics relation is _inductively_ defined we know that the judgement $\mathsf{if}\ x \leq 2\ \mathsf{then}\ S\ \mathsf{else}\ S,\, \sigma \Downarrow \sigma'$ must come from a derivation.
That is to say, there exists a derivation tree concluding with the judgement $\mathsf{if}\ x \leq 2\ \mathsf{then}\ S\ \mathsf{else}\ S,\, \sigma \Downarrow \sigma'$.
This may seem like a trivial observation, but it actually provides a lot of information.
In particular, we can now consider what form this derivation tree might take.
As our judgement concerns an $\mathsf{if}$ statement, we know that the associated derivation must use one of the following rules:

$$
  \begin{array}{cc}
    \dfrac
    {S_1,\, \sigma \Downarrow \sigma'}
    {\mathsf{if}\ e\ \mathsf{then}\ S_1\ \mathsf{else}\ S_2,\, \sigma \Downarrow \sigma'}
    \llbracket e \rrbracket_\mathcal{B}(\sigma) = \top

    &
    \dfrac
    {S_2,\, \sigma \Downarrow \sigma'}
    {\mathsf{if}\ e\ \mathsf{then}\ S_1\ \mathsf{else}\ S_2,\, \sigma \Downarrow \sigma'}
    \llbracket e \rrbracket_\mathcal{B}(\sigma) = \bot
  \end{array}
$$

If we instantiate the metavariables with the particular statement we are interested in, then one of the following must be true:

  * Either, $\llbracket e \rrbracket_\mathcal{B}(\sigma) = \top$ and $S,\, \sigma \Downarrow \sigma'$;
  * Or, $\llbracket e \rrbracket_\mathcal{B}(\sigma) = \bot$ and $S,\, \sigma \Downarrow \sigma'$.

In either case, we have that $S,\, \sigma \Downarrow \sigma'$ and can, therefore, conclude that $S$ and $\mathsf{if}\ x \leq 2\ \mathsf{then}\ S\ \mathsf{else}\ S,\, \sigma \Downarrow \sigma'$ are semantically equivalent.

This style of reasoning is known as the _inversion principle_ and can be applied to any inductively defined set.
Intuitively, the inversion principle simply states that for each inhabitant of an inductively defined set there exists a derivation of that judgement.
What this means in practice is that we can extract additional information from a judgement by considering what derivations could have produced.

We can unpack this idea a bit further by considering the particular from a While statement and their associated inference rules.

<div class="defn" markdown="1">
  The __inversion principle__ states that, if $S,\, \sigma \Downarrow \sigma'$ for some statement $S \in \mathcal{S}$ and states $\sigma,\, \sigma' \in \mathsf{State}$, then one of the following applies:

  * If $S$ is $\mathsf{skip}$, then $\sigma' = \sigma$;

  * If $S$ is $x \leftarrow e$, then $\sigma' = \sigma[x \mapsto \llbracket e \rrbracket_\mathcal{A}(\sigma)]$;

  * If $S$ is $S_1;\; S_2$, then $S_1,\, \sigma \Downarrow \sigma''$ and $S_2,\, \sigma'' \Downarrow \sigma'$ for some $\sigma'' \in \mathsf{State}$;

  * If $S$ is $\mathsf{if}\ e\ \mathsf{then}\ S_1\ \mathsf{else}\ S_2$, then either:
      * $\llbracket e \rrbracket_\mathcal{B} = \top$ and $S_1,\, \sigma \Downarrow \sigma'$,
      * Or $\llbracket e \rrbracket_\mathcal{B} = \bot$ and $S_2,\, \sigma \Downarrow \sigma'$;

  * If $S$ is $\mathsf{while}\ e\ \mathsf{do}\ S$, then either:
    * $\llbracket e \rrbracket_\mathcal{B} = \top$ and $S,\, \sigma \Downarrow \sigma''$ and $\mathsf{while}\ e\ \mathsf{do}\ S,\, \sigma'' \Downarrow \sigma'$ for some $\sigma'' \in \mathsf{State}$,
    * Or $\llbracket e \rrbracket_\mathcal{B} = \bot$ and $\sigma = \sigma'$.
</div>