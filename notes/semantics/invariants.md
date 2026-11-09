---
layout: math
title: Invariants
nav_order: 6
mathjax: true
parent: Semantics
---

# Loop Invariants

Last time we introduced Hoare triples, and saw how to calculate the weakest pre-condition for code without while loops.
Today, we will look at while loops.

As while loops may iterate an arbitrary number of times, we cannot directly calculate their weakest pre-condition.
Intuitively, they require some kind of inductive reasoning, much like a proof with infinitely many cases.

Instead of computing $$\mathsf{WP}(\mathsf{while}\ e\ \{ S \},\, Q)$$ itself, we will find a more set of states that we can show is _contained_ within it.
Then any pre-condition $$P \subseteq I$$ is also contained within it, and so $$\{ P \}\ \mathsf{while}\ e\ \{ S \}\ \{ Q \}$$ will hold.
The set $$I$$ is called a loop _invariant_.

## Invariants

<div class="defn" markdown="1">
  An assertion $$I$$ is a __loop invariant__ for $$\mathsf{while}\ e \{ S \}$$ if

  $$
    \{ e \andop I \}\ S\ \{ I \}
  $$

  or, equivalently, if $$I \andop e \subseteq \mathsf{WP}(S,\, I)$$.
</div>

In other words, when the body of the loop is executed in a state that satisfies the branch condition $$e$$ and this other property $$I$$, then any terminal state will satisfy $$I$$ as well.
So $$I$$ is a property that is _preserved_ across each iteration of the loop: if it is true before, then it is true after.
As a result, no matter how many times the loop is executed, we still know that it will hold after the loop has terminated (if indeed it terminates), and we also know that $$\mathop{!}e$$ must be satisfied by the terminal state (otherwise the loop would carry on).
We can use this to show that the triple holds:

$$
  \{ I \}\ \mathsf{while}\ e\ \{ S \}\ \{ \mathop{!}e \andop I \}
$$

for any choice of invariant $$I$$.
<!-- 
To see why this works, we show by induction on $$n$$ that $$I \subseteq W_n$$ for every $$n$$, with $$W_n$$ as above.

  - For $$n = 0$$, let $$\sigma \in I$$. We must show that $$\sigma$$ satisfies $$\mathop{!}e \Rightarrow Q$$. If $$\sigma$$ satisfies $$\mathop{!}e$$, then it satisfies $$I \andop \mathop{!}e$$ and hence $$Q$$ by assumption.

  - For $$n + 1$$, suppose $$I \subseteq W_n$$ and let $$\sigma \in I$$. We must show that $$\sigma$$ satisfies $$e \Rightarrow \mathsf{WP}(S,\, W_n)$$. If $$\sigma$$ satisfies $$e$$, then it satisfies $$I \andop e$$, and so it is in $$\mathsf{WP}(S,\, I)$$ because $$I$$ is a loop invariant. As $$I \subseteq W_n$$, monotonicity of the weakest pre-condition (in the post-condition) gives $$\mathsf{WP}(S,\, I) \subseteq \mathsf{WP}(S,\, W_n)$$, and so $$\sigma \in \mathsf{WP}(S,\, W_n)$$.

Therefore, $$I \subseteq \bigcap_n W_n = \mathsf{WP}(\mathsf{while}\ e\ \{ S \},\, Q)$$.
Taking $$Q$$ to be $$\mathop{!}e \andop I$$ gives the form you may see in textbooks: if $$\{ e \andop I \}\ S\ \{ I \}$$ then $$\{ I \}\ \mathsf{while}\ e\ \{ S \}\ \{ \mathop{!}e \andop I \}$$. -->


An invariant $$I$$ is an _under-approximation_ of the weakest pre-condition - a poor choice of $$I$$ may be too small to contain the pre-condition we want.
For example, $$\mathsf{false}$$ is always a loop invariant satisfying the conditions above, but it contains no states at all!

## A Simple Example

