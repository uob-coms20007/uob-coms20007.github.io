---
layout: math
title: Functionality
nav_order: 5
mathjax: true
parent: Semantics
---

Last time we saw how some programs may or may not terminate and thus are not strictly mathematical functions.
Although some programs might not have a final state for all inputs states, no program can produce multiple final states.
Therefore, we can think of programs as _partial functions_, i.e. functions that may or may not produce an output.

Intuitively, we can see that each inference rule applies in a particular set of circumstances and, therefore, any instance of the operational semantics relation can only have at most one derivation.
To formally prove this fact, however, we are going to need proof by induction.
This time, we will do induction over the derivations themselves.

<div class="defn" markdown="1">
  The induction principle for operational derivation stats that to prove $S,\, \sigma \Downarrow \sigma' \Rightarrow P(S,\, \sigma,\, \sigma')$ for all $S \in \mathcal{S}$ and $\sigma,\, \sigma' \in \mathsf{State}$ where $P$ is some property of interest, we must prove that:

    * (Base Case) $P(\mathsf{skip},\, \sigma,\, \sigma)$.

    * (Base Case) $P(x \leftarrow e,\, \sigma,\, \sigma[x \mapsto \llbracket e \rrbracket_\mathcal{A}(\sigma)])$.

    * (Inductive Case) Assuming $P(S_1,\, \sigma_1,\, \sigma_2)$ and $P(S_2,\, \sigma_2,\, \sigma_3)$ prove that $P(S_1;\; S_2,\, \sigma_1,\, \sigma_3)$.

    * (Inductive Case) Assuming $P(S_1,\, \sigma_1,\, \sigma_2)$ and $\llbracket e \rrbracket_\mathcal{B} = \top$ prove that $P(\mathsf{if}\ e\ \mathsf{then}\ S_1\ \mathsf{else}\ S_2,\, \sigma_1,\, \sigma_2)$.

    * (Inductive Case) Assuming $P(S_2,\, \sigma_1,\, \sigma_2)$ and $\llbracket e \rrbracket_\mathcal{B} = \bot$ prove that $P(\mathsf{if}\ e\ \mathsf{then}\ S_1\ \mathsf{else}\ S_2,\, \sigma_1,\, \sigma_2)$.

    * (Inductive Case) Assuming $P(S,\, \sigma_1,\, \sigma_2)$ and  $P(\mathsf{while}\ e\ \mathsf{then}\ S,\, \sigma_2,\, \sigma_3)$ and $\llbracket e \rrbracket_\mathcal{B} = \top$ prove that $P(\mathsf{while}\ e\ \mathsf{then}\ S,\, \sigma_1,\, \sigma_3)$.

    * (Base Case) Assuming $\llbracket e \rrbracket_\mathcal{B} = \bot$ prove that $P(\mathsf{while}\ e\ \mathsf{then}\ S,\, \sigma,\, \sigma)$.
</div>

Notice that, as with the induction principle for natural numbers or expressions, there is a proof obligation for each inference rule.
Additionally, we can see that the premises of each inference rule becomes an induction hypothesis.
If we think of the tree-like structure of derivations, where each premise must be justified by a sub-derivation, then the induction hypotheses merely tell us that the property holds over these sub-trees.

We can use this induction principle to prove that While programs are functional.
In particular, we will show the following theorem:

$$
  \textrm{If}\ S,\, \sigma \Downarrow \sigma'\ \textrm{and}\ S,\, \sigma \Downarrow \sigma''\ \textrm{then}\ \sigma = \sigma''
$$

You may notice that there are multiple ways in which the induction principle could be applied - to either of the assumptions about $S$.
As its symmetrical, however, it doesn't really matter which we pick.
Let's perform induction on the first assumption, and derive the following proof obligations:

  * (Base Case)
    Given that $\sigma = \sigma'$ and $\mathsf{skip},\, \sigma \Downarrow \sigma''$ show that $\sigma' = \sigma''$.
    The first of these assumptions comes from the induction principle, which tell us that if we perform induction on $S,\,\sigma \Downrrow \sigma'$ then in the $\mathsf{skip}$ case we may assume that $\sigma = \sigma'$.
    To derive that $\sigma$ and $\sigma''$ are equal we can apply the inversion principle to our second assumption.
    Therefore, $\sigma' = \sigma''$ as required.

  * (Base Case)
    Given that $\sigma' = \sigma[x \llbracket e \rrbracket_\mathcal{A}(\sigma)]$ and $x \leftarrow e,\, \sigma \Downarrow \sigma''$ show that $\sigma' = \sigma''$.
    As with the previous case, the first assumption comes from the induction principle.
    Applying inversion to the second premise tells us that $\sigma'' = \sigma[x \llbracket e \rrbracket_\mathcal{A}(\sigma)]$.
    Therefore, $\sigma' = \sigma''$ as required.

  * (Inductive Case)
    In our inductive case ... 
    Assuming $P(S_1,\, \sigma_1,\, \sigma_2)$ and $P(S_2,\, \sigma_2,\, \sigma_3)$ prove that $P(S_1;\; S_2,\, \sigma_1,\, \sigma_3)$.

  * (Inductive Case) Assuming $P(S_1,\, \sigma_1,\, \sigma_2)$ and $\llbracket e \rrbracket_\mathcal{B} = \top$ prove that $P(\mathsf{if}\ e\ \mathsf{then}\ S_1\ \mathsf{else}\ S_2,\, \sigma_1,\, \sigma_2)$.

  * (Inductive Case) Assuming $P(S_2,\, \sigma_1,\, \sigma_2)$ and $\llbracket e \rrbracket_\mathcal{B} = \bot$ prove that $P(\mathsf{if}\ e\ \mathsf{then}\ S_1\ \mathsf{else}\ S_2,\, \sigma_1,\, \sigma_2)$.

  * (Inductive Case) Assuming $P(S,\, \sigma_1,\, \sigma_2)$ and  $P(\mathsf{while}\ e\ \mathsf{then}\ S,\, \sigma_2,\, \sigma_3)$ and $\llbracket e \rrbracket_\mathcal{B} = \top$ prove that $P(\mathsf{while}\ e\ \mathsf{then}\ S,\, \sigma_1,\, \sigma_3)$.

  * (Base Case) Assuming $\llbracket e \rrbracket_\mathcal{B} = \bot$ prove that $P(\mathsf{while}\ e\ \mathsf{then}\ S,\, \sigma,\, \sigma)$.

<!-- (In Lab: Proof of Monotonicity, Prove inversion principle using the induction principle, Small-Step Semantics) --> 