---
layout: math
title: Determinism
nav_order: 4
mathjax: true
parent: Semantics
---

## Inversion and Determinism

Recall the table sumarising the transition rules from the last lecture:

| Configuration $\gamma$ | Rules that can derive a step from $\gamma$ | Which rule applies? |
|---|---|---|
| $\sigma$ | none | -- |
| $\langle \mathsf{skip},\, \sigma \rangle$ | (Skip) | always |
| $\langle x \leftarrow e,\, \sigma \rangle$ | (Assign) | always |
| $\langle S_1;\; S_2,\, \sigma \rangle$ | (Seq-Done), (Seq-Step) | depends on whether $\langle S_1,\, \sigma \rangle$ steps to a state or to a configuration containing a statement |
| $\langle \mathsf{if}\ b\ \{ S_1 \}\ \mathsf{else}\ \{ S_2 \},\, \sigma \rangle$ | (If-True), (If-False) | depends on whether $b$ is true or false in $\sigma$ |
| $\langle \mathsf{while}\ b\ \{ S \},\, \sigma \rangle$ | (While-True), (While-False) | depends on whether $b$ is true or false in $\sigma$ |

We described the transition relation $\rightarrow$ as the _least_ relation closed under the rules.
This means that not only does each rule describe a possible step, but every possible step is derived from one of these rules.
Therefore, we can inspect an arbitrary step $\gamma \rightarrow \gamma'$ and ask which rule must have been used to derive it using a principle known as _inversion_.

