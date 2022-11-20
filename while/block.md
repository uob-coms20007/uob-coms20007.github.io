---
layout: math
title: Block Structures
nav_order: 4
mathjax: true
parent: The While Language
---

We extend the syntax and semantics of the While language with __blocks__, which
allow us to declare variables whose scope is delimited, as opposed to being
global to the entire programme. This will then be a key ingredient in
augmenting the language with procedures.

We generally call the language below "While with blocks", but will formally
refer to it as its own language, called Blocks, with its own syntax and
semantics. We also overload notations (including meta-variables, and semantics
relations), making sure that the language they refer to is clear from
context—While being the default if nothing is specified.

## Syntax of the Blocks Language

<div class="defn" markdown="1">
A __statement__ is either:
* $\texttt{skip}$;
* $\texttt{v}\;\leftarrow\;a$, where $\texttt{v}$ is a variable and $a$ is an arithmetic expression;
* $s_1;\; s_2$, where $s_1$ and $s_2$ are already statements;
* $\texttt{if}\;b\;\texttt{then}\;s_1\;\texttt{else}\;s_2$, where $b$ is a boolean expression, and $s_1$ and $s_2$ are statements;
* $\texttt{while}\;b\;\texttt{do}\;s$, where $b$ is a boolean expression and $s$ is a statement; or
* $\texttt{begin}\;d_v\texttt{;}\;s\;\texttt{end}$, where $d_v$ is a set of __variable declarations__ and $s$ is already a statement.

A set of __variable declarations__ is either:
* empty; or
* $\texttt{var x}\leftarrow a\texttt{;}\; d_v$, where $\texttt{x}$ is a variable,
  $a$ is an arithmetic expression, and $d_v$ is already a set of variable
  declarations.

We will use $s$ and indexed variants as variables that stand for statements. We
will use $d_v$ as a variable that stands for sets of variable declarations.

We denote the set of Blocks statements as $\mathcal{S}$, and the set of
variable declarations as $\mathcal{D}$.
</div>

We use the same conventions as for the simple While statements, and often use
curly braces to denote \texttt{begin} and \texttt{end}; enjoying and
overloading our existing use of curly braces in disambiguating their syntactic
interpretation to augment their semantics as well.

## Semantics of the Blocks Language