Let's look at an example:

$$
  \begin{array}{l}
    \mathsf{while}\ x < y\ \{ \\
    \quad x \leftarrow x + 1 \\
    \}
  \end{array}
$$

This rather straightforward program, which we will call $$S_1$$, increments $$x$$ until it reaches $$y$$.
We already know that $$x \geq y$$ when the loop exits, but suppose we wish to know that, if $$x \leq y$$ to begin with, then it will still be after.
In other words, we want to prove the Hoare triple:

$$
  \{ x \leq y \}\ S_1\ \{ x \leq y \}
$$

We choose $$x \leq y$$ as our candidate loop invariant $$I$$ as it is the pre-condition - it is precisely what we know before, so let's see if it forms a loop invariant.

To verify that it is a loop invariant, we calculate the weakest pre-condition of the loop body for the post-condition $$I$$:

$$
  \mathsf{WP}(x \leftarrow x + 1,\, x \leq y) = x + 1 \leq y
$$

We then need to check that $$I \andop e \subseteq x + 1 \leq y$$, i.e. that $$x \leq y \andop x < y$$ implies $$x + 1 \leq y$$.
This holds as we are working with integers.
Therefore, $$x \leq y$$ is a loop invariant - it is preserved under execution of the loop.

Finally, $$I \andop \mathop{!}(x < y)$$ is equivalent to $$x \leq y \andop x \geq y$$ which certainly implies the post-condition $$x \leq y$$.
Therefore, $$x \leq y \subseteq \mathsf{WP}(S_1,\, x \leq y)$$ and the triple holds.
That is, if the loop terminates, then we will have $$x \leq y$$ after.

## The Sweet Spot

In the above example, the loop invariant required was chosen to be the pre-condition.
We can also see that it was forced upon us by the post-condition - we wanted some invariant $$I$$ such that $$I \andop \mathop{!}(x < y)$$ would imply $$x \leq y$$ for which $$x \leq y$$ is the most general solution.

However, it isn't always the case that the loop invariant can be determined by the pre- or post-condition.
More often than not finding the right loop invariant will take some tinkering, much like finding the right induction hypothesis.

Consider the following program $$S_2$$:

$$
  \begin{array}{l}
    sum \leftarrow 0;\; \\
    i \leftarrow 0;\; \\
    \mathsf{while}\ i < n\ \{ \\
    \quad sum \leftarrow sum + i;\; \\
    \quad i \leftarrow i + 1 \\
    \}
  \end{array}
$$

This program will sum all the numbers from $$0$$ up to $$n - 1$$ (assuming $$n$$ is non-negative), which is equal to $$n * (n - 1) / 2$$.
Ultimately, we want to show that this program $$S_2$$ satisfies the Hoare triple:

$$
  \{ n \geq 0 \}\ S_2\ \{ 2 * sum = n * (n - 1) \}
$$

Let $$L$$ denote the while loop.
We want to show that $$n \geq 0 \subseteq \mathsf{WP}(sum \leftarrow 0;\; i \leftarrow 0;\; L,\, 2 * sum = n * (n - 1))$$.
Working backwards, this is $$\mathsf{WP}(sum \leftarrow 0;\; i \leftarrow 0,\, \mathsf{WP}(L,\, 2 * sum = n * (n - 1)))$$ but we cannot compute the weakest pre-condition of the loop.
Instead, we will look for a loop invariant $$I$$ that satisfies the following conditions:

  1. $$n \geq 0 \subseteq \mathsf{WP}(sum \leftarrow 0;\; i \leftarrow 0,\, I)$$; the invariant holds once the initialisation has finished.
  2. $$I \andop i < n \subseteq \mathsf{WP}(sum \leftarrow sum + i;\; i \leftarrow i + 1,\, I)$$; the invariant is preserved by the loop body.
  3. $$I \andop \mathop{!}(i < n) \subseteq (2 * sum = n * (n - 1))$$; the invariant implies the post-condition once the loop is finished.

