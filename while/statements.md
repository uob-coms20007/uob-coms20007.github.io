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

### Semantics

We can now give meaning to sstatements. We give "small-step" operational
semantics, which describe a single computation step from given configurations.

<div class="defn" markdown="1">
The set of semantic configurations for the While language is $\mathcal{S} \times \mathsf{State}$
</div>

<div class="defn" markdown="1">
We define the small step semantics of the While language as a relation
$\rightarrow \in (\mathcal{S} \times \mathsf{State}) \times (\mathcal{S} \times \mathsf{State})$,
using the following rules.

$$
\begin{prooftree}
\AxiomC{}
\LeftLabel{$\rlnm{SSkip}$}
\UnaryInfC{$\left\langle\texttt{skip}, \sigma\right\rangle \rightarrow \left\langle\texttt{skip}, \sigma\right\rangle$}
\end{prooftree}
\qquad\qquad
\begin{prooftree}
\AxiomC{}
\LeftLabel{$\rlnm{SAss}$}
\BinaryInfC{$\left\langle\texttt{v}\;\leftarrow\;a, \sigma\right\rangle \rightarrow \left\langle\texttt{skip}, \sigma[\texttt{v} \mapsto \left\llbracket a \right\rrbracket^{\mathcal{A}}(\sigma)] \right\rangle$}
\end{prooftree}
$$

<br/>

$$
\begin{prooftree}
\AxiomC{}
\LeftLabel{$\rlnm{SSeqSkip}$}
\UnaryInfC{$\left\langle\texttt{skip};\;s, \sigma\right\rangle \rightarrow \left\langle s, \sigma\right\rangle$}
\end{prooftree}
\qquad\qquad
\begin{prooftree}
\AxiomC{$\left\langle s_1, \sigma\right\rangle \rightarrow \left\langle s_1', \sigma'\right\rangle$}
\LeftLabel{$\rlnm{SSeqStep}$}
\BinaryInfC{$\left\langle s_1;\;s_2, \sigma\right\rangle \rightarrow \left\langle s_1';\;s_2, \sigma' \right\rangle$}
\end{prooftree}
$$

<br/>

$$
\begin{prooftree}
\AxiomC{\left\llbracket b \right\rrbracket^{\mathcal{B} = \top}
\LeftLabel{$\rlnm{SIfTrue}$}
\UnaryInfC{$\left\langle\texttt{if}\;b\;\texttt{then}\;s_1\;\texttt{else}\;s_2, \sigma\right\rangle \rightarrow \left\langle s_1, \sigma\right\rangle$}
\end{prooftree}
\qquad\qquad
\begin{prooftree}
\AxiomC{\left\llbracket b \right\rrbracket^{\mathcal{B} = \bot}
\LeftLabel{$\rlnm{SIfFalse}$}
\UnaryInfC{$\left\langle\texttt{if}\;b\;\texttt{then}\;s_1\;\texttt{else}\;s_2, \sigma\right\rangle \rightarrow \left\langle s_2, \sigma\right\rangle$}
\end{prooftree}
$$

<br/>

$$
\begin{prooftree}
\AxiomC{\left\llbracket b \right\rrbracket^{\mathcal{B} = \top}
\LeftLabel{$\rlnm{SWhileTrue}$}
\UnaryInfC{$\left\langle\texttt{while}\;b\;\texttt{do}\;s, \sigma\right\rangle \rightarrow \left\langle s;\;\texttt{while}\;b\;\texttt{do}\;s, \sigma\right\rangle$}
\end{prooftree}
\qquad\qquad
\begin{prooftree}
\AxiomC{\left\llbracket b \right\rrbracket^{\mathcal{B} = \bot}
\LeftLabel{$\rlnm{SWhileFalse}$}
\UnaryInfC{$\left\langle\texttt{while}\;b\;\texttt{do}\;s, \sigma\right\rangle \rightarrow \left\langle \texttt{skip}, \sigma\right\rangle$}
\end{prooftree}
$$
</div>

As with previous small-step semantics (for regular expressions, for example),
we consider execution traces—sequences of configurations where each pair of
successive confiigurations are related by $\rightarrow$.

We consider a set of __terminal configurations__, which help us decide when
execution has finished. For the While language, all configurations of the form
$\left\langle \texttt{skip}, \sigma \right\rangle$ are terminal.

A __complete__ trace is either a finite trace that ends in a terminal
configuration, or an infinite trace.
