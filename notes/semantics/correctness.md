---
layout: math
title: Partial Correctness (Hoare Logic)
nav_order: 4
mathjax: true
parent: Semantics
---

# Partial Correctness

Having a mathematical description of a programming language's semantics enables us to formally reason about the behaviour of programs.
However, it can be quite cumbersome doing so directly, and much of the field of programming languages is based on designing effective reasoning techniques.
Hoare logic is one such framework.

The sort of properties Hoare logic can be used for are _functional correctness_ properties; that is, whether a program computes an intended function or not.
More specifically, we will look at _partial correctness_ which states that when the program terminates, i.e. reaches a terminal configuration, it produces the correct result.
Partial correctness disentangles the input-output expectations from the termination condition.

A naive way to approach partial correctness would be to define a partial function on states that we expect our program to compute and then manually prove that the operational semantics aligns with this expectation.
The problem with this approach is that it quickly becomes unwieldy for complex programs which may have infinitely many distinct traces.
Instead, it is much easier to specify what is known as _pre-conditions_ and _post-conditions_ that describe sets of states before and after the execution of a given statement.
These conditions allow us to abstract away details of specific individual configurations and focus on what properties hold of the state initially (i.e. the pre-condition) and what holds after the program if it terminates (i.e. the post-condition).

<div class="defn" markdown="1">
  Given two subsets of states $$P,\, Q \subseteq \mathsf{State}$$ and a statement $$S \in \mathcal{S}$$, we write $$\{ P \}\ S\ \{ Q \}$$ to indicate that:

  $$
    \text{if}\ \sigma_0 \in P\ \text{and}\ \langle S,\, \sigma_0 \rangle \rightarrow^* \sigma_1\ \text{then}\ \sigma_1 \in Q
  $$

  for any states $$\sigma_0,\, \sigma_1 \in \mathsf{State}$$.

  This is referred to as a _Hoare triple_.
</div>

In other words, a Hoare triple, i.e. a pre- and post-condition for a given statement, consists of two conditions on states such that when the statement is executed in a state that satisfies the pre-condition, if it terminates, then the terminal state will also satisfy the post-condition.
If the statement doesn't terminate, however, then the Hoare triple says nothing about its behaviour and there is nothing to say that it will terminate.
In particular, a statement that never terminates will in fact satisfy _any_ Hoare triple.

As an example, we may assert that $$\{ x > 0 \}\ x \leftarrow x + 1\ \{ x > 1 \}$$ as, for any state in which $$x > 0$$, when we execute this statement, it will reach a state in which $$x > 1$$.
This gives us a way to summarise the behaviour of the program $$x \leftarrow x + 1$$ across infinitely many traces.

It is worth noting that Hoare triples are more flexible than specifying the exact input-output behaviour of a statement; rather, they describe a relation that subsumes the particular function.
For instance, the triple $$\{ x > 0 \}\ S\ \{ x > 1 \}$$ is satisfied by any statement that will increase $$x$$ during any terminating trace, but it doesn't just have to be $$x \leftarrow x + 1$$ and nor does it need to be limited to updating $$x$$.

## Assertion Languages
Within the above examples, we have been using Boolean expressions to implicitly represent sets of states, e.g. where $$x > 0$$ represents the set $$ \{ \sigma \in \mathsf{State} \mid \llbracket x > 0 \rrbracket_\mathcal{B}(\sigma) = \top \}$$.
In general, Hoare logic is defined in relation to a specific _assertion language_ that dictates what pre- and post-conditions are expressible.
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

We will use extended Boolean expressions interchangeably with sets of states.
You don't need to worry too much about the specifics of extended Boolean expressions; for the most part, they behave exactly like standard logical formulas and satisfy all the usual identities - feel free to use any logical expression within assertions.
The key thing to remember, however, is that _free variables_ within assertions, i.e. those that are not bound by a quantifier, refer to _program variables_ and thus depend on the current state, e.g. $$\exists y.\, x = (2 * y) + 1$$ corresponds to the set of states in which the current value of the variable $$x$$ is odd.

# Reasoning with Hoare Triples

