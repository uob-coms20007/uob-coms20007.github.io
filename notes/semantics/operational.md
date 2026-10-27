---
layout: math
title: Operational Semantics
nav_order: 3
mathjax: true
parent: Semantics
---

# Operational Semantics

Operational semantics is an alternative style of semantics that emphasises the steps taken during the execution of a program.
We will construct an operational semantics for While programs (our toy imperative language) by building on the denotational semantics for arithmetic and Boolean expressions.

The statements of the While language are defined as follows.

<div class="defn" markdown="1">
A __statement__ is an element of the following grammar:

$$
  S \Coloneqq \mathsf{skip} \mid x \leftarrow A \mid S; S \mid \mathsf{if}\ B\ \{ S \}\ \mathsf{else}\ \{ S \} \mid \mathsf{while}\ B\ \{ S \}
$$

where $A$ stands for any arithmetic expression and $B$ stands for any Boolean expression.
</div>

As with the denotational semantics for expressions, we will work with the _abstract syntax tree_ of a statement rather than the string that produced it, using $\mathcal{S}$ to refer to the set of statements.

The type of operational semantics that we will employ is sometimes called _small-step_ operational semantics as it considers each individual step of the program's execution (as opposed to _big-step_ semantics in which the internal structure of a computation is hidden away).
The small-step semantics for While is defined over _configurations_.

<div class="defn" markdown="1">
  A __configuration__ is either a pair $\langle S,\, \sigma \rangle$ where $S \in \mathcal{S}$ is a statement and $\sigma \in \mathsf{State}$ is a state, or it is just a state $\sigma \in \mathsf{State}$.
  We write $\mathcal{C}$ for the set of configurations and use $\gamma$ to represent an arbitrary configuration.
</div>

There are two types of configurations: _non-terminal_ configurations that consist of a statement being executed and a state under which it is executed, written $\langle S,\, \sigma \rangle$, and _terminal_ configurations that are just a state, written $\sigma$.
As we shall see, the first of these may represent the entire program whose behaviour we are considering as well as sub-programs considered during execution, whereas the terminal configurations represent the states after the execution of a statement has been completed.

The small-step semantics defines what is known as a _transition system_ over configurations; that is, a binary relation between configurations that dictates how one configuration evolves into another.

<div class="defn" markdown="1">
  The _one-step transition_ relation ${\rightarrow} \subseteq \mathcal{C} \times \mathcal{C}$ is the binary relation between configurations defined by the rules that we will see in a moment.
  More precisely, it is the _least_ relation that is closed under these rules; we explain what this means, and why it matters, once we have seen all the rules.
</div>

