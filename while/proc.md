---
layout: math
title: Procedures
nav_order: 5
mathjax: true
parent: The While Language
---

We further extend the syntax and semantics of the While language with
__procedures__, which allow us to reuse chunks of code in controlled ways—and
without clobbering the entire memory.

Giving procedure calls semantics turns out to be a nest of bad choices. We make
the "right" ones for you, giving formal semantics for procedures where both
variables and procedures are statically scoped—the behaviour of the procedure
is defined by where it is defined, and not where it is called. We make sure
that our semantics allows recursive calls—although our procedural style still
requires a bit of care.

We generally call the language below "While with procedures", but will formally
refer to it as its own language, called Procs, with its own syntax and
semantics. We also overload notations (including meta-variables, and semantics
relations), making sure that the language they refer to is clear from
context—While being the default if nothing is specified.

## Syntax of the Procs Language

<div class="defn" markdown="1">
A __statement__ is either:
* $\texttt{skip}$;
* $\texttt{v}\;\leftarrow\;a$, where $\texttt{v}$ is a variable and $a$ is an arithmetic expression;
* $s_1;\; s_2$, where $s_1$ and $s_2$ are already statements;
* $\texttt{if}\;b\;\texttt{then}\;s_1\;\texttt{else}\;s_2$, where $b$ is a boolean expression, and $s_1$ and $s_2$ are statements;
* $\texttt{while}\;b\;\texttt{do}\;s$, where $b$ is a boolean expression and $s$ is a statement; or
* $\texttt{call}\;\texttt{p}$, where $\texttt{p}$ is a __procedure name__ (we will conflate them with variable names);
* $\texttt{begin}\;d_v\texttt{;}\;d_p\texttt{;}\;s\;\texttt{end}$, where $d_v$ is a set of __variable declarations__, $d_p$ is a set of __procedure definitions__, and $s$ is already a statement.

A set of __variable declarations__ is either:
* empty; or
* $\texttt{var x}\leftarrow a\texttt{;}\; d_v$, where \texttt{x} is a variable,
  $a$ is an arithmetic expression, and $d_v$ is already a set of variable
  declarations.

A set of __procedure definitions__ is either:
* empty; or
* $\texttt{proc p :=}\; s$, where \texttt{x} is a procedure name, and $s$ is a
  statement.

We will use $s$ and indexed variants as variables that stand for statements. We
will use $d_v$ as a variable that stands for sets of variable declarations, and
$d_p$ as a variable that stands for sets of procedure definitions.

We denote the set of Procs statements as $\mathcal{S}$, the set of
variable declarations as $\mathcal{D}_{v}$, and the set of procedure
definitions as $\mathcal{D}_{p}$.
</div>

We use the same conventions as for the simple While statements, and often use
curly braces to denote \texttt{begin} and \texttt{end}; enjoying and
overloading our existing use of curly braces in disambiguating their syntactic
interpretation to augment their semantics as well.

## Semantics of the Procs Language

Because we choose to use static scoping, it is important for us to be able to
keep track of the (static) variables and procedures used when a procedure is
defined, so we can execute its body in the correct context. This requires us to:
* split the state into:
  1. a __store__ which partially maps a set of __locations__ to values in
     $\mathbb{Z}$; (you can think of this as an array that we expand with more
     cells as we require, and into which we can index using locations)
  1. an __environment__ which keeps track of the _current_ mapping from
     variable names to locations; and
* additionally keep track of a __procedure environment__, which maps procedure
  names to:
  1. the procedure's definition—its body;
  2. the environment (mapping from variables to locations) in which the
     procedure is defined;
  3. the procedure environment in which the procedure is defined.

### Stores and Environments

This splitting is also useful to give slightly more _operational_ semantics for
Blocks. In practice, splitting things like this is particularly useful when
_compiling_ a programme, since the environments (which are typically called a
_symbol table_ in a compiler) are not needed once the programme has been
compiled.

