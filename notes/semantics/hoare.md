---
layout: math
title: Hoare Logic
nav_order: 4
mathjax: true
parent: Semantics
---

# Hoare Logic

There are a number of reasons why it is useful to have a mathematic description of a programming language's semantics.
For instance, it can serve as a specification for a compiler or interpreter - a benchmark to ensure that an implementation matches the intended behaviour and to ensure that different implementations behave in the same way.
However, it also gives us a framework for formally _reasoning_ about programs.

The first property of a program that we will look at is _functional correctness_; that is, whether a program computes its intended (partial) function or not.
More specifically, we will look at _partial correctness_ which states that when the program terminates, i.e. reaches a terminal configuration, it produces the correct result.
Partial correctness disentangles the input-output expectations from the termination condition that we will look at next week.

A naive way to approach partial correctness would be to define a (partial) function on stores that we expect our program to compute and then manually prove that the operational semantics aligns with this expectation.
The problem with this approach is that it quickly becomes unwieldy for complex programs with may have countably many distinct traces.
<!-- % - directly relating initial stores to final stores via the program's semantics can be cumbersome, as it requires reasoning about every possible execution path and intermediate state explicitly. -->
Instead, it is much easier to specify what is known as _pre-conditions_ and _post-conditions_ that describe sets of stores before and after the execution of a given statement.
These conditions allow us to abstract away details of specific individual configuration and focus on what must hold of the store initially (the pre-condition) and, when it does, what holds after the program terminates (the post-condition).

<div class="defn" markdown="1">
  Given two Boolean expresisons $$p,\, q \in \mathcal{B}$$ and a statement $$S \in \mathcal{S}$$, we write $$\{ p \}\ S\ \{ q \}$$ to indicate that:

  $$
    \text{if}\ \llbracket p \rrbracket_\mathcal{B}(\sigma) = \top\ \text{and}\ \langle S,\, \sigma \rangle \rightarrow^* \sigma'\ \text{then}\ \llbracket q \rrbracket_\mathcal{B}(\sigma') 
  $$

  for any store $$\sigma \in \mathsf{Store}$$.

  This is referred to as a _Hoare triple_.
</div>

In other words, a Hoare triple, i.e. a pre- and post-conditions for a given statement, consists of two Boolean of stores such that when the statement is executed with a store that satisfies the pre-condition, if it terminates, then the terminal store will also satisfy the post-condition.
If the statement doesn't terminate, then the Hoare triple says nothing about its behaviour.
Therefore, a statement that never terminates will in fact satisfy _any_ Hoare triple.

For instance, we may assert that $$\{ x > 0 \}\ x \leftarrow x + 1\ \{ x > 1 \}$$ as, for any store in which $$x > 0$$, when we execute the statement, we will reach a store in which $$x > 1$$.
This gives us a way to summarise the behaviour of program $$x \leftarrow x + 1$$ across infinitely many traces.

It is worth noting that Hoare triples in general are more flexible than specifying the exact input-output behaviour of a statement.
For instance, the triple $$\{ x > 0 \}\ S\ \{ x > 1 \}$$ is satisfied by any statement that will increase $$S$$ in any terminating trace, but it doesn't just have to be $$x \leftarrow x + 1$$ and nor doesn't need to be limited to updating $$x$$.
As we shall see, this generality allows us to compose Hoare triples without having to reanalyse the same statement.

**Assertion Languages**
More generally, Hoare logic is defined in relation to a specific _assertion language_ that dictates what pre- and post-conditions are expressible.
Typically, this logic is chosen to be easily (and sometimes automatically) reasoned about.
For simplicity, we are re-use Boolean expressions as our assertion language that represent the set of stores in which they evaluate to true; remember, a subset is in one-to-one correspondence with a Boolean-valued function.

<!-- <div class="defn" markdown="1">
  An __assertion__ is a Boolean expression interpreted as a assertion on stores.
  The corresponding set of stores satisfying an assertion $P \in \mathcal{B}$ is:

  $$
  \{ \sigma \in \mathsf{Store} \mid \llbracket P \rrbracket(\sigma) = \texttt{true} \}
  $$  
</div> -->

# Compositionality

One of the advantages of considering pre- and post-conditions over manually proving correctness by deferring to traces, is that we can combine pre- and post-conditions using certain rules (this is what we mean by _composing_ triples).
Using these rules, we can analyse individual parts of the program separately, without regard for the context in which they appear, and then later combine these summaries - a very useful feature to have when programs and their dependencies reach across hundreds of file, not all of which are available!