Remember that a relation is just a set of pairs; in this case, $(\gamma_1,\, \gamma_2) \in {\rightarrow}$ means that the configuration $\gamma_1$ evolves to $\gamma_2$ after one execution step.
Rather than writing $(\gamma_1,\, \gamma_2) \in {\rightarrow}$ however, we will typically write $\gamma_1 \rightarrow \gamma_2$.
It will always be the case that $\gamma_1$ is a non-terminal configuration (terminal configurations don't make any execution steps) but $\gamma_2$ will either be another non-terminal configuration or a terminal configuration.

## Skip and Assignment

  - The skip statement $\mathsf{skip}$ does not affect the program state - it is the "do nothing" operation.
    Therefore, if the program $\mathsf{skip}$ starts under the state $\sigma$, it will also terminate with the state $\sigma$.
    We characterise this behaviour by the rule **(Skip)**:

    $$
      \dfrac
      {}
      {\langle \mathsf{skip},\, \sigma \rangle \rightarrow \sigma}
    $$

    The fraction-esque notation denotes an _inference rule_.
    Above the line is a series of _premises_ which we must show in order to use the rule, and below the line is the _conclusion_ - you can read it as "if everything above the line holds, then everything below the line holds."
    Such inference rules are a common way of _inductively_ defining a relation.
    Formally, the relation is defined as the least relation satisfying these inference rules; we return to what this means after we have seen all of the rules.

    Within these rules there are _metavariables_ such as $\sigma$ that act as parameters of the rule and can be instantiated as required; in other words, the rule is universally quantified by such variables.
    Note they are referred to as metavariables rather than simply variables to distinguish them from the program's variables.

    This rule says, therefore, that a non-terminal configuration of the form $\langle \mathsf{skip},\, \sigma \rangle$ will step to the terminal configuration $\sigma$.
    For instance, the rule allows us to conclude that $\langle \mathsf{skip},\, [x \mapsto 2] \rangle \rightarrow [x \mapsto 2]$ where the metavariable $\sigma$ has been instantiated with the concrete state $[x \mapsto 2]$.
    As there are no premises, we don't need to show anything first in order to derive this conclusion.

  - Intuitively, when the assignment statement $x \leftarrow e$ is executed with a given state $\sigma$, the value of the arithmetic expression $e$ is calculated in this state using its denotation function, and the state is updated so that $x$ is mapped to this value.
    Correspondingly, the rule **(Assign)** describing the behaviour of the assignment statement is as follows:

    $$
      \dfrac
      {}
      {\langle x \leftarrow e,\, \sigma \rangle \rightarrow \sigma[x \mapsto \llbracket e \rrbracket_\mathcal{A}(\sigma)]}
    $$

    Recall that the notation $\sigma[x \mapsto n]$ refers to the state that results from updating the value assigned to $x$ to be $n$.
    Note that $e$ is evaluated under the previous state $\sigma$, not the newly derived state.
    The evaluation of the arithmetic expression doesn't constitute an execution step in its own right - our operational semantics only cares about the evolution of statements.

    For example, the rule tells us that $\langle x \leftarrow (x + 1),\, [x \mapsto 2] \rangle \rightarrow [x \mapsto 3]$ where we have instantiated the rule with the variable $x$, the arithmetic expression $x + 1$, and the state $[x \mapsto 2]$.
    The state $[x \mapsto 3]$ is determined as $[x \mapsto 2]$ updated such that $x \mapsto \llbracket x + 1 \rrbracket_\mathcal{A}([x \mapsto 2])$; hence, $[x \mapsto 3]$.
    As with the $\mathsf{skip}$ statement, this rule doesn't require any premises as its behaviour can be described without making reference to other statements: it is not a compound statement (i.e. it is a base case of the grammar).

## Sequence

The next set of rules we will look at are those governing the operational semantics of the sequence construct $S_1;\; S_2$.
Intuitively, such programs proceed by first executing $S_1$ and then subsequently executing $S_2$.

As we are considering small-step semantics, however, it may be the case that $S_1$ executes over several steps (or indeed never terminates).
So there are two rules for sequence - **(Seq-Step)** for when $S_1$ steps to another non-terminal configuration and **(Seq-Done)** for when it steps to a terminal configuration:

$$
  \dfrac
  {\langle S_1,\, \sigma \rangle \rightarrow \langle S_1',\, \sigma' \rangle}
  {\langle S_1;\; S_2,\, \sigma \rangle \rightarrow \langle S_1';\; S_2,\, \sigma' \rangle}
$$

We can use this rule if the first statement $S_1$ makes a step to a non-terminal configuration.
To be precise, the rule says that if the relation $\rightarrow$ includes the pair $(\langle S_1,\, \sigma \rangle,\, \langle S_1',\, \sigma' \rangle)$ then it will also include the pair $(\langle S_1;\; S_2,\, \sigma \rangle,\, \langle S_1';\; S_2,\, \sigma' \rangle)$.

$$
  \dfrac
  {\langle S_1,\, \sigma \rangle \rightarrow \sigma'}
  {\langle S_1;\; S_2,\, \sigma \rangle \rightarrow \langle S_2,\, \sigma' \rangle}
$$

The second rule applies when the first statement $S_1$ steps directly to a terminal configuration.
Its execution has been completed, and the program moves onto $S_2$ with the updated state $\sigma'$.

As with the previous rules, these rules apply for all statements $S_1,\, S_2 \in \mathcal{S}$ and all states $\sigma,\, \sigma' \in \mathsf{State}$ - these are the rules' metavariables.
In order to use a rule, however, we need not only to instantiate metavariables but to find another instance of $\rightarrow$ that determines the behaviour of the statement $S_1$.
Once we have done so, we can derive a new step for the compound statement $S_1;\; S_2$.
For example, as we know that $\langle x \leftarrow 2,\, [x \mapsto 1] \rangle \rightarrow [x \mapsto 2]$, we can conclude by (Seq-Done) that:

$$
  \langle x \leftarrow 2;\; x \leftarrow 3,\, [x \mapsto 1] \rangle \rightarrow \langle x \leftarrow 3,\, [x \mapsto 2] \rangle
$$

Notice that the premise of each of these rules is itself a step of the relation that we are defining: steps of a compound statement are built from steps of its parts.
We will see an example in which both sequence rules are used, one on top of the other, once we have seen all of the rules.

## Traces and Many-steps

Before we look at the final rules concerning $\mathsf{if}$ and $\mathsf{while}$, it is worth thinking about how these mathematically defined rules actually give us an "operational" semantics.
We have seen how a statement and a particular state make progress by taking a single computational step, e.g. by updating a variable, skipping over a command, or transitioning to a sub-statement.
But programs don't just take one step - they take many, and so, to capture the notion of a computation unfolding over time, we consider _traces_.

<div class="defn" markdown="1">
A __trace__ is a finite or infinite sequence of configurations $\gamma_1,\, \gamma_2,\, \ldots \in \mathcal{C}$ such that $\gamma_i \rightarrow \gamma_{i+1}$ for every $i \geq 1$ for which $\gamma_{i+1}$ exists.
We typically write a trace as $\gamma_1 \rightarrow \gamma_2 \rightarrow \cdots$.
A finite trace $\gamma_1 \rightarrow \cdots \rightarrow \gamma_{n+1}$ is said to have $n$ steps.
</div>

The behaviour of the statement $x \leftarrow 2;\; x \leftarrow 3$ for a particular state $[x \mapsto 1]$ involves several execution steps, which can be summarised by a trace:

$$
  \begin{array}{l}
    \langle x \leftarrow 2;\; x \leftarrow 3,\, [x \mapsto 1] \rangle \\
    \quad \rightarrow \langle x \leftarrow 3,\, [x \mapsto 2] \rangle \\
    \quad \rightarrow [x \mapsto 3]
  \end{array}
$$

Note that, although we used the fact that $\langle x \leftarrow 2,\, [x \mapsto 1] \rangle \rightarrow [x \mapsto 2]$ in order to calculate the first step in this trace, it isn't part of the trace itself.
It is part of the _justification_ for the step, which we will discuss in more detail below.

We will often want to talk about the number of steps in a trace.

<div class="defn" markdown="1">
We write $\gamma \rightarrow^n \gamma'$ if there is a trace with exactly $n$ steps from $\gamma$ to $\gamma'$.
Precisely, $\gamma \rightarrow^0 \gamma'$ if and only if $\gamma = \gamma'$, and $\gamma \rightarrow^{n+1} \gamma'$ if and only if $\gamma \rightarrow \gamma''$ and $\gamma'' \rightarrow^n \gamma'$ for some configuration $\gamma''$.
</div>

Often we only care that _some_ number of steps will get us from one configuration to another.

<div class="defn" markdown="1">
The __many-step transition__ relation ${\rightarrow^*} \subseteq \mathcal{C} \times \mathcal{C}$ is again a binary relation between configurations.
This relation is defined as the _reflexive-transitive closure_ of the one-step transition relation - that is, it is the smallest relation that:

  - includes the one-step transition relation, i.e. if $\gamma_1 \rightarrow \gamma_2$, then $\gamma_1 \rightarrow^* \gamma_2$ for any configurations $\gamma_1$ and $\gamma_2$;

  - is closed under _reflexivity_ so that $\gamma \rightarrow^* \gamma$ for any configuration $\gamma$; and

  - is closed under _transitivity_ so that if $\gamma_1 \rightarrow^* \gamma_2$ and $\gamma_2 \rightarrow^* \gamma_3$, then $\gamma_1 \rightarrow^* \gamma_3$ for any configurations $\gamma_1$, $\gamma_2$, $\gamma_3$.
</div>

The many-step transition relation can be understood as summarising a trace.
If there exists a trace $\gamma_1 \rightarrow \gamma_2 \rightarrow \cdots \rightarrow \gamma_n$, then we have that $\gamma_1 \rightarrow^* \gamma_n$ and vice versa.
Equivalently, $\gamma \rightarrow^* \gamma'$ if and only if $\gamma \rightarrow^n \gamma'$ for some $n \geq 0$ (the trace consisting of the single configuration $\gamma$, with zero steps, accounts for reflexivity).
So, for instance, we may write $\langle x \leftarrow 2;\; x \leftarrow 3,\, [x \mapsto 1] \rangle \rightarrow^* [x \mapsto 3]$.

<!-- A fact that we will use constantly is that a step can be put at the front of a many-step transition: if $\gamma \rightarrow \gamma'$ and $\gamma' \rightarrow^* \gamma''$, then $\gamma \rightarrow^* \gamma''$.
This holds because a one-step transition is a many-step transition and many-step transitions compose by transitivity.
So to show that $\langle S,\, \sigma \rangle \rightarrow^* \sigma'$ it suffices to give the first step and then show the rest. -->

## Conditional Branching

The behaviour of the if construct is naturally conditional on whether the branch condition (i.e. the Boolean expression) evaluates to true or false under the current state.
Therefore, there are two rules for such statements, **(If-True)** and **(If-False)**:

$$
  \dfrac
  {}
  {\langle \mathsf{if}\ b\ \{ S_1 \}\ \mathsf{else}\ \{ S_2 \},\, \sigma \rangle \rightarrow \langle S_1,\, \sigma \rangle}
  \quad \llbracket b \rrbracket_\mathcal{B}(\sigma) = \top
$$

$$
  \dfrac
  {}
  {\langle \mathsf{if}\ b\ \{ S_1 \}\ \mathsf{else}\ \{ S_2 \},\, \sigma \rangle \rightarrow \langle S_2,\, \sigma \rangle}
  \quad \llbracket b \rrbracket_\mathcal{B}(\sigma) = \bot
$$

When the branch condition $b$ evaluates to true, we transition in a single step to the first branch $S_1$.
As with the assignment rule, the evaluation of the branch condition isn't considered an operational step.
Conversely, when the branch condition $b$ evaluates to false, we transition in a single step to the second branch $S_2$.

We don't write the condition $$\llbracket b \rrbracket_\mathcal{B}(\sigma) = \top$$ or $$\llbracket b \rrbracket_\mathcal{B}(\sigma) = \bot$$ as a premise directly as it isn't another operational step.
These are referred to as _side-conditions_, but they effectively act as premises.

For example, consider $$\mathsf{if}\ x \leq 0\ \{ x \leftarrow 0 - x \}\ \mathsf{else}\ \{ \mathsf{skip} \}$$.
From the state $[x \mapsto -3]$ the condition is true, so we take the first branch:

$$
  \begin{array}{l}
    \langle \mathsf{if}\ x \leq 0\ \{ x \leftarrow 0 - x \}\ \mathsf{else}\ \{ \mathsf{skip} \},\, [x \mapsto -3] \rangle \\
    \quad \rightarrow \langle x \leftarrow 0 - x,\, [x \mapsto -3] \rangle \\
    \quad \rightarrow [x \mapsto 3]
  \end{array}
$$

From the state $[x \mapsto 2]$ the condition is false, so we take the second branch:

$$
  \begin{array}{l}
    \langle \mathsf{if}\ x \leq 0\ \{ x \leftarrow 0 - x \}\ \mathsf{else}\ \{ \mathsf{skip} \},\, [x \mapsto 2] \rangle \\
    \quad \rightarrow \langle \mathsf{skip},\, [x \mapsto 2] \rangle \\
    \quad \rightarrow [x \mapsto 2]
  \end{array}
$$

Notice that the evaluation of the condition does not appear in the trace, only its effect: the other branch is discarded and the chosen branch is executed from the same state.

## While Loops

Similarly, the behaviour of the while construct depends on whether the branch condition is met or not, and thus there are two rules, **(While-False)** and **(While-True)**:

$$
  \dfrac
  {}
  {\langle \mathsf{while}\ b\ \{ S \},\, \sigma \rangle \rightarrow \sigma}
  \quad \llbracket b \rrbracket_\mathcal{B}(\sigma) = \bot
$$

$$
  \dfrac
  {}
  {\langle \mathsf{while}\ b\ \{ S \},\, \sigma \rangle \rightarrow \langle S;\; \mathsf{while}\ b\ \{ S \},\, \sigma \rangle}
  \quad \llbracket b \rrbracket_\mathcal{B}(\sigma) = \top
$$

Under the first rule, when the branch condition is not met, we transition to a terminal configuration with the same state.
Compare this rule to that of skip - if the branch condition is not met, the while construct does nothing.
If, on the other hand, the branch condition is met, the while loop is "unfolded".
After unfolding the loop, the statement of the new configuration is a sequence of the body of the loop and the loop itself; in this way, any subsequent steps will execute the body of the loop, and then revisit the loop itself and perhaps unfold it further.

Let's consider an example program $$\mathsf{while}\ (x \leq 1)\ \{ x \leftarrow x + 1 \}$$ and an initial state $[x \mapsto 0]$ to get a sense of how this works in practice.
Initially, the branch condition is met and so the program will execute by unfolding the loop:

$$
  \begin{array}{l}
    \langle \mathsf{while}\ (x \leq 1)\ \{ x \leftarrow x + 1 \},\, [x \mapsto 0] \rangle \\
    \quad \rightarrow \langle x \leftarrow x + 1;\; \mathsf{while}\ (x \leq 1)\ \{ x \leftarrow x + 1 \},\, [x \mapsto 0] \rangle
  \end{array}
$$

Now we have reached a configuration where the statement is a sequence.
As the first statement $x \leftarrow x + 1$ will reach a terminal configuration in one step, in particular $\langle x \leftarrow x + 1,\, [x \mapsto 0] \rangle \rightarrow [x \mapsto 1]$, our next step will return to the loop with the updated state:

$$
  \begin{array}{l}
    \langle x \leftarrow x + 1;\; \mathsf{while}\ (x \leq 1)\ \{ x \leftarrow x + 1 \},\, [x \mapsto 0] \rangle \\
    \quad \rightarrow \langle \mathsf{while}\ (x \leq 1)\ \{ x \leftarrow x + 1 \},\, [x \mapsto 1] \rangle
  \end{array}
$$

As the branch condition is still satisfied, this process will repeat an additional time:

$$
  \begin{array}{l}
    \langle \mathsf{while}\ (x \leq 1)\ \{ x \leftarrow x + 1 \},\, [x \mapsto 1] \rangle \\
    \quad \rightarrow \langle x \leftarrow x + 1;\; \mathsf{while}\ (x \leq 1)\ \{ x \leftarrow x + 1 \},\, [x \mapsto 1] \rangle \\
    \quad \rightarrow \langle \mathsf{while}\ (x \leq 1)\ \{ x \leftarrow x + 1 \},\, [x \mapsto 2] \rangle
  \end{array}
$$

However, in this final configuration, the branch condition is not satisfied by the state and so the loop does not unfold a third time:

$$
  \langle \mathsf{while}\ (x \leq 1)\ \{ x \leftarrow x + 1 \},\, [x \mapsto 2] \rangle
   \rightarrow [x \mapsto 2]
$$

The combined trace for this initial configuration is thus:

$$
  \begin{array}{l}
    \langle \mathsf{while}\ (x \leq 1)\ \{ x \leftarrow x + 1 \},\, [x \mapsto 0] \rangle  \\
    \quad \rightarrow \langle x \leftarrow x + 1;\; \mathsf{while}\ (x \leq 1)\ \{ x \leftarrow x + 1 \},\, [x \mapsto 0] \rangle \\
    \quad \rightarrow \langle \mathsf{while}\ (x \leq 1)\ \{ x \leftarrow x + 1 \},\, [x \mapsto 1] \rangle \\
    \quad \rightarrow \langle x \leftarrow x + 1;\; \mathsf{while}\ (x \leq 1)\ \{ x \leftarrow x + 1 \},\, [x \mapsto 1] \rangle \\
    \quad \rightarrow \langle \mathsf{while}\ (x \leq 1)\ \{ x \leftarrow x + 1 \},\, [x \mapsto 2] \rangle \\
    \quad \rightarrow [x \mapsto 2]
  \end{array}
$$

As we have reached a terminal configuration, we can see that the statement and this initial state will terminate with the state $[x \mapsto 2]$.

We now have all of the rules for the transition relation $\rightarrow$.
Notice that each statement form has its own rules, so which rules could possibly apply to a configuration is determined by the form of its statement.
We can summarise these as the following table:

| Configuration $\gamma$ | Rules that can derive a step from $\gamma$ | Which rule applies? |
|---|---|---|
| $\sigma$ | none | -- |
| $\langle \mathsf{skip},\, \sigma \rangle$ | (Skip) | always |
| $\langle x \leftarrow e,\, \sigma \rangle$ | (Assign) | always |
| $\langle S_1;\; S_2,\, \sigma \rangle$ | (Seq-Done), (Seq-Step) | depends on whether $\langle S_1,\, \sigma \rangle$ steps to a state or to a configuration containing a statement |
| $$\langle \mathsf{if}\ b\ \{ S_1 \}\ \mathsf{else}\ \{ S_2 \},\, \sigma \rangle$$ | (If-True), (If-False) | depends on whether $b$ is true or false in $\sigma$ |
| $$\langle \mathsf{while}\ b\ \{ S \},\, \sigma \rangle$$ | (While-True), (While-False) | depends on whether $b$ is true or false in $\sigma$ |

## Termination

There is nothing that guarantees a configuration will lead to a terminal configurations.

<div class="defn" markdown="1">
Let $S \in \mathcal{S}$ be a statement and $\sigma \in \mathsf{State}$ be a state.

  - $S$ __terminates__ from $\sigma$ with final state $\sigma'$ if $\langle S,\, \sigma \rangle \rightarrow^* \sigma'$.
    We say that $S$ terminates from $\sigma$ if there is some such $\sigma'$.

  - $S$ __diverges__ (or does not terminate) from $\sigma$ if it does not terminate from $\sigma$; that is, there is no state $\sigma'$ with $\langle S,\, \sigma \rangle \rightarrow^* \sigma'$.
</div>

The simplest diverging statement is a loop whose condition is always true.
For any state $\sigma$, the statement $$\mathsf{while}\ (x \leq x)\ \{ \mathsf{skip} \}$$ has the infinite trace:

$$
  \begin{array}{l}
    \langle \mathsf{while}\ (x \leq x)\ \{ \mathsf{skip} \},\, \sigma \rangle \\
    \quad \rightarrow \langle \mathsf{skip};\; \mathsf{while}\ (x \leq x)\ \{ \mathsf{skip} \},\, \sigma \rangle \\
    \quad \rightarrow \langle \mathsf{while}\ (x \leq x)\ \{ \mathsf{skip} \},\, \sigma \rangle \\
    \quad \rightarrow \cdots
  \end{array}
$$

However, diverging statements need not repeat a configuration.
Consider, for example, the program $W = \mathsf{while}\ (0 \leq x)\ \{ x \leftarrow x + 1 \}$ and the state $[x \mapsto 0]$ which also leads to an infinite trace but without repetition:

$$
  \begin{array}{l}
    \langle W,\, [x \mapsto 0] \rangle \\
    \quad \rightarrow \langle x \leftarrow x + 1;\; W,\, [x \mapsto 0] \rangle \\
    \quad \rightarrow \langle W,\, [x \mapsto 1] \rangle \\
    \quad \rightarrow \langle x \leftarrow x + 1;\; W,\, [x \mapsto 1] \rangle \\
    \quad \rightarrow \langle W,\, [x \mapsto 2] \rangle \\
    \quad \rightarrow \cdots
  \end{array}
$$