<!-- The definition of a Hoare triple quantifies over all executions of a statement, so it is not something we want to check directly. -->
The advantage of working with Hoare triples is that they let us analyse the parts of a program separately and then combine what we learn about each part.
We will start with an example of this kind of reasoning, and then turn to a systematic method for doing it.

## Reasoning Compositionally

Suppose we are using a statement $$\mathsf{isPos}_{x,b}$$ (perhaps a library function whose code we cannot inspect) about which all we know is the following specification:

  - **IP1.** if $$\sigma(x) > 0$$, then $$\langle \mathsf{isPos}_{x,b},\, \sigma \rangle \rightarrow^* \sigma[b \mapsto 1]$$;
  - **IP2.** otherwise, if $$\sigma(x) \leq 0$$, then $$\langle \mathsf{isPos}_{x,b},\, \sigma \rangle \rightarrow^* \sigma[b \mapsto 0]$$.

The subscripts are just a label for the variables the statement is concerned with.
In this case, the specification is complete - these are the only terminal states the statement can reach (and it terminates in each case).

Our goal is to show that, for the program $$ \mathsf{isPos}_{x,b};\; x \leftarrow x - b$$, satisfies the Hoare triple:

$$
  \{ x \geq 0 \}\ \mathsf{isPos}_{x,b};\; x \leftarrow x - b\ \{ x \geq 0 \}
$$

We will do so by analysing each statement on its own, and then combining the results.

**Step 1: a triple from the specification.**
Let $$\sigma_0$$ be a state with $$\sigma_0(x) \geq 0$$.
There are two possibilities.
If $$\sigma_0(x) > 0$$, then case IP1 applies and $$\mathsf{isPos}_{x,b}$$ terminates in $$\sigma_0[b \mapsto 1]$$, in which $$x > 0$$ and $$b = 1$$.
Otherwise $$\sigma_0(x) = 0$$ so $$\sigma_0(x) \leq 0$$, case IP2 applies, and it terminates in $$\sigma_0[b \mapsto 0]$$, in which $$x = 0$$ and $$b = 0$$.
Either way, the terminal state satisfies the following post-condition, and so:

$$
  \{ x \geq 0 \}\ \mathsf{isPos}_{x,b}\ \{ (x > 0 \andop b = 1) \orop (x = 0 \andop b = 0) \}
$$

Notice that we could say something about $$x$$ in the post-condition only because the specification tells us that the statement leaves $$x$$ alone.

**Step 2: weakening the post-condition.**
The post-condition we found is more precise than we need.
In both disjuncts we have $$x \geq b$$: if $$x > 0$$ and $$b = 1$$ then $$x \geq 1 = b$$, and if $$x = 0$$ and $$b = 0$$ then $$x \geq 0 = b$$.
Every terminal state satisfying the more precise post-condition also satisfies $$x \geq b$$, and so we may conclude the weaker triple:

$$
  \{ x \geq 0 \}\ \mathsf{isPos}_{x,b}\ \{ x \geq b \}
$$

**Step 3: the assignment.**
Executing $$x \leftarrow x - b$$ in a state $$\sigma$$ terminates in $$\sigma[x \mapsto \sigma(x) - \sigma(b)]$$.
The new value of $$x$$ is non-negative exactly when the old value of $$x$$ is at least the value of $$b$$, so:

$$
  \{ x \geq b \}\ x \leftarrow x - b\ \{ x \geq 0 \}
$$

**Step 4: combining the triples.**
The post-condition of the first triple is the pre-condition of the second, so we can chain them.
To see why this works, suppose $$\sigma_0(x) \geq 0$$ and $$\langle \mathsf{isPos}_{x,b};\; x \leftarrow x - b,\, \sigma_0 \rangle \rightarrow^* \sigma_2$$.
The first statement must execute first, terminating in some state $$\sigma_1$$ such that $$\langle \mathsf{isPos}_{x,b},\, \sigma_0 \rangle \rightarrow^* \sigma_1$$, and so $$\sigma_1(x) \geq \sigma_1(b)$$ by the triple from Step 2.
The assignment then executes from $$\sigma_1$$ and terminates in $$\sigma_2$$, and so $$\sigma_2(x) \geq 0$$ by the triple from Step 3.
Therefore we have shown, as required:

