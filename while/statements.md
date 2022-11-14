---
layout: math
title: Statements
nav_order: 1
mathjax: true
parent: The While Language
---

# While Statements

Let us first consider the kind of arithmetic expressions you all know.

## Syntax

<div class="defn" markdown="1">
A __statement__ is either:
* $\texttt{skip}$;
* $\texttt{v}\;\leftarrow\;a$, where $\texttt{v}$ is a variable and $a$ is an arithmetic expression;
* $s_1;\; s_2$, where $s_1$ and $s_2$ are already statements;
* $\texttt{if}\;b\;\texttt{then}\;s_1\;\texttt{else}\;s_2$, where $b$ is a boolean expression, and $s_1$ and $s_2$ are statements; or
* $\texttt{while}\;b\;\texttt{do}\;s$, where $b$ is a boolean expression and $s$ is a statement.

We will use $s$ and indexed variants as variables that stand for statements.

We denote the set of simple arithmetic expressions as $\mathcal{S}$.
</div>

We assume that $\texttt{;}$ (sequential composition) is left-associative.
Composite statements should be parenthesised to avoid ambiguity. We often
follow convention and use curly braces and/or indentation for this in linear
notation.

## Semantics

We can now give meaning to sstatements. We give "small-step" operational
semantics, which describe a single computation step from given configurations.

<div class="defn" markdown="1">
The set of semantic configurations for the While language is $\mathcal{S} \times \mathsf{State}$
</div>

<div class="defn" markdown="1">
We define the small step semantics of the While language as a relation
$\rightarrow \subseteq (\mathcal{S} \times \mathsf{State}) \times (\mathcal{S} \times \mathsf{State})$,
using the following rules.

$$
\begin{prooftree}
\AxiomC{}
\LeftLabel{$\rlnm{SSkip}$}
\UnaryInfC{$\langle\texttt{skip}, \sigma\rangle \rightarrow \langle\texttt{skip}, \sigma\rangle$}
\end{prooftree}
\qquad\qquad
\begin{prooftree}
\AxiomC{}
\LeftLabel{$\rlnm{SAss}$}
\UnaryInfC{$\langle\texttt{v}\;\leftarrow\;a, \sigma\rangle \rightarrow \langle\texttt{skip}, \sigma[\texttt{v} \mapsto ⟦a⟧^{\mathcal{A}}(\sigma)] \rangle$}
\end{prooftree}
$$

<br/>

$$
\begin{prooftree}
\AxiomC{}
\LeftLabel{$\rlnm{SSeqSkip}$}
\UnaryInfC{$\langle\texttt{skip};\;s, \sigma\rangle \rightarrow \langle s, \sigma\rangle$}
\end{prooftree}
\qquad\qquad
\begin{prooftree}
\AxiomC{$\langle s_1, \sigma\rangle \rightarrow \langle s_1', \sigma'\rangle$}
\LeftLabel{$\rlnm{SSeqStep}$}
\UnaryInfC{$\langle s_1;\;s_2, \sigma\rangle \rightarrow \langle s_1';\;s_2, \sigma' \rangle$}
\end{prooftree}
$$

<br/>

$$
\begin{prooftree}
\AxiomC{$⟦b⟧^{\mathcal{B}} = \top$}
\LeftLabel{$\rlnm{SIfTrue}$}
\UnaryInfC{$\langle\texttt{if}\;b\;\texttt{then}\;s_1\;\texttt{else}\;s_2, \sigma\rangle \rightarrow \langle s_1, \sigma\rangle$}
\end{prooftree}
\qquad\qquad
\begin{prooftree}
\AxiomC{$⟦b⟧^{\mathcal{B}} = \bot$}
\LeftLabel{$\rlnm{SIfFalse}$}
\UnaryInfC{$\langle\texttt{if}\;b\;\texttt{then}\;s_1\;\texttt{else}\;s_2, \sigma\rangle \rightarrow \langle s_2, \sigma\rangle$}
\end{prooftree}
$$

<br/>

$$
\begin{prooftree}
\AxiomC{$⟦b ⟧^{\mathcal{B}} = \top$}
\LeftLabel{$\rlnm{SWhileTrue}$}
\UnaryInfC{$\langle\texttt{while}\;b\;\texttt{do}\;s, \sigma\rangle \rightarrow \langle s;\;\texttt{while}\;b\;\texttt{do}\;s, \sigma\rangle$}
\end{prooftree}
\qquad\qquad
\begin{prooftree}
\AxiomC{$⟦b ⟧^{\mathcal{B}} = \bot$}
\LeftLabel{$\rlnm{SWhileFalse}$}
\UnaryInfC{$\langle\texttt{while}\;b\;\texttt{do}\;s, \sigma\rangle \rightarrow \langle \texttt{skip}, \sigma\rangle$}
\end{prooftree}
$$
</div>

As with previous small-step semantics (for regular expressions, for example),
we consider execution traces—sequences of configurations where each pair of
successive confiigurations are related by $\rightarrow$.

We consider a set of __terminal configurations__, which help us decide when
execution has finished. For the While language, all configurations of the form
$\langle \texttt{skip}, \sigma \rangle$ are terminal.

A __complete__ trace is either a finite trace that ends in a terminal
configuration, or an infinite trace.

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
(In general, we'll also have more sleep before writing exercise sheets and use
the abstract syntax above when giving you programmes to look at.)
