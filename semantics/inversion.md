---
layout: math
title: Reasoning with Derivations
nav_order: 4
mathjax: true
parent: Semantics
---

# Proving Programs Correct

As with the denotation of expressions, the point of study language semantics is often so that we can reason about programs in a way that goes beyond their execution.
One of the most important applications of semantics is program verification, i.e. proving that a program is correct.
Although we could consider testing our program on lots of inputs, this would not give us any guarantee that we had covered all possible cases.
With a formal definition of the semantics, we can rigorously _prove_ that our program is correct as a mathematical theorem.

## Termination

There are many things you might wish to prove about your program, the first we will look at is termination, i.e. that our program eventually comes to a stop on a given set of inputs.

To see how we will model this property in our operational semantics consider the object that we defined: a relation ${\Downarrow} \subseteq \mathsf{S} \times \mathsf{State} \times \mathsf{State}$.
This object is really just a set of triple $(S,\, \sigma_1,\, \sigma_2) \in {\Downarrow}$ that says in the initial state $\sigma_1 \in \mathsf{State}$ the program $S \in \mathcal{S}$ will execute to the final state $\sigma_2$, which we more conveniently write as $S,\, \sigma_1 \Downarrow \sigma_2$.
This is not a function, however, in the strict mathematical sense as there is not necessarily a final state for any given program and initial state.
For example, the program $\mathsf{while}\ \mathsf{true}\ \mathsf{do}\ \mathsf{skip}$ will continually do nothing and never leave us with a final state.
Non-terminating computations are precisely those that do not have a final state, and therefore we can frame the termination problem as follows:

<div class="defn" markdown="1">
  A statement $S \in \mathcal{S}$ is said to __terminate__ on some initial state $\sigma \in \mathsf{State}$ if, and only if, there exists a final state $\sigma' \in \mathsf{State}$ such that $S,\, \sigma \Downarrow \sigma'$.
</div>

Consider the following program, for example, that computes $y^x$ (when $x \geq 0$):

```
z <- 1;
while 0 <= x do
  z <- z * y;
  x <- x - 1
```

To prove this program terminates on all inputs, we must show that, for any initial state $\sigma_0 \in \mathsf{State}$, there exists some final state $\sigma_F \in \mathsf{State}$ such that $S,\, \sigma_0 \Downarrow \sigma_F$ where $S$ is the above program.
As with program execution, this statement can be shown by constructing a derivation of the judgement $S,\, \sigma_0 \Downarrow \sigma_F$ for some final state $\sigma_F$ except we must now do so for all input states $\sigma_0$.
Our proof can be thought of as a recipe for constructing this derivation in general.

Initially, we can apply the rule for composition to construct the first part of our derivation:

$$
  \dfrac
  {
      \dfrac
      {}
      {z \leftarrow 1,\, \sigma_0 \Downarrow \sigma_0[z \mapsto 1]}
      \quad
      
      \dfrac
      {\vdots}
      {\mathsf{while}\ 0 \leq x\ \mathsf{do}\ z \leftarrow z * y;\; x \leftarrow x - 1,\, \sigma_0[z \mapsto 1] \Downarrow \sigma_F}
  }
  {S,\, \sigma_0 \Downarrow \sigma_F}
$$

<!-- where $$ is the while loop: $\mathsf{while}\ 0 \leq x\ \mathsf{do}\ z \leftarrow z * y;\; x \leftarrow x - 1$. -->

