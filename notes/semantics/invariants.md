---
layout: math
title: Invariants
nav_order: 5
mathjax: true
parent: Semantics
---

# Loop Invariants

Last time we introduced Hoare triples, and saw how to calculate the strongest post-condition for code without while loops.
Today, we will look at the Hoare logic rule concerning while loops and why it poses a challenge for computing the weakest pre-condition.

The rule for while loops is as follows:

$$
  \dfrac
  {\{ e \andop P \}\ S\ \{ P \}}
  {\{ P \}\ \mathsf{while}\ e\ \mathsf{do}\ S\ \{ \mathop{!}e \andop P \}}
$$

Recall that the notation $$\{ e \andop P \}\ S\ \{ P \}$$ says that if $\sigma$ satisfies $e \andop P$ and $\langle S,\, \sigma \rangle \rightarrow^* \sigma'$ then $\sigma'$ will satisfy $P$.
In other words, when the body of the loop is executed in a state that satisfies $e$ (i.e. the branch condition) and this other property $P$, then any terminal state will satisfy $P$ as well.

So $P$ is a property that is _preserved_ across each iteration of the loop: if it true before, then it is true after; the premise to this rule requires us to identify some property that will stay true across each iteration of the loop.
As a result, no matter how many times the loop is executed, we still know that it will hold after the loop has terminated (if indeed it terminates).
Therefore, the conclusion $$\{ P \}\ \mathsf{while}\ e\ \mathsf{do}\ S\ \{ \mathop{!}e \andop P \}$$ allows us to show that if $P$ is true before the loop is executed then it will still be true after, and we also know that $\mathop{!}e$ must be satisfied by the terminal state (otherwise the loop would carry on).

We call the predicate $P$ a loop _invariant_ - a property that is invariant under execution of the loop.

Let's look at an example:

$$
  \begin{array}{l}
    \mathsf{while}\ x < y\ \mathsf{do} \\
    \quad x \leftarrow x + 1
  \end{array}
$$

This rather straightforward program, which we will call $S_1$, increments $x$ until it exceeds $y$.
We already know that $x \geq y$ when the loop exits, but suppose we wish to know that, if $x \leq y$ to begin with, then it will still be after.
In other words, we want to prove the Hoare triple:


$$
  \{ x \leq y \}\ S_1\ \{ x \leq y \}
$$

The Hoare rule for the While construct only allows us to conclude a triple of the form $$\{ x \leq y \}\ \mathsf{while}\ x < y\ \mathsf{do}\ x \leftarrow x + 1\ \{ \mathop{!}(x < y) \andop x \leq y \}$$ if we can show that $$\{ x < y \andop x \leq y \}\ x \leftarrow x + 1\ \{ x \leq y \}$$.
Here, we have chosen $x \leq y$ as our loop invariant as it is the pre-condition - it is precisely what we know before, so let's see if it forms a loop invariant.