### Skip Rule:

$$
  \dfrac
  {\{ p \}\ \mathsf{skip}\ \{ p \}}
$$

Somewhat predictably, the summary of the skip command tells us that the store doesn't change when executing this command.
Therefore, the pre- and post-conditions are the same; if executed with a store $$\sigma \in \mathsf{Store}$$ such that $$\llbracket p \rrbracket_\mathcal{B}(\sigma) = \top$$, then any terminal store will also satisfy $$p$$ in this way.

The notation we are using here is a general notation for inference rule: above the line is a series of _premises_ which we must show in order to use the rule, and below the line is the _conclusion_.
In this case, there are no premises.
As with the small-step semantics, $$p \in \mathcal{B}$$ here isn't a fixed assertion, but rather this rule can be applied to any assertion we are interested.
The same structural for each of the subsequent rule we will look at.

### Assignment Rule:

$$
  \dfrac
  {}
  {\{ p[e/x] \}\ x \leftarrow e\ \{ p \}}
$$

The notation $$p[e/x]$$ refers to the Boolean expression $$p \in \mathcal{B}$$ where the variable $$x$$ has been substituted for the arithmetic expression $$e$$ (see the problem sheet for details of this operation).
For instance, if $$e$$ was $$x + 1$$ and $$p$$ was $$x > 1$$, then $$p[e/x]$$ would be $$x + 1 > 1$$.

It may at first glance seem like this rule is back-to-front, assignment updates the state so that $$x$$ is replaced by the value of $$e$$ but here the pre-condition has been modified, not the post-condition.
However, after executing the statement, the variable $$x$$ will then hold the value of $$e$$ _before_ the assignment occurred.
In other words, what is true of $$x$$ _after_ execution is exactly what was true of $$e$$ _prior_ to execution.
For instance, this rule allows us to conclude that:

$$\{ x > 0 \}\ x \leftarrow x + 1\ \{ x > 1 \}$$

As $$(x > 1)[x + 1/x]$$ is just the expression $$x + 1 > 1$$, which is of course equivalent to $$x > 0$$ (as pre-conditions, they denote the same set of stores).

As with the previous rule, there are no premises to this rule.

### Sequence Rule:

$$
  \dfrac
  {\{ p \}\ S_1\ \{ q \} \quad \{ q \}\ S_2\ \{ r \}}
  {\{ p \}\ S_1;\; S_2\ \{ r \}}
$$

The rule for the sequence construct is where we start to see the compositionality of pre- and post-conditions.
To derive a pre- and post-condition for a sequence statement, we first identify a Hoare triple for each sub-statement $$S_1$$ and $$S_2$$ such that the post-condition for $$S_1$$ lines-up with the pre-condition of $$S_2$$, these serve as the premises of the rule.
Then the derived Hoare triple for the compound statement $$S_1;\; S_2$$ has the pre-condition of $$S_1$$ and the post-condition of $$S_2$$.

To see why this rule works, consider what the two assumptions tell us:

  - As $$\{ p \}\ S_1\ \{ q \}$$, we know that whenever $$\llbracket p \rrbracket_\mathcal{B}(\sigma)$$ and $$\langle S_1,\, \sigma \rangle \rightarrow^* \sigma'$$ then $$\llbracket q \rrbracket_\mathcal{B}(\sigma')$$.

  - Likewise, as $$\{ q \}\ S_2\ \{ r \}$$, we know that whenever $$\llbracket q \rrbracket_\mathcal{B}(\sigma)$$ and $$\langle S_2,\, \sigma \rangle \rightarrow^* \sigma'$$ then $$\llbracket r \rrbracket_\mathcal{B}(\sigma')$$.

Now let us suppose we have some initial store $$\llbracket p \rrbracket_\mathcal{B}(\sigma_0)$$ and $$\langle S_1;\; S_2,\, \sigma \rangle \rightarrow^* \sigma_T$$.
By inspecting how this execution could proceed, we can see that $$S_1$$ will execute until it ultimately reaches some terminal configuration with the store $$\sigma_1$$, and so we have that $$\langle S_1;\; S_2,\, \sigma \rangle \rightarrow^* \langle S_2,\, \sigma_1 \rangle$$.
Subsequent $$\langle S_2,\, \sigma_1 \rangle \rightarrow^* \sigma_T$$.