$$
  \{ x \geq 0 \}\ \mathsf{isPos}_{x,b};\; x \leftarrow x - b\ \{ x \geq 0 \}
$$

Observe that we never looked inside $$\mathsf{isPos}_{x,b}$$: we only used a triple derived from its specification, and that triple can be reused in any other program that uses the statement.
This is the pattern that we will use again and again:

  - turn the specification of each statement into a Hoare triple, considering each case of the specification;
  - adjust the pre- and post-conditions, by weakening the post-condition or strengthening the pre-condition (this is always safe, as we will see in a moment), so that the triples line up;
  - chain the triples together according to the structure of the program.

The assertion in the middle, $$x \geq b$$, is the interface between the two statements.
Finding a good one took some insight - it was the one assertion that was both established by the first statement and sufficient for the second.
Our next step is to replace this insight with a calculation, by working systematically from the desired post-condition.

## Weakest Pre-condition

The reasoning above relied on our spotting the right assertion to put between each pair of statements.
To approach it more systematically, we will work _backwards_ from the post-condition.
Given a statement $$S$$ and a post-condition $$Q$$, which states are guaranteed to end up in $$Q$$, if the statement terminates?
Rather than reasoning about one particular state, e.g. $$[x \mapsto 2]$$, we reason about a whole set of states at once, which we describe with an assertion.
The calculation will be driven by the structure of the statement, with one equation for each language construct.

<div class="defn" markdown="1">
  For a given post-condition $$Q \subseteq \mathsf{State}$$ and a statement $$S \in \mathcal{S}$$, the __weakest pre-condition__ is the set of states:

  $$
    \mathsf{WP}(S,\, Q) = \{ \sigma_0 \in \mathsf{State} \mid \text{for all}\ \sigma_1,\ \text{if}\ \langle S,\, \sigma_0 \rangle \rightarrow^* \sigma_1\ \text{then}\ \sigma_1 \in Q \}
  $$

  That is, the set of all states from which every terminating execution of the statement ends in a state satisfying $$Q$$.
</div>

It is called the weakest pre-condition because it is the most general thing we can assume about the initial state: it is the largest set of states that is guaranteed to lead to $$Q$$, and so it asks the _least_ of the initial state.
(Strictly speaking, since we ignore termination, this is known as the weakest _liberal_ pre-condition.)
For example, $$\mathsf{WP}(x \leftarrow x + 1,\, x > 1)$$ is $$x > 0$$: executing the statement in a state with $$x > 0$$ gives $$x > 1$$, and executing it in any state with $$x \leq 0$$ gives a state where $$x \leq 1$$.

The reason this notion is useful is that it gives us an exact characterisation of Hoare triples.

<div class="defn" markdown="1">
  $$\{ P \}\ S\ \{ Q \}$$ holds if and only if $$P \subseteq \mathsf{WP}(S,\, Q)$$.
</div>

This is more or less just the definition of a Hoare triple unfolded.
Suppose $$\{ P \}\ S\ \{ Q \}$$ and let $$\sigma_0 \in P$$.
Then any terminating execution $$\langle S,\, \sigma_0 \rangle \rightarrow^* \sigma_1$$ has $$\sigma_1 \in Q$$ by the triple, and so $$\sigma_0 \in \mathsf{WP}(S,\, Q)$$ by definition.
Conversely, suppose $$P \subseteq \mathsf{WP}(S,\, Q)$$ and that $$\sigma_0 \in P$$ and $$\langle S,\, \sigma_0 \rangle \rightarrow^* \sigma_1$$.
Then $$\sigma_0 \in \mathsf{WP}(S,\, Q)$$, and so $$\sigma_1 \in Q$$ by definition.

So, to show a Hoare triple is valid, we calculate the weakest pre-condition of the statement for the desired post-condition and check that the given pre-condition is included in it.
Moreover, if the inclusion fails, then the triple is definitely invalid: any state in $$P$$ that is not in $$\mathsf{WP}(S,\, Q)$$ has, by definition, some terminating execution that ends outside $$Q$$.

The remaining question is how to calculate $$\mathsf{WP}(S,\, Q)$$ without enumerating executions.
We do so by considering each construct of the language in turn, and describing which states lead into $$Q$$ in terms of the operational semantics.

