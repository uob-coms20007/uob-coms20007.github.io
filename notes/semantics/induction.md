---
layout: math
title: Induction for Expressions
nav_order: 2
mathjax: true
parent: Semantics
---

# Arithmetic Induction

Many of you will have seen a form of proof by induction that goes something like this:

<div class="defn" markdown="1">
Suppose $P$ is some property of natural numbers.
To prove that $P(n)$ holds for _all_ natural numbers $n \in \mathbb{N}$, you must prove that:
* $P(0)$ holds - the _base case_;
* And, assuming $P(m)$ holds for some $m \in \mathbb{N}$, show that $P(m + 1)$ - the _inductive case_.

This is known as the __induction principle__ of natural numbers - a recipe for showing that a given property is true of all natural numbers.
</div>

Seeing as we have reduced a statement about an infinite set of objects to just two cases you might ask why is this a valid proof strategy?
For example, how do we know $P(1)$ is true?

We can answer this question by noticing that the inductive case allows us to conclude that $P(m + 1)$ is true whenever $P(m)$ is true.
As the base case tell us $P(0)$ is true, we can combine these two facts to conclude that $P(0 + 1) (= P(1))$ is also true.

Ok ... but how about $P(2)$?

Well again we can apply our inductive case, to show that $P(1 + 1) (= P(2))$ is also true using the fact that $P(1)$ is true as we just discovered.

Hopefully you're convinced that you can apply the inductive case as many times as you need to reach any given natural number.

#### An Example Proof

Now let us turn to how the induction principle for the natural numbers is applied in practice.
We will prove the following theorem:

<div class="defn" markdown="1">
  For all natural numbers $n \in \mathbb{N}$, the sum $0 + \cdots + n$ is equal to $1/2 \times n \times (n + 1)$.
</div>

