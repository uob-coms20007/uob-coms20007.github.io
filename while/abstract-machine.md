---
layout: math
title: The Abstract Machine and its language
nav_order: 4
mathjax: true
parent: The While Language
---

# Abstract Syntax

The abstract syntax for the Abstract Machine's language is slightly simpler
than that for the While language, although it retains a recursive (inductive)
definition since its control-flow is still structured.

The set $P$ of abstract machine programs is the smallest set such that:
- <tt>PUSH</tt> $v$ is in $P$ whenever $v \in \mathbb{Z} \cup \\{\top,\bot\\}$;
- <tt>STORE</tt> <tt>x</tt> is in $P$ whenever <tt>x</tt> is a valid While variable identifier;
- <tt>LOAD</tt> <tt>x</tt> is in $P$ whenever <tt>x</tt> is a valid While variable identifier;
- <tt>ADD</tt>, <tt>SUB</tt>, <tt>MUL</tt>, <tt>AND</tt>, <tt>NOT</tt>, <tt>EQ</tt> and <tt>LE</tt> are in $P$;
- if $P_t$ and $P_e$ are in $P$, then $\texttt{IF(}P_t\texttt{,}P_e\texttt{)}$ is in $P$; and
- if $P_c$ and $P_b$ are in $P$, then $\texttt{LOOP(}P_c\texttt{,}P_b\texttt{)}$ is in $P$.

The Haskell type for Abstract Machine ASTs groups together all binary
operations over integers to reduce code repetitions.

# Small-Step Semantics

The Abstract Machine is specified over configurations composed of a program, a
state (mapping variable names to values in $\mathbb{Z}$), and a stack of
integer and boolean values, the top of which serves as working memory both for
arithmetic operators and control-flow statements.

We use Haskell list notations for stacks (this means their top is denoted to
the left), using a lowercase letter $s$ to denote an abstract stack, and
$\epsilon$ to denote an empty stack.

The AM semantic relation $\Rightarrow$ (we overload notation here, since
configurations have a different type) is the smallest relation such that:
- $\left\langle \texttt{PUSH}\ v;\ P, s, \sigma\right\rangle \Rightarrow \left\langle P, v : s, \sigma\right\rangle$;
- $\left\langle \texttt{STORE x};\ P, i : s, \sigma\right\rangle \Rightarrow \left\langle P, s, \left[\texttt{x} \mapsto i\right]\sigma\right\rangle$ when $i \in \mathbb{Z}$;
- $\left\langle \texttt{LOAD x};\ P, s, \sigma\right\rangle \Rightarrow \left\langle P, \sigma(\texttt{x}) : s, \sigma\right\rangle$;
- $\left\langle \texttt{ADD};\ P, i_2 : i_1 : s, \sigma\right\rangle \Rightarrow \left\langle P, (i_1 + i_2) : s, \sigma\right\rangle$ when $i_1, i_2 \in \mathbb{Z}$;
- $\left\langle \texttt{SUB};\ P, i_2 : i_1 : s, \sigma\right\rangle \Rightarrow \left\langle P, (i_1 - i_2) : s, \sigma\right\rangle$ when $i_1, i_2 \in \mathbb{Z}$;
- $\left\langle \texttt{MUL};\ P, i_2 : i_1 : s, \sigma\right\rangle \Rightarrow \left\langle P, (i_1 * i_2) : s, \sigma\right\rangle$ when $i_1, i_2 \in \mathbb{Z}$;
- $\left\langle \texttt{EQ};\ P, i_2 : i_1 : s, \sigma\right\rangle \Rightarrow \left\langle P, (i_1 = i_2) : s, \sigma\right\rangle$ when $i_1, i_2 \in \mathbb{Z}$;
- $\left\langle \texttt{LE};\ P, i_2 : i_1 : s, \sigma\right\rangle \Rightarrow \left\langle P, (i_1 ≤ i_2) : s, \sigma\right\rangle$ when $i_1, i_2 \in \mathbb{Z}$;
- $\left\langle \texttt{NOT};\ P, b : s, \sigma\right\rangle \Rightarrow \left\langle P, !b : s, \sigma\right\rangle$ when $b \in \\{\top,\bot\\}$;
- $\left\langle \texttt{AND};\ P, b_2 : b_1 : s, \sigma\right\rangle \Rightarrow \left\langle P, (b_1 \wedge b_2) : s, \sigma\right\rangle$ when $b_1, b_2 \in \\{\top,\bot\\}$;
- $\left\langle \texttt{IF}(P_t, P_e);\ P, \top : s, \sigma\right\rangle \Rightarrow \left\langle P_t; P, s, \sigma\right\rangle$;
- $\left\langle \texttt{IF}(P_t, P_e);\ P, \bot : s, \sigma\right\rangle \Rightarrow \left\langle P_e; P, s, \sigma\right\rangle$;
- $\left\langle \texttt{LOOP}(P_c, P_b);\ P, s, \sigma\right\rangle \Rightarrow \left\langle P_c; \texttt{IF}(P_b; \texttt{LOOP}(P_c, P_b),), s, \sigma\right\rangle$.