More formally, let $\mathsf{Loc} = \mathbb{N}$ be our set of locations. We can
define stores.

<div class="defn" markdown="1">
Stores, in set $\mathsf{Store} = (\mathsf{Loc} \rightharpoonup \mathbb{Z})
\times \mathsf{Loc}$ are composed of a _partial_ function from locations to
integer values, along with the index of the _next_ free location.

Given a store $\mathit{sto} = (\mathit{st},n)$ and a location $\ell$, we write
$\mathit{sto}(\ell) \overset{\mathit{def}}{=} \mathit{st}(\ell)$ for _the value
stored in $\mathit{sto}$ at location $\ell$_,
and $\mathsf{next}(\mathit{sto}) \overset{\mathit{def}}{=} n$ for _the next
free location_. We will also write $\mathsf{new}(\mathit{sto}) = (\mathit{st}, n + 1)$
for the store whose next index has been incremented by one.
</div>

We can now define variable and procedure environments.

<div class="defn" markdown="1">
The set $\mathsf{Env}^{v}$ of __variable environments__ (or simply
environments) is the set of partial functions from variable names to locations.

Formally, $\mathsf{Env}^{v} = \mathsf{Variable} \rightharpoonup \mathsf{Loc}$.

The set $\mathsf{Env}^{p}$ of __procedure environment__ is the set of partial
functions from variable names to triples composed of a statement (the
procedure's __body__), a variable environment and a procedure environment
(together, the procedures' __environment__).

Formally,
$\mathsf{Env}^{p} = \mathsf{Variable} \rightharpoonup (\mathcal{S} \times \mathsf{Env}^{v} \times \mathsf{Env}^{p})$.
</div>

(A quick note: The definition of $\mathsf{Env}^{p}$ is Scarily Recursive™. We
will be careful to only construct environments that have finite descriptions.
In particular, recursion will be supported by inserting the procedure's body in
the procedure environment when the function is called, not when it is defined.)

### Semantics

We give big-step semantics (as a relation $\Downarrow \subseteq (\mathcal{S}
\times (\mathsf{Env}^{v}, \mathsf{Store}) \times \mathsf{Env}^{p}) \times
\mathsf{Store}$ directly. Most of the rules from the While language are almost
unchanged (simply treating the composition of store and variable environment as
a state), but rules for blocks change (in particular avoiding the "reset" of
state variables when we exit a block), and adding rules for building procedure
environments, and handling procedure calls.

We also redefine big-step semantics for variable declarations
$\Downarrow_{\mathcal{D}} \subseteq (\mathcal{D}_v \times (\mathsf{Env}^{v}
\times \mathsf{Store})) \times (\mathsf{Env}^{v} \times \mathsf{Store})$—we
essentially interpret variable declarations as creating new locations in the
store for each declared variable, ensuring that the environment is updated to
make use of these new locations for declared variables.

We finally also define big-step semantics for procedure definitions
$\Downarrow_{\mathcal{P}} \subseteq (\mathcal{D}_p \times \mathsf{Env}^{v} \times \mathsf{Env}^{p}) \times \mathsf{Env}^{p}$.

<div class="defn" markdown="1">
$$
\begin{prooftree}
\AxiomC{}
\LeftLabel{$\rlnm{BSkip}$}
\UnaryInfC{$\langle \texttt{skip}, (\mathit{env}_v, \mathit{sto}), \mathit{env}_p \rangle \Downarrow \mathit{sto}$}
\end{prooftree}
$$

<br/>

$$
\begin{prooftree}
\AxiomC{}
\LeftLabel{$\rlnm{BAss}$}
\UnaryInfC{$\langle \texttt{v}\;\leftarrow\;a, (\mathit{env}_v, \mathit{sto}), \mathit{env}_p \rangle \Downarrow \mathit{sto}[\mathit{env}_v(\texttt{v}) \mapsto ⟦a⟧^{\mathcal{A}}(\mathit{sto} \circ \mathit{env}_v)]$}
\end{prooftree}
$$

<br/>

$$
\begin{prooftree}
\AxiomC{$\langle s_1, (\mathit{env}_v, \mathit{sto}), \mathit{env}_p \rangle \Downarrow \mathit{sto}'$}
\AxiomC{$\langle s_2, (\mathit{env}_v, \mathit{sto}'), \mathit{env}_p \rangle \Downarrow \mathit{sto}''$}
\LeftLabel{$\rlnm{BSeq}$}
\BinaryInfC{$\langle s_1;\;s_2, (\mathit{env}_v, \mathit{sto}), \mathit{env}_p \rangle \Downarrow \mathit{sto}''$}
\end{prooftree}
$$

<br/>

$$
\begin{prooftree}
\AxiomC{$⟦b⟧^{\mathcal{B}}(\mathit{sto} \circ \mathit{env}_v) = \top$}
\AxiomC{$\langle s_1, (\mathit{env}_v, \mathit{sto}), \mathit{env}_p  \rangle \Downarrow \mathit{sto}'$}
\LeftLabel{$\rlnm{BIf_{\top}}$}
\BinaryInfC{$\langle \texttt{if}\;b\;\texttt{then}\;s_1\;\texttt{else}\;s_2, (\mathit{env}_v, \mathit{sto}), \mathit{env}_p \rangle \Downarrow \mathit{sto}'$}
\end{prooftree}
$$

<br/>

$$
\begin{prooftree}
\AxiomC{$⟦b⟧^{\mathcal{B}}(\mathit{sto} \circ \mathit{env}_v) = \bot$}
\AxiomC{$\langle s_2, (\mathit{env}_v, \mathit{sto}), \mathit{env}_p  \rangle \Downarrow \mathit{sto}'$}
\LeftLabel{$\rlnm{BIf_{\bot}}$}
\BinaryInfC{$\langle \texttt{if}\;b\;\texttt{then}\;s_1\;\texttt{else}\;s_2, (\mathit{env}_v, \mathit{sto}), \mathit{env}_p \rangle \Downarrow \mathit{sto}'$}
\end{prooftree}
$$

<br/>

$$
\begin{prooftree}
\AxiomC{$⟦b ⟧^{\mathcal{B}}(\mathit{sto} \circ \mathit{env}_v) = \top$}
\AxiomC{$\langle s, (\mathit{env}_v, \mathit{sto}), \mathit{env}_p \rangle \Downarrow \mathit{sto}'$}
\AxiomC{$\langle \texttt{while}\;b\;\texttt{do}\;s, (\mathit{env}_v, \mathit{sto}'), \mathit{env}_p \rangle \Downarrow \mathit{sto}''$}
\LeftLabel{$\rlnm{BWhile_{\top}}$}
\TrinaryInfC{$\langle \texttt{while}\;b\;\texttt{do}\;s, (\mathit{env}_v, \mathit{sto}), \mathit{env}_p \rangle \Downarrow \mathit{sto}''$}
\end{prooftree}
$$

<br/>

$$
\begin{prooftree}
\AxiomC{$⟦b ⟧^{\mathcal{B}}(\mathit{sto} \circ \mathit{env}_v) = \bot$}
\LeftLabel{$\rlnm{BWhile_{\bot}}$}
\UnaryInfC{$\langle \texttt{while}\;b\;\texttt{do}\;s, (\mathit{env}_v, \mathit{sto}), \mathit{env}_p \rangle \Downarrow \mathit{sto}$}
\end{prooftree}
$$

<br/>

$$
\begin{prooftree}
\AxiomC{$\langle d_v, (\mathit{env}_v, \mathit{sto}) \rangle \Downarrow_{\mathcal{D}} (\mathit{env}'_v, \mathit{sto}')$}
\AxiomC{$\langle d_p, \mathit{env}'_v, \mathit{env}_p \rangle \Downarrow_{\mathcal{P}} \mathit{env}'_p$}
\AxiomC{$\langle s, (\mathit{env}'_v, \mathit{sto}), \mathit{env}'_p \rangle \Downarrow \mathit{sto}''$}
\LeftLabel{$\rlnm{BBlock}$}
\TrinaryInfC{$\langle \texttt{begin}\;d_v\texttt{;}\;d_p\texttt{;}\;s\;\texttt{end}, (\mathit{env}_v, \mathit{sto}), \mathit{env}_p \rangle \Downarrow \mathit{sto}''$}
\end{prooftree}
$$

<br/>

$$
\begin{prooftree}
\AxiomC{$\langle s, (\mathit{env}'_v, \mathit{sto}), \mathit{env}'_p[\texttt{p} \mapsto (s, \mathit{env}'_v, \mathit{env}'_p)] \rangle \Downarrow \mathit{sto}'$}
\LeftLabel{$\rlnm{BCall}$}
\RightLabel{$\qquad$ where $(s, \mathit{env}'_v, \mathit{env}'_p) = \mathit{env}_p(\texttt{p})$}
\UnaryInfC{$\langle \texttt{call p}, (\mathit{env}_v, \mathit{sto}), \mathit{env}_p \rangle \Downarrow \mathit{sto}'$}
\end{prooftree}
$$

<br/>

$$
\begin{prooftree}
\AxiomC{$\langle d_v, (\mathit{env}_v[\texttt{x} \mapsto \mathsf{next}(\mathit{sto})], \mathsf{new}(\mathit{sto}[\mathsf{next}(\mathit{sto}) \mapsto ⟦a⟧^{\mathcal{A}}(\mathit{sto} \circ \mathit{env}_v)]) \rangle \Downarrow_{\mathcal{D}} (\mathit{env}'_v, \mathit{sto}')$}
\LeftLabel{$\rlnm{VCons}$}
\UnaryInfC{$\langle \texttt{var x}\leftarrow a\texttt{;}\;d_v, (\mathit{env}_v, \mathit{sto}) \rangle \Downarrow_{\mathcal{D}} (\mathit{env}'_v, \mathit{sto}')$}
\end{prooftree}
$$

<br/>

$$
\begin{prooftree}
\AxiomC{}
\LeftLabel{$\rlnm{VNil}$}
\UnaryInfC{$\langle \epsilon, (\mathit{env}_v, \mathit{sto}) \rangle \Downarrow_{\mathcal{D}} (\mathit{env}_v, \mathit{sto})$}
\end{prooftree}
$$

<br/>

$$
\begin{prooftree}
\AxiomC{$\langle d_p, \mathit{env}_v, \mathit{env}_p[\texttt{p} \mapsto (s, \mathit{env}_v, \mathit{env}_p)]) \rangle \Downarrow_{\mathcal{P}} \mathit{env}'_p$}
\LeftLabel{$\rlnm{PCons}$}
\UnaryInfC{$\langle \texttt{proc p := } s\texttt{;}\;d_p, \mathit{env}_v, \mathit{env}_p) \rangle \Downarrow_{\mathcal{P}} \mathit{env}'_p$}
\end{prooftree}
$$

<br/>

$$
\begin{prooftree}
\AxiomC{}
\LeftLabel{$\rlnm{PNil}$}
\UnaryInfC{$\langle \epsilon, \mathit{env}_v, \mathit{env}_p \rangle \Downarrow_{\mathcal{P}} \mathit{env}_p$}
\end{prooftree}
$$

</div>

With quite a significant impact on the usability of the Proc language for
anything relevant, our language is purely procedural. Procedures are not
methods or functions that take inputs and produce results; they are pieces of
code that operate on the store. To use them properly, you will therefore need
to use the store (and the environment, manipulated by defining blocks and
declaring variables) to implement some form of calling mechanism for each
procedure you define—this is particularly important when using recursion.