First, we must check that the induction principle is applicable for this theorem.
The induction principle allows us to prove properties of the form "for all $n \in \mathbb{N}$, $P(n)$" (or $\forall n \in \mathbb{N}. P(n)$ if you're feeling fancy).
Our theorem is indeed of this form, where $P(n)$ is the statement "the sum $0 + \cdots + n$ is equal to $1/2 \times n \times (n + 1)$".
Therefore, we can apply the induction principle for the natural numbers.

Next we _instantiate_ the induction principle with our property of interest.
This is just a fancy way of saying we replace the generic property $P$ in the definition with our concrete property.
The result is the following proof obligations (i.e. things we must show to be true):

 1. $0 + \cdots + 0 = 1/2 \times 0 \times (0 + 1)$ holds - the base case;
 2. And, assuming $0 + \cdots + m = 1/2 \times m \times (m + 1)$ holds for some $m \in \mathbb{N}$, show that $0 + \cdots + (m + 1) = 1/2 \times (m + 1) \times (m + 1 + 1)$ - the inductive case.

The first proof obligation is relatively straightforward.
After simplifying both sides of the equation, we see that the base case amounts to the equality $0 = 0$ that is of course true.

The second proof obligation is a little bit more interesting.
Our goal in this case is to show that $0 + \cdots + (m + 1) = 1/2 \times (m + 1) \times (m + 1 + 1)$ for some unknown $m \in \mathbb{N}$.
Or, equivalently, $0 + \cdots + (m + 1) = 1/2 m^2 + 3/2 m + 1$.
This goal by itself is no easier to prove that the theorem itself.
Thankfully, however, we've been given the assumption that $0 + \cdots + m = 1/2 \times m \times (m + 1)$ holds for this particular $m$.
This additional assumption is referred to as the _induction hypothesis_ (or hypotheses when there is more than one).
It turns out that this induction hypothesis is sufficient to prove the desired equation:

$$
  \begin{array}{rll}
    0 + \cdots + (m + 1) &= (0 + \cdots + m) + (m + 1)                & \textrm{Expand} \\
                         &= (1/2 \times m \times (m + 1)) + (m + 1)   & \textrm{Induction Hypothesis} \\
                         &= (1/2 \times m^2 + 1/2 \times m) + (m + 1) & \textrm{Simplify} \\
                         &= 1/2 m^2 + 3/2 m + 1
  \end{array}
$$

First, we expand rearrange the summation to derive $(0 + \cdots + m) + (m + 1)$.
In doing so, we reveal a crucial aspect of the induction step: the summation $0 + \cdots + (m + 1)$ contains within in the summation $0 + \cdots + m$.
This is a promising sign that our induction proof is going to work out!
Having expanded the summation, we can apply the induction hypothesis to replace the summand $0 + \cdots + m$ with the formula $1/2 \times m \times (m + 1)$.
The rest of the proof follows from standard identities about addition and multiplication.

# Structural Induction

Structural induction is a direct generalisation of numerical induction where we substitute the natural numbers for some other set of interest; in particular, an _inductively defined_ structure.
For the time being, inductively defined structures will only extend to those sets defined by grammars.
To get an idea of how we will generalise the induction principle for the natural numbers to an arbitrary grammar, let us unpack the definition of the natural numbers.

For us, the natural numbers are defined by the following grammar: $N \to 0 \mid N + 1$ where our usual notation for a number such as $3$ is just shorthand for $0 + 1 + 1 + 1$.
That is, a natural number $n \in \mathbb{N}$ is either $0$ or $m + 1$ for some other natural number $m \in \mathbb{N}$.
If we compare these two production rules to the two cases of our inductive proof, then a similarity appears: there is a base case $N \rightarrow 0$ and an inductive case $N \rightarrow N + 1$...

It is precisely because the natural numbers can only have one of these forms that we can be sure that $P(n)$ is true for any particular $n$ in this set.
Any natural number is derived by adding $1$ to $0$ a certain number of times.
Therefore, for any natural number $n$, we can combine the induction step with the base case a certain number of time to prove $P(n)$.

<!-- It is no coincident that these two production rules correspond to the two cases of our inductive proof.
It is precisely because the natural numbers can only have one of these forms that we can be sure that $P(n)$ is true for any particular $n$ in this set.
A given natural number $n$ is derived via the production rules of this grammar and that derivation can be mimicked by our inductive principle. -->

To see how this correspondence plays out for a concrete number, let's revisit the case of $0 + 1 + 1$.
We know that $0 + 1 + 1$ is a natural number because $0 + 1$ is a natural number, and in turn $0 + 1$ is a natural number because $0$ is a natural number.
In the first two steps the non-terminal $N$ is replaced by $N + 1$, then finally we replace $N$ with the terminal $0$.
This derivation can be mimicked by our induction principle as follows: $P(0 + 1 + 1)$ holds because $P(0 + 1)$ holds (an inductive step), likewise $P(0 + 1)$ holds because $P(0)$ holds (another inductive step), and finally $P(0)$ holds as it is a base case.

## Induction over Arithmetic Expressions

There is nothing special about the natural numbers (despite what some philosophers might tell you).
We can apply the strategy of structural induction to any grammar of your choosing.

For example, consider the set of arithmetic expressions $\mathcal{A}$ defined by the following grammar:

$$
  A \Coloneqq x \mid n \mid A + A \mid A - A \mid A * A
$$

We will derive the induction principle for this set, i.e. the form of an inductive proof, by following the same recipe as for the natural numbers.
There must be a case for each production rule of the grammar and, in non-base cases, we may assume the property holds of all sub-expressions.

<div class="defn" markdown="1">
The __induction principle for arithmetic expressions__ is the following.

> Suppose $P$ is some property of arithmetic expressions.
> To prove that $P(e)$ holds for _all_ arithmetic expressions $e \in \mathcal{A}$, you prove that:
>
>  1. $P(x)$ holds for all variables $x$ - a _base_ case;
>  2. $P(n)$ holds for all numeric literals $n$ - a _base_ case;
>  3. $P(e_1 + e_2)$ holds assuming $P(e_1)$ and $P(e_2)$ hold - an _inductive_ case;
>  4. $P(e_1 - e_2)$ holds assuming $P(e_1)$ and $P(e_2)$ hold - an _inductive_ case;
>  5. And, that $P(e_1 * e_2)$ holds assuming $P(e_1)$ and $P(e_2)$ hold - an _inductive_ case;
</div>

To see why this is a valid argument, let us revisit the argument we used when inspecting the induction principle for natural numbers.
If we pick an arbitrary arithmetic expression, then by definition it is generated through the application of the grammar's production rules.
For example, $3 * x$ is an arithmetic expression as both $3$ and $x$ are arithmetic expressions and the grammar allows us to combine any two arithmetic expressions in this manner to form a new arithmetic expression.
Just as with the natural numbers, our induction principle allows us to mimic this derivation to derive a prove that $P(3 * x)$ holds - the first and second base cases tell us that $P(x)$ and $P(3)$ holds respectively, and the fifth case allows us to combine these facts to show that $P(3 * x)$ holds.

### Free Variables

Let's put the induction principle for arithmetic expressions to good use and prove a property about these expressions.
This proof might get a little hairy, but it is worth trying to digest what happens in each step.

First, consider the following definition.

<div class="defn" markdown="1">
The __free variables__ of an expression is the set of variables that appear in that expression.
Formally, we define the function $\mathsf{FV} : \mathcal{A} \rightarrow \mathcal{P}(\mathsf{Var})$ by recursion over the structure of expressions:

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

Intuitively, the denotation of an arithmetic expression should only depend on the value its free variables are given.
For example, $\llbracket x + 1\rrbracket_{\mathcal{A}}(\sigma)$ will be unchanged regardless if $\sigma(y) = 1$ or $\sigma(y) = 2$.
The formal way in which we will express this property is as follows:

<div class="defn" markdown="1">
  Let $P$ be the property of arithmetic expressions where $P(e)$ holds if, and only if, $\llbracket e \rrbracket_{\mathcal{A}}(\sigma) = \llbracket e \rrbracket_{\mathcal{A}}(\sigma')$ for any two stores $\sigma$ and $\sigma'$ such that $\sigma(y) = \sigma'(y)$ for all $y \in \mathsf{FV}(e)$.
</div>

In other words, this property states that if two stores agree on the value of every variable appearing in the expression, then the expression will have the same denotation under each store.

We will prove that this property is in fact true of _all_ arithmetic expressions.
As this claim is of the form "for _all_ arithmetic expressions, ...", we can use the induction principle for arithmetic expressions.

> #### Rule of Thumb
> Sometimes a property will have multiple possible induction principles to try.
> You might have to experiment a bit but, if the property uses a definition that _recurses_ over a particular structure, try using the _induction_ principle for that structure in your proof.
> In our case, the property is about the denotation function and free variables both of which recurse over arithmetic expressions.

Let us look at the proof obligations we derive for the two base cases of the induction principle:

 1. (The variable case) If $\sigma$ and $\sigma'$ are stores such that $\sigma(y) = \sigma'(y)$ for all $y \in \mathsf{FV}(x)$, then $\llbracket x \rrbracket_{\mathcal{A}}(\sigma) = \llbracket x \rrbracket_{\mathcal{A}}(\sigma')$.

    In this case, $\mathsf{FV}(x)$ is simple the set $\lbrace x \rbrace$ and, by definition, $\llbracket x \rrbracket_{\mathcal{A}}(\sigma) = \sigma(x)$ for any store $\sigma$.
    Therefore, our property allows us to assume that $\sigma(x) = \sigma'(x)$ and then asks us to show that $\sigma(x) = \sigma'(x)$ - which is trivial!

 2. (The literal case) If $\sigma$ and $\sigma'$ are stores such that $\sigma(y) = \sigma'(y)$ for all $y \in \mathsf{FV}(n)$, then $\llbracket n \rrbracket_{\mathcal{A}}(\sigma) = \llbracket n \rrbracket_{\mathcal{A}}(\sigma')$.

    In this case, $\mathsf{FV}(n)$ is the empty set, and therefore we do not know anything about $\sigma$ or $\sigma'$.
    However, as $\llbracket n \rrbracket_{\mathcal{A}}(\sigma) = n$ and likewise for $\sigma'$, we have that $\llbracket n \rrbracket_{\mathcal{A}}(\sigma) = \llbracket n \rrbracket_{\mathcal{A}}(\sigma')$ for all $\sigma$ and $\sigma'$.

In both of these cases, we may assume that $\sigma(y) = \sigma'(y)$ for all $y \in \mathsf{FV}(e)$ as it is only under this condition that our property is specified.
It is important to note that this is not an induction hypothesis that we are assuming - in fact there are no induction hypotheses in the base cases.
However, as our property is conditional on this fact, our induction hypotheses will be conditional on this fact as well.
This means that, when we come to use our induction hypotheses, we will have to make sure this condition is met.

Now let us look at one of these inductive cases.
To avoid getting lost in a fog of symbols, we will leave our property as $P(e)$ and unpack the definition as it is needed.

 3. (The addition case) If $P(e_1)$ and $P(e_2)$ hold, then $P(e_1 + e_2)$ holds.

    By definition $P(e_1 + e_2)$ is the statement: if $\sigma$ and $\sigma'$ are stores such that $\sigma(y) = \sigma'(y)$ for all $y \in \mathsf{FV}(e_1 + e_2)$, then $\llbracket e_1 + e_2 \rrbracket_{\mathcal{A}}(\sigma) = \llbracket e_1 + e_2 \rrbracket_{\mathcal{A}}(\sigma')$.
    Therefore, we assume that $\sigma$ and $\sigma'$ are two particular stores such that $\sigma(y) = \sigma'(y)$ for all $y \in \mathsf{FV}(e_1 + e_2)$, and we must subsequently show that $\llbracket e_1 + e_2 \rrbracket_{\mathcal{A}}(\sigma) = \llbracket e_1 + e_2 \rrbracket_{\mathcal{A}}(\sigma')$.

    Now recall that $\llbracket e_1 + e_2 \rrbracket_{\mathcal{A}}(\sigma) = \llbracket e_1 \rrbracket_{\mathcal{A}}(\sigma) + \llbracket e_2 \rrbracket_{\mathcal{A}}(\sigma)$ for any store $\sigma$.
    Therefore, we can simplify our goal to the equation $\llbracket e_1 \rrbracket_{\mathcal{A}}(\sigma) + \llbracket e_2 \rrbracket_{\mathcal{A}}(\sigma) = \llbracket e_1 \rrbracket_{\mathcal{A}}(\sigma') + \llbracket e_2 \rrbracket_{\mathcal{A}}(\sigma')$.
    To prove this equation, we will separately show that the equations $\llbracket e_1 \rrbracket_{\mathcal{A}}(\sigma) = \llbracket e_1 \rrbracket_{\mathcal{A}}(\sigma')$ and $\llbracket e_2 \rrbracket_{\mathcal{A}}(\sigma) = \llbracket e_2 \rrbracket_{\mathcal{A}}(\sigma')$ hold.
    
    Take note: we have reduced the property of the compound expression to a property of sub-expressions.
    This is exactly what we did in the summation example (where we first highlighted that $0 + \cdots + (m + 1)$ is equal to $(0 + \cdots + m) + (m + 1)$) and is a key pattern to all induction proofs.
    
    In order to conclude that the two smaller equations hold, we are going to need the induction hypotheses.
    In each inductive case, we may assume that the property we are trying to prove holds for _both_ sub-expressions thanks to our induction principle.
    There are two induction hypotheses here as there are two sub-expression: one for $e_1$ and one for $e_2$.
    Therefore, we assume that both $P(e_1)$ and $P(e_2)$ hold.
    These hypotheses state that, if $\sigma$ and $\sigma'$ are any two stores such that $\sigma(y) = \sigma'(y)$ for all $y \in \mathsf{FV}(e_1)$, then $\llbracket e_1 \rrbracket_{\mathcal{A}}(\sigma) = \llbracket e_1 \rrbracket_{\mathcal{A}}(\sigma')$, and likewise for $e_2$.

    To use these hypotheses, we need to make sure their conditions are met, i.e. that our $\sigma$ and $\sigma'$ are stores such that $\sigma(y) = \sigma'(y)$ for all $y \in \mathsf{FV}(e_1)$.
    We already know that $\sigma(y) = \sigma'(y)$ for all $y \in \mathsf{FV}(e_1 + e_2)$ as we made this assumption earlier on.
    And, as $\mathsf{FV}(e_1 + e_2) = \mathsf{FV}(e_1) \cup \mathsf{FV}(e_2)$, we also know $\sigma(y) = \sigma'(y)$ for all $y \in \mathsf{FV}(e_1)$ and for all $y \in \mathsf{FV}(e_2)$.
    Thus, the conditions of our induction hypotheses are satisfied, and we can use the induction hypotheses to conclude that $\llbracket e_1 \rrbracket_{\mathcal{A}}(\sigma) = \llbracket e_1 \rrbracket_{\mathcal{A}}(\sigma')$ and $\llbracket e_1 \rrbracket_{\mathcal{A}}(\sigma) = \llbracket e_2 \rrbracket_{\mathcal{A}}(\sigma')$.

    Finally, we can complete this case by putting the pieces back together and proving the property follow from the properties of its sub-expressions:

    $$
      \begin{array}{rll}
        \llbracket e_1 + e_2 \rrbracket_{\mathcal{A}}(\sigma) &= \llbracket e_1 \rrbracket_{\mathcal{A}}(\sigma) + \llbracket e_2 \rrbracket_{\mathcal{A}}(\sigma)                & \textrm{Expand} \\
                            &= \llbracket e_1 \rrbracket_{\mathcal{A}}(\sigma') + \llbracket e_2 \rrbracket_{\mathcal{A}}(\sigma')   & \textrm{Induction Hypotheses} \\
                            &= \llbracket e_1 + e_2 \rrbracket_{\mathcal{A}}(\sigma') & \textrm{Simplify}
      \end{array}
    $$

The other cases of our induction proof for subtraction (4) and multiplication (5) will follow the same pattern of: using definitions to break down the property into properties of sub-expressions, applying the induction hypotheses, and then recombining the derived property of sub-expressions.
These cases are left as exercises.