## Stuck Configurations
Note that, unlike the While language, the AM language has \emph{stuck
configurations}, where the program has not been fully executed but no further
transitions are possible. This is, for example, the case when the stack is not
tall enough, or the top of the stack does not have the correct type for the
next transition to take place.

When an execution starting from some initial configuration $\left\langle P,
\epsilon, \sigma\right\rangle$ can transition to a stuck state, we say that $P$
gets stuck in state $\sigma$.

# Compiling While to the Abstract Machine

The main challenge in compiling While programs to the Abstract Machine is in
compiling expressions down. Loops also require a bit of care to avoid infinite
recursion at compile-time.

## Compiling arithmetic expressions

We define a function $\mathcal{CA}$ from While arithmetic expressions to AM
programs, inductively over the structure of the input expression:

$$
\begin{align}
\mathcal{CA}⟦i⟧          & = \texttt{PUSH}\ i\\
\mathcal{CA}⟦\texttt{x}⟧ & = \texttt{LOAD x}\\
\mathcal{CA}⟦e_1 + e_2⟧  & = \mathcal{CA}⟦e_1⟧; \mathcal{CA}⟦e_2⟧; \texttt{ADD}\\
\mathcal{CA}⟦e_1 - e_2⟧  & = \mathcal{CA}⟦e_1⟧; \mathcal{CA}⟦e_2⟧; \texttt{SUB}\\
\mathcal{CA}⟦e_1 * e_2⟧  & = \mathcal{CA}⟦e_1⟧; \mathcal{CA}⟦e_2⟧; \texttt{MUL}
\end{align}
$$

The core idea of this definition is that the program produced leaves the
existing stack untouched, but ends with the value of the expression pushed at
the top of the stack.

## Compiling boolean expressions

We define a function $\mathcal{CB}$ from While boolean expressions to AM
programs, inductively over the structure of the input expression:

$$
\begin{align}
\mathcal{CB}⟦\texttt{true}⟧  & = \texttt{PUSH}\ \top\\
\mathcal{CB}⟦\texttt{false}⟧ & = \texttt{PUSH}\ \bot\\
\mathcal{CB}⟦e_1 = e_2⟧      & = \mathcal{CA}⟦e_1⟧; \mathcal{CA}⟦e_2⟧; \texttt{EQ}\\
\mathcal{CB}⟦e_1 ≤ e_2⟧      & = \mathcal{CA}⟦e_1⟧; \mathcal{CA}⟦e_2⟧; \texttt{LE}\\
\mathcal{CB}⟦!b⟧             & = \mathcal{CB}⟦b⟧; \texttt{NOT}\\
\mathcal{CB}⟦b_1 \wedge b_2⟧ & = \mathcal{CB}⟦b_1⟧; \mathcal{CB}⟦b_2⟧; \texttt{AND}
\end{align}
$$

## Compiling statements

We define a function $\mathcal{C}$ from While statements to AM programs,
inductively over the structure of the input statement.

$$
\begin{align}
\mathcal{C}⟦\texttt{x} := e⟧                                        & = \mathcal{CA}⟦e⟧; \texttt{STORE x}\\
\mathcal{C}⟦\texttt{if}\ b\ \texttt{then}\ s_t\ \texttt{else}\ s_e⟧ & = \mathcal{CB}⟦b⟧; \texttt{IF}(\mathcal{C}⟦s_t⟧, \mathcal{C}⟦s_e⟧)\\
\mathcal{C}⟦\texttt{while}\ b\ s⟧                                   & = \texttt{LOOP}(\mathcal{CB}⟦b⟧,\mathcal{C}⟦s⟧)\\
\mathcal{C}⟦s_1; s_2⟧                                               & = \mathcal{C}⟦s_1⟧; \mathcal{C}⟦s_2⟧
\end{align}
$$