We give big-step semantics (as a relation $\Downarrow \subseteq (\mathcal{S}
\times \mathsf{State}) \times \mathsf{State}$ directly, augmenting those of the
While language with simple rules for building local environments and
interpreting blocks.
%%
In order to do so, we also define a big-step semantics for variable
declarations, as a relation $\Downarrow_{\mathcal{D}} \subseteq (\mathcal{D}
\times \mathsf{State}) \times \mathsf{State}$—we essentially interpret variable
declarations as producing a new state only _some_ of which is persisted upon
exiting the current block.

<div class="defn" markdown="1">
$$
\begin{prooftree}
\AxiomC{}
\LeftLabel{$\rlnm{BSkip}$}
\UnaryInfC{$\langle \texttt{skip}, \sigma \rangle \Downarrow \sigma$}
\end{prooftree}
\qquad\qquad
\begin{prooftree}
\AxiomC{}
\LeftLabel{$\rlnm{BAss}$}
\UnaryInfC{$\langle \texttt{v}\;\leftarrow\;a, \sigma\rangle \Downarrow \sigma[\texttt{v} \mapsto ⟦a⟧^{\mathcal{A}}(\sigma)]$}
\end{prooftree}
$$

<br/>

$$
\begin{prooftree}
\AxiomC{$\langle s_1, \sigma  \rangle \Downarrow \sigma'$}
\AxiomC{$\langle s_2, \sigma' \rangle \Downarrow \sigma''$}
\LeftLabel{$\rlnm{BSeq}$}
\BinaryInfC{$\langle s_1;\;s_2, \sigma \rangle \Downarrow \sigma''$}
\end{prooftree}
$$

<br/>

$$
\begin{prooftree}
\AxiomC{$⟦b⟧^{\mathcal{B}}(\sigma) = \top$}
\AxiomC{$\langle s_1, \sigma  \rangle \Downarrow \sigma'$}
\LeftLabel{$\rlnm{BIf_{\top}}$}
\BinaryInfC{$\langle \texttt{if}\;b\;\texttt{then}\;s_1\;\texttt{else}\;s_2, \sigma \rangle \Downarrow \sigma'$}
\end{prooftree}
\qquad\qquad
\begin{prooftree}
\AxiomC{$⟦b⟧^{\mathcal{B}}(\sigma) = \bot$}
\AxiomC{$\langle s_2, \sigma \rangle \Downarrow \sigma'$}
\LeftLabel{$\rlnm{BIf_{\bot}}$}
\BinaryInfC{$\langle \texttt{if}\;b\;\texttt{then}\;s_1\;\texttt{else}\;s_2, \sigma \rangle \Downarrow \sigma'$}
\end{prooftree}
$$

<br/>

$$
\begin{prooftree}
\AxiomC{$⟦b ⟧^{\mathcal{B}}(\sigma) = \top$}
\AxiomC{$\langle s, \sigma \rangle \Downarrow \sigma'$}
\AxiomC{$\langle \texttt{while}\;b\;\texttt{do}\;s, \sigma' \rangle \Downarrow \sigma''$}
\LeftLabel{$\rlnm{BWhile_{\top}}$}
\TrinaryInfC{$\langle \texttt{while}\;b\;\texttt{do}\;s, \sigma \rangle \Downarrow \sigma''$}
\end{prooftree}
$$

<br/>

$$
\begin{prooftree}
\AxiomC{$⟦b ⟧^{\mathcal{B}}(\sigma) = \bot$}
\LeftLabel{$\rlnm{BWhile_{\bot}}$}
\UnaryInfC{$\langle \texttt{while}\;b\;\texttt{do}\;s, \sigma \rangle \Downarrow \sigma$}
\end{prooftree}
$$

<br/>

$$
\begin{prooftree}
\AxiomC{$\langle d_v, \sigma \rangle \Downarrow_{\mathcal{D}} \sigma'$}
\AxiomC{$\langle s, \sigma' \rangle \Downarrow \sigma''$}
\LeftLabel{$\rlnm{BBlock}$}
\BinaryInfC{$\langle \texttt{begin}\;d_v\texttt{;}\;s\;\texttt{end}, \sigma \rangle \Downarrow \sigma''[DV(d_v) \mapsto \sigma]$}
\end{prooftree}
$$

<br/>

$$
\begin{prooftree}
\AxiomC{$\langle d_v, \sigma[\texttt{x} \mapsto ⟦a⟧^{\mathcal{A}}(\sigma)] \rangle \Downarrow_{\mathcal{D}} \sigma'$}
\LeftLabel{$\rlnm{VCons}$}
\UnaryInfC{$\langle \texttt{var x}\leftarrow a\texttt{;}\;d_v, \sigma \rangle \Downarrow_{\mathcal{D}} \sigma'$}
\end{prooftree}
\qquad\qquad
\begin{prooftree}
\AxiomC{}
\LeftLabel{$\rlnm{VNil}$}
\UnaryInfC{$\langle \epsilon, \sigma \rangle \Downarrow_{\mathcal{D}} \sigma$}
\end{prooftree}
$$

</div>

In the definition above, we denote with $DV(d_v)$ the set of variables declared
in $d_v$, and with $\sigma[X \mapsto \sigma']$ (with $X$ a set of variables)
the state obtained by _resetting_ in $\sigma$ all variables in $X$ to their
value in $\sigma'$.

These two functions are defined below.

$$
\begin{eqnarray*}
DV(\texttt{var x}\leftarrow a\texttt{;}\; d_v) & = & {\texttt{x}} \cup DV(d_v)\\
DV(\epsilon)                                   & = & \emptyset
\end{eqnarray*}
$$

$$
\sigma[X \mapsto \sigma'](\texttt{x}) = \begin{cases} \sigma'(\texttt{x}) & \text{if $\texttt{x} \in X$}\\ \sigma(\texttt{x}) & \text{otherwise} \end{cases}
$$
