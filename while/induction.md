---
layout: math
title: Proof by Induction
nav_order: 2
mathjax: true
parent: Semantics
---

# Proof by Induction

Before we go on to look at statements and operational semantics, it will be useful to familiarise ourselves with _proof by induction_.

## Numerical Induction

Many of you will have seen a form of proof by induction that goes something like this:

<div class="defn" markdown="1">
Suppose $P$ is some property of natural numbers.
To prove that $P(n)$ holds for _all_ natural numbers $n \in \mathbb{N}$, you prove that:
* $P(0)$ holds - the _base case_;
* And, assuming $P(m)$ holds for some $m \in \mathbb{N}$, show that $P(m + 1)$ - the _inductive case_.

This is known as the __induction principle__ of natural numbers.
</div>

Seeing as we have reduced a statement about an infinite set of objects to just two cases you might ask why is this a valid proof strategy?

The key lies in the definition of the natural numbers.
We can think of the natural numbers as being defined by the following grammar: $N \to 0 \mid N + 1$ where our usual notation for a number such as $3$ is just shorthand for $0 + 1 + 1 + 1$.

A natural number $n$ is thus either $0$ or $m + 1$ for some other natural number $m$.
The two cases of the induction principle (the base case and the induction step) then directly correspond to the two production rules of this grammar:

 * If a natural number $n$ is $0$, then the base case tells us that $P(n)$ holds true.

 * If, on the other hand, a natural number $n$ is of the form $m + 1$ for some $m$, then the induction step tell us that $P(m + 1)$ holds true if $P(m)$ we assume that is true.

And the grammar tell us that there are no other cases to consider.

To show that the induction principle is sufficient to conclude that $P(n)$ holds of all $n$, let's look at an example.
Suppose that $P(3)$ were not true.
From the induction step, we can see that $P(2)$ could not be true either otherwise we'd have a contradiction!
However, the same argument can then be applied to $2$, and we see that neither $P(1)$ nor $P(0)$ can be true.
As we have already shown that $P(0)$ is true, we have a contradiction.
This allows us to conclude that $P(3)$ was in fact true all along...

If we now replace $3$ with some hypothetical _smallest_ counter-example, i.e. the smallest number such that $P(n)$ does not hold, then we see why this principle works more generally.
First, such a counter-example cannot be a base case (i.e. $0$) as we already know that the base case holds true.
Therefore, it must be a compound case $m + 1$.
However, as we assumed that $n$ was the smallest counter-example, we know that $P(m)$ is true.
Therefore, the inductive case tells us that $P(n)$ is true as well.
We have derived a contradiction, and thus can conclude that there is no smallest counter-example, and indeed, no counter-example.

In summary, our induction principle works by:
* The proof covers each production rule of the associated grammar;
* And, in non-base cases, assuming the property holds of the sub-expressions.

## Another Grammar

There is nothing special about the natural numbers (despite what some philosophers might tell you).
We can apply this strategy to any grammar of your choosing.

For example, consider arithmetic expressions:

$$
  A \to x \mid n \mid A + A \mid A - A \mid A * A
$$

We will derive the induction principle for this grammar, i.e. the form of an inductive proof, by following the same recipe as for the natural numbers.
There must be a case for each production rule of the grammar and, in non-base cases, we assume the property holds of all sub-expressions.

<div class="defn" markdown="1">
The __induction principle for exprssions__ is the following.

> Suppose $P$ is some property of arithmetic expressions.
> To prove that $P(e)$ holds for _all_ arithmetic expressions $e \in A$, you prove that:
> * $P(x)$ holds for all variables $x$ - a _base_ case;
> * $P(n)$ holds for all numeric literals $n$ - a _base_ case;
> * $P(e_1 + e_2)$ holds assuming $P(e_1)$ and $P(e_2)$ hold - an _inductive_ case;
> * $P(e_1 - e_2)$ holds assuming $P(e_1)$ and $P(e_2)$ hold - an _inductive_ case;
> * And, that $P(e_1 * e_2)$ holds assuming $P(e_1)$ and $P(e_2)$ hold - an _inductive_ case;
</div>