Thanks to the first Hoare triple, we know that $$\llbracket q \rrbracket_\mathcal{B}(\sigma_1)$$ - we have assumed that the initial store satisfies the pre-condition of $$S_1$$, i.e. $$\llbracket p \rrbracket_\mathcal{B}(\sigma_0)$$, and so, after executing $$S_1$$, this intermediate store $$\sigma_1$$ will satisfy the post-condition $$Q$$.
More over, we can then use this fact to see that, as $$\langle S_2,\, \sigma_1 \rangle \rightarrow^* \sigma_T$$, the terminal store $$\sigma_T$$ must satisfy $$R$$ by virtue of the second Hoare triple.
Thus making $$\{ p \}\ S_1;\; S_2\ \{ q \}$$ a valid Hoare triple.

This particular rule illustrates the utility of considering Hoare triples as we don't have to consider the executing of the compound statement $$S_1;\; S_2$$ directly, but rather can derive our result from Hoare triples concerning each sub-statement.
It is worth clarifying that the reasoning above explains why the rule works, but it is not necessary when using this rule.

### Conditional Rule:

$$
  \dfrac
  {\{ e \andop p \}\ S_1\ \{ q \} \quad \{ \mathop{!}e \andop p \}\ S_2\ \{ q \}}
  {\{ p \}\ \mathsf{if}\ e\ \mathsf{then}\ S_1\ \mathsf{else}\ S_2\ \{ q \}}
$$.

The rule for the if-then-else construct is again an example of a compositional rule.
To show that the Hoare triple $$\{ p \}\ \mathsf{if}\ e\ \mathsf{then}\ S_1\ \mathsf{else}\ S_2\ \{ q \}$$ holds, we split our reasoning according to the two branches.
For the first branch, we additional get to assume that $$e$$ will hold of the initial store as it is only under this condition that this branch is considered.
Likewise, for the second branch, we get to assume that $$\mathop{!}e$$ holds.

 <!-- when executed in a store that satisfies $p$, will either not-terminate or reach a terminal store that satisfies $q$, by showung  that, for each branch, if it terminates then it will reach a store in $Q$; however, we get to make additional assumptions about the pre-condition in each branch.
In particular, the first branch is only executed when considering a store that satisfies the branch condition $e$, and likewise the second branch is only executed when considering a store that satisfies its negation. -->

As with the previous rule, this rule can be formallu understood in relation to the operational semantics.
Suppose $$\langle \mathsf{if}\ e\ \mathsf{then}\ S_1\ \mathsf{else}\ S_2,\, \sigma \rangle \rightarrow^* \sigma'$$ for some $$\sigma$$ that satisfies $$p$$.
There are two cases to consider:

  - If $$\llbracket e \rrbracket_\mathcal{B}(\sigma) = \top$$, then it must be the case that $$\langle \mathsf{if}\ e\ \mathsf{then}\ S_1\ \mathsf{else}\ S_2,\, \sigma \rangle \rightarrow \langle S_1,\, \sigma \rangle \rightarrow^* \sigma'$$.
    In this case, we can appeal the Hoare-triple concerning the first branch to see that $$\sigma'$$ must satisfy $$q$$ as the pre-condition $$e \andop p$$ is satisfy by $$\sigma$$.
  
  - Otherwise, if $$\llbracket e \rrbracket_\mathcal{B}(\sigma) = \bot$$ and $$\langle \mathsf{if}\ e\ \mathsf{then}\ S_1\ \mathsf{else}\ S_2,\, \sigma \rangle \rightarrow \langle S_2,\, \sigma \rangle \rightarrow^* \sigma'$$, our reasoning is symmetric.
    We know that $$\mathop{!}e \andop e$$ will be satisfied by the store $$\sigma$$ and so, according to the Hoare triple concerning the second branch, we can conclude that $$\sigma'$$ will satisfy the post-condition $$q$$.

In either case, we have concluded that the post-condition $$q$$ will be met and thus this rule for contructing Hoare triples is correct.

### While Rule:

$$
  \dfrac
  {\{ e \andop p \}\ S\ \{ p \}}
  {\{ p \}\ \mathsf{while}\ e\ \mathsf{do}\ S\ \{ \mathop{!}e \andop p \}}
$$

We'll look at this rule in more detail in the next lecture.

### Consequence/Subsumption Rule: 

$$
  \dfrac
  {\{ p' \}\ S\ \{ q' \}}
  {\{ p \}\ S\ \{ q \}}
  p \vDash p' \text{and}\ q' \vDash q
$$

