---
layout: math
title: Invariants
nav_order: 5
mathjax: true
parent: Semantics
---

# Loop Invariants

Last time we introduced Hoare triples, and saw how to calculate the weakest pre-condition for code without while loops.
Today, we will look at the Hoare logic rule concerning while loops and why it poses a challenge for computing the weakest pre-condition.

The rule for while loops is as follows:

$$
  \dfrac
  {\{ e \andop p \}\ S\ \{ p \}}
  {\{ p \}\ \mathsf{while}\ e\ \mathsf{do}\ S\ \{ \mathop{!}e \andop p \}}
$$

Recall that the notation $$\{ e \andop p \}\ S\ \{ p \}$$ says that if $\llbracket e \andop p \rrbracket_\mathcal{B}(\sigma)$ for some store $\sigma \in \mathsf{Store}$ and $\langle S,\, \sigma \rangle \rightarrow^* \sigma'$ then $\llbracket p \rrbracket_\mathcal{B}(\sigma')$.
In other words, when the body of the loop is executed with a store that satisfies $e$ (i.e. the branch condition) and this other property $p$, then any terminal store will satisfy $p$ as well.

So $p$ is a property that is preserved across each iteration of the loop: if it true before, then it is true after; the premise to this rule requires us to identify some property that will stay true across each iteration of the loop.
As a result, no matter how many times the loop is executed, we still know that it will hold after the loop has terminated (if indeed it terminates).
Therefore, the conclusion $$\{ p \}\ \mathsf{while}\ e\ \mathsf{do}\ S\ \{ \mathop{!}e \andop p \}$$ allows us to show that if $p$ is true before the loop is executed then it will still be true after, and we also know that $\mathop{!}e$ must be satisfied by the terminal state otherwise the loop would carry on.

We call the predicate $p$ a loop _invariant_ - a property that is invariant to the loops' execution.

Let's look at an example:

$$
  \begin{array}{l}
    \mathsf{while}\ x < y \mathsf{do} \\
      x \leftarrow x + 1
  \end{array}
$$

This rather straightforward program, which we will call $P_1$, increments $x$ until it exceeds $y$.
We already know that $x \geq y$ when the loop exits, but suppose we wish to know that if $x \leq y$ to begin with then it will still be after.
In other words, we want to prove the Hoare triple:


$$
  \{ x \leq y \}\ P_1\ \{ x \leq y \}
$$

The Hoare rule for the While construct only allows us to conclude a triple of the form $$\{ p \}\ \mathsf{while}\ x < y\ \mathsf{do}\ x \leftarrow x + 1\ \{ \mathop{!}(x < y) \andop p \}$$ for some loop invariant $p$.
So we will need to use the consequence rule to this rule; in particular, we need that $x \leq y$ implies $p$ and $\mathop{!}(x < y) \andop p$ implies $x \leq y$.
In this example, the only predicate that satisfies these constraints is $x \leq y$, giving us a candidate loop invariant.

Next, we need to show that $x \leq y$ is indeed a loop invariant so we must verify that:

$$
  \{ x < y \andop x \leq y \}\ x \leftarrow x + 1\ \{ x \leq y \}
$$

By the assignment rule, we have that $$\{ x + 1 \leq y \}\ x \leftarrow x + 1\ \{ x \leq y \}$$ which we can then weaken to the above triple as $x + 1 \leq y$ implies that $x < y \andop x \leq y$.
Therefore, this property is true across each iteration of the loop, making it a loop invariant.
In particular, it must be true after the loop and so we can see that $$\{ x \leq y \}\ P_1\ \{ x \leq y \}$$ as required.

## The Sweet Spot

In the above example, the loop invariant required was forced upon us the particular pre- and post-conditions that we were trying to verify.
However, this isn't always the case - more often than not finding the right loop invariant will take some tinkering.

Consider the following program $P_2$:

$$
  \begin{array}{l}
    sum \leftarrow 0 \\
    i \leftarrow 0 \\
    \mathsf{while}\ i < n \mathsf{do} \\
      sum \leftarrow sum + i;\; \\
      i \leftarrow i + 1
  \end{array}