## Calculating the Weakest Pre-condition

### Skip
Executing $$\mathsf{skip}$$ doesn't change the state, so a state leads to $$Q$$ exactly when it is already in $$Q$$:

$$
  \mathsf{WP}(\mathsf{skip},\, Q) = Q
$$

### Assignment
Executing $$x \leftarrow e$$ in a state $$\sigma$$ terminates in the state $$\sigma[x \mapsto \llbracket e \rrbracket_\mathcal{A}(\sigma)]$$, so $$\sigma$$ leads to $$Q$$ precisely when this updated state is in $$Q$$:

$$
  \mathsf{WP}(x \leftarrow e,\, Q) = \{ \sigma \mid \sigma[x \mapsto \llbracket e \rrbracket_\mathcal{A}(\sigma)] \in Q \}
$$

This set has a neat description as an assertion: it is $$Q$$ with $$e$$ in place of $$x$$.

$$
  \mathsf{WP}(x \leftarrow e,\, Q) = Q[e/x]
$$

The notation $$Q[e/x]$$ refers to the assertion $$Q$$ in which every free occurrence of the variable $$x$$ has been replaced by the arithmetic expression $$e$$.
For instance, if $$Q$$ is $$x > 1$$, then $$Q[x + 1/x]$$ is $$x + 1 > 1$$.
(This is a syntactic operation on assertions, much like the substitution operations from the problem sheet.)

To see why this works, note that $$x$$ in $$Q$$ refers to the value of $$x$$ _after_ the assignment, and this is the value of $$e$$ in the state _before_ the assignment.
For example, with $$Q = \{ \sigma \mid \sigma(x) > 1 \}$$:

$$
  \begin{array}{rl}
    Q[x+1/x] &= \{ \sigma \mid \sigma[x \mapsto \llbracket x + 1 \rrbracket_\mathcal{A}(\sigma)] \in Q \} \\
             &= \{ \sigma \mid \sigma[x \mapsto \llbracket x + 1 \rrbracket_\mathcal{A}(\sigma)](x) > 1 \} \\
             &= \{ \sigma \mid \llbracket x + 1 \rrbracket_\mathcal{A}(\sigma) > 1 \} \\
             &= \{ \sigma \mid \sigma(x) + 1 > 1 \} \\
  \end{array}
$$

which is the set of states satisfying $$x + 1 > 1$$, i.e. $$x > 0$$.
Therefore $$\mathsf{WP}(x \leftarrow x + 1,\, x > 1)$$ is $$x > 0$$, and so, by the characterisation above, $$\{ x > 0 \}\ x \leftarrow x + 1\ \{ x > 1 \}$$.
Notice that if $$x$$ does not occur in $$Q$$ then $$Q[e/x]$$ is just $$Q$$; assigning to a variable we don't care about does not change anything we do care about.

### Sequence
Executing $$S_1;\; S_2$$ in some state executes $$S_1$$ until it terminates in some intermediate state, and then executes $$S_2$$ from that state.
For the whole sequence to end in $$Q$$, every such intermediate state must be one from which $$S_2$$ is guaranteed to end in $$Q$$, i.e. a state in $$\mathsf{WP}(S_2,\, Q)$$.
And the initial state must be one from which $$S_1$$ is guaranteed to end in that set.
Therefore:

$$
  \mathsf{WP}(S_1;\; S_2,\, Q) = \mathsf{WP}(S_1,\, \mathsf{WP}(S_2,\, Q))
$$

Notice that we work through the sequence _backwards_: we start with the final post-condition, push it back through $$S_2$$ to find what must hold between the two statements, and then push that back through $$S_1$$.
Notice also that we never have to consider the execution of the compound statement directly; we calculate it from the behaviour of each sub-statement.

### Conditional
Executing $$\mathsf{if}\ e\ \mathsf{then}\ S_1\ \mathsf{else}\ S_2$$ in a state $$\sigma$$ executes $$S_1$$ from $$\sigma$$ if $$\llbracket e \rrbracket_\mathcal{B}(\sigma) = \top$$ and executes $$S_2$$ from $$\sigma$$ otherwise.
So a state satisfying $$e$$ leads to $$Q$$ exactly when it is in $$\mathsf{WP}(S_1,\, Q)$$, and a state satisfying $$\mathop{!}e$$ leads to $$Q$$ exactly when it is in $$\mathsf{WP}(S_2,\, Q)$$:

