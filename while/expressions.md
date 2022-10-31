---
layout: math
title: Expressions
nav_order: 1
mathjax: true
parent: The While Language
---

# Arithmetic Expressions

We formally define the language of *arithmetic expressions*, which we will use
in While programmes. As with regular expressions, we will define formally their
abstract syntax, then define their semantics.

## Simple Arithmetic Expressions

Let us first consider the kind of arithmetic expressions you all know.

### Syntax of Simple Arithmetic Expressions

<div class="defn" markdown="1">
A __simple arithmetic expression__ is either:
* an integer literal $\texttt{n}$ with value in $\mathbb{Z}$;
* $a_1\;\texttt{+}\;a_2$, where $a_1$ and $a_2$ are already arithmetic expressions;
* $a_1\;\texttt{-}\;a_2$, where $a_1$ and $a_2$ are already arithmetic expressions; or
* $a_1\;\texttt{*}\;a_2$, where $a_1$ and $a_2$ are already arithmetic expressions.

We will use $a$ and indexed variants as variables that stand for arithmetic
expressions, and $\texttt{n}$ and indexed variants to stand for integer literals.

We denote the set of simple arithmetic expressions as $\mathcal{A}^{-}$.
</div>

Before we go further, it is important to note that symbols $\texttt{+}$,
$\texttt{-}$ and $\texttt{\*}$ in the definition above are part of the *syntax*
of expressions, they are *not* integer operations.

Formally, integer literals are also syntactic strings recognised by $(- \|
\epsilon)\texttt{[0-9]}^{+}$ (where $\|$ stands for alternation—to avoid
ambiguity). We don't treat this as formally as we should in these notes, and
will simply assume that all such integer literals have the intuitive meaning as
a value in $\mathbb{Z}$. We denote with
$⟦\texttt{n}⟧^{\mathbb{Z}}$ (a value in $\mathbb{Z}$) the
meaning of integer literal $\texttt{n}$.
For example, we have $⟦\texttt{-42}⟧^{\mathbb{Z}} = -42$; and we'll try to
remain careful with fonts!

To avoid drowning in parentheses, we will here again follow some conventions:
operators are left-associative, and our multiplication operator binds more
tightly than the others (which have the same priority).

### Semantics of Simple Arithmetic Expressions

We can now give meaning to simple arithmetic expressions. Quite obviously,
their meaning will be values in $\mathbb{Z}$.

<div class="defn" markdown="1">
We define the semantics of simple arithmetic expressions as a relation
$\Downarrow_{\mathcal{A}^{-}} \in \mathcal{A}^{-} \times \mathbb{Z}$, using the
following rules.

$$
\begin{prooftree}
\AxiomC{}
\LeftLabel{$\rlnm{SVal}$}
\UnaryInfC{$\texttt{n} \Downarrow_{\mathcal{A}^{-}} ⟦\texttt{n}⟧^{\mathbb{Z}}$}
\end{prooftree}
\qquad\qquad
\begin{prooftree}
\AxiomC{$a_1 \Downarrow_{\mathcal{A}^{-}} v_1$}
\AxiomC{$a_2 \Downarrow_{\mathcal{A}^{-}} v_2$}
\LeftLabel{$\rlnm{SAdd}$}
\BinaryInfC{$a_1\;\texttt{+}\;a_2 \Downarrow_{\mathcal{A}^{-}} v_1 + v_2$}
\end{prooftree}
$$

<br/>

$$
\begin{prooftree}
\AxiomC{$a_1 \Downarrow_{\mathcal{A}^{-}} v_1$}
\AxiomC{$a_2 \Downarrow_{\mathcal{A}^{-}} v_2$}
\LeftLabel{$\rlnm{SSub}$}
\BinaryInfC{$a_1\;\texttt{-}\;a_2 \Downarrow_{\mathcal{A}^{-}} v_1 - v_2$}
\end{prooftree}
\qquad\qquad
\begin{prooftree}
\AxiomC{$a_1 \Downarrow_{\mathcal{A}^{-}} v_1$}
\AxiomC{$a_2 \Downarrow_{\mathcal{A}^{-}} v_2$}
\LeftLabel{$\rlnm{SAdd}$}
\BinaryInfC{$a_1\;\texttt{*}\;a_2 \Downarrow_{\mathcal{A}^{-}} v_1 \cdot v_2$}
\end{prooftree}
$$
</div>