Unlike the other rules we have seen so far, this rule doesn't involve any particular syntactic construct but rather works for an arbitrary statement.
It allows us to weaken a strong claim $$\{ p' \}\ S\ \{ q' \}$$, into a more specific claim $$\{ p \}\ S\ \{ q \}$$ that may fit with our overall goal.
What we mean by a stronger claim is one that is more general, i.e. applies to more situations.
For example, the triple $$\{ \top \}\ S\ \{ x \geq 0 \}$$ is stronger than $$\{ x < 0 \}\ S\ \{ x > 0 \}$$ as it (a) assumes less about the initial store, and (b) tells us more about the possible terminal store.
Notice that the direction of implication changes between the pre- and post-condition: a pre-condition is stronger is it covers a larger set of initial stores, and a post-condition is stronger if it describes a more specific property of the terminal stores, i.e. a smaller set.

Being able to weaken a pre-existing Hoare triple is primarily useful in conjunction with our compositional rules.
For instance, using the rule for assignment, we may conclude that $$\{ x > 0 \}\ x \leftarrow x + 1\ \{ x > 1 \}$$ and we wish to combine this with the fact that $$\{ x \geq 0 \}\ y \leftarrow x\ \{ y \geq 0 \}$$ to derive a triple for $$x \leftarrow x + 1;\; y \leftarrow y + 1$$.
We cannot immediately use the sequence rule as the post-condition of the first statement doesn't align with the pre-condition of the second.
However, if we apply consequence, we can weaken the first triple to $$\{ x > 0 \}\ x \leftarrow x + 1\ \{ x \geq 0 \}$$, which is of course still valid, and then combine these triples to conclude that $$\{ x > 0 \}\ x \leftarrow x + 1;\; y \leftarrow y + 1\ \{ y \geq 0 \}$$.
Notice we derived this triple without consider why the assumed triples are true or deferring to the operational semantics, so we could have equally done this with some arbitrary statements that perhaps refers to some external function.

## Weakest Pre-condition and Consequent

As we have seen, we can derive Hoare triples from the rules given above.
In practice, however, we are more likely to be given Hoare triple holds, e.g.

$$
  \{ \top \}\ \mathsf{if}\ x < 0\ \mathsf{then}\ x \leftarrow 0 - x\ \mathsf{else}\ \mathsf{skip}\ \{ x \geq 0 \}
$$

and asked to determine whether it is valid or not.

To answer this question, we will work backwards starting with the post-condition $$x \geq 0$$ to try to determine the *weakest* pre-condition that will ensure the post-condition is met.

<div class="defn" markdown="1">
  For a given post-condition $$q \in \mathcal{B}$$ and a statement $$S \in \mathcal{S}$$, the weakest pre-condition is some assertion $$p \in \mathcal{B}$$ such that:
  
  - $$\{ p \}\ S\ \{ q \}$$, i.e. the weakest pre-condition is indeed a pre-condition;
  - And, if $$\{ p' \}\ S\ \{ q \}$$, then $$\llbracket p' \rrbracket_\mathcal{B}(\sigma) \Rightarrow \llbracket p \rrbracket_\mathcal{B}(\sigma)$$ for any $$\sigma \in \mathsf{Store}$$ - that is, any other pre-condition is more specific.

  We write $$\mathsf{wp}(S,\, q)$$ to denote the weakest pre-condition of some statement $$S$$ and some post-condition $$q$$.
</div>

The weakest pre-condition is not only a pre-condition that will ensure the given post-condition is met, but it is in some sense the canonical such pre-condition and will subsume any other such pre-condition.
For example, $$\mathsf{wp}(x \leftarrow x + 1,\, x > 1)$$ is precisely $$x > 0$$; it is indeed a pre-condition for the triple $$\{ x > 0 \}\ x \leftarrow x + 1\ \{ x > 1 \}$$ and if we had some other pre-condition, e.g. $$\{ x > 1 \}\ x \leftarrow x + 1\ \{ x > 1 \}$$, then we'd have that $$\llbracket x > 1 \rrbracket_\mathcal{B}(\sigma) \Rightarrow \llbracket x > 0 \rrbracket_\mathcal{B}(\sigma)$$ so it is weaker than this alternative pre-condition.
Rather than prove it is weaker than all pre-conditions, we will see certain rules for computing the weakest pre-condition in a moment.  

Intuitively, the weakest pre-condition corresponds to the exact set of stores such that, when the given statement has a terminating executed in one of these stores, then the terminal a store will satisfy the post-condition:

$$
  \mathsf{wp}(S,\, q) \approx \{ \sigma \in \mathsf{Store} \mid \text{if}\ \langle S,\, \sigma \rangle \rightarrow^* \sigma'\ \text{then}\ \llbracket q \rrbracket_\mathcal{B}(\sigma') \}
