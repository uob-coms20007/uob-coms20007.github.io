---
layout: math
title: Hoare Logic
nav_order: 4
mathjax: true
parent: Semantics
---

# Hoare Logic

Having a mathematical description of a programming language's semantics enables us to formal reason about the behaviour of programs.
However, it can be quite cumbersome doing so directly, and much of the field of programming language's is based on designing effective reasoning techniques.
Hoare logic is one such framework.

The sort of properties Hoare logic can be used for are _functional correctness_ properties; that is, whether a program computes an intended function or not.
More specifically, we will look at _partial correctness_ which states that when the program terminates, i.e. reaches a terminal configuration, it produces the correct result.
Partial correctness disentangles the input-output expectations from the termination condition.

A naive way to approach partial correctness would be to define a partial function on states that we expect our program to compute and then manually prove that the operational semantics aligns with this expectation.
The problem with this approach is that it quickly becomes unwieldy for complex programs with may have countably many distinct traces.
<!-- % - directly relating initial states to final states via the program's semantics can be cumbersome, as it requires reasoning about every possible execution path and intermediate state explicitly. -->
Instead, it is much easier to specify what is known as _pre-conditions_ and _post-conditions_ that describe sets of states before and after the execution of a given statement.
These conditions allow us to abstract away details of specific individual configuration and focus on what properties hold of the state initially (i.e. the pre-condition) and what holds after the program if it terminates (i.e. the post-condition).

<div class="defn" markdown="1">
  Given two subsets of states $$P,\, Q \subseteq \mathsf{State}$$ and a statement $$S \in \mathcal{S}$$, we write $$\{ P \}\ S\ \{ Q \}$$ to indicate that:

  $$
    \text{if}\ \sigma_0 \in P\ \text{and}\ \langle S,\, \sigma_0 \rangle \rightarrow^* \sigma_1\ \text{then}\ \sigma_1 \in Q
  $$

  for any state $$\sigma_0 \in \mathsf{State}$$.

  This is referred to as a _Hoare triple_.
</div>

In other words, a Hoare triple, i.e. a pre- and post-conditions for a given statement, consists of two conditions on states such that when the statement is executed in a state that satisfies the pre-condition, if it terminates, then the terminal state will also satisfy the post-condition.
If the statement doesn't terminate, however, then the Hoare triple says nothing about its behaviour and there is nothing to say that it will terminate.
In particular, a statement that never terminates will in fact satisfy _any_ Hoare triple.

As an example, we may assert that $$\{ x > 0 \}\ x \leftarrow x + 1\ \{ x > 1 \}$$ as, for any state in which $$x > 0$$, when we execute this statement, it will reach a state in which $$x > 1$$.
This gives us a way to summarise the behaviour of program $$x \leftarrow x + 1$$ across infinitely many traces.

It is worth noting that Hoare triples are more flexible than specifying the exact input-output behaviour of a statement, rather they describe a relation that subsumes the particular function.
For instance, the triple $$\{ x > 0 \}\ S\ \{ x > 1 \}$$ is satisfied by any statement that will increase $$x$$ during any terminating trace, but it doesn't just have to be $$x \leftarrow x + 1$$ and nor doesn't need to be limited to updating $$x$$.
As we shall see, this generality allows us to compose Hoare triples without having to reanalyse the same statement.

## Assertion Languages
Within the above examples, we have been using Boolean expressions to implicitly represent sets of states, e.g. where $$x > 0$$ represents the set $$ \{ \sigma \in \mathsf{State} \mid \llbracket x > 0 \rrbracket_\mathcal{B}(\sigma) = \top \}$$.
In generally, Hoare logic is defined in relation to a specific _assertion language_ that dictates what pre- and post-conditions are expressible.
Typically, this logic is chosen to be easily (and sometimes automatically) reasoned about.

We will consider an assertion language based on an extension of Boolean expressions with quantifiers.

<div class="defn" markdown="1">
  An __extended Boolean expression__ $$\mathcal{B}^+$$ is an element of the following grammar:
  
  $$
    B^+ \Coloneqq \tt \mid \ff \mid \mathop{!}B \mid B \andop B \mid B \orop B \mid A = A \mid A \leq A \mid \forall x.\, B^+ \mid \exists x.\, B^+
  $$

  with the denotation function extended correspondingly:

  $$
  \begin{array}{rl}
    \llbracket \forall x.\, e \rrbracket_{\mathcal{B}^+}(\sigma) & = \forall n.\, \llbracket e \rrbracket_{\mathcal{B}^+}(\sigma[x \mapsto n]) \\
    \llbracket \exists x.\, e \rrbracket_{\mathcal{B}^+}(\sigma) & = \exists n.\, \llbracket e \rrbracket_{\mathcal{B}^+}(\sigma[x \mapsto n])
  \end{array}  
  $$