To see why this is a valid argument, let us revisit the argument we used for the natural numbers.
Suppose $e$ is the _smallest_ expression such that $P(e)$ did not hold.
Then $e$ cannot be a variable or a numeric literal, else it would contradict one of the base cases.
Therefore, $e$ is of the form $e_1 + e_2$ or $e_1 - e_2$ or $e_1 * e_2$.
In each of these cases, we know that the property holds of $P(e_1)$ and $P(e_2)$ else $e$ would not be the smallest counter-example.
However, the inductive cases allow us to conclude that $P(e)$ holds and thus is not a counter-example.
Again, we have derived a contradiction and shown that no such counter-example can exist.

### Free Variables

Let put this induction principle to good use and prove a property about arithmetic expressions.

<div class="defn" markdown="1">
The __free variables__ of an expression is the set of variables that appear in that expression.
Formally, we define the function $\mathsf{FV} : A \rightarrow \mathcal{P}(\mathsf{Var})$ by recursion over the structure of expressions:

$$
\begin{array}{rl}
  \mathsf{FV}(x) & = \{ x \} \\
  \mathsf{FV}(n) & = \emptyset \\
  \mathsf{FV}(e_1 + e_2) & = \mathsf{FV}(e_1) \cup \mathsf{FV}(e_2) \\
  \mathsf{FV}(e_1 - e_2) & = \mathsf{FV}(e_1) \cup \mathsf{FV}(e_2) \\
  \mathsf{FV}(e_1 * e_2) & = \mathsf{FV}(e_1) \cup \mathsf{FV}(e_2) \\
\end{array}
$$

</div>

Intuitively, the denotation of an arithmetic expression only depends on the value its free variables are given.
For example, $\llbracket x + 1\rrbracket_A(\sigma)$ will be unchanged regardless if $\sigma(y) = 1$ or $\sigma(y) = 2$.
The formal way in which we will express this property is as follows:

<div class="defn" markdown="1">
Let $P$ be the property of arithmetic expressions where $P(e)$ holds if, and only if, $\llbracket e \rrbracket_A(\sigma) = \llbracket e \rrbracket_A(\sigma')$ for any two states $\sigma$ and $\sigma'$ such that $\sigma(y) = \sigma'(y)$ for all $y \in \mathsf{FV}(e)$.
</div>

In other words, this property states that if two states agree on the value of every variable appearing in the expression, then the expression will have the same denotation under each state.

We will prove that this property is in fact true of _all_ arithmetic expressions.
As this claim is of the form "for _all_ arithmetic expressions, ...", we can use the induction principle for arithmetic expressions.

> #### Rule of Thumb
> Sometimes a property will have multiple possible induction principles to try.
> You might have to experiment a bit but, if the property uses a definition that _recurses_ over a particular structure, try using the _induction_ principle for that structure in your proof.
> In our case, the property is about the denotation function and free variables both of which recurse over arithmetic expressions.

First, let us look at the two base cases of the induction principle:

* (Variable Case) If $\sigma$ and $\sigma'$ are states such that $\sigma(y) = \sigma'(y)$ for all $y \in \mathsf{FV}(x)$, then $\llbracket x \rrbracket_A(\sigma) = \llbracket x \rrbracket_A(\sigma')$.

  * In this case, $\mathsf{FV}(x)$ is simple the set $\lbrace x \rbrace$ and, by definition, $\llbracket x \rrbracket_A(\sigma) = \sigma(x)$ for any state $\sigma$.
    Therefore, our property allows us to assume that $\sigma(x) = \sigma'(x)$ and then asks us to show that $\sigma(x) = \sigma'(x)$ - which is trivial!

* (Literal Case) If $\sigma$ and $\sigma'$ are states such that $\sigma(y) = \sigma'(y)$ for all $y \in \mathsf{FV}(n)$, then $\llbracket n \rrbracket_A(\sigma) = \llbracket n \rrbracket_A(\sigma')$.

  * In this case, $\mathsf{FV}(n)$ is the empty set, and therefore we do not know anything about $\sigma$ or $\sigma'$.
    However, as $\llbracket n \rrbracket_A(\sigma) = n$ and likewise for $\sigma'$, we have that $\llbracket n \rrbracket_A(\sigma) = \llbracket n \rrbracket_A(\sigma')$ for all $\sigma$ and $\sigma'$.

Next let us look at one of the inductive cases.
In each of these inductive cases, we may assume that the property we are trying to prove holds for _both_ sub-expressions thanks to our induction principle.
To avoid getting lost in a fog of symbols, we will leave our property as $P(e)$ and unpack the definition as it is needed.

* (Addition Case) If $P(e_1)$ and $P(e_2)$ hold, then $P(e_1 + e_2)$ holds.

    * By definition $P(e_1 + e_2)$ is the statement: if $\sigma$ and $\sigma'$ are states such that $\sigma(y) = \sigma'(y)$ for all $y \in \mathsf{FV}(e_1 + e_2)$, then $\llbracket e_1 + e_2 \rrbracket_A(\sigma) = \llbracket e_1 + e_2 \rrbracket_A(\sigma')$.
      Let us assume that $\sigma$ and $\sigma'$ are two states such that $\sigma(y) = \sigma'(y)$ for all $y \in \mathsf{FV}(e_1 + e_2)$.
      Now recall that $\llbracket e_1 + e_2 \rrbracket_A(\sigma) = \llbracket e_1 \rrbracket_A(\sigma) + \llbracket e_2 \rrbracket_A(\sigma)$ for any state $\sigma$.
      Therefore, we really are trying to show that $\llbracket e_1 \rrbracket_A(\sigma) + \llbracket e_2 \rrbracket_A(\sigma) = \llbracket e_1 \rrbracket_A(\sigma') + \llbracket e_2 \rrbracket_A(\sigma')$.
      For this, we will separately show that $\llbracket e_1 \rrbracket_A(\sigma) = \llbracket e_1 \rrbracket_A(\sigma')$ and $\llbracket e_2 \rrbracket_A(\sigma) = \llbracket e_2 \rrbracket_A(\sigma')$ hold.
      
      Take note: we have reduced the property of the compound expression to a property of sub-expressions.
      This is a key pattern to all induction proofs.
      
      In order to conclude that these two equations hold, we are going to need the induction hypotheses $P(e_1)$ and $P(e_2)$.
      These hypotheses state that, if $\sigma$ and $\sigma'$ are any two states such that $\sigma(y) = \sigma'(y)$ for all $y \in \mathsf{FV}(e_1)$, then $\llbracket e_1 \rrbracket_A(\sigma) = \llbracket e_1 \rrbracket_A(\sigma')$, and likewise for $e_2$.
      To use these hypotheses, we need to make sure their conditions are met, i.e. that our $\sigma$ and $\sigma'$ are states such that $\sigma(y) = \sigma'(y)$ for all $y \in \mathsf{FV}(e_1)$.
      We already know that $\sigma(y) = \sigma'(y)$ for all $y \in \mathsf{FV}(e_1 + e_2)$.
      And, as $\mathsf{FV}(e_1 + e_2) = \mathsf{FV}(e_1) \cup \mathsf{FV}(e_2)$, we also know $\sigma(y) = \sigma'(y)$ for all $y \in \mathsf{FV}(e_1)$ and for all $y \in \mathsf{FV}(e_2)$.
      Thus, the conditions are satisfied, and we can use the induction hypotheses to conclude that $\llbracket e_1 \rrbracket_A(\sigma) = \llbracket e_1 \rrbracket_A(\sigma')$ and $\llbracket e_1 \rrbracket_A(\sigma) = \llbracket e_2 \rrbracket_A(\sigma')$.

      Finally, we can complete this case by putting the pieces back together and proving the property follow from the properties of its sub-expressions.
      That is, if $\llbracket e_1 \rrbracket_A(\sigma) = \llbracket e_1 \rrbracket_A(\sigma')$ and $\llbracket e_1 \rrbracket_A(\sigma) = \llbracket e_2 \rrbracket_A(\sigma')$, then $\llbracket e_1 + e_2 \rrbracket_A(\sigma) = \llbracket e_1 + e_2 \rrbracket_A(\sigma')$ as required.

The other cases of our induction proof will follow the same pattern of: using definitions to break down the property into properties of sub-expressions, applying the induction hypotheses, and then recombining the derived property of sub-expressions.

* (Subtraction Case) If $P(e_1)$ and $P(e_2)$ hold, then $P(e_1 - e_2)$ holds.

    * Let us assume that $\sigma$ and $\sigma'$ are states such that $\sigma(y) = \sigma'(y)$ for all $y \in \mathsf{FV}(e_1 - e_2)$.
      As before, we will show that $\llbracket e_1 \rrbracket_A(\sigma) = \llbracket e_1 \rrbracket_A(\sigma')$ and $\llbracket e_2 \rrbracket_A(\sigma) = \llbracket e_2 \rrbracket_A(\sigma')$.

      The induction hypotheses states that, if $\sigma$ and $\sigma'$ are states such that $\sigma(y) = \sigma'(y)$ for all $y \in \mathsf{FV}(e_1)$, then $\llbracket e_1 \rrbracket_A(\sigma) = \llbracket e_1 \rrbracket_A(\sigma')$, and likewise for $e_2$.
      As $\mathsf{FV}(e_1 - e_2) = \mathsf{FV}(e_1) \cup \mathsf{FV}(e_2)$, we know that $\sigma$ and $\sigma'$ are states such that $\sigma(y) = \sigma'(y)$ for all $y \in \mathsf{FV}(e_1)$ and for all $y \in \mathsf{FV}(e_2)$.
      Therefore, $\llbracket e_1 \rrbracket_A(\sigma) = \llbracket e_1 \rrbracket_A(\sigma')$ and $\llbracket e_1 \rrbracket_A(\sigma) = \llbracket e_2 \rrbracket_A(\sigma')$.
      It then follows that $\llbracket e_1 - e_2 \rrbracket_A(\sigma) = \llbracket e_1 - e_2 \rrbracket_A(\sigma')$ as required.

* (Multiplication Case) If $P(e_1)$ and $P(e_2)$ hold, then $P(e_1 * e_2)$ holds.

    * Let us assume that $\sigma$ and $\sigma'$ are states such that $\sigma(y) = \sigma'(y)$ for all $y \in \mathsf{FV}(e_1 * e_2)$.
      As before, we will show that $\llbracket e_1 \rrbracket_A(\sigma) = \llbracket e_1 \rrbracket_A(\sigma')$ and $\llbracket e_2 \rrbracket_A(\sigma) = \llbracket e_2 \rrbracket_A(\sigma')$.

      The induction hypotheses states that, if $\sigma$ and $\sigma'$ are states such that $\sigma(y) = \sigma'(y)$ for all $y \in \mathsf{FV}(e_1)$, then $\llbracket e_1 \rrbracket_A(\sigma) = \llbracket e_1 \rrbracket_A(\sigma')$, and likewise for $e_2$.
      As $\mathsf{FV}(e_1 * e_2) = \mathsf{FV}(e_1) \cup \mathsf{FV}(e_2)$, we know that $\sigma$ and $\sigma'$ are states such that $\sigma(y) = \sigma'(y)$ for all $y \in \mathsf{FV}(e_1)$ and for all $y \in \mathsf{FV}(e_2)$.
      Therefore, $\llbracket e_1 \rrbracket_A(\sigma) = \llbracket e_1 \rrbracket_A(\sigma')$ and $\llbracket e_1 \rrbracket_A(\sigma) = \llbracket e_2 \rrbracket_A(\sigma')$.
      It then follows that $\llbracket e_1 * e_2 \rrbracket_A(\sigma) = \llbracket e_1 * e_2 \rrbracket_A(\sigma')$ as required.

As with everything in life, proof by induction may take some practice.
But don't be intimidated, just try and follow it step-by-step and be as explicit as possible with your proof.