$$

This correspondance is only intuitive as not all subsets are described by a Boolean expression.
For example, you cannot describe the set of stores $\sigma \in \mathsf{Store}$ for which $\sigma(x)$ is prime just using Boolean expressions.
The strict definition of the weakest pre-condition only requires that it is the ``best'' Boolean expression that approximates this set.
Although it is worth noting that when a set is not precisely described by a Boolean expression, there will always be a better expression and thus the weakest pre-condition is undefined.

The reason the weakest pre-condition is useful is that when we are tasked with assessing the validity of a given Hoare triple, e.g. $$\{ \top \}\ \mathsf{if}\ x < 0\ \mathsf{then}\ x \leftarrow 0 - x\ \mathsf{else}\ \mathsf{skip}\ \{ x \geq 0 \}$$, we can work out the weakest pre-condition and then we need only check whether the specified pre-condition is a stronger predicate than the weakest pre-condition, and if so we can apply the consequence rul;e to get the desired triple.

So how are we going to derive the weakest pre-condition $$\mathsf{wp}(x \leftarrow y,\, \mathsf{if}\ x < 0\ \mathsf{then}\ x \leftarrow 0 - x\ \mathsf{else}\ \mathsf{skip},\, x \geq 0)$$?
Well it just so happens that the basic rules we defined for each language construct (other than while) calculate their weakest pre-condition - they are the most precise claims we can make.
Therefore, as long as we retain maximum generality when applying this rules, we can calculate the weakest pre-condition compositionally.

At the top-level, our statement is a sequence statemtn and thus we will apply the rule for this construct.
To do so, we must find some $p$ and $q$ such that:

  - $$\{ p \}\ x \leftarrow y\ \{ q \}$$
  - And, $$\{ q \}\ \mathsf{if}\ x < 0\ \mathsf{then}\ x \leftarrow 0 - x\ \mathsf{else}\ \mathsf{skip}\ \{ x \geq 0 \}$$.

The weakest pre-condition will reflect the strength of the post-condition.
For instance, if we have a trivially true post-condition, then the weakest pre-condition will also be trivially true, and if we have a trivially false post-condition, then the weakest pre-condition must ensure the statement never terminates.
In other words, if we want to find the weakest possible $p$, we need to start by finding the weakest possible $q$.
In general, we may characterise the weakest pre-condition for the sequence construct as:

$$
  \mathsf{wp}(S_1;\; S_2, p) = 
    \mathsf{wp}(S_1,\, \mathsf{wp}(S_2, p))
$$

Let us turn our attention, therefore, to the second statement.
As this statement uses the if-then-else construct, we will apply the corresponding rule to decompose this objective into two simpler objectives:

$$
  \{ x < 0 \andop q \}\ x \leftarrow 0 - x\ \{ x \geq 0 \}\ \text{and}\ \{ \mathop{!}(x < 0) \andop q \}\ \mathsf{skip}\ \{ x \geq 0 \}
$$

From which, we may infer $$\{ q \}\ \mathsf{if}\ x < 0\ \mathsf{then}\ x \leftarrow 0 - x\ \mathsf{else}\ \mathsf{skip}\ \{ x \geq 0 \}$$.

Again, we shall proceed recursively and find the weakest pre-condition to each of these branches:

  - $$\{ 0 - x \geq 0 \}\ x \leftarrow 0 - x\ \{ x \geq 0 \}$$
    Here we have instantiate the rule for assignment with the desired post-condition, and this immediately gives us the weakest pre-condition.
    That is,

    $$
      \mathsf{wp}(x \leftarrow e, p) = p[e/x]
    $$

  - $$\{ x \geq 0 \}\ \mathsf{skip}\ \{ x \geq 0 \}$$
    Likewise, the rule for skip immediately gives us the weakest pre-condition.
    That is,

    $$
      \mathsf{wp}(\mathsf{skip}, p) = p
    $$

