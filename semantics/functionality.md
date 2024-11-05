---
layout: math
title: Functionality
nav_order: 5
mathjax: true
parent: Semantics
---

# Proof of Functionality

Last time we saw how some programs may or may not terminate and thus are not strictly mathematical functions.
Although some programs might not have a final state for all inputs states, no program can produce multiple final states.
Therefore, we can think of programs as _partial functions_, i.e. functions that may or may not produce an output.

Intuitively, we can see that each inference rule applies in a particular set of circumstances and, therefore, any instance of the operational semantics relation can only have at most one derivation.
To formally prove this fact, however, we are going to need proof by induction.
This time, we will do induction over the derivations themselves.

<div class="defn" markdown="1">
  The induction principle for operational derivation stats that to prove $S,\, \sigma \Downarrow \sigma' \Rightarrow P(S,\, \sigma,\, \sigma')$ for all $S \in \mathcal{S}$ and $\sigma,\, \sigma' \in \mathsf{State}$ where $P$ is some property of interest, we must prove that:

  - (Skip - Base Case) $P(\mathsf{skip},\, \sigma,\, \sigma)$.

  - (Assign - Base Case) $P(x \leftarrow e,\, \sigma,\, \sigma[x \mapsto \llbracket e \rrbracket_\mathcal{A}(\sigma)])$.

  - (Composition - Inductive Case) Assuming $P(S_1,\, \sigma_1,\, \sigma_2)$ and $P(S_2,\, \sigma_2,\, \sigma_3)$ prove that $P(S_1;\; S_2,\, \sigma_1,\, \sigma_3)$.

  - (IfTrue - Inductive Case) Assuming $P(S_1,\, \sigma_1,\, \sigma_2)$ and $\llbracket e \rrbracket_\mathcal{B} = \top$ prove that $P(\mathsf{if}\ e\ \mathsf{then}\ S_1\ \mathsf{else}\ S_2,\, \sigma_1,\, \sigma_2)$.

  - (IfFalse - Inductive Case) Assuming $P(S_2,\, \sigma_1,\, \sigma_2)$ and $\llbracket e \rrbracket_\mathcal{B} = \bot$ prove that $P(\mathsf{if}\ e\ \mathsf{then}\ S_1\ \mathsf{else}\ S_2,\, \sigma_1,\, \sigma_2)$.

  - (WhileTrue - Inductive Case) Assuming $P(S,\, \sigma_1,\, \sigma_2)$ and  $P(\mathsf{while}\ e\ \mathsf{then}\ S,\, \sigma_2,\, \sigma_3)$ and $\llbracket e \rrbracket_\mathcal{B} = \top$ prove that $P(\mathsf{while}\ e\ \mathsf{then}\ S,\, \sigma_1,\, \sigma_3)$.

  - (WhalseFalse - Base Case) Assuming $\llbracket e \rrbracket_\mathcal{B} = \bot$ prove that $P(\mathsf{while}\ e\ \mathsf{then}\ S,\, \sigma,\, \sigma)$.
</div>

Notice that, as with the induction principle for natural numbers or expressions, there is a proof obligation for each inference rule.
Additionally, we can see that the premises of each inference rule becomes an induction hypothesis.
If we think of the tree-like structure of derivations, where each premise must be justified by a sub-derivation, then the induction hypotheses merely tell us that the property holds over these subtrees.

We can use this induction principle to prove that While programs are functional.
In particular, we will show the following theorem:

$$
  \textrm{If}\ S,\, \sigma \Downarrow \sigma'\ \textrm{and}\ S,\, \sigma \Downarrow \sigma''\ \textrm{then}\ \sigma' = \sigma''
$$