Relation $\Downarrow_{\mathcal{A}^{-}}$ is in fact a total function (that is, there
is exactly one integer related to any given simple arithmetic expression): all
simple arithmetic expressions have meaning, and that meaning is unique. We can
prove this by structural induction. (Exercise sheet 5 will serve as a short
refresher for mathematical and structural induction—otherwise, check your
Logic/Introduction to Proofs, and Functional Programming notes from Year 1.)

<div class="defn" markdown="1">
$\Downarrow_{\mathcal{A}^{-}}$ is a total function. For a simple arithmetic
expression $a$ and an integer $n$, if $a \Downarrow_{\mathcal{A}^{-}} n$, we
write $⟦a⟧^{\mathcal{A}^{-}} = n$.
</div>

**Proof**

By structral induction, we prove that, for any $a \in \mathcal{A}^{-}$ there
exists a unique $v \in \mathbb{Z}$ such that $a \Downarrow_{\mathcal{A}^{-}}
n$.

- Case $a = \texttt{n}$.
  Then we have $a \Downarrow_{\mathcal{A}^{-}} ⟦\texttt{n}⟧^{\mathbb{Z}}$ (and
  no other rule applies), which concludes the base case with
  $v = ⟦\texttt{n}⟧^{\mathbb{Z}}$
- Case $a = a_1\;\texttt{+}\;a_2$. By induction hypothesis, we must have:
  + A unique $v_1 \in \mathbb{Z}$ such that $a_1 \Downarrow_{\mathcal{A}^{-}} v_1$; and
  + A unique $v_2 \in \mathbb{Z}$ such that $a_2 \Downarrow_{\mathcal{A}^{-}} v_2$.

  Therefore, by rule $\rlnm{SAdd}$, we have $a_1\;\texttt{+}\;a_2
  \Downarrow_{\mathcal{A}^{-}} v$ with $v = v_1 + v_2$. In addition,
  $\rlnm{SAdd}$ is the only rule that applies.

The other two cases are similar to the $\texttt{+}$ case.

## Arithmetic Expressions

Arithmetic expressions in a programming language are _much_ more useful if
their result can be stored and recalled from memory. We extend our simple
arithmetic expressions with _variables_.

### Syntax of Arithmetic Expressions

<div class="defn" markdown="1">
An __arithmetic expression__ is either:
* an integer literal $\texttt{n}$ with value in $\mathbb{Z}$;
* a variable $x$ in $\texttt{[a-z]}^{+}$;
* $a_1\;\texttt{+}\;a_2$, where $a_1$ and $a_2$ are already arithmetic expressions;
* $a_1\;\texttt{-}\;a_2$, where $a_1$ and $a_2$ are already arithmetic expressions; or
* $a_1\;\texttt{*}\;a_2$, where $a_1$ and $a_2$ are already arithmetic expressions.

We will use $a$ and indexed variants as variables that stand for arithmetic
expressions, and $\texttt{n}$ and indexed variants to stand for integer
literals. (Overriding the previous abbreviation: all our arithmetic expressions
from now on are non-simple.)

We denote the set of arithmetic expressions as $\mathcal{A}$.
</div>

### Semantics of Arithmetic Expressions

The meaning of (full) arithmetic expressions is more complex than that of
simple arithmetic expressions (ah!). Indeed, they do not (and in general
cannot) simply have value in $\mathbb{Z}$: variables do not have intrinsic
value.

Full arithmetic expressions must therefore be interpreted in a _state_ a
mapping from variable names to values in $\mathbb{Z}$.

<div class="defn" markdown="1">
A __state__ is a total function in $\mathsf{State} = \texttt{[a-z]}^{+} \rightarrow \mathbb{Z}$.

