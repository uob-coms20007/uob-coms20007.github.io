---
layout: obs
title: Logic
mathjax: true
parent: Prerequesites
nav_order: 3
grand_parent: Part I
---

## Basic Logic

You should make sure you can recall the basics of the propositional connectives and the quantifiers.  We fix our notation for these in the following.

### Syntax

We will use the following notation for the logical connectives and quantifiers:

| Symbol              | Connective |
|:--------------------|:-----------|
| $$\wedge$$          | and        |
| $$\vee$$            | or         |
| $$\neg$$            | not        |
| $$\Rightarrow$$     | implies    |
| $$\Leftrightarrow$$ | iff        |
| $$\forall$$         | for all    |
| $$\exists$$         | exists     |

To avoid a plethora of parentheses, we adopt the usual conventions:
* $$\neg$$ binds tighter than $$\wedge$$ which binds tighter than $$\vee$$ which binds tigter than $$\Rightarrow$$ which binds tighter than $$\Leftrightarrow$$ which binds tighter than the quantifiers.
* $$\wedge$$ and $$\vee$$ and $$\Leftrightarrow$$ are associative.  $$\Rightarrow$$ is assumed to associate to the right, so e.g.
$$
P \Rightarrow Q \Rightarrow R \ \text{means}\ P \Rightarrow (Q \Rightarrow R)
$$
* The scope of the quantifiers extends as far to the right as possible, so e.g.
$$
  \forall x.\ P \wedge Q \ \text{means}\ \forall x.\ (P \wedge Q)
$$

The variable $$x$$ occurring in $$\forall x.\ P$$ is said to be __bound__.  Any variable not bound by an enclosing quantifier is said to be __free__.  Changing the name of *bound* variables does not change the formula, e.g.

$$
\forall x.\, x > 0 \wedge y <= 10 \quad\text{is the same formula as}\quad \forall z.\,z>0 \wedge y <= 10
$$

However, this formula is *different* to $$\forall x.\,x>0 \wedge z <= 10$$ in which the *free* variable $$y$$ was renamed $$z$$.