<!-- If they hold, then $$I \subseteq \mathsf{WP}(L,\, 2 * sum = n * (n - 1))$$ and, by monotonicity, $$\mathsf{WP}(sum \leftarrow 0;\; i \leftarrow 0,\, I) \subseteq \mathsf{WP}(sum \leftarrow 0;\; i \leftarrow 0,\, \mathsf{WP}(L,\, 2 * sum = n * (n - 1)))$$ as required. -->

**First attempt.**
Perhaps the post-condition is a loop invariant, i.e. $$I = (2 * sum = n * (n - 1))$$.
To check that it is preserved, we calculate the weakest pre-condition of the loop body, working backwards:

$$
  \begin{array}{rl}
    \mathsf{WP}(i \leftarrow i + 1,\, I) &= 2 * sum = n * (n - 1) \\
    \mathsf{WP}(sum \leftarrow sum + i,\, 2 * sum = n * (n - 1)) &= 2 * (sum + i) = n * (n - 1)
  \end{array}
$$

But $$i < n \andop 2 * sum = n * (n - 1)$$ does not imply $$2 * (sum + i) = n * (n - 1)$$ as this requires $$i = 0$$.
For example, the state $$[n \mapsto 2,\, sum \mapsto 1,\, i \mapsto 1]$$ satisfies $$I \andop i < n$$ but, after one iteration, it reaches a state with $$sum = 2$$ which no longer satisfies $$I$$.
This isn't that surprising: it will not be the case that $$2 * sum = n * (n - 1)$$ _throughout_ execution, just at the end.

**Second attempt.**
Another tactic might be to try the information we have after the initialisation, i.e. $$I = (n \geq 0 \andop sum = 0 \andop i = 0)$$.
This satisfies the first condition, but the weakest pre-condition of the loop body for this assertion is

$$
  n \geq 0 \andop sum + i = 0 \andop i + 1 = 0
$$

which is not satisfied by, say, $$[n \mapsto 1,\, sum \mapsto 0,\, i \mapsto 0]$$, which does satisfy $$I \andop i < n$$.
In hindsight, this isn't that surprising either: the loop is going to increase $$i$$ and, after the first iteration, $$sum$$ so they won't always be $$0$$.

**Third attempt.**
What _should_ be the case is that $$2 * sum = i * (i - 1)$$ as on each iteration $$sum$$ is the summation of the numbers up to $$i - 1$$.
So let's see if $$I = (2 * sum = i * (i - 1))$$ is an invariant.
The weakest pre-condition of the loop body is

$$
  2 * (sum + i) = (i + 1) * ((i + 1) - 1)
$$

If $$2 * sum = i * (i - 1)$$ then $$2 * (sum + i) = i * (i - 1) + 2 * i = i * i + i = (i + 1) * i$$ as required.
Therefore, we have a loop invariant!

But there's a snag...
When the loop exits, we know that $$i \geq n$$ and $$2 * sum = i * (i - 1)$$ but this doesn't imply that $$2 * sum = n * (n - 1)$$.
In particular, we know that $$i \geq n$$ will hold after the loop, but not that it is equal to $$n$$; for example, the state $$[n \mapsto 2,\, i \mapsto 3,\, sum \mapsto 3]$$ satisfies $$I \andop \mathop{!}(i < n)$$ but not the post-condition.
This leads us to the final invariant $$I = (2 * sum = i * (i - 1) \andop i \leq n)$$.