$$

This program will sum all the numbers from $0$ up to $n$ (assuming $n$ is positive), which is equal to $n * (n - 1) / 2$.
Ultimately, we want to show that this program $P$ satisfies the Hoare triple:

$$
  \{ n \geq 0 \}\ P_2\ \{ 2 * sum = n * (n - 1) \} 
$$

As with last time, we will work backwards to find a pre-condition that we can then compare to $n \geq 0$.
So what is a pre-condition for the while loop that will ensure $2 * sum = n * (n - 1)$?

Recall that the form of the while rule requires us to find a predicate $p$ such that $$\{ i < n \andop p \}\ sum \leftarrow sum + i; i \leftarrow i + 1\ \{ p \}$$ to be our loop invariant.
This will allow us to conclude that $$\{ p \}\ \mathsf{while}\ i < n\ \mathsf{do}\ ...\ \{ \mathop{!}(i < n) \andop p \}$$.
If we can then weaken this post-condition to $2 * sum = n * (n - 1)$, i.e. $\mathop{!}(i < n) \andop p$ implies $2 * sum = n * (n - 1)$, we have found a suitable pre-condition.

At first glance, it might seem that $2 * sum = n * (n - 1)$ is thus a good candidate for our loop invariant.
However, $sum$ is going to start at $0$ and work up towards this value, so this won't actually hold across the entire loop.
In particular, the following triple does _not_ hold.

$$
  \{ i < n \andop 2 * sum = n * (n - 1) \}\ sum \leftarrow sum + i;\; i \leftarrow i + 1\ \{ 2 * sum = n * (n - 1) \}
$$

We can see this by considering a concrete counterexample for the initial store $[i \mapsto 1,\, n \mapsto 3,\, sum \mapsto 1]$. 
Or we can double-check by seeing that the weakest pre-condition would be $i + 1 \andop 2 * (sum + i) = n * (n - 1)$, which the above pre-condition $i < n \andop 2 * sum = n * (n - 1)$ does not imply.

As we have already acknowledged, our initial candidate invariant didn't actually fit with the expected behaviour of the program anyway.
Instead, what should be the case is that $2 * sum = i * (i - 1)$ as on each iteration $sum$ is summation of the first $i$ numbers.
So let's see if this predicate is an invariant.
We need to verify the triple:

$$
  \{ i < n \andop 2 * sum = i * (i - 1) \}\ sum \leftarrow sum + i;\; i \leftarrow i + 1\ \{ 2 * sum = i * (i - 1) \}
$$

The weakest pre-condition here is $2 * (sum + i) = (i + 1) * (i + 1 - 1)$, which implies that $2 * sum = i * (i - 1)$ as required!
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
  \{ i < n \andop 2 * sum = i * (i - 1) \andop i \leq n \}\ sum \leftarrow sum + i;\; i \leftarrow i + 1\ \{ 2 * sum = i * (i - 1) \andop i \leq n \}
$$

and thus we can conclude:

$$
  \{ 2 * sum = i * (i - 1) \andop i \leq n \}\ \mathsf{while}\ i < n\ \mathsf{do}\ ...\ \{ i \geq n \andop 2 * sum = i * (i - 1) \andop i \leq n \}
$$

with post-condition being weakenable to $2 * sum = n * (n - 1)$ as required because $i \geq n$ and $i \leq n$ is only satisfied when $i = n$.

To get a pre-condition for the entire program, we just need to apply our weakest pre-condition formulas from last time to get $2 * 0 = 0 * (0 - 1) \andop 0 \leq n$, which is equivalent to $0 \leq n$.
Therefore, we can conclude the program has the correct behaviour and indeed will compute the sum of the first $n$ numbers when $n$ is initially greater than or equal to $0$.


## Constraints

Even in this simple case, verifying program's with loops can become surprisingly challenging!
In fact, the construction of a loop invariant is _undecidable_ problem, i.e. it cannot be automated and there is no set method for constructing a loop invariant that will always work.
You will see more about undecidable problems in the next section of the unit.

<!-- Although loop invariants ... -->