Now we have two distinct pre-conditions that we have to rearrange into the form $$x < 0 \andop q$$ and $$ \mathop{!}(x< 0) \andop q$$ for some $q$ respectively.
To do this most generally, we ensure $q$ is equivalent to $$(\mathop{!}(x < 0) \orop 0 - x \geq 0) \andop (x < 0 \orop x \geq 0)$$ as we then have that $x < 0 \andop q$ is equivalent to $0 - x \geq 0$ and $\mathop{!}(x < 0) \andop q$ is equivalent to $x \geq 0$.
In general, we may characterise the weakest pre-condition for the if-then-else construct as:

$$
  \mathsf{wp}(\mathsf{if}\ e\ \mathsf{then}\ S_1\ \mathsf{else}\ S_2, p) = 
    (e \Rightarrow \mathsf{wp}(S_1, p)) \andop (\mathop{!}e \Rightarrow \mathsf{wp}(S_2, p))
$$

where $p \Rightarrow q$ is syntactic sugar for $\mathop{!}p \orop q$.

At this point, we have calculated the weakest pre-condition for the second statement in our sequence $$x \leftarrow y,\, \mathsf{if}\ x < 0\ \mathsf{then}\ x \leftarrow 0 - x\ \mathsf{else}\ \mathsf{skip}$$, and we can consider this to be the post-condition to $$x \leftarrow y$$.
As it is an assignment statement, we can see the weakest pre-condition for the compound expression is $$(\mathop{!}(y < 0) \orop 0 - y \geq 0) \andop (y < 0 \orop y \geq 0)$$.
Thus we have concluded that:

$$
  \{ (\mathop{!}(y < 0) \orop 0 - y \geq 0) \andop (y < 0 \orop y \geq 0) \}\ x \leftarrow y,\, \mathsf{if}\ x < 0\ \mathsf{then}\ x \leftarrow 0 - x\ \mathsf{else}\ \mathsf{skip}\ \{ x \geq 0 \}
$$

And, moreover, if there were any other pre-condition $p$ we would have that $$\llbracket p \rrbracket_\mathcal{B}(\sigma) \Rightarrow \llbracket (\mathop{!}(y < 0) \orop 0 - y \geq 0) \andop (y < 0 \orop y \geq 0) \rrbracket_\mathcal{B}(\sigma)$$.
As we wish to check if $$\top$$ is a valid pre-condition, we therefore need to ascertain whether $$\llbracket \top \rrbracket_\mathcal{B}(\sigma) \Rightarrow \llbracket (\mathop{!}(y < 0) \orop 0 - y \geq 0) \andop (y < 0 \orop y \geq 0) \rrbracket_\mathcal{B}(\sigma)$$ or not.
Or more concretely, whether the Boolean formula $$(\mathop{!}(y < 0) \orop 0 - y \geq 0) \andop (y < 0 \orop y \geq 0)$$ is always true.

When $$y < 0$$ we have that $$0 - y \geq 0$$ and so both conjuncts are valid, and likewise when $$y > 0$$ we have that $$y \geq 0$$ and so the both conjuncts are again valid.
Therefore, we can conclude the original triple $$\{ \top \}\ \mathsf{if}\ x < 0\ \mathsf{then}\ x \leftarrow 0 - x\ \mathsf{else}\ \mathsf{skip}\ \{ x \geq 0 \}$$ is indeed valid using the consequence rule. 

This may seem rather long-winded, but actually that is because we have been closely inspecting the various steps taken along the way.
We can summarize the process more compactly as follows:

To check whether $$\{ p \} S \{ q \}$$ is valid:

  - Calculate $$\mathsf{wp}(S,\, q)$$ using the equations laid-out above.

  - Determine whether $$\llbracket \mathsf{wp}(S,\, q) \rrbracket_\mathcal{S}(\sigma) \Rightarrow \llbracket q \rrbracket_\mathcal{S}(\sigma)$$ for all $$\sigma$$. 

For programs that don't involve the while statement, the strategy we have seen for checking whether a given triple holds is _complete_.
That is, if the triple holds, then are strategy will indeed be able to derive it from the rules we have outlined.
Moreover, if the strategy fails as the final implication does not hold, we know the triple does not hold.
The completeness here is derived precisely from the fact we consider the _weakest_ pre-condition - any given pre-condition must be weaker; or, if it is not weaker, then it is not a pre-condition.

For programs involving while loops, the process breaks down as we cannot always computer the weakest pre-condition precisely.
Instead, we may have to _over-approximate_ it, i.e. compute a less general pre-condition.
We can still use the process to check whether a triple holds, but if it doesn't work we don't know whether this is due to our over-approximation or not - it is sound, but incomplete.
But more on that next week!