To verify $$\{ x < y \andop x \leq y \}\ x \leftarrow x + 1\ \{ x \leq y \}$$, we calculate the trongest post-condition $\mathsf{SPC}(x \leftarrow x + 1,\, x < y \andop x \leq y)$ to be $$\exists x'. x' < y \andop (x \leq y)[x'/x] \andop x = x' + 1$$.
And we can then see that this post-condition is equivalent to $$x \leq y$$ as required.
Therefore, $$x \leq y$$ is a loop invariant - it is preserved under execution of the loop.

Given this loop invariant, the rule for the while construct gives us $$\{ x \leq y \}\ \mathsf{while}\ x < y\ \mathsf{do}\ x \leftarrow x + 1\ \{ \mathop{!}(x < y) \andop x \leq y \}$$, which we can then weaken to $$\{ x \leq y \}\ \mathsf{while}\ x < y\ \mathsf{do}\ x \leftarrow x + 1\ \{ x \leq y \}$$ by the consequent rule.
That is, if the loop terminates, then we will have $x \leq y$ after.

## The Sweet Spot

In the above example, the loop invariant required was chosen to be the pre-condition.
We can also see that it was forced upon us by the post-condition - we wanted some invariant $P$ such that the post-condition $!(x < y) \andop P$ would imply $x \leq y$ for which $x \leq y$ is the most general solution

However, it isn't always the case that the loop invariant can be determined by the pre- or post-condition.
More often than not finding the right loop invariant will take some tinkering, much like finding the right induction hypothesis.

Consider the following program $S_2$:

$$
  \begin{array}{l}
    sum \leftarrow 0 \\
    i \leftarrow 0 \\
    \mathsf{while}\ i < n\ \mathsf{do} \\
    \quad sum \leftarrow sum + i;\; \\
    \quad i \leftarrow i + 1
  \end{array}
$$

This program will sum all the numbers from $0$ up to $n$ (assuming $n$ is positive), which is equal to $n * (n - 1) / 2$.
Ultimately, we want to show that this program $S_2$ satisfies the Hoare triple:

$$
  \{ n \geq 0 \}\ S_2\ \{ 2 * sum = n * (n - 1) \} 
$$

As with last time, we will first work forward from the pre-condition to find a post-condition that we can then compare to $2 * sum = n * (n - 1)$.
Before reaching the while loop, there are two assignment statements for which we can calculate the strongest post-condition directly:

$$
  \mathsf{SPC}(sum \leftarrow 0;\; i \leftarrow 0,\, n \geq 0) = n \geq 0 \andop sum = 0 \andop i = 0
$$

So we are left with the objective:

$$
  \begin{array}{l}
    \{ n \geq 0 \andop sum = 0 \andop i = 0 \} \\
    \mathsf{while}\ i < n\ \mathsf{do} \\
    \quad sum \leftarrow sum + i;\; \\
    \quad i \leftarrow i + 1 \\
    \{ 2 * sum = n * (n - 1) \}
  \end{array}
$$

Recall that the Hoare rule for the while construct requires us to find a predicate $P$ such that $$\{ i < n \andop P \}\ sum \leftarrow sum + i; i \leftarrow i + 1\ \{ P \}$$ to be our loop invariant.
Suppose we try taking the pre-condition $n \geq 0 \andop sum = 0 \andop i = 0$ as a candidate loop invariant.
Then, the strongest post-condition of the loop's body will be:

$$
  \exists sum'\ i'.\, n \geq 0 \andop sum' = 0 \andop i' = 0 \andop sum = sum' + i' \andop i = i' + 1
$$

Or, equivalently:

$$
  n \geq 0 \andop sum = 0 \andop i = 1
$$

As this post-condition does not match up with what we know prior to execution, i.e. $n \geq 0 \andop sum = 0 \andop i = 0$, we can see that this is not going to be a loop invariant.
In hindsight, this isn't that surprising... The loop is going to increase $i$ and, after the first iteration, $sum$ so they won't always be $0$.

Another tactic might be to start with the desired post-condition $2 * sum = n * (n - 1)$ and see if this constitutes a loop invariant.
To verify this, we need to check that:

$$
  \begin{array}{l}
    \{ i < n \andop 2 * sum = n * (n - 1) \} \\
    \quad sum \leftarrow sum + i;\; \\
    \quad i \leftarrow i + 1 \\
    \{ 2 * sum = n * (n - 1) \}
  \end{array}
$$

However, in this case, the strongest post-condition turns out to be:

$$
  \exists sum'\ i'.\, i' < n \andop 2 * sum' = n * (n - 1) \andop sum = sum' + i' \andop i = i' + 1
$$

Or, equivalently:

$$
  i - 1 < n \andop 2 * (sum - i + 1) = n * (n - 1)
$$

Which again does not ensure that $2 * sum = n * (n - 1)$, so it is not a loop invariant either.
As with our first candidate assertion, this doesn't actually match the expected behaviour either.
It will not be the case that $2 * sum = n * (n - 1)$ _throughout_ execution, just at the end.
Instead, what should be the case is that $2 * sum = i * (i - 1)$ as on each iteration $sum$ is summation of the first $i$ numbers.
So let's see if this predicate is an invariant.
We need to verify the triple:

$$
  \{ i < n \andop 2 * sum = i * (i - 1) \}\ sum \leftarrow sum + i;\; i \leftarrow i + 1\ \{ 2 * sum = i * (i - 1) \}
$$

The strongest pre-condition here is $2 * (sum + i) = (i + 1) * (i + 1 - 1)$, which implies that $2 * sum = i * (i - 1)$ as required!
Therefore, we have a loop invariant, and so we can conclude that:

$$
  \{ 2 * sum = i * (i - 1) \}\ \mathsf{while}\ i < n\ \mathsf{do}\ ...\ \{ i \geq n \andop 2 * sum = i * (i - 1) \}
$$

But there's a snag...
Our post-condition doesn't imply that $2 * sum = n * (n - 1)$.
In particular, we know that $i \geq n$ will hold after the loop, but not that it is equal to $n$.
This leads us to the final invariant $2 * sum = i * (i - 1) \andop i \leq n$.

It is indeed an invariant as we have:

$$
  \begin{array}{l}
    \{ i < n \andop 2 * sum = i * (i - 1) \andop i \leq n \}\\
    sum \leftarrow sum + i;\;\\
    i \leftarrow i + 1\\
    \{ 2 * sum = i * (i - 1) \andop i \leq n \}
  \end{array}
$$

and thus we can conclude:

$$
  \begin{array}{l}
    \{ 2 * sum = i * (i - 1) \andop i \leq n \}\\
    \mathsf{while}\ i < n\ \mathsf{do}\\
    \quad sum \leftarrow sum + i;\;\\
    \quad i \leftarrow i + 1\\
  \{ i \geq n \andop 2 * sum = i * (i - 1) \andop i \leq n \}
  \end{array}
$$

where the post-condition can be weakened to $2 * sum = n * (n - 1)$ (because $i \geq n$ and $i \leq n$ is only satisfied when $i = n$) as required.

To get a pre-condition for the entire program, we just need to apply our weakest pre-condition formulas from last time to get $2 * 0 = 0 * (0 - 1) \andop 0 \leq n$, which is equivalent to $0 \leq n$.
Therefore, we can conclude the program has the correct behaviour and indeed will compute the sum of the first $n$ numbers when $n$ is initially greater than or equal to $0$.

## Constraints

Even in this simple case, verifying program's with loops can become surprisingly challenging!
In fact, the construction of a loop invariant is _undecidable_ problem, i.e. it cannot be automated and there is no set method for constructing a loop invariant that will always work.
You will see more about undecidable problems in the next section of the unit.

Typically, identifying a loop invariant involves some insight into the behaviour of the program - you need to consider how you expect the program to behave.
That said, we can characterise the  of a loop invariant in the form of logical constraints.

To ensure that $$\{ P \}\ \mathsf{while}\ e\ \mathsf{do}\ S\ \{ Q \}$$ holds, we need some invariant $$I$$ such that:

 - $$P \Rightarrow I$$; that is, it is true prior to the execution of the loop.

 - $$\mathsf{SPC}(S,\, I \andop e) \Rightarrow I$$; that is, if it holds at the start of the loop and the branch condition is true, then it will holds after execution of the loop.

 - $$I \andop {!e} \Rightarrow Q$$; that is, if the loop has terminated and the invariant is still true, then the desired post-condition is also true.

So, for our earlier example:

$$
  \begin{array}{l}
    \{ n \geq 0 \andop sum = 0 \andop i = 0 \} \\
    \mathsf{while}\ i < n\ \mathsf{do} \\
    \quad sum \leftarrow sum + i;\; \\
    \quad i \leftarrow i + 1 \\
    \{ 2 * sum = n * (n - 1) \}
  \end{array}
$$

We need to find some $I$ such that:

 - $$n \geq 0 \andop sum = 0 \andop i = 0 \Rightarrow I$$.
 - $$\mathsf{SPC}(sum \leftarrow sum + i;\; i \leftarrow i + 1,\, I \andop i < n) \Rightarrow I$$.
 - $$I \andop i \geq n \Rightarrow 2 * sum = n * (n - 1)$$.

And we can see that $2 * sum = i * (i - 1) \andop i \leq n$ satisfies these constraints.