However, the structure of the rest of this derivation will depend on the value of $x$ as that will dictate how many times the loop will be executed.
Let us write $\sigma_1$ for the resulting intermediate state $\sigma_0[z \mapsto 1]$.
Let us consider two cases, in each of which we must show there exists some final state:

  * When $\sigma_1(x) < 0$ the loop is not executed at all; so, we can complete our derivation as follows:

    $$
      \dfrac
      {
        \dfrac
        {}
        {z \leftarrow 1,\, \sigma_0 \Downarrow \sigma_1}
        \quad
        
        \dfrac
        {}
        {\mathsf{while}\ 0 \leq x\ \mathsf{do}\ z \leftarrow z * y;\; x \leftarrow x - 1,\, \sigma_1 \Downarrow \sigma_1}
      }
      {S,\, \sigma_0 \Downarrow \sigma_1}
    $$

    Giving us the final state $\sigma_F = \sigma_1 = \sigma_0[z \mapsto 1]$.

  * Now suppose that $\sigma_1(x) \geq 0$.
    In this case, we can't directly construct a derivation as it depends on the number of iterations of the While loop.
    However, we can still imagine a general recipe for constructing a derivation.
    Our recipe will apply the inference rule for $\mathsf{while}$ rule $\sigma_1(x)$ times and, on each iteration, we will update the state according to the body.
    
    To formalise this idea, we will need to use proof by induction.
    Remember that the induction principle applies when trying to prove a statement of the form $\forall x \in X.\, P(x)$ where $X$ is an inductively defined set.
    To apply the induction principle here, therefore, we will technically be proving the following statement:

    $$
      \forall n \in \mathbb{N}.\, \forall \sigma \in \mathsf{State}.\, \textrm{if}\ \sigma(x) = n,\, \textrm{then}\  \exists \sigma' \in \mathsf{State}.\, L,\, \sigma \Downarrow \sigma'
    $$

    where $L$ is the while loop in question.

    Notice that we will have to prove the loop terminates for any state where $\sigma(x) \geq 0$ rather than the specific state we have reached thus far $\sigma_1$.
    The reason for this is that our induction argument must cover all iterations of the loop, not just the first; so, if we restrict ourselves to proving it terminates for $\sigma_1$, we would not be able to complete our inductive case.
    However, if we can show it terminates for all such $\sigma$, then it will certainly terminate for $\sigma_1$ as required.

    Rather than carrying out induction over $n \in \mathbb{N}$ and then assuming $\sigma(x) = n$, we can simplify this process by thinking of it as induction on the value of $\sigma(x)$ directly.
    In this case, $\sigma(x)$ will correspond to the number of iterations of the loop.

    This gives us the following proof obligations:

    * Given that $\sigma(x) = 0$ show that there exists some $\sigma' \in \mathsf{State}.\, L,\, \sigma \Downarrow \sigma'$.
      In this case, the loop is not executed at all, and we can construct the derivation:

      $$
        \dfrac
        {}
        {\mathsf{while}\ 0 \leq x\ \mathsf{do}\ z \leftarrow z * y;\; x \leftarrow x - 1,\, \sigma \Downarrow \sigma'}
      $$

      Giving us the final state $\sigma' = \sigma$.

    * Now suppose $\sigma(x) = n + 1$ for some $n \geq 0$.
      Our induction hypothesis tells us that, for any $\sigma$ such that $\sigma(x) = n$, there exists some $\sigma' \in \mathsf{State}.\, L,\, \sigma \Downarrow \sigma'$.
      In other words, we know the loop will terminate if executed in the initial state has $\sigma(x) = n$, and we must proof that the loop will terminate if executed the initial state has $\sigma(x) = n + 1$.

      By applying the definition of the operational semantics, the statement $z \leftarrow z * y;\; x \leftarrow x - 1$ when executed in any state $\sigma$ will terminate in the state $\sigma[z \mapsto \sigma(z) \cdot \sigma(y),\, x \mapsto \sigma(x) - 1]$ where $\sigma(x) - 1$ is simply $n$.
      In particular, after one application of the loops body, we can arrive at a state where the value of $x$ has decreased by $1$.
      Therefore, we can apply the induction hypothesis to this intermediate state $\sigma[z \mapsto \sigma(z) \cdot \sigma(y),\, x \mapsto n]$.
      The induction hypothesis then tells us that there exists a final state $\sigma'$ as required.

      This argument corresponds to the following derivation where the right-hand premise is justified by our induction hypothesis:
      
      $$
        \dfrac
        {
          \dfrac
          {\vdots}
          {z \leftarrow z * y;\; x \leftarrow x - 1,\, \sigma \Downarrow \sigma[z \mapsto \sigma(z) \cdot \sigma(y),\, x \mapsto n]}
          \quad
          
          \dfrac
          {\vdots}
          {L,\, \sigma[z \mapsto \sigma(z) \cdot \sigma(y),\, x \mapsto n] \Downarrow \sigma'
          }
        }
        {L,\, \sigma \Downarrow \sigma'}
      $$

## Functional Correctness

We can use a sort of reasoning to prove stronger properties such as the _functional correctness_ of a program, i.e. the statement that it computes the correct function.
If we consider the main loop of the previous program:

```
while 0 <= x do
  z <- z * y;
  x <- x - 1
```

Our formal statement of correctness might be that, for any state $\sigma \in \mathsf{State}$ with $\sigma(x) \geq 0$ and $\sigma(z) = 1$, the loop $L$ executes in this initial state to produce a final state $\sigma' \in \mathsf{State}$ such that $\sigma'(z) = \sigma(z) \cdot \sigma(y)^{\sigma(x)}$.
Just like in our previous proof, proving the desired property of this while program will require is to generalise the statement so that it applies of all iterations rather than just the first.
In particular, we will show that it computes the function $z \cdot y^x$ as this allows for $z$ to be an arbitrary value to begin with.
Therefore, we will prove that:

$$
  \forall \sigma \in \mathsf{State}\ \textrm{such that}\ \sigma(x) \geq 0 .\, \exists \sigma' \in \mathsf{State}.\, \sigma'(z) = \sigma(z) \cdot \sigma(y)^{\sigma(x)}\ \textrm{and}\ L,\, \sigma \Downarrow \sigma' 
$$

We will prove this by induction on $\sigma(x)$:

  * In the base case, we have that $L,\, \sigma \Downarrow \sigma$ and the final state $\sigma'$ is simply $\sigma$.
    As $\sigma'(z) = \sigma(z)$ and $\sigma(x) = 0$, we have that $\sigma'(z) = \sigma(z) \cdot \sigma(y)^\sigma(x)$ as required. 

  * Now let us consider the inductive case where $\sigma(x)$ is $n + 1$ for some $n \geq 0$.
    Executing the loop body in the state $\sigma$ results in the state $\sigma[x \mapsto n,\, z \mapsto \sigma(z) \cdot y]$ as seen by the following derivation:

    $$
      \dfrac
      {
        \dfrac
        {}
        {z \leftarrow z * y,\, \sigma \Downarrow \sigma[z \mapsto \sigma(z) * \sigma(y)]}
        \quad

        \dfrac
        {}
        {x \leftarrow x - 1,\, \sigma[z \mapsto \sigma(z) \cdot \sigma(y) \Downarrow \sigma[x \mapsto n,\, z \mapsto \sigma(z) \cdot \sigma(y)]]}
      }
      {z \leftarrow z * y;\; x \leftarrow x - 1,\, \sigma \Downarrow \sigma[z \mapsto \sigma(z) \cdot \sigma(y),\, x \mapsto n]}
    $$

    At this point, we have decreased the value of $x$ and we can use the induction hypothesis to conclude that $L,\, \sigma[z \mapsto \sigma(z) \cdot \sigma(y),\, x \mapsto n] \Downarrow \sigma'$ for some $\sigma'$ such that $\sigma'(z) = \sigma(z) \cdot \sigma(y) \cdot \sigma(y)^n$.
    This then allows us to conclude that the loop will terminate in a state where $\sigma'(z) = \sigma(z) \cdot \sigma(y)^{n + 1}$ as required.

# Inversion Principle

So far, derivations have served as a proof or a _witness_ that programs have a particular execution.
This perspective considers the derivation itself as an artefact.
By taking a bottom-up reading of these derivations, we can also see how the process of constructing a derivation can correspond to the execution of a program and allows us to compute a final state.
However, sometimes we will also want to _deconstruct_ a derivation as we will look into now.

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
We might wish to prove that these two programs are semantically equivalent, so we can optimise away the redundant branching.
To do so, we must prove that:

  * If $S,\, \sigma \Downarrow \sigma'$ for some states $\sigma,\, \sigma' \in \mathsf{State}$, then $\mathsf{if}\ x \leq 2\ \mathsf{then}\ S\ \mathsf{else}\ S,\, \sigma \Downarrow \sigma'$;

  * And, if $\mathsf{if}\ x \leq 2\ \mathsf{then}\ S\ \mathsf{else}\ S,\, \sigma \Downarrow \sigma'$ for some states $\sigma,\, \sigma' \in \mathsf{State}$, then $S,\, \sigma \Downarrow \sigma'$.

The first of these proof obligations is relatively straightforward.
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

In either case, we have that $S,\, \sigma \Downarrow \sigma'$ and can, therefore, conclude that the statement $S$ and the statement $\mathsf{if}\ x \leq 2\ \mathsf{then}\ S\ \mathsf{else}\ S,\, \sigma \Downarrow \sigma'$ are semantically equivalent.

This style of reasoning is known as the _inversion principle_ and can be applied to any inductively defined set.
Intuitively, the inversion principle simply states that for each inhabitant of an inductively defined set there exists a derivation of that judgement.
What this means in practice is that we can extract additional information from a judgement by considering what derivations could have produced.

We can unpack this idea a bit further by considering the particular from a While statement and their associated inference rules.

<div class="defn" markdown="1">
  The __inversion principle__ states that, if $S,\, \sigma \Downarrow \sigma'$ for some statement $S \in \mathcal{S}$ and for two states $\sigma,\, \sigma' \in \mathsf{State}$, then one of the following applies:

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

Another way in which inversion gives us power beyond execution is to consider how a program may have reached a final state.
Let us suppose that we have executed a program $y \leftarrow z;\; x \leftarrow y * 2$, and it reached some final state $\sigma$ where $\sigma(x) = 2$ and we want to know _why_ this happened.
That is to say, suppose we are given that $y \leftarrow z;\; x \leftarrow y * 2,\, \sigma' \Downarrow \sigma$ for some states $\sigma$ and $\sigma'$ where $\sigma(x) = 2$ and we want to know what this tells us about $\sigma'$.
The inversion principle tells us that there must exist a derivation of this fact and that derivation must be of the form:

$$
  \dfrac
  {
    y \leftarrow z,\, \sigma' \Downarrow \sigma''
    \quad

    x \leftarrow y * 2,\, \sigma'' \Downarrow \sigma
  }
  {y \leftarrow z;\; x \leftarrow y * 2,\, \sigma' \Downarrow \sigma}
$$

However, we can apply the inversion principle again to each of these premises to see that $\sigma'' = \sigma'[y \mapsto \sigma'(z)]$ and $\sigma = \sigma''[x \mapsto \sigma''(y) \cdot 2]$.
By applying these equations we can see that $\sigma = \sigma'[y \mapsto \sigma'(z),\, x \mapsto \sigma'(z) \cdot 2]$.
In particular, if $\sigma(x) = 2$, then $\sigma'(z) \cdot 2 = 2$ and $\sigma'(z) = 1$.
Thus, we know that the program must have been executed in a state where $z$ was assigned the value $1$ in order to reach a final state where $\sigma(x) = 2$.