</div>

<!-- We will write $\sigma \models P$ for some extended Boolean expression $P \in \mathcal{B}^+$ to represent the fact that $\llbracket P \rrbracket_{\mathcal{B}^+}(\sigma) = \top$. -->

We will use extended Boolean expressions interchangeably with sets of states.
You don't need to worry too much about the specifics of extended Boolean expressions; for the most part, they behave exactly like standard logical formulas and satisfy all the usual identities - feel free to use any logical expression within assertions.
The key thing to remember, however, is that _free variables_ within assertions, i.e. those that are not bound to a quantifier, refer to _program variables_ and thus depend on the current state, e.g. $$\exists y.\, x = (2 * y) + 1$$ corresponds to the set of states in which the current value of the variable $x$ is even.

# Compositionality

One of the advantages of considering pre- and post-conditions over manually proving correctness by deferring to traces, is that we can combine pre- and post-conditions using certain rules (this is what we mean by _composing_ triples).
Using these rules, we can analyse individual parts of the program separately, without regard for the context in which they appear, and then later combine these summaries - a very useful feature to have when programs and their dependencies reach across hundreds of file, not all of which are available!

### Skip Rule:

$$
  \dfrac
  {}
  {\{ P \}\ \mathsf{skip}\ \{ P \}}
$$

Somewhat predictably, the summary of the skip command tells us that the state doesn't change when executing this command.
Therefore, the pre- and post-conditions are the same; if executed with a state $$\sigma \in \mathsf{State}$$ such that $$\sigma \in P$$, then any terminal state will also satisfy $$P \subseteq \mathsf{State}$$.

The notation we are using here is similar to that of the operational semantics rule: above the line is a series of _premises_ which we must show in order to use the rule, and below the line is the _conclusion_ - you can read it as "if everything above the line holds, then everything below the line holds."
In this case, there are no premises, so we can use the conclusion without having to show anything first.
As with the small-step semantics, $$P \subseteq \mathsf{State}$$ here isn't a fixed object, but rather a metavariable so that this rule can be applied to any assertion we are interested.
The same structure holds for each of the subsequent rule we will look at.

### Assignment Rule:

