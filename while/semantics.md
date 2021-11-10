---
layout: math
title: Small-Step Semantics of the While language
nav_order: 3
mathjax: true
parent: The While Language
---

The small-step semantics of the While language associate a transition system to every valid While program. That transition system is defined over _semantic configurations_, which include the program itself, and its _state_.

# States

A _program state_ is a map $\sigma \in \mathsf{Var} \rightarrow \mathbb{Z}$ (where $\mathsf{Var}$ is the set of valid variable identifiers). We often represent program states graphically as simple tables. For example, the following state stores value $2$ in variable <tt>x</tt> and value $42$ in variable <tt>n</tt>.

| <tt>x</tt> | 2  |
| <tt>n</tt> | 42 |

In formulas, we represent states as partially-specified functions in square brackets. The same state as above will be denoted as below, for example.

$$
\left[
  \begin{alignat}{1}
    \texttt{x} & \mapsto\ 2\\
    \texttt{n} & \mapsto\ 42
  \end{alignat}
\right]
$$

If $\sigma$ is a state and <tt>x</tt> a variable name, then $\sigma(\texttt{x})$ is the value of variable <tt>x</tt> in $\sigma$. When that variable is not specified in the state, its value is $0$. For example, with $\sigma$ the example state from above, $\sigma(\texttt{x}) = 2$, and $\sigma(\texttt{y}) = 0$.

If $\sigma$ is a state, <tt>x</tt> a variable name and $n \in \mathbb{Z}$ an integer value, then $\left[\texttt{x} \mapsto n\right]\sigma$ is the state that results from storing value $n$ in variable <tt>x</tt>. For example, with $\sigma$ the example state from above, the state $\left[\texttt{x} \mapsto 8\right]\sigma$ is shown below.

$$
\left[
  \begin{alignat}{1}
    \texttt{x} & \mapsto\ 8\\
    \texttt{n} & \mapsto\ 42
  \end{alignat}
\right]
$$

# Semantics of Arithmetic Expressions

We define the semantics of arithmetic expressions inductively over the structure of arithmetic expressions as a function $⟦\cdot⟧_{\mathcal{A}} \in E \rightarrow \mathsf{State} \rightarrow \mathbb{Z}$: the meaning of an expression is a function from states to integer values.

$$
\begin{alignat}{1}
⟦i⟧_{\mathcal{A}}(\sigma)          & = i\\
⟦\texttt{x}⟧_{\mathcal{A}}(\sigma) & = \sigma(\texttt{x})\\
⟦e_1 + e_2⟧_{\mathcal{A}}(\sigma)  & = ⟦e_1⟧_{\mathcal{A}}(\sigma) + ⟦e_2⟧_{\mathcal{A}}(\sigma)\\
⟦e_1 - e_2⟧_{\mathcal{A}}(\sigma)  & = ⟦e_1⟧_{\mathcal{A}}(\sigma) - ⟦e_2⟧_{\mathcal{A}}(\sigma)\\
⟦e_1 * e_2⟧_{\mathcal{A}}(\sigma)  & = ⟦e_1⟧_{\mathcal{A}}(\sigma) \cdot ⟦e_2⟧_{\mathcal{A}}(\sigma)
\end{alignat}
$$

# Semantics of Boolean Expressions

Similarly, the semantics of boolean expressions is defined inductively over the structure of boolean expressions as a function $⟦\cdot⟧_{\mathcal{B}} \in B \rightarrow \mathsf{State} \rightarrow \left\\{\top,\bot\right\\}$: the meaning of a boolean expression is a function from states to boolean values. The state is used only in evaluating arithmetic expressions that appear in comparisons.

$$
\begin{alignat}{1}
⟦\texttt{true}⟧_{\mathcal{B}}(\sigma)      & = \top\\
⟦\texttt{false}⟧_{\mathcal{B}}(\sigma)     & = \bot\\
⟦!b⟧_{\mathcal{B}}(\sigma)                 & = ¬⟦b⟧_{\mathcal{B}}(\sigma)\\
⟦b_1 \wedge b_2⟧_{\mathcal{B}}(\sigma)     & = ⟦b_1⟧_{\mathcal{B}}(\sigma) \wedge ⟦b_2⟧_{\mathcal{B}}(\sigma)\\
⟦e_1 = e_2⟧_{\mathcal{B}}(\sigma)          & = ⟦e_1⟧_{\mathcal{A}}(\sigma) = ⟦e_2⟧_{\mathcal{A}}(\sigma)\\
⟦e_1 ≤ e_2⟧_{\mathcal{B}}(\sigma)          & = ⟦e_1⟧_{\mathcal{A}}(\sigma) ≤ ⟦e_2⟧_{\mathcal{A}}(\sigma)
\end{alignat}
$$