$$
  \mathsf{WP}(\mathsf{if}\ e\ \mathsf{then}\ S_1\ \mathsf{else}\ S_2,\, Q) =
    (e \Rightarrow \mathsf{WP}(S_1,\, Q)) \andop (\mathop{!}e \Rightarrow \mathsf{WP}(S_2,\, Q))
$$

Each implication only constrains the states that actually take that branch: if $$e$$ is false, the first conjunct is trivially satisfied, and so only the second conjunct matters, and vice versa.
(Equivalently, we could write $$(e \andop \mathsf{WP}(S_1,\, Q)) \orop (\mathop{!}e \andop \mathsf{WP}(S_2,\, Q))$$, which describes the same set of states.)

#### Aside
Whilst the set of states corresponding to the weakest pre-condition will always exist, it won't necessarily be describable by any given assertion language.
A common choice for assertion language is linear arithmetic, which does not permit quantifiers or multiplication, as it can be algorithmically checked.
Notice that the equations above for skip, assignment, sequence and conditional only ever use substitution and the Boolean connectives, and so they stay inside linear arithmetic - no quantifiers are needed - which is one reason why weakest pre-conditions are popular in practice.
It is only the while loop, with its infinite intersection, that may take us outside of any given assertion language.

## Statements Specified by Their Behaviour

As in the $$\mathsf{isPos}_{x,b}$$ example at the start of this section, we are sometimes given not the code of a sub-statement but only a specification of its behaviour in terms of the operational semantics.
We now see how to handle this using weakest pre-conditions.
For example, let $$\mathsf{sign}_{x,s}$$ be some statement such that:

  - **SG1.** if $$\sigma(x) < 0$$, then $$\langle \mathsf{sign}_{x,s},\, \sigma \rangle \rightarrow^* \sigma[s \mapsto -1]$$;
  - **SG2.** otherwise, if $$\sigma(x) \geq 0$$, then $$\langle \mathsf{sign}_{x,s},\, \sigma \rangle \rightarrow^* \sigma[s \mapsto 1]$$.

The subscripts are just a label for the variables the statement is concerned with.
We take such a specification to be _complete_: when we say the behaviour is determined by these cases, we mean that these are the only terminal states reachable (and that the statement terminates in each case).
Notice that $$\sigma[s \mapsto -1]$$ also tells us what the statement _doesn't_ do: every variable other than $$s$$ keeps its value.
This is just as important as the change itself.

### Executing with a specified statement
To run a program containing such a statement, we treat the specification as a (big-step) execution rule for it.
For example, starting from $$[x \mapsto -3,\, s \mapsto 7]$$, the program $$\mathsf{sign}_{x,s};\; x \leftarrow x + s$$ proceeds as follows:

  1. As $$[x \mapsto -3,\, s \mapsto 7](x) = -3 < 0$$, case SG1 applies and $$\mathsf{sign}_{x,s}$$ terminates in $$[x \mapsto -3,\, s \mapsto -1]$$.
  2. The assignment then updates $$x$$ to $$-3 + (-1) = -4$$, so the final state is $$[x \mapsto -4,\, s \mapsto -1]$$.

When justifying such a step, name the case of the specification that you are using and show that its condition holds in the _current_ state, i.e. the state at the point the statement is executed, which may differ from the initial state of the whole program.
Remember also to carry over every variable the statement doesn't mention.

### Weakest pre-condition of a specified statement
Each case of a specification describes what looks exactly like an assignment applied to those states that satisfy the case's condition.
So, for a state to lead to $$Q$$, it must satisfy, for each case, that _if_ the case's condition holds, _then_ the updated state is in $$Q$$:

$$
  \mathsf{WP}(\mathsf{sign}_{x,s},\, Q) = (x < 0 \Rightarrow Q[-1/s]) \andop (x \geq 0 \Rightarrow Q[1/s])
$$