$$
  \dfrac
  {}
  {\{ P \}\ x \leftarrow e\ \{ \exists x'.\, P[x'/x] \andop x = e[x'/x] \}}
$$

The notation $$P[e/x]$$ refers to the assertion $$P$$ where the variable $$x$$ has been substituted for the arithmetic expression $$e$$.
For instance, if $$P$$ was the extended Boolean expression $$x > 1$$, then $$P[x + 1/x]$$ would be $$x + 1 > 1$$.

If $$P$$ is represented as an extended Boolean expression, then we can define this recursively in a similar manner to the substitution operations from the problem sheet.
Or, for a set of states $$P \subseteq \mathsf{State}$$, we can define it as $$\{ \sigma \mid \sigma[x \mapsto \llbracket e \rrbracket_\mathcal{A}(\sigma)] \in P \}$$, i.e. those states that would be in $$P$$ if $$x$$ is replaced with $$e$$.
For instance:

$$
  \begin{array}{rl}
    P &= \{ \sigma \mid \sigma(x) > 0 \} \\ \\
    P[x+1/x] &= \{ \sigma \mid \sigma[x \mapsto \llbracket x + 1 \rrbracket_\mathcal{A}(\sigma)] \in P \} \\
             &= \{ \sigma \mid \sigma[x \mapsto \llbracket x + 1 \rrbracket_\mathcal{A}(\sigma)](x) > 0 \} \\
             &= \{ \sigma \mid \llbracket x + 1 \rrbracket_\mathcal{A}(\sigma) > 0 \} \\
             &= \{ \sigma \mid \sigma(x) + 1 > 0 \} \\
  \end{array}
$$

Within the post-condition, we have an existential variable $$x'$$. 
Intuitively, this variable corresponds to the value of $$x$$ _prior_ to execution of this assignment statement.
So, in particular, we know that $$p[x'/x]$$ holds as this was specified by our pre-condition.
Additionally, we know that $x$ after the assignment statement, i.e. its new value, will be equal to $e[x'/x]$ as $x$ has been assigned the value of $e$ as was calculated according to the prior value of $x$, i.e. its old value $x'$.
<!-- Remember $\langle x \leftarrow e,\, \sigma \rangle \rightarrow \sigma[x \mapsto \llbracket e \rrbracket_\mathcal{B}(\sigma)]$ where the resulting state is updated such that $x$ maps to the denotation of $e$ as was calculated according to the prior value $x$. -->

For example, this rule allows us to conclude that:

$$\{ x > 0 \}\ x \leftarrow x + 1\ \{ \exists x'.\, x' > 0 \andop x = x' + 1 \}$$

Here the assertion $P$, i.e. our pre-condition, is $x > 0$ hence $P[x'/x]$ becomes $x' > 0$.
As expected, this post-condition is equivalent to $x > 1$ (in the sense that they denote the same set of states).
Therefore, we could have equivalently said $$\{ x > 0 \}\ x \leftarrow x + 1\ \{ x > 1 \}$$.

### Sequence Rule:

$$
  \dfrac
  {\{ P \}\ S_1\ \{ Q \} \quad \{ Q \}\ S_2\ \{ R \}}
  {\{ P \}\ S_1;\; S_2\ \{ R \}}
$$

The rule for the sequence construct is where we start to see the compositionality of pre- and post-conditions.
To derive a pre- and post-condition for a sequence statement, we first identify a Hoare triple for each sub-statement $$S_1$$ and $$S_2$$ such that the post-condition for $$S_1$$ lines-up with the pre-condition of $$S_2$$, these serve as the premises of the rule.
Then the derived Hoare triple for the compound statement $$S_1;\; S_2$$ has the pre-condition of $$S_1$$ and the post-condition of $$S_2$$.

To see why this rule works, consider what the two assumptions tell us:

  - As $$\{ P \}\ S_1\ \{ Q \}$$, we know that whenever $$\sigma_0 \in P$$ and $$\langle S_1,\, \sigma_0 \rangle \rightarrow^* \sigma_1$$ then $$\sigma_1 \in Q$$.

  - Likewise, as $$\{ Q \}\ S_2\ \{ R \}$$, we know that whenever $$\sigma_0 \in Q$$ and $$\langle S_2,\, \sigma_0 \rangle \rightarrow^* \sigma_1$$ then $$\sigma_1 \in R$$.

Now let us suppose we have some initial state $$\sigma$$ such that $$\sigma_0 \in P$$ and $$\langle S_1;\; S_2,\, \sigma \rangle \rightarrow^* \sigma_2$$.
By inspecting how this execution could proceed, we can see that $$S_1$$ will execute first until it ultimately reaches some terminal configuration with the state $$\sigma_1$$, and so we have that $$\langle S_1; S_2,\, \sigma \rangle \rightarrow^* \langle S_2,\, \sigma_1 \rangle$$.
Subsequently, $$\langle S_2,\, \sigma_1 \rangle \rightarrow^* \sigma_2$$.

The first premise tell us that $$\sigma_1 \in Q$$ - we have assumed that the initial state satisfies the pre-condition of $$S_1$$, i.e. $$\sigma_0 \in P$$, and so, after executing $$S_1$$, this intermediate state $$\sigma_1$$ will satisfy the post-condition $$Q$$.
More over, we can then use this fact to see that, as $$\langle S_2,\, \sigma_1 \rangle \rightarrow^* \sigma_2$$, the terminal state $$\sigma_2$$ must satisfy $$R$$ by virtue of the second Hoare triple.
Thus making $$\{ P \}\ S_1;\; S_2\ \{ Q \}$$ a valid Hoare triple.

This particular rule illustrates the utility of considering Hoare triples as we don't have to consider the executing of the compound statement $$S_1;\; S_2$$ directly, but rather can derive our result from Hoare triples concerning each sub-statement.
It is worth clarifying that the reasoning above explains why the rule works, but it is not necessary when using this rule.

### Conditional Rule:

$$
  \dfrac
  {\{ e \andop P \}\ S_1\ \{ Q_1 \} \quad \{ \mathop{!}e \andop p \}\ S_2\ \{ Q_2 \}}
  {\{ P \}\ \mathsf{if}\ e\ \mathsf{then}\ S_1\ \mathsf{else}\ S_2\ \{ Q_1 \orop Q_2 \}}
$$

<!-- To show that the Hoare triple $$\{ p \}\ \mathsf{if}\ e\ \mathsf{then}\ S_1\ \mathsf{else}\ S_2\ \{ q \}$$ holds, we split our reasoning according to the two branches.
For the first branch, we additional get to assume that $$e$$ will hold of the initial state as it is only under this condition that this branch is considered.
Likewise, for the second branch, we get to assume that $$\mathop{!}e$$ holds. -->

 <!-- when executed in a state that satisfies $p$, will either not-terminate or reach a terminal state that satisfies $q$, by showung  that, for each branch, if it terminates then it will reach a state in $Q$; however, we get to make additional assumptions about the pre-condition in each branch.
In particular, the first branch is only executed when considering a state that satisfies the branch condition $e$, and likewise the second branch is only executed when considering a state that satisfies its negation. -->

The rule for the if-then-else construct is again an example of a compositional rule.
As with the previous rule, this rule can be formally understood in relation to the operational semantics.
Suppose $$\langle \mathsf{if}\ e\ \mathsf{then}\ S_1\ \mathsf{else}\ S_2,\, \sigma_0 \rangle \rightarrow^* \sigma_1$$ for some $$\sigma_0$$ that satisfies $$p$$.
There are two cases to consider:

  - If $$\llbracket e \rrbracket_\mathcal{B}(\sigma_0) = \top$$, then it must be the case that $$\langle \mathsf{if}\ e\ \mathsf{then}\ S_1\ \mathsf{else}\ S_2,\, \sigma \rangle \rightarrow \langle S_1,\, \sigma_0 \rangle \rightarrow^* \sigma_1$$.
    In this case, we can appeal the Hoare-triple concerning the first branch to see that $$\sigma_1$$ must satisfy $$Q_1$$ as the pre-condition $$e \andop P$$ is satisfy by $$\sigma$$.
    We get to assume $e$ as well because it is only under this condition that the branch will be executed.
  
  - Otherwise, if $$\llbracket e \rrbracket_\mathcal{B}(\sigma_0) = \bot$$ and $$\langle \mathsf{if}\ e\ \mathsf{then}\ S_1\ \mathsf{else}\ S_2,\, \sigma_0 \rangle \rightarrow \langle S_2,\, \sigma_0 \rangle \rightarrow^* \sigma_1$$, our reasoning is symmetric.
    We know that $$\mathop{!}e \andop P$$ will be satisfied by the state $$\sigma_0$$ and so, according to the Hoare triple concerning the second branch, we can conclude that $$\sigma_1$$ will satisfy the post-condition $$Q_2$$.
    We get to assume $!e$ as well because it is only under this condition that the branch will be executed.

Therefore, we know that the post-condition will satisfy either $Q_1$ or $Q_2$, but we don't know which.
Hence, we can only conclude that the post-condition will satisfy $Q_1 \orop Q_2$.

### Consequence/Subsumption Rule: 

$$
  \dfrac
  {\{ P_1 \}\ S\ \{ Q_1 \}}
  {\{ P_2 \}\ S\ \{ Q_2 \}}
  P_2 \vDash P_1 \text{and}\ Q_1 \vDash Q_2
$$

Unlike the other rules we have seen so far, this rule doesn't involve any particular syntactic construct but rather works for an arbitrary statement.
It allows us to weaken a strong assertion $$\{ P_1 \}\ S\ \{ Q_1 \}$$, into a more specific claim $$\{ P_2 \}\ S\ \{ Q_2 \}$$ that fits with our particular objective.
The _side conditions_ $$P_2 \subseteq P_1$$ and $$Q_1 \subseteq Q_2$$ say that whenever $$p_2$$ is true $$p_1$$ is also true, and likewise whenever $$q_1$$ is true $$q_2$$ is true (formally, $$p_2 \vDash p_1$$ just if $$\llbracket p_2 \rrbracket_\mathcal{B}(\sigma) \Rightarrow \llbracket p_1 \rrbracket_\mathcal{B}(\sigma)$$ for all states $$\sigma \in \mathsf{state}$).

What we mean by a "weaker" assertion is one that is more general, i.e. is true of more statess.
This may sound contradictory, but it is weaker in the sense that it tells us less about the initial or terminal state.

A Hoare triple $$\{ p_1 \}\ S\ \{ q_1 \}$$ is stronger than another triple $$\{ p_2 \}\ S\ \{ q_2 \}$$ if it gives us more information about the behaviour of $S$.
For example, the triple $$\{ \top \}\ S\ \{ x \geq 0 \}$$ is stronger than $$\{ x < 0 \}\ S\ \{ x > 0 \}$$ as it (a) assumes less about the initial state, i.e. has a weaker pre-condition and thus applies in more scenarios, and (b) tells us more about the possible terminal state, i.e. has a stronger post-condition and thus tells us more about the terminal states.
Notice that the direction of implication changes between the pre- and post-condition: a weaker pre-condition makes for a stronger triple as it covers a larger set of initial states, and a stronger post-condition makes for a stronger triple as it describes a more specific property of the terminal states.

Being able to weaken a pre-existing Hoare triple is often useful in conjunction with our compositional rules.
For instance, using the rule for assignment, we may conclude that $$\{ x > 0 \}\ x \leftarrow x + 1\ \{ x > 1 \}$$, but we may wish to combine this with the fact that $$\{ x \geq 0 \}\ y \leftarrow x\ \{ y \geq 0 \}$$ to derive a Hoare triple for $$x \leftarrow x + 1;\; y \leftarrow y + 1$$.
We cannot immediately use the sequence here rule as the post-condition of the first statement doesn't align with the pre-condition of the second.
We could go back and re-analyse one our assignment statements, but instead we can apply the consequence rule to show that $$\{ x > 0 \}\ x \leftarrow x + 1\ \{ x \geq 0 \}$$ (which is of course still valid).
Now we can combine these triples to conclude that $$\{ x > 0 \}\ x \leftarrow x + 1;\; y \leftarrow y + 1\ \{ y \geq 0 \}$$.

By weakening an existing triple is this manner, we save ourselves the effort of re-analysing statements.
In this case, it wouldn't be too difficult to derive the triple $\{ x > 0 \}\ x \leftarrow x + 1\ \{ x \geq 0 \}$$ from first principles, i.e. using the assignment rule, but when the statements are large or perhaps refers to some external function that we cannot inspect, this is very useful.

### While Rule:

$$
  \dfrac
  {\{ e \andop P \}\ S\ \{ P \}}
  {\{ P \}\ \mathsf{while}\ e\ \mathsf{do}\ S\ \{ \mathop{!}e \andop P \}}
$$

Finally, there is the while rule.
This rule doesn't behave as nicely as the others, so we will look at it in more detail next time.

## Strongest Post-condition

As we have seen, we can derive Hoare triples from the rules given above.
In practice, however, we are more likely to be given a pre- and post-condition that serves as a specification for our program and we wish to verify that the implementation matches this expectation.
For instance, when given the triple:

$$
  \{ \top \}\ y \leftarrow x;\ \mathsf{if}\ x < 0\ \mathsf{then}\ x \leftarrow 0 - x\ \mathsf{else}\ \mathsf{skip}\ \{ x \geq 0 \}
$$

how do we determine whether it is valid or not?
That is, does this program ensure that $x$ is non-negative after execution.

To answer this question, we will start with the pre-condition $$x \geq 0$$ to try to determine the *strongest* post-condition that follows from the pre-condition.

<div class="defn" markdown="1">
  For a given pre-condition $$P \subseteq \mathsf{State}$$ and a statement $$S \in \mathcal{S}$$, the strongest post-condition is some assertion $$Q \subseteq \mathsf{State}$$ such that:
  
  - $$\{ P \}\ S\ \{ Q \}$$, i.e. the strongest post-condition is indeed a post-condition;
  - And, if $$\{ P \}\ S\ \{ Q' \}$$, then $$Q \subseteq Q'$$ that is, any other post-condition is a larger, and thus less specific, than the strongest post-condition.

  We write $$\mathsf{SPC}(S,\, P)$$ to denote the strongest post-condition of some statement $$S$$ and some pre-condition $$P$$.
</div>

The strong post-condition is not only a post-condition that follows from the give pre-condition, but it is in some sense the canonical such post-condition and will subsume any other such post-condition.
For example, $$\mathsf{SPC}(x \leftarrow x + 1,\, x > 0)$$ is $$x > 1$$ as it is the most specific thing we can say that will be true after executing this statement.
<!-- a valid pre-condition, but also any other pre-condition such as $x > 1$ will be less general. -->
<!-- This is what we mean by _weakest_; it is logically weakest in that it requires the least restriction amongst pre-conditions. -->
<!-- ; it is indeed a pre-condition for the triple $$\{ x > 0 \}\ x \leftarrow x + 1\ \{ x > 1 \}$$ and if we had some other pre-condition, e.g. $$\{ x > 1 \}\ x \leftarrow x + 1\ \{ x > 1 \}$$, then we'd have that $$\llbracket x > 1 \rrbracket_\mathcal{B}(\sigma) \Rightarrow \llbracket x > 0 \rrbracket_\mathcal{B}(\sigma)$$ so it is weaker than this alternative pre-condition.
Rather than prove it is weaker than all pre-conditions, we will see certain rules for computing the weakest pre-condition in a moment.   -->
Intuitively, the strong post-condition corresponds to the exact set of states that the statement may terminate in when executed in any state satisfy the pre-condition:
$$
  \mathsf{SPC}(S,\, q) = \{ \sigma \in \mathsf{State} \mid \text{if}\ \langle S,\, \sigma \rangle \rightarrow^* \sigma'\ \text{then}\ \llbracket q \rrbracket_\mathcal{B}(\sigma') \}
$$

In this sense, we can imagine the strong post-condition as a form of _symbolic_ execution.
That is, rather than executing the statement in a specific state, e.g. $[x \mapsto 2]$, we are executing it in a symbolic state, e.g. $x > 0$, that represents a set of states.
Unlike normal execution, symbolic execution requires us to consider all the different paths the program may go down and collect the results from each, which is why the if-then-else rule has a disjunction over the post-conditions of each branch.

#### Aside
Whilst the set of states corresponding to the strongest post-condition will always exist, it won't necessarily be describable by any given assertion language.
A common choice for assertion language is linear arithmetic, which does not permit quantifiers or multiplication, as it can be algorithmically checked.
However, there are strongest post-conditions that it cannot express.
Our extended Boolean arithmetic can express all strongest post-conditions.

<!-- This correspondence is only intuitive as not all sets of states can be described by an extended Boolean expression. -->
<!-- For example, you cannot describe the set of states $\sigma \in \mathsf{State}$ for which $\sigma(x)$ is prime just using an extended Boolean expression. -->
<!-- The strict definition of the strongest post-condition, however, only requires that it is the strongest extended Boolean expression that approximates this set. -->
<!-- When a set can not be precisely described by an extended Boolean expression, we can imagine an ever increase sequence of approximations; and so, in such case the strongest post-condition would not be defined anyway. -->

The importance of the strongest post-condition comes from when we are tasked with assessing the validity of a given Hoare triple, e.g. $$\{ \top \}\ S \{ x \geq 0 \}$$ where $$S$$ is the program we saw earlier.
If we can work out the strongest post-condition $\mathsf{SPC}(S,\, \top)$ then all we need to check is whether the specified post-condition is a implied by the strongest post-condition.
If so, then we can apply the consequence rule to get the desired triple, and if not, then we know the triple is invalid by the fact that the strongest post-condition would imply any other post-condition.

So, how are we going to derive the strongest post-condition for $$S$$ with the pre-condition $$\top$$?
Well it just so happens that the basic rules we defined for each language construct (other than while) calculate their strong post-condition directly - they are the most precise claims we can make.
Therefore, as long as we retain maximum generality when applying this rules, we can calculate the strongest pre-condition compositionally.

At the top-level, our statement is a sequence statement, and so we will apply the rule for this construct splitting our objective into two: we must the strongest post-condition $P$ such that $$\{ \top \}\ x \leftarrow y\ \{ P \}$$, then we can use this to determine where $$\{ P \}\ \mathsf{if}\ x < 0\ \mathsf{then}\ x \leftarrow 0 - x\ \mathsf{else}\ \mathsf{skip}\ \{ x \geq 0 \}$$ holds.
We can summarise this step by the equation:

$$
  \mathsf{SPC}(S_1;\; S_2, P) = 
    \mathsf{SPC}(S_2,\, \mathsf{SPC}(S_1, P))
$$

i.e. the strongest post-condition of a sequence $S_1;\; S_2$ for a pre-condition $p$ is the strongest post-condition for $S_2$ for the pre-condition that is itself the strong post-condition of $S_2$.
From the perspective of symbol execution, this amounts to executing $S_1$ in the symbolic state $p$, and then executing $S_2$ in the derived intermediate state.

As the first statement is the assignment $y \leftarrow x$, we can calculate that $p$ will be $\exists y'.\, \top[t'/x] \andop y = x$, which is just equivalent to $y = x$.
In general, we have that:

$$
  \mathsf{SPC}(x \leftarrow e, P) = \exists x'.\, P[x'/x] \andop x = e[x'/x]
$$

This gives us the strongest post-condition to the first statement that we can then use to find the strongest post-condition to the second statement $\mathsf{if}\ x < 0\ \mathsf{then}\ x \leftarrow 0 - x\ \mathsf{else}\ \mathsf{skip}$.
As this statement uses the if-then-else construct, whose rule is composed of the behaviour of each branch, we need to calculate the strongest post-condition for each branch given the pre-condition $y = x$ as well as the branch condition:

The first statement follows from the assignment rule:

$$
  \{ y = x \andop x < 0 \}\ x \leftarrow 0 - x\ \{ \exists x'.\, y = x' \andop x' < 0 \andop x = 0 - x' \}
$$

Here we assume $x < 0$ as it is only under this condition that the branch is reached.
The post-condition $\exists x'.\, y = x' \andop x' < 0 \andop x = 0 - x'$ is equivalent to $y < 0 \andop x = 0 - y$.

For the second branch, we use the skip rule and we may to assume that $!(x < 0)$ otherwise this branch isn't reached:

$$
  \{ y = x \andop !(x < 0) \}\ \mathsf{skip}\ \{ y = x \andop !(x < 0) \}
$$

In general, we have that:

$$
  \mathsf{SPC}(\mathsf{skip},\, P) = P
$$

Now we have the strongest post-condition for each branch, we can combine them using them using the if-then-else rule:

$$
  \{ y = x \}\ \mathsf{if}\ x < 0\ \mathsf{then}\ x \leftarrow 0 - x\ \mathsf{else}\ \mathsf{skip}\ \{ (y < 0 \andop x = 0 - y) \orop !(x < 0) \}
$$

Remember, as our Hoare triple must summarise the entire behaviour of its subject, the post-condition is derived from the disjunction of both branches.
We can summarise the strongest post-condition for the if-then-else construct as follows:

$$
  \mathsf{SPC}(\mathsf{if}\ e\ \mathsf{then}\ S_1\ \mathsf{else}\ S_2,\, P) = 
    \mathsf{SPC}(S_1,\, P \andop e) \orop \mathsf{SPC}(S_2,\, P \andop !e)
$$

Therefore, we have the strongest post-condition for the statement $S$: $(y < 0 \andop x = 0 - y) \orop {!(x < 0)}$.
All that remains is to check whether this can be weakened into the desired post-condition $x \geq 0$.
Indeed, we have that $x \geq 0$ when $y < 0 \andop x = 0 - y$ and, likewise, when $!(x < 0)$.
Therefore, the desired post-condition follows from the strongest post-condition and we can conclude that the Hoare triple is valid.

<!-- 
<!-- 


Now we have broken down the problem of finding the strongest post-condition of this compound statement into two smaller problems that requires us to find the weakest pre-conditions for sub-statements.
If we find the weakest pre-condition to the second statement we can deduce the best possible $$q$$, which then specifies a post-condition for our first statement that we can use to deduce the overall weakest-precondition.
We can summarise this process by the equation:

$$
  \mathsf{wp}(S_1;\; S_2, p) = 
    \mathsf{wp}(S_1,\, \mathsf{wp}(S_2, p))
$$

Let us turn our attention, therefore, to the second statement.
As this statement uses the if-then-else construct, we will apply the corresponding rule to again decompose this objective into two simpler objectives:

$$
  \{ x < 0 \andop q \}\ x \leftarrow 0 - x\ \{ x \geq 0 \}\ \text{and}\ \{ \mathop{!}(x < 0) \andop q \}\ \mathsf{skip}\ \{ x \geq 0 \}
$$ 

From which, we may infer $$\{ q \}\ \mathsf{if}\ x < 0\ \mathsf{then}\ x \leftarrow 0 - x\ \mathsf{else}\ \mathsf{skip}\ \{ x \geq 0 \}$$.

We shall proceed recursively, therefore, by finding the weakest pre-condition for each of these branches:

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

Now we have the weakest pre-conditions for each branch.
However, in order to fit this into the rule for the if-then-else construct, we have to rearrange these into the form $$x < 0 \andop q$$ and $$ \mathop{!}(x< 0) \andop q$$ for some $q$ respectively.
The general solution to this is to ensure that $q$ is equivalent to $$(\mathop{!}(x < 0) \orop 0 - x \geq 0) \andop (x < 0 \orop x \geq 0)$$ as we then have that $x < 0 \andop q$ is equivalent to $0 - x \geq 0$ and $\mathop{!}(x < 0) \andop q$ is equivalent to $x \geq 0$.
In general, we may characterise the weakest pre-condition for the if-then-else construct as:

$$
  \mathsf{wp}(\mathsf{if}\ e\ \mathsf{then}\ S_1\ \mathsf{else}\ S_2, p) = 
    (e \Rightarrow \mathsf{wp}(S_1, p)) \andop (\mathop{!}e \Rightarrow \mathsf{wp}(S_2, p))
$$

where $p \Rightarrow q$ is syntactic sugar for $\mathop{!}p \orop q$.

At this point, we have calculated the weakest pre-condition for the second statement in our sequence $$x \leftarrow y;\; \mathsf{if}\ x < 0\ \mathsf{then}\ x \leftarrow 0 - x\ \mathsf{else}\ \mathsf{skip}$$, and we can consider this to be the post-condition to $$x \leftarrow y$$.
As it is an assignment statement, we can see the weakest pre-condition for the compound expression is $$(\mathop{!}(y < 0) \orop 0 - y \geq 0) \andop (y < 0 \orop y \geq 0)$$.
Thus we have concluded that:

$$
  \begin{array}{c}
    \{ (\mathop{!}(y < 0) \orop 0 - y \geq 0) \andop (y < 0 \orop y \geq 0) \} \\
    x \leftarrow y;\; \mathsf{if}\ x < 0\ \mathsf{then}\ x \leftarrow 0 - x\ \mathsf{else}\ \mathsf{skip} \\
    \{ x \geq 0 \}
  \end{array}
$$

And, moreover, if there were any other pre-condition $p$ we would have that $$p \vDash (\mathop{!}(y < 0) \orop 0 - y \geq 0) \andop (y < 0 \orop y \geq 0)$$; that is, it is the weakest pre-condition.
As we wish to check if $$\top$$ is a valid pre-condition, we therefore need to ascertain whether $$\top \vDash (\mathop{!}(y < 0) \orop 0 - y \geq 0) \andop (y < 0 \orop y \geq 0)$$ or not.
Or more concretely, whether the Boolean formula $$(\mathop{!}(y < 0) \orop 0 - y \geq 0) \andop (y < 0 \orop y \geq 0)$$ is always true.

When $$y < 0$$ we have that $$0 - y \geq 0$$ and so both conjuncts are valid, and likewise when $$y > 0$$ we have that $$y \geq 0$$ and so the both conjuncts are again valid.
Therefore, we can conclude the original triple $$\{ \top \}\ \mathsf{if}\ x < 0\ \mathsf{then}\ x \leftarrow 0 - x\ \mathsf{else}\ \mathsf{skip}\ \{ x \geq 0 \}$$ is indeed valid using the consequence rule. -->

This may seem rather long-winded, but actually that is because we have been closely inspecting the various steps taken along the way.
We can summarize the process more compactly as follows:

To check whether $$\{ P \} S \{ Q \}$$ is valid:

  - Calculate $$\mathsf{SPC}(S,\, P)$$ using the equations laid-out above.

  - Determine whether $$\mathsf{SPC}(S,\, P) \subseteq Q$$.

For programs that don't involve the while statement, the strategy we have seen for checking whether a given triple holds is _complete_.
That is, if the triple holds, then are strategy will indeed be able to derive it from the rules we have outlined.
Moreover, if the strategy fails as the final implication does not hold, we know the triple does not hold.
The completeness here is derived precisely from the fact we consider the _strongest_ post-condition - any given post-condition must be weaker; or, if it is not weaker, then it is not a post-condition.

For programs involving while loops, the process breaks down as we cannot always computer the strongest post-condition directly.
Instead, we may have to _over-approximate_ it, i.e. compute a less general post-condition.
We can still use the process to check whether a triple holds, but if it doesn't work we don't know whether this is due to our over-approximation or not - it is sound, but incomplete.
But more on that next week!