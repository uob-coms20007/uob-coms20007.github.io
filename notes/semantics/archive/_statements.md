---
layout: math
title: Statements
nav_order: 1
mathjax: true
parent: The While Language
---

# While Statements



## Syntax

<div class="defn" markdown="1">
A __statement__ is a tree described by the following grammar. We exclude
strings $\texttt{if}$, $\texttt{then}$, $\texttt{else}$, and $\texttt{while}$
from the set of variable names.

$$
  S, S_1, S_2 ::= \epsilon \mid \texttt{x}\;\leftarrow\;A \mid S_1;\;S_2 \mid \texttt{if}\;B\;\texttt{then}\;S_1\;\texttt{else}\;S_2 \mid \texttt{while}\;B\;S
$$

The empty statement (also denoted $\texttt{skip}$ to avoid ambiguity in places)
is a constant. $\leftarrow$ and $\texttt{;}$ are infix binary operators.
$\texttt{while}$ is a prefix binary operator.
$\texttt{if}\;\cdot\;\texttt{then}\;\cdot\;\texttt{else}\;\cdot$ is a mixfix
ternary operator.

We will use $S$ and indexed variants as variables that stand for statements.

We denote the set of simple arithmetic expressions as $\mathcal{S}$.
</div>

We assume that $\texttt{;}$ (sequential composition) is right-associative.
Composite statements should be parenthesised to avoid ambiguity, and we do not
define conventions here. We often follow convention and use curly braces and/or
new lines and indentation for this instead of purely linear notation.

## Semantics

We can now give meaning to statements. We give "big-step" operational
semantics, which describe the relation between initial and final
configurations—when they exist!

<div class="defn" markdown="1">
The set of semantic configurations for the While language is $\mathcal{S} \times \mathsf{State}$
</div>

We define a relation
$$\Downarrow \subseteq (\mathcal{S} \times \mathsf{State}) \times
\mathsf{State}$$ using the following inference rules.

<div class="defn" markdown="1">
$$
\begin{prooftree}
\AxiomC{}
\LeftLabel{$\rlnm{BSkip}$}
\UnaryInfC{$\langle \epsilon, \sigma \rangle \Downarrow \sigma$}
\end{prooftree}
\qquad\qquad
\begin{prooftree}
\AxiomC{}
\LeftLabel{$\rlnm{BAss}$}
\UnaryInfC{$\langle \texttt{x}\;\leftarrow\;A, \sigma\rangle \Downarrow \sigma[\texttt{x} \mapsto ⟦A⟧^{\mathcal{A}}(\sigma)]$}
\end{prooftree}
$$

<br/>

$$
\begin{prooftree}
\AxiomC{$\langle S_1, \sigma  \rangle \Downarrow \sigma'$}
\AxiomC{$\langle S_2, \sigma' \rangle \Downarrow \sigma''$}
\LeftLabel{$\rlnm{BSeq}$}
\BinaryInfC{$\langle S_1;\;S_2, \sigma \rangle \Downarrow \sigma''$}
\end{prooftree}
$$

<br/>

$$
\begin{prooftree}
\AxiomC{$⟦B⟧^{\mathcal{B}}(\sigma) = \top$}
\AxiomC{$\langle S_1, \sigma  \rangle \Downarrow \sigma'$}
\LeftLabel{$\rlnm{BIf_{\top}}$}
\BinaryInfC{$\langle \texttt{if}\;B\;\texttt{then}\;S_1\;\texttt{else}\;S_2, \sigma \rangle \Downarrow \sigma'$}
\end{prooftree}
\qquad\qquad
\begin{prooftree}
\AxiomC{$⟦B⟧^{\mathcal{B}}(\sigma) = \bot$}
\AxiomC{$\langle S_2, \sigma \rangle \Downarrow \sigma'$}
\LeftLabel{$\rlnm{BIf_{\bot}}$}
\BinaryInfC{$\langle \texttt{if}\;B\;\texttt{then}\;S_1\;\texttt{else}\;S_2, \sigma \rangle \Downarrow \sigma'$}
\end{prooftree}
$$

<br/>

$$
\begin{prooftree}
\AxiomC{$⟦B⟧^{\mathcal{B}}(\sigma) = \top$}
\AxiomC{$\langle S, \sigma \rangle \Downarrow \sigma'$}
\AxiomC{$\langle \texttt{while}\;B\;S, \sigma' \rangle \Downarrow \sigma''$}
\LeftLabel{$\rlnm{BWhile_{\top}}$}
\TrinaryInfC{$\langle \texttt{while}\;B\;S, \sigma \rangle \Downarrow \sigma''$}
\end{prooftree}
$$

<br/>

$$
\begin{prooftree}
\AxiomC{$⟦B⟧^{\mathcal{B}}(\sigma) = \bot$}
\LeftLabel{$\rlnm{BWhile_{\bot}}$}
\UnaryInfC{$\langle \texttt{while}\;B\;S, \sigma \rangle \Downarrow \sigma$}
\end{prooftree}
$$

</div>

# Syntactic Sugar

The While language—in the abstract syntax we introduced—is very limited. This
is good when proving things about the language (less cases to consider), but is
not ideal when trying to write complex programmes.

As with regular expressions, we can define syntactic sugar—notations or
abbreviations that allow us to express very simply some common programming
patterns _without_ extending the abstract syntax. We'll tend to introduce such
abbreviations with an $\equiv$ symbol, which we'll use regardless of the object
we're defining an abbreviation for (arithmetic or boolean expression, or
statement).

For example, we could use syntactic sugar to define (true) for loops, which
iterate through a piece of code a fixed number of time.

$$
\begin{aligned}
  \begin{aligned}
  &\texttt{for}\;\texttt{v}\;\texttt{from}\;\texttt{n}_1\;\texttt{upto}\;\texttt{n}_2\;\texttt{by}\;\texttt{n}_3\;\texttt{do}\;s
  \end{aligned}
&
  \equiv
&
  \begin{aligned}
  &\texttt{v} \leftarrow \texttt{n}_1;\\
  &\texttt{while}\;(\texttt{v} < \texttt{n}_2 + 1)\;\{\\
  &\quad s\\
  &\quad\texttt{v} \leftarrow \texttt{v} + \texttt{n}_3\\
  &\}
  \end{aligned}
\end{aligned}
$$

In general, we'll be expecting programmes in the abstract syntax above, and
will explicitly relax requirements when we expect something else.
(In general, we'll also sleep before writing exercise sheets and use the
abstract syntax above when giving you programmes to look at. But sometimes we
won't, and we ask for your understanding—you will certainly have ours.)