This is exactly the weakest pre-condition of $$\mathsf{if}\ x < 0\ \mathsf{then}\ s \leftarrow -1\ \mathsf{else}\ s \leftarrow 1$$, and in general we get one conjunct per case of the specification.
The conditions of the specification describe the state _before_ the statement runs, which is just what the pre-condition describes, so they can be used as they are; we don't need to rename any variables, even if a condition mentions the variable that is being updated.
The completeness of the specification is what makes this valid: it tells us that these cases are all the behaviours we need to account for.

As a worked example, let us check the triple

$$
  \{ \top \}\ \mathsf{sign}_{x,s};\; x \leftarrow x + s\ \{ (x < 0 \andop s = -1) \orop (x > 0 \andop s = 1) \}
$$

which says that $$s$$ records the sign of $$x$$ at the end.
Let $$Q$$ be the post-condition.
We work backwards through the sequence, starting with the second statement:

$$
  \mathsf{WP}(x \leftarrow x + s,\, Q) = (x + s < 0 \andop s = -1) \orop (x + s > 0 \andop s = 1)
$$

Call this assertion $$R$$.
Now we push $$R$$ back through the first statement.
Substituting $$s = -1$$ in $$R$$ gives $$(x - 1 < 0 \andop -1 = -1) \orop (x - 1 > 0 \andop -1 = 1)$$ which simplifies to $$x < 1$$, and substituting $$s = 1$$ gives $$(x + 1 < 0 \andop 1 = -1) \orop (x + 1 > 0 \andop 1 = 1)$$ which simplifies to $$x > -1$$.
Hence:

$$
  \mathsf{WP}(\mathsf{sign}_{x,s};\; x \leftarrow x + s,\, Q) = (x < 0 \Rightarrow x < 1) \andop (x \geq 0 \Rightarrow x > -1)
$$

Both conjuncts are true of every state, so the weakest pre-condition is equivalent to $$\top$$.
Therefore $$\top \subseteq \mathsf{WP}(\mathsf{sign}_{x,s};\; x \leftarrow x + s,\, Q)$$ and the triple is valid.

## Strengthening and Weakening

Not every triple we care about has the weakest pre-condition as its pre-condition.
In practice we are usually given a specification $$\{ P \}\ S\ \{ Q \}$$ where $$P$$ is simply something we are willing to assume; it may be much more restrictive than $$\mathsf{WP}(S,\, Q)$$.
That the triple is still valid whenever $$P \subseteq \mathsf{WP}(S,\, Q)$$ is the sense in which a pre-condition can be _strengthened_: if a triple holds with pre-condition $$P_1$$, then it holds with any $$P_2$$ such that $$P_2 \subseteq P_1$$.

Similarly, the weakest pre-condition is _monotonic_ in the post-condition: if $$Q_1 \subseteq Q_2$$ then $$\mathsf{WP}(S,\, Q_1) \subseteq \mathsf{WP}(S,\, Q_2)$$, as a state that is guaranteed to end up in $$Q_1$$ is also guaranteed to end up in the larger set $$Q_2$$.
Hence a triple that holds for the post-condition $$Q_1$$ also holds for any more general post-condition $$Q_2 \supseteq Q_1$$.

Putting these together, if $$\{ P_1 \}\ S\ \{ Q_1 \}$$ holds, $$P_2 \subseteq P_1$$ and $$Q_1 \subseteq Q_2$$, then $$\{ P_2 \}\ S\ \{ Q_2 \}$$ also holds:

$$
  P_2 \subseteq P_1 \subseteq \mathsf{WP}(S,\, Q_1) \subseteq \mathsf{WP}(S,\, Q_2)
$$

What we mean by a "weaker" assertion is one that is more general, i.e. is true of more states.
This may sound contradictory, but it is weaker in the sense that it tells us less about the initial or terminal state.