# Semantics of Statements and Programs

The small-step semantics of statements is given as a transition relation $\Rightarrow\in (\mathsf{Statement} \times \mathsf{State}) \times (\mathsf{Statement} \times \mathsf{State})$ over configurations that contain a statement and a state. The statement component keeps track of the program steps that remain to execute. The state keeps track of the current contents of variables.

The relation $\Rightarrow$ is the smallest relation such that:
- For any variable <tt>x</tt>, expression $e$ and state $\sigma$, $\left\langle \texttt{x} := e, \sigma\right\rangle \Rightarrow \left\langle \texttt{skip}, \left[\texttt{x} \mapsto ⟦e⟧_{\mathcal{A}}(\sigma)\right]\sigma\right\rangle$;
- For any boolean expression $b$, statements $s_1$ and $s_2$, and state $\sigma$:
  + if $⟦b⟧_{\mathcal{B}}(\sigma) = \top$, then $\left\langle \texttt{if}\ b\ \texttt{then}\ s_1\ \texttt{else}\ s_2, \sigma\right\rangle \Rightarrow \left\langle s_1, \sigma\right\rangle$;
  + if $⟦b⟧_{\mathcal{B}}(\sigma) = \bot$, then $\left\langle \texttt{if}\ b\ \texttt{then}\ s_1\ \texttt{else}\ s_2, \sigma\right\rangle \Rightarrow \left\langle s_2, \sigma\right\rangle$;
- For any boolean expression $b$, statement $s$, and state $\sigma$:
  + if $⟦b⟧_{\mathcal{B}}(\sigma) = \top$, then $\left\langle \texttt{while}\ b\ s, \sigma\right\rangle \Rightarrow \left\langle s; \texttt{while}\ b\ s, \sigma\right\rangle$;
  + if $⟦b⟧_{\mathcal{B}}(\sigma) = \bot$, then $\left\langle \texttt{while}\ b\ s, \sigma\right\rangle \Rightarrow \left\langle \texttt{skip}, \sigma\right\rangle$; and
- For any two statements $s_1$ and $s_2$ and state $\sigma$:
  + if $s_1 = \texttt{skip}$, then $\left\langle s_1; s_2, \sigma\right\rangle \Rightarrow \left\langle s_2, \sigma\right\rangle$;
  + for every state $\sigma'$ such that $\left\langle s_1, \sigma\right\rangle \Rightarrow \left\langle \texttt{skip}, \sigma'\right\rangle$, we have $\left\langle s_1; s_2, \sigma\right\rangle \Rightarrow \left\langle s_2, \sigma'\right\rangle$; and
  + for every statement $s'_1$ and state $\sigma'$ such that $\left\langle s_1, \sigma\right\rangle \Rightarrow \left\langle s'_1, \sigma'\right\rangle$, we have $\left\langle s_1; s_2, \sigma\right\rangle \Rightarrow \left\langle s'_1; s_2, \sigma'\right\rangle$.

This defines a transition system, and we use all notations and concepts introduced in their context. In particular, we will consider the traces produced when execution a given program $S$ in a given initial state $\sigma$, and will often refer to these as _execution traces of $S$ in $\sigma$_.

Configurations of the form $\left\langle \texttt{skip}, \sigma\right\rangle$ for some state $\sigma$ are called _final configurations_. They are exactly the terminal configurations of our transition systems (although we need to prove this).

It also turns out that the definition above, although presented as a relation, is in fact a partial function (so the semantics of While is deterministic). We also need to prove this.

# Other Semantics

Small-step semantics define a transition system for each While program. Other semantics exist, that focus only on describing the input-output relation defined by a program (big-step semantics), for example.