<div class="defn" markdown="1">
__Inversion.__ Suppose $\gamma \rightarrow \gamma'$.
Then $\gamma = \langle S,\, \sigma \rangle$ is a non-terminal configuration (there is no step from a state) and, depending on the form of $S$:

  - if $S = \mathsf{skip}$, then $\gamma' = \sigma$;
  - if $S = x \leftarrow e$, then $\gamma' = \sigma[x \mapsto \llbracket e \rrbracket_\mathcal{A}(\sigma)]$;
  - if $S = S_1;\; S_2$, then either $\langle S_1,\, \sigma \rangle \rightarrow \sigma'$ and $\gamma' = \langle S_2,\, \sigma' \rangle$, or $\langle S_1,\, \sigma \rangle \rightarrow \langle S_1',\, \sigma' \rangle$ and $\gamma' = \langle S_1';\; S_2,\, \sigma' \rangle$;
  - if $S = \mathsf{if}\ b\ \{ S_1 \}\ \mathsf{else}\ \{ S_2 \}$, then $\gamma' = \langle S_1,\, \sigma \rangle$ when $$\llbracket b \rrbracket_\mathcal{B}(\sigma) = \top$$ and $\gamma' = \langle S_2,\, \sigma \rangle$ when $$\llbracket b \rrbracket_\mathcal{B}(\sigma) = \bot$$;
  - if $S = \mathsf{while}\ b\ \{ S' \}$, then $\gamma' = \sigma$ when $$\llbracket b \rrbracket_\mathcal{B}(\sigma) = \bot$$ and $\gamma' = \langle S';\; \mathsf{while}\ b\ \{ S' \},\, \sigma \rangle$ when $$\llbracket b \rrbracket_\mathcal{B}(\sigma) = \top$$.
</div>

For instance, if $\langle \mathsf{skip},\, \sigma \rangle \rightarrow \gamma$, then we know that $\gamma$ is the terminal configuration $\sigma$.
The same reasoning applies to many step traces.
If $\langle S_1;\; S_2,\, \sigma \rangle \rightarrow^* \gamma$, for example, then we know there are only two possible cases depending on whether $S_1$ reaches a terminal configuration or not - either $\langle S_1,\, \sigma \rangle \sigma'$ and $\langle S_2,\, \sigma'\rangle \rightarrow^* \gamma$ or $\langle S_1,\, \sigma \rangle \langle S_1',\, \sigma' \rangle$ and $\langle S_1';\; S_2,\, \sigma'\rangle \rightarrow^* \gamma$.

In other words, inversion tells us that the form of a statement and the state _completely_ determine which rules could apply.
As a result, the programs may only result in at most one output - a property known as _determinism_. 

<div class="defn" markdown="1">
__Determinism.__ If $\gamma \rightarrow \gamma'$ and $\gamma \rightarrow \gamma''$, then $\gamma' = \gamma''$.
</div>

_Proof._
By inversion, $\gamma = \langle S,\, \sigma \rangle$.
We prove, by structural induction on the statement $S$, that for all states $\sigma$ the configuration $\langle S,\, \sigma \rangle$ has at most one successor.

  - If $S$ is $\mathsf{skip}$ or an assignment, inversion says there is only one possible step.
  - If $S$ is an $\mathsf{if}$ or a $\mathsf{while}$, then $$\llbracket b \rrbracket_\mathcal{B}(\sigma)$$ is either $\top$ or $\bot$ but not both, so by inversion exactly one rule can apply, and it yields a single configuration.
  - If $S = S_1;\; S_2$, then by inversion any step from $\langle S_1;\; S_2,\, \sigma \rangle$ comes from a step of $\langle S_1,\, \sigma \rangle$.
    By the induction hypothesis for $S_1$, there is at most one such step, to some configuration $\gamma_1$.
    If $\gamma_1$ is a state $\sigma_1$, then only (Seq-Done) can apply (the premise of (Seq-Step) needs a configuration containing a statement), and the step must be to $\langle S_2,\, \sigma_1 \rangle$.
    If $\gamma_1 = \langle S_1',\, \sigma_1 \rangle$, then only (Seq-Step) can apply, and the step must be to $\langle S_1';\; S_2,\, \sigma_1 \rangle$.
    Either way there is at most one successor.

The same argument extends to many-step traces.

As a result, we can think of the semantics of programs not just as arbitrary relations but specifically as _partial functions_.

## Semantic Equivalence

As with denotational semantics, there is a natural notion for when two program statements have the same behaviour.

<div class="defn" markdown="1">
  We say that two statements $S_1,\, S_2 \in \mathcal{S}$ are __semantically equivalent__ if $$
    \langle S_1,\, \sigma \rangle \rightarrow^* \sigma' \Leftrightarrow \langle S_2,\, \sigma \rangle \rightarrow^* \sigma'
  $$ for all states $\sigma,\, \sigma' \in \mathsf{State}$.
</div>

Viewing statements as relations between initial and final states, $S_1$ and $S_2$ are semantically equivalent if the relations $$\{ (\sigma,\, \sigma') \mid \langle S_1,\, \sigma \rangle \rightarrow^* \sigma' \}$$ and $$\{ (\sigma,\, \sigma') \mid \langle S_2,\, \sigma \rangle \rightarrow^* \sigma' \}$$ are equal in that they contain the same pairs.
As these relations are partial functions, however, equivalently we have that, from every state $\sigma$, either both statements diverge or both terminate with the same final state.

<!-- This is a different use of the word "equivalent" from the denotational semantics, where two _expressions_ are equivalent if they have the same value in every state; here we compare _statements_ by the final states that they can reach.
When we say that a Boolean expression $e$ is equivalent to $\mathsf{false}$, we mean that $$\llbracket e \rrbracket_\mathcal{B}(\sigma) = \bot$$ for every state $\sigma$. -->

To prove that two statements are semantically equivalent, fix arbitrary states $\sigma$ and $\sigma'$ and prove both directions of the bi-implication.
Often it is easiest to use determinism to work out _every_ trace from $\langle S_1,\, \sigma \rangle$ and from $\langle S_2,\, \sigma \rangle$, and compare where they end up.
To prove that two statements are _not_ semantically equivalent, exhibit a state from which they behave differently.
Here are three examples.

__Example 1.__
The statements $x \leftarrow x + 1;\; x \leftarrow x + 1$ and $x \leftarrow x + 2$ are semantically equivalent.
Fix a state $\sigma$ and let $n = \sigma(x)$.
By (Seq-Done) and (Assign), and then (Assign) again, we have the trace

$$
  \begin{array}{l}
    \langle x \leftarrow x + 1;\; x \leftarrow x + 1,\, \sigma \rangle \\
    \quad \rightarrow \langle x \leftarrow x + 1,\, \sigma[x \mapsto n + 1] \rangle \\
    \quad \rightarrow \sigma[x \mapsto n + 1][x \mapsto n + 2] = \sigma[x \mapsto n + 2]
  \end{array}
$$

where the final equality is the second update law.
By determinism this is the only trace from this configuration, and so $\langle x \leftarrow x + 1;\; x \leftarrow x + 1,\, \sigma \rangle \rightarrow^* \sigma'$ if and only if $\sigma' = \sigma[x \mapsto n + 2]$.
Likewise, $\langle x \leftarrow x + 2,\, \sigma \rangle \rightarrow \sigma[x \mapsto n + 2]$ is the only trace from this configuration, so $\langle x \leftarrow x + 2,\, \sigma \rangle \rightarrow^* \sigma'$ if and only if $\sigma' = \sigma[x \mapsto n + 2]$.
The two statements reach exactly the same states.

__Example 2.__
The statements $x \leftarrow y$ and $y \leftarrow x$ are _not_ semantically equivalent.
Take $\sigma = [y \mapsto 1]$.
Then $\langle x \leftarrow y,\, \sigma \rangle \rightarrow [x \mapsto 1,\, y \mapsto 1]$, whereas $\langle y \leftarrow x,\, \sigma \rangle \rightarrow \sigma[y \mapsto 0] = []$, using the first and second update laws.
Since $[x \mapsto 1,\, y \mapsto 1] \neq []$, by determinism the state $[]$ is reachable from $\langle y \leftarrow x,\, \sigma \rangle$ but not from $\langle x \leftarrow y,\, \sigma \rangle$.

__Example 3.__
A while loop is semantically equivalent to its unfolding: for any statement $S$ and Boolean expression $b$, writing $W$ for $$\mathsf{while}\ b\ \{ S \}$$,

$$
  W \quad \text{is semantically equivalent to} \quad \mathsf{if}\ b\ \{ S;\; W \}\ \mathsf{else}\ \{ \mathsf{skip} \}.
$$

Write $I$ for the $\mathsf{if}$ statement and fix a state $\sigma$.
We consider two cases, according to the value of $b$ in $\sigma$.

  - Suppose $$\llbracket b \rrbracket_\mathcal{B}(\sigma) = \top$$.
    By inversion, the only step from $\langle W,\, \sigma \rangle$ is to $\langle S;\; W,\, \sigma \rangle$ by (While-True), and the only step from $\langle I,\, \sigma \rangle$ is also to $\langle S;\; W,\, \sigma \rangle$ by (If-True).
    Neither $\langle W,\, \sigma \rangle$ nor $\langle I,\, \sigma \rangle$ is a state, so by inversion for many steps any trace from either of them to a state $\sigma'$ begins with this step.
    Hence $\langle W,\, \sigma \rangle \rightarrow^* \sigma'$ if and only if $\langle S;\; W,\, \sigma \rangle \rightarrow^* \sigma'$ if and only if $\langle I,\, \sigma \rangle \rightarrow^* \sigma'$.

  - Suppose $$\llbracket b \rrbracket_\mathcal{B}(\sigma) = \bot$$.
    By inversion, the only step from $\langle W,\, \sigma \rangle$ is to $\sigma$ by (While-False), and the only step from $\langle I,\, \sigma \rangle$ is to $\langle \mathsf{skip},\, \sigma \rangle$ by (If-False), whose only step is to $\sigma$ by (Skip).
    So both statements reach $\sigma$ and nothing else.

Note that the definition of semantically equivalent statements does not require them to have traces of the same length.
For instancce, in this final case, the loop takes one step and the $\mathsf{if}$ statement takes two.
Equivalence concerns which states can be reached, not how many steps it takes to reach them.

<!-- - Equivalence only observes terminating behaviour.
  Any two statements that diverge from every state are semantically equivalent, even if they do very different things along the way: for instance, $$\mathsf{while}\ (x \leq x)\ \{ \mathsf{skip} \}$$ and $$\mathsf{while}\ (x \leq x)\ \{ x \leftarrow x + 1 \}$$. -->

## Extending the Language
<!-- 
Operational semantics makes it straightforward to add new constructs to the language: add a production to the grammar of statements and add rules describing its steps. -->

Suppose we add a new conditional construct $$\mathsf{if}\ b\ \{ S \}$$ with the rules

$$
  \dfrac
  {}
  {\langle \mathsf{if}\ b\ \{ S \},\, \sigma \rangle \rightarrow \langle S,\, \sigma \rangle}
  \quad \llbracket b \rrbracket_\mathcal{B}(\sigma) = \top
  \qquad
  \dfrac
  {}
  {\langle \mathsf{if}\ b\ \{ S \},\, \sigma \rangle \rightarrow \sigma}
  \quad \llbracket b \rrbracket_\mathcal{B}(\sigma) = \bot
$$

Adding a new set of rules changes adds new rows to the table of rules, and therefore changes the inversion principle.
In this case, determinisim isn't affected; but not all extensions are deterministic, e.g. concurrency.

A natural question is then whether the new construct adds anything.
It does not if it can be _desugared_, i.e. replaced by a statement of the original language that is semantically equivalent to it.
For example, with the above rule, $$\mathsf{if}\ b\ \{ S \}$$ is semantically equivalent to $$\mathsf{if}\ b\ \{ S \}\ \mathsf{else}\ \{ \mathsf{skip} \}$$.
If $$\llbracket b \rrbracket_\mathcal{B}(\sigma) = \top$$ then both statements have the single step to $\langle S,\, \sigma \rangle$, and so they reach the same states.
If, on the other hand, $$\llbracket b \rrbracket_\mathcal{B}(\sigma) = \bot$$ then the new statement steps in one step to $\sigma$, whereas the desugared statement steps to $\langle \mathsf{skip},\, \sigma \rangle$ and then to $\sigma$; again, they reach the same states, but in a different number of steps.