Given a state $\sigma$, a variable $x$ and an integer value $v$, we denote with
$[x \mapsto v]\sigma$ the function that maps $x$ to $v$, and maps all $x' \neq
x$ to $\sigma(x')$.

By convention, when a variable $x$ is not explicitly defined in state $\sigma$,
we will always take $\sigma(x) = 0$.
</div>

We can now give meaning to arithmetic expressions—the meaning of an arithmetic
expression is now a function from states to integer values. We could define it
as a relation first, then prove that it is a total function, but we won't
because we have better things to do.

<div class="defn" markdown="1">
We define the semantics of arithmetic expressions as a total function
$⟦\cdot⟧^{\mathcal{A}} \in \mathcal{A} \rightarrow \mathsf{State} \rightarrow
\mathbb{Z}$, inductively on the expression's structure.

$$
\begin{align}
⟦\texttt{n}⟧^{\mathcal{A}}(\sigma)           & = ⟦\texttt{n}⟧^{\mathbb{Z}}\\
⟦x⟧^{\mathcal{A}}(\sigma)                    & = \sigma(x)\\
⟦a_1\;\texttt{+}\;a_2⟧^{\mathcal{A}}(\sigma) & = ⟦a_1⟧^{\mathcal{A}}(\sigma) + ⟦a_2⟧^{\mathcal{A}}(\sigma)\\
⟦a_1\;\texttt{-}\;a_2⟧^{\mathcal{A}}(\sigma) & = ⟦a_1⟧^{\mathcal{A}}(\sigma) - ⟦a_2⟧^{\mathcal{A}}(\sigma)\\
⟦a_1\;\texttt{*}\;a_2⟧^{\mathcal{A}}(\sigma) & = ⟦a_1⟧^{\mathcal{A}}(\sigma) \cdot ⟦a_2⟧^{\mathcal{A}}(\sigma)
\end{align}
$$
</div>

## Boolean Expressions

### Syntax of Arithmetic Expressions

<div class="defn" markdown="1">
A __boolean expression__ is either:
* a boolean literal $\texttt{true}$ or $\texttt{false}$;
* $\texttt{!}b$, where $b$ is already a boolean expression;
* $b_1\;\texttt{&&}\;b_2$, where $b_1$ and $b_2$ are already boolean expressions;
* $b_1\;\texttt{\|\|}\;b_2$, where $b_1$ and $b_2$ are already boolean expressions;
* $a_1\;\texttt{=}\;a_2$, where $a_1$ and $a_2$ are arithmetic expressions; or
* $a_1\;\texttt{<}\;a_2$, where $a_1$ and $a_2$ are arithmetic expressions.

We will use $b$ and indexed variants as variables that stand for arithmetic
expressions.

We denote the set of boolean expressions as $\mathcal{B}$.
</div>

As before, we take the convention that binary operators are left-associative.
Unary negation takes precedence (and so negates the smallest boolean expression
that follows it).

<div class="defn" markdown="1">
We define the semantics of boolean expressions as a total function
$⟦\cdot⟧^{\mathcal{B}} \in \mathcal{B} \rightarrow \mathsf{State} \rightarrow
\mathbb{B}$, inductively on the expression's structure.

$$
\begin{align}
⟦\texttt{true}⟧^{\mathcal{B}}(\sigma)         & = \top\\
⟦\texttt{false}⟧^{\mathcal{B}}(\sigma)        & = \bot\\
⟦\texttt{!}b⟧^{\mathcal{B}}(\sigma)           & = ¬⟦b⟧^{\mathcal{B}}(\sigma)\\
⟦b_1\;\texttt{&&}\;b_2⟧^{\mathcal{B}}(\sigma) & = ⟦b_1⟧^{\mathcal{B}}(\sigma) \wedge ⟦b_2⟧^{\mathcal{B}}(\sigma)\\
⟦b_1\;\texttt{||}\;b_2⟧^{\mathcal{B}}(\sigma) & = ⟦b_1⟧^{\mathcal{B}}(\sigma) \vee ⟦b_2⟧^{\mathcal{B}}(\sigma)\\
⟦a_1\;\texttt{=}\;a_2⟧^{\mathcal{B}}(\sigma)  & = ⟦a_1⟧^{\mathcal{A}}(\sigma) = ⟦a_2⟧^{\mathcal{A}}(\sigma)\\
⟦a_1\;\texttt{<}\;a_2⟧^{\mathcal{B}}(\sigma)  & = ⟦a_1⟧^{\mathcal{A}}(\sigma) < ⟦a_1⟧^{\mathcal{A}}(\sigma)
\end{align}
$$
</div>