A Hoare triple $$\{ P_1 \}\ S\ \{ Q_1 \}$$ is _stronger_ than another triple $$\{ P_2 \}\ S\ \{ Q_2 \}$$ if it gives us more information about the behaviour of $$S$$, i.e. if $$P_2 \subseteq P_1$$ and $$Q_1 \subseteq Q_2$$.
For example, the triple $$\{ \top \}\ S\ \{ x > 1 \}$$ is stronger than $$\{ x > 0 \}\ S\ \{ x \geq 0 \}$$ as it (a) assumes less about the initial state, i.e. has a weaker pre-condition and thus applies in more scenarios, and (b) tells us more about the possible terminal states, i.e. has a stronger post-condition.
Notice that the direction of inclusion changes between the pre- and post-condition: a weaker pre-condition makes for a stronger triple as it covers a larger set of initial states, and a stronger post-condition makes for a stronger triple as it describes a more specific property of the terminal states.

### Composing triples
Being able to weaken a triple is useful when combining the facts we know about different statements.
Suppose we know that $$\{ x > 0 \}\ x \leftarrow x + 1\ \{ x > 1 \}$$ and that $$\{ x \geq 0 \}\ y \leftarrow x\ \{ y \geq 0 \}$$ and we wish to derive a triple for $$x \leftarrow x + 1;\; y \leftarrow x$$.
The post-condition of the first statement doesn't line up with the pre-condition of the second, but as $$x > 1 \subseteq x \geq 0$$ we can weaken the first triple to $$\{ x > 0 \}\ x \leftarrow x + 1\ \{ x \geq 0 \}$$.

In general, if $$\{ P \}\ S_1\ \{ Q \}$$ and $$\{ Q \}\ S_2\ \{ R \}$$, then $$\{ P \}\ S_1;\; S_2\ \{ R \}$$, and we can see this directly from the equations above:

$$
  P \subseteq \mathsf{WP}(S_1,\, Q) \subseteq \mathsf{WP}(S_1,\, \mathsf{WP}(S_2,\, R)) = \mathsf{WP}(S_1;\; S_2,\, R)
$$

where the second inclusion is by monotonicity, using $$Q \subseteq \mathsf{WP}(S_2,\, R)$$.
Applied to our example, we conclude $$\{ x > 0 \}\ x \leftarrow x + 1;\; y \leftarrow x\ \{ y \geq 0 \}$$.

Reasoning this way means we can analyse individual parts of the program separately, without regard for the context in which they appear, and then later combine these summaries - a very useful feature to have when programs and their dependencies reach across hundreds of files, not all of which are available!
In particular, we can use a triple for a statement we cannot inspect (e.g. an external function) in the same way, after weakening or strengthening it to fit our needs.

## Example: Checking a Triple

Let us return to the question of how to determine whether a triple is valid.
For instance, given the triple:

$$
  \{ \top \}\ y \leftarrow x;\ \mathsf{if}\ x < 0\ \mathsf{then}\ x \leftarrow 0 - x\ \mathsf{else}\ \mathsf{skip}\ \{ x \geq 0 \}
$$

does this program ensure that $$x$$ is non-negative after execution?
Let $$S$$ denote the program.
We calculate $$\mathsf{WP}(S,\, x \geq 0)$$ following the equations above, working backwards from the post-condition.

**Sequence.** $$S$$ is a sequence, so we first calculate the weakest pre-condition of the conditional for $$x \geq 0$$, and then use this as the post-condition of $$y \leftarrow x$$.

**Conditional.** As the second statement uses the if-then-else construct, we calculate the weakest pre-condition of each branch for the post-condition $$x \geq 0$$.
For the first branch, by the equation for assignment:

$$
  \mathsf{WP}(x \leftarrow 0 - x,\, x \geq 0) = 0 - x \geq 0
$$

and for the second branch, by the equation for skip:

$$
  \mathsf{WP}(\mathsf{skip},\, x \geq 0) = x \geq 0
$$

Combining them, we obtain:

$$
  \mathsf{WP}(\mathsf{if}\ x < 0\ \mathsf{then}\ x \leftarrow 0 - x\ \mathsf{else}\ \mathsf{skip},\, x \geq 0) = (x < 0 \Rightarrow 0 - x \geq 0) \andop (\mathop{!}(x < 0) \Rightarrow x \geq 0)
$$

**First statement.** We now push this assertion back through $$y \leftarrow x$$.
Since $$y$$ doesn't occur in the assertion, the substitution has no effect and so:

$$
  \mathsf{WP}(S,\, x \geq 0) = (x < 0 \Rightarrow 0 - x \geq 0) \andop (\mathop{!}(x < 0) \Rightarrow x \geq 0)
$$

**Check.** All that remains is to check whether $$\top \subseteq \mathsf{WP}(S,\, x \geq 0)$$, i.e. whether this assertion is satisfied by every state.
In the first conjunct, if $$x < 0$$ then $$0 - x > 0$$, and so $$0 - x \geq 0$$.
In the second, if $$\mathop{!}(x < 0)$$ then $$x \geq 0$$ by definition.
Therefore both conjuncts hold in every state, and the Hoare triple is valid.

Had we instead asked for the post-condition $$x > 0$$, the same calculation would give $$(x < 0 \Rightarrow 0 - x > 0) \andop (\mathop{!}(x < 0) \Rightarrow x > 0)$$, which is equivalent to $$x \neq 0$$.
The check $$\top \subseteq x \neq 0$$ fails: the state $$[x \mapsto 0,\, y \mapsto 0]$$ is not in the weakest pre-condition, and indeed it terminates with $$x = 0$$.
As the weakest pre-condition describes exactly the states that are guaranteed to end up in the post-condition, this tells us the triple is genuinely invalid, and moreover any state in $$P$$ outside the weakest pre-condition gives us a counterexample.

## Checking Inclusions

The final step of the method, showing $$P \subseteq \mathsf{WP}(S,\, Q)$$, is a statement about sets of states, so it is worth being systematic about it.
The usual approach is to take an arbitrary state that satisfies the left-hand side and argue that it satisfies the right-hand side.
Some tips:

  - **Handle implications by assuming the antecedent.** To show a state satisfies $$A \Rightarrow B$$, suppose it satisfies $$A$$ and show that it satisfies $$B$$, as we did above.
    Since weakest pre-conditions of conditionals and specified statements are conjunctions of implications, you will use this often.
    Show each conjunct separately.
  - **Split into cases.** For a disjunction on the left, show that each disjunct implies the right-hand side separately.
    For a disjunction on the right, it suffices to show that the state satisfies one of the disjuncts - choosing which one will usually be guided by the left-hand side.
  - **Use the arithmetic.** Use facts about the integers, e.g. $$x < 0$$ implies $$0 - x > 0$$, or that for any $$d$$ and $$n$$ either $$d \mid n$$ or not $$d \mid n$$.
    Here $$d \mid n$$ is shorthand for the assertion $$\exists k.\, n = k * d$$, and so it is itself an extended Boolean expression.
    If it appears as an assumption, you may pick a witness $$k$$ and use it; if it appears in a goal, you must supply a witness.
  - **Let substitution do the bookkeeping.** Substitution only changes the variable being assigned, so anything the pre-condition says about other variables is unaffected, and the pre-condition's facts about them can be used directly once the substitution is performed.
  - **Simplify as you go.** Substituting a known value, e.g. $$s = 1$$, will often make a disjunct trivially false, as in the example above, and this can shrink the assertion considerably.

Finally, recall that a triple is _not_ established by exhibiting a single execution; to show a triple is valid, argue about all states in the pre-condition, but to show it is invalid, a single state in the pre-condition that terminates outside the post-condition suffices.

# Summary

To check whether $$\{ P \}\ S\ \{ Q \}$$ is valid:

  - Calculate $$\mathsf{WP}(S,\, Q)$$ using the equations laid out above.

  - Determine whether $$P \subseteq \mathsf{WP}(S,\, Q)$$.

For programs that don't involve the while statement, this strategy is _complete_.
That is, if the triple holds, then our strategy will indeed show it, and if the final inclusion fails, we know the triple does not hold.
The completeness here is derived precisely from the fact we consider the _weakest_ pre-condition - any valid pre-condition must be included in it; or, if it is not, then it is not a pre-condition.

For programs involving while loops, the process breaks down as we cannot always compute the weakest pre-condition directly, since it requires an infinite intersection.
Instead, we may have to _under-approximate_ it, i.e. compute a stronger, more restrictive, pre-condition.
We can still use the process to check whether a triple holds, but if it doesn't work we don't know whether this is due to our under-approximation or not - it is sound, but incomplete.
But more on that next week!