You may notice that there are multiple ways in which the induction principle could be applied - to either of the assumptions about $S$.
As its symmetrical, however, it doesn't really matter which we pick.
Let's perform induction on the first assumption, and derive the following proof obligations:

  * (Skip Case)
    For the case where $S$ is $\mathsf{skip}$, we may assume that $\sigma = \sigma'$ and $\mathsf{skip},\, \sigma \Downarrow \sigma''$ in order to show that $\sigma' = \sigma''$.
    The first of these assumptions comes from the induction principle, which tell us that if we perform induction on $S,\,\sigma \Downarrow \sigma'$ then in the $\mathsf{skip}$ case we may assume that $\sigma = \sigma'$.
    To derive that $\sigma$ and $\sigma''$ are equal we can apply the inversion principle to our second assumption.
    Therefore, $\sigma' = \sigma''$ as required.

  * (Assign Case)
    For the case where $S$ is $x \leftarrow e$, we may assume that $\sigma' = \sigma[x \llbracket e \rrbracket_\mathcal{A}(\sigma)]$ and $x \leftarrow e,\, \sigma \Downarrow \sigma''$ in order to show that $\sigma' = \sigma''$.
    As with the previous case, the first assumption comes from the induction principle.
    Applying inversion to the second premise tells us that $\sigma'' = \sigma[x \llbracket e \rrbracket_\mathcal{A}(\sigma)]$.
    Therefore, $\sigma' = \sigma''$ as required.

  * (Composition Case)
    In the inductive case for a statement of the form $S_1;\; S_2,\, \sigma_1 \Downarrow \sigma_3$ may seem a little odd, so let's work though it in a bit more detail.
    The inference rule this case is cover is:

    $$
      \dfrac
      { 
        S_1,\, \sigma_1 \Downarrow \sigma_2
        \quad
        
        S_2,\, \sigma_2 \Downarrow \sigma_3
      }
      {S_1;\; S_2,\, \sigma_1 \Downarrow \sigma_3}
    $$
    
    And recall our theorem states that:

    $$
      \textrm{If}\ S,\, \sigma \Downarrow \sigma'\ \textrm{then}\ \textrm{if}\ S,\, \sigma \Downarrow \sigma''\ \textrm{then}\ \sigma = \sigma''
    $$

    In this statement we have deliberately separated out the two assumptions to make it clear that our property is $P(S,\, \sigma,\, \sigma') := \textrm{if} S,\, \sigma \Downarrow \sigma''\ \textrm{then}\ \sigma = \sigma''$ for any $\sigma'' \in \mathsf{State}$.
    Therefore, the composition case of our induction principle tells us there is some intermediate state $\sigma_2 \in \mathsf{State}$ for which the following induction hypotheses apply:

      * If $S_1,\, \sigma_1 \Downarrow \sigma_2'$, then $\sigma_2 = \sigma_2'$

      * If $S_2,\, \sigma_2 \Downarrow \sigma_3'$, then $\sigma_3 = \sigma_3'$

    As with the base cases, we must now derive that $\sigma_3 = \sigma_3'$ by inversion.
    We have been given the assumption that $S_1;\; S_2,\, \sigma_1 \Downarrow \sigma_3'$.
    And by inversion, we know that there is _some_ $\sigma_2'$ such that $S_1,\, \sigma_1 \Downarrow \sigma_2'$ and $S_2,\, \sigma_2' \Downarrow \sigma_3$, i.e. the first and second premise of its derivation.
    
    Then we may combine the first induction hypothesis with the first premises to tell us that $\sigma_2 = \sigma_2'$, which in turn allows us to apply the second hypothesis to the second premises tell us that $\sigma_3 = \sigma_3'$ as required.

  * (IfTrue Case)
    There are two cases for $\mathsf{if}$ statements - one for each inference rule.
    They are largely similar, so we will only cover the first.

    We are considering the judgement $\mathsf{if}\ e\ \mathsf{then}\ S_1\ \mathsf{else}\ S_2,\, \sigma \Downarrow \sigma'$ where $\llbracket e \rrbracket_{\mathcal{B}}(\sigma) = \top$ and we have also been given that $\mathsf{if}\ e\ \mathsf{then}\ S_1\ \mathsf{else}\ S_2,\, \sigma \Downarrow \sigma''$.
    The induction principle tells us that the premise of this inference rule will satisfy our functionality property, i.e.:

    $$ 
      \textrm{If}\ S_1,\, \sigma \Downarrow \sigma''\ \textrm{then}\ \sigma' = \sigma''$.
    $$

    Now to conclude that $\sigma' = \sigma''$ we clearly are going to have to use our induction hypothesis.
    And as with the previous cases, this requires us to invert the assumption that we have been given.
    In particular, as $\llbracket e \rrbracket_{\mathcal{B}}(\sigma) = \top$ the judgement $\mathsf{if}\ e\ \mathsf{then}\ S_1\ \mathsf{else}\ S_2,\, \sigma \Downarrow \sigma''$ can only be due to $S_1,\, \sigma \Downarrow \sigma''$.
    Therefore, we can apply the induction hypothesis to see that $\sigma' = \sigma''$ as required.

  * (IfFalse Case)
    By analogy to the previous case.

  * (WhileTrue Case)
    As with the inference rules for $\mathsf{if}$ we must consider each case for the $\mathsf{while}$ statement separately.
    In the first cases, we are considering where $\mathsf{while}\ e\ \mathsf{do}\ S,\, \sigma_1 \Downarrow \sigma_3$ has been derived from $\llbracket e \rrbracket_\mathcal{B}(\sigma) = \top$ and $S,\, \sigma_1 \Downarrow \sigma_2$ and $\mathsf{while}\ e\ \mathsf{do}\ S,\, \sigma_2 \Downarrow \sigma_3$.
    And we must derive that if, additionally, $\mathsf{while}\ e\ \mathsf{do}\ S,\, \sigma_1 \Downarrow \sigma_3'$, then $\sigma_3 = \sigma_3'$.
    Here we have two induction hypotheses, one for each premise:

      * That, if $S,\, \sigma_1 \Downarrow \sigma_2$ and $S,\, \sigma_1 \Downarrow \sigma_2'$, then $\sigma_2 = \sigma_2'$

      * And that, if $\mathsf{while}\ e\ \mathsf{do}\ S,\, \sigma_2 \Downarrow \sigma_3'$, then $\sigma_3 = \sigma_3'$.

    Again we will use the inversion principle to our other hypothesis $\mathsf{while}\ e\ \mathsf{do}\ S,\, \sigma_1 \Downarrow \sigma_3'$, then $\sigma_3 = \sigma_3'$ to see that either there exists some $\sigma_2'$ such that $S,\, \sigma_1 \Downarrow \sigma_2'$ and $\mathsf{while}\ e\ \mathsf{do}\ S,\, \sigma_2 \Downarrow \sigma_3'$.
    We know by assumption that the branch condition is true in the initial state $\sigma_1$ so the loop is executed at least once.

    The rest of the proof looks somewhat like the composition case.
    By the first induction hypothesis, we can see that $\sigma_2 = \sigma_2'$.
    This fact then allows us to apply the second induction hypothesis to conclude that $\sigma_3 = \sigma_3'$ as required.

 * (WhileFalse Case)
      Finally, when $\mathsf{while}\ e\ \mathsf{do}\ S,\, \sigma \Downarrow \sigma'$ has been derived from $\llbracket e \rrbracket_\mathcal{B}(\sigma) = \bot$ and $\sigma = \sigma'$.
      The case is analogous to the skip case.

The up-shot of this theorem is that we can consider While programs as partial functions from initial states to final states.
As you will see in the lab, we can refine this further to consider them as partial functions from integers to integers.
This fact will be used in the next section of the course to reason about what functions programs can express more abstractly.

## Denotational Semantics for While

#### Notice: The content is _not_ assessed, so feel free to skip if you are revising.

Using the functionality of our operational semantics, gives us a hint as to how we would give a denotational semantics for While programs.
Our denotational semantics must take into account partiality as we know not all programs terminate.
Therefore, our semantic domain will be the space of partial functions.

<div class="defn" markdown="1">
  Let us write $X \rightharpoonup Y$ for the set of partial functions from $X$ to $Y$.
</div>

We could define a denotation function $\llbracket S \rrbracket_\mathcal{S}(\sigma)$ for some statement $S \in \mathcal{S}$ and state $\sigma \in \mathsf{State}$ to be equal to the unique state $\sigma'$ (if one exists) such that $S,\, \sigma \Downarrow \sigma'$.
This would indeed give us a denotation function mapping statements to a partial function on states.
However, it is quite different to the definition we gave to the denotation function for expressions.

If you recall, the denotational semantics for expression is given by recursion over the structure of the expressions.
In this way it is _compositional_ as it maps language constructs to semantic operations independently of one another.
The operationally derived denotational semantics instead interprets programs as monolithic entities.

Instead, we would like a denotational semantic for While programs that is defined by recursion.
The first few cases are straightforward to define:

$$
  \begin{array}{rl}
    \llbracket \mathsf{skip} \rrbracket_\mathcal{S}(\sigma) &= \sigma \\ 
    \llbracket x \leftarrow e \rrbracket_\mathcal{S}(\sigma) &= \sigma[x \mapsto \llbracket e \rrbracket_\mathcal{A}(\sigma)] \\
    \llbracket S_1;\; S_2  \rrbracket_\mathcal{S}(\sigma) &= 
          \llbracket S_2 \rrbracket_\mathcal{S}(\llbracket S_1 \rrbracket_\mathcal{S}(\sigma)) \\
    \llbracket \mathsf{if}\ e\ \mathsf{else}\ S_1\ \mathsf{else}\ S_2 \rrbracket_\mathcal{S}(\sigma) &= 
          \begin{cases}
            \llbracket S_1 \rrbracket_\mathcal{S}(\sigma) & \textrm{if}\ \llbracket e \rrbracket_\mathcal{B}(\sigma) \\
            \llbracket S_2 \rrbracket_\mathcal{S}(\sigma) & \textrm{otherwise}
          \end{cases}

  \end{array}
$$

Each of these equations work as if we were looking at expressions, we are mapping language constructs to operations on the domain.
For example, the composition statement is replaced by the composition of our partial functions and conditional statements are replaced by a mathematical conditional.

The difficulty comes from the While rule.
If we recall the inference rules for the While construct then we see that they're not recursive in the sense that they don't derive meaning from the meaning of sub-statements:

$$
  \begin{array}{cc}
    \dfrac
    {}
    {\mathsf{while}\ e \mathsf{do}\ S,\, \sigma \Downarrow \sigma}
    \llbracket e \rrbracket_\mathcal{B} = \bot

    &
    \dfrac
    { S,\, \sigma_1 \Downarrow \sigma_2
      \quad

      \mathsf{while}\ e \mathsf{do}\ S,\, \sigma_2 \Downarrow \sigma_3
    }
    {\mathsf{while}\ e \mathsf{do}\ S,\, \sigma_1 \Downarrow \sigma_3}
    \llbracket e \rrbracket_\mathcal{B} = \top
  \end{array}
$$

To give a suitable denotational semantics, we need a way of transforming the denotation of the loops body into the denotation of loop itself.
Intuitively, the semantics should capture the iterative nature of the loop and, therefore, be equivalent in some way to the infinite statement:

$$
  \mathsf{if}\ e\ \mathsf{then}\ (S;\;\mathsf{if}\ e\ \mathsf{then}\ (S;\;\dots)\ \mathsf{else}\ \mathsf{skip})\ \mathsf{else}\ \mathsf{skip}
$$

At the heart of this infinite unfolding of the loop, is the infinite application of a function.
The function is condition on the branch condition but, when true, the function should be applied again to the output.
We can't just say it is recursive and call it a day because maths doesn't allow for arbitrarily recursive functions in the same way that we can write arbitrarily recursive code in a programming language.
If it did, then we could prove some very suspect results...

Instead, we need to introduce a _fixed-point_ operator.
The fixed-point operator is given the following definition:

<div class="defn" markdown="1">
  Given a partial function $f : (X \rightharpoonup X) \rightarrow (X \rightharpoonup X)$, we can define a sequence of partial functions $\mathsf{fix}_n(f) : X \rightharpoonup X$ for $n \geq 0$ where:

  $$
    \begin{array}{rl}
      \mathsf{fix}_0(f)(x) &= \bot \textrm{(read: "undefined")}\\
      \mathsf{fix}_{n+1}(f)(x) &= f(\mathsf{fix}_n(f))(x)
    \end{array}
  $$

  Intuitively, this sequence of functions gives us the $n^\mathrm{th}$ approximation of the fixed-point, i.e. the $n^\mathrm{th}$ unfolding of a loop.
  To derive the complete infinite unfolding, we need to take the combination of all of these.
  We thus define $\mathsf{fix}(f)(x) = \bigsqcup_{n \geq 0} \mathsf{fix}_n(f)(x)$, i.e. the least iteration to become defined or undefined if none of them are defined.
</div>

The reason this function is called a fixed-point is that if we take $f(\mathsf{fix}(f))$, i.e. if we applied the result to $f$ an additional time, then this would make no difference to the answer as, in some sense, we have already applied the function infinitely many times.
Therefore, $f(\mathsf{fix}(f)) = \mathsf{fix}(f)$.

We can use the idea of a fixed-point to complete the definition of the denotational semantics with the following equations:

$$
  \begin{array}{l}
  \llbracket \mathsf{while}\ e \mathsf{do}\ S \rrbracket_\mathcal{S}(\sigma) = \mathsf{fix}(f)(\sigma) \\
  \quad \textrm{where} f(g)(\sigma) = 
      \begin{cases}
        g (\llbracket S \rrbracket_\mathcal{S}(\sigma)) & \textrm{if} \llbracket e \rrbracket_\mathcal{B} \\
        \sigma  &\textrm{otherwise}
    \end{cases}
  \end{array}
$$

<!-- # Small-Step Semantics

## Notice: The definition of small-step semantics is _not_ assessed, this section exists only to give more induction examples of structural induction.

So far, we have been looking at _big-step_ operational semantics.
It is referred to as big-step semantics as the judgement itself only considers the initial and final state, with the details of this computation hidden within the structure of derivation.
With _small-step_ operational semantics, we expose each individual step in the computation one at a time.

<div class="defn" markdown="1">
  The small-step operational semantics relation concerns _configurations_, which are either a pair $\langle S,\, \sigma \rangle$ where $S \in \mathcal{S}$ is a While statement and $\sigma \in \mathsf{State}$ is a state or a final state $\sigma \in \mathsf{State}$.
  We write $\mathsf{Config}$ to refer to the set of configurations.

  We then define the relation ${\Rightarrow} \subseteq \mathsf{Config} \times \mathsf{Config}$ inductively by the following inference rules:

  $$
    \begin{array}{cc}
      \dfrac
      {}
      {\langle \mathsf{skip},\, \sigma \rangle \Rightarrow \sigma}

      &
      \dfrac
      {}
      {\langle x \leftarrow e,\, \sigma \rangle \Rightarrow \sigma[x \leftarrow \llbracket e \rrbracket_\mathcal{A}(\sigma)]}
      \\[10pt]

      \dfrac
      {\langle S_1,\, \sigma \rangle \Rightarrow \langle S_1',\, \sigma' \rangle}
      {\langle S_1;\; S_2,\, \sigma \rangle \Rightarrow \langle S_1;\; S_2,\, \sigma' \rangle}
      
      &
      \dfrac
      {\langle S_1,\, \sigma \rangle \Rightarrow \sigma'}
      {\langle S_1;\; S_2,\, \sigma \rangle \Rightarrow \langle S_2,\, \sigma' \rangle}
      \\[10pt]

      \dfrac
      {}
      {\langle \mathsf{if}\ e\ \mathsf{then}\ S_1\ \mathsf{else}\ S_2,\, \sigma \rangle \Rightarrow \langle S_1,\, \sigma \rangle}
      \llbracket e \rrbracket_\mathcal{B}(\sigma) = \top

      &
      \dfrac
      {}
      {\langle \mathsf{if}\ e\ \mathsf{then}\ S_1\ \mathsf{else}\ S_2,\, \sigma \rangle \Rightarrow \langle S_2,\, \sigma \rangle}
      \llbracket e \rrbracket_\mathcal{B}(\sigma) = \bot
      \\[10pt]

      \multicolumn{2}{c}{
        \dfrac
        {}
        {\langle \mathsf{while}\ e\ \mathsf{do}\ S,\, \sigma \rangle \Rightarrow \langle \mathsf{if}\ e\ \mathsf{then}\ S;\; \mathsf{while}\ e\ \mathsf{do}\ S\ \mathsf{else}\ \mathsf{skip},\, \sigma \rangle}
      }
    \end{array}
  $$

</div>

Small-step operational semantics in some sense provide more information than big-step operational semantics as we consider a sequence of execution steps rather than just the end result.
This distinction can be useful in situations where some notion of "time" is important, e.g. non-termination, the number of execution steps, or concurrency where two programs may be interleaved.

However, we would also hope that these two definitions are in some sense equivalent.
We will prove one side of this equivalence by induction:

<div class="defn" markdown="1">
  We write $\langle S_0,\, \sigma_0 \rangle \Rightarrow^* \sigma_F$ if there is a sequence of small-steps $\langle S_0,\, \sigma_0 \rangle \Rightarrow \langle S_1,\, \sigma_1 \rangle \Rightarrow \cdots \Rightarrow \sigma_F$.
</div>

We will prove that if $S,\, \sigma \Downarrow \sigma'$, then $\langle S,\, \sigma \rangle \Rightarrow^* \sigma'$ by induction on the big-step derivation:

  * (Skip Case)
    In this case, we have that $\mathsf{skip},\, \sigma \Downarrow \sigma'$ occurs precisely because $\sigma = \sigma'$.
    By definition, we have that $\langle \mathsf{skip},\, \sigma \rangle \Rightarrow \sigma$ and therefore $\langle \mathsf{skip},\, \sigma \rangle \Rightarrow^* \sigma'$ as required.

  * (Assign Case)
    This case is analogous to the first. -->