**Final invariant.**
Let us check each of the three conditions for $$I = (2 * sum = i * (i - 1) \andop i \leq n)$$.

  1. We have that $$\mathsf{WP}(sum \leftarrow 0;\; i \leftarrow 0,\, I)$$ is $$2 * 0 = 0 * (0 - 1) \andop 0 \leq n$$, which is equivalent to $$0 \leq n$$.
     So $$n \geq 0 \subseteq \mathsf{WP}(sum \leftarrow 0;\; i \leftarrow 0,\, I)$$ as required.
  2. The weakest pre-condition of the loop body under the invariant is $$2 * (sum + i) = (i + 1) * i \andop i + 1 \leq n$$.
     Suppose $$2 * sum = i * (i - 1) \andop i \leq n \andop i < n$$. The first conjunct follows as above, and $$i + 1 \leq n$$ follows from $$i < n$$ as we are working with integers.
  3. If $$I \andop \mathop{!}(i < n)$$, then $$i \leq n$$ and $$i \geq n$$, so $$i = n$$ and thus $$2 * sum = n * (n - 1)$$ as required.

Therefore, we can conclude the program has the correct behaviour and indeed will compute the sum of the first $$n$$ numbers when $$n$$ is initially greater than or equal to $$0$$.

## Loops with Specified Statements

The body of a loop can also contain statements that we only know by their specification.
Recall the statement $$\mathsf{isPos}_{x,b}$$ from last time:

  - **IP1.** if $$\sigma(x) > 0$$, then $$\langle \mathsf{isPos}_{x,b},\, \sigma \rangle \rightarrow^* \sigma[b \mapsto 1]$$;
  - **IP2.** otherwise, if $$\sigma(x) \leq 0$$, then $$\langle \mathsf{isPos}_{x,b},\, \sigma \rangle \rightarrow^* \sigma[b \mapsto 0]$$.

and recall that its weakest pre-condition is $$\mathsf{WP}(\mathsf{isPos}_{x,b},\, Q) = (x > 0 \Rightarrow Q[1/b]) \andop (x \leq 0 \Rightarrow Q[0/b])$$.

Consider the following program $$S_3$$, which counts $$x$$ down to $$0$$, only decrementing it when it is positive, and uses $$c$$ to count the number of steps:

$$
  \begin{array}{l}
    c \leftarrow 0;\; \\
    \mathsf{while}\ \mathop{!}(x = 0)\ \{ \\
    \quad \mathsf{isPos}_{x,b};\; \\
    \quad x \leftarrow x - b;\; \\
    \quad c \leftarrow c + 1 \\
    \}
  \end{array}
$$

We would like to show that $$\{ x = n \andop n \geq 0 \}\ S_3\ \{ c = n \}$$.
Counting down from $$x$$ should take $$n$$ steps, and the loop should only exit when $$x = 0$$.
This tells us that $$c + x = n$$ throughout the loop, and, as $$\mathsf{isPos}$$ would behave in a different way if $$x$$ were negative (and the loop would never reach $$0$$), we will also need to know that $$x \geq 0$$.
So, let us try the invariant $$I = (x \geq 0 \andop c + x = n)$$.

  1. The initialisation is $$c \leftarrow 0$$, and $$\mathsf{WP}(c \leftarrow 0,\, I) = x \geq 0 \andop 0 + x = n$$. This is implied by $$x = n \andop n \geq 0$$ as required.

  2. We work backwards through the loop body.

     $$
       \begin{array}{rl}
         \mathsf{WP}(c \leftarrow c + 1,\, I) &= x \geq 0 \andop (c + 1) + x = n \\
         \mathsf{WP}(x \leftarrow x - b,\, \cdots) &= x - b \geq 0 \andop (c + 1) + (x - b) = n \\
         \mathsf{WP}(\mathsf{isPos}_{x,b},\, \cdots) &= (x > 0 \Rightarrow x - 1 \geq 0 \andop (c + 1) + (x - 1) = n) \\
           & \quad \andop (x \leq 0 \Rightarrow x - 0 \geq 0 \andop (c + 1) + (x - 0) = n)
       \end{array}
     $$

     where the final step substitutes $$1$$ and $$0$$ for $$b$$ respectively.
     Now suppose $$x \geq 0 \andop c + x = n \andop \mathop{!}(x = 0)$$.
     Then $$x > 0$$, so the second conjunct holds vacuously as its antecedent $$x \leq 0$$ is false.
     For the first, we have $$x - 1 \geq 0$$ because $$x > 0$$ and we are working with integers, and $$(c + 1) + (x - 1) = c + x = n$$.
     So the invariant is preserved.

  3. If $$I \andop x = 0$$ then $$c + 0 = n$$ and so $$c = n$$ as required.

Notice that, in step 2, the branch condition told us which case of the specification applies.
Also notice how the substitutions bring in the _new_ value of $$b$$ - the only variable that the specification says $$\mathsf{isPos}$$ changes - while everything else is left alone.

## Checking a Loop

Even in these simple cases, verifying programs with loops can become surprisingly challenging!
In fact, the construction of a loop invariant is an _undecidable_ problem, i.e. it cannot be automated and there is no set method for constructing a loop invariant that will always work.
You will see more about undecidable problems in the next section of the unit.

Typically, identifying a loop invariant involves some insight into the behaviour of the program - you need to consider how you expect the program to behave.
That said, we can characterise the requirements of a loop invariant in the form of logical constraints.

To ensure that

$$
  \begin{array}{l}
    \{ P \} \\
    S_0;\; \\
    \mathsf{while}\ e\ \{ S \} \\
    \{ Q \}
  \end{array}
$$

holds, where $$S_0$$ is whatever precedes the loop (possibly $$\mathsf{skip}$$), we need some invariant $$I$$ such that:

 - $$P \subseteq \mathsf{WP}(S_0,\, I)$$; that is, it is true prior to the execution of the loop.

 - $$I \andop e \subseteq \mathsf{WP}(S,\, I)$$; that is, if it holds at the start of an iteration and the branch condition is true, then it will hold after execution of the loop body.

 - $$I \andop {\mathop{!}e} \subseteq Q$$; that is, if the loop has terminated and the invariant is still true, then the desired post-condition is also true.

Together, these give $$P \subseteq \mathsf{WP}(S_0,\, I) \subseteq \mathsf{WP}(S_0,\, \mathsf{WP}(\mathsf{while}\ e\ \{ S \},\, Q))$$ as required.
When answering a question like this, it is a good idea to state the invariant, and then check each of the three constraints in turn, calculating weakest pre-conditions where needed.
When the loop body or initialisation contains a statement known only by its specification, refer to the relevant case(s) of its specification in your calculation.

Some tips for finding an invariant:

  - **Start from the post-condition.** The invariant, together with the failure of the branch condition, must imply the post-condition. What do you get for free from $$\mathop{!}e$$, and what must the invariant supply?
  - **Start from the pre-condition.** What is true after initialisation, and which parts of it stay true across iterations?
  - **Trace a few iterations.** Write down the values of the variables after $$0, 1, 2, \ldots$$ iterations and look for a relationship that persists, like $$2 * sum = i * (i - 1)$$. This is often a _generalisation_ of the post-condition, with a loop variable in place of its final value, much like an induction hypothesis.
  
## Showing a Loop Does Not Terminate

Recall that a statement that never terminates satisfies _any_ Hoare triple, and in particular $$\{ P \}\ S\ \{ \mathsf{false} \}$$ holds exactly when there are no terminating executions of $$S$$ from states in $$P$$.
We can use a loop invariant to show this: if $$I$$ is a loop invariant for $$\mathsf{while}\ e\ \{ S \}$$ and $$I \andop \mathop{!}e$$ is unsatisfiable, then $$I \andop \mathop{!}e \subseteq \mathsf{false}$$ and so the loop can never exit from a state in $$I$$.
For instance, for $$\mathsf{while}\ x > 0\ \{ x \leftarrow x + 1 \}$$ the invariant $$x > 0$$ is preserved, and $$x > 0 \andop \mathop{!}(x > 0)$$ is unsatisfiable, so the loop doesn't terminate when $$x > 0$$ initially.