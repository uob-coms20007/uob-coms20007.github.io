---
layout: math
title: Big-Step Semantics
nav_order: 3
mathjax: true
parent: The While Language
---

# Big-Step Semantics of While

The semantics we've seen so far define __traces__ or sequences of
configurations that follow the transition relation. Most of the time—and
especially when concerned with the class of computations we can express in the
While language—we will be more interested in the relation a programme defines
between its initial and final states.

We define a new relation
$$\Downarrow \subseteq (\mathcal{S} \times \mathsf{State}) \times \mathsf{State}$$
which captures this initial-final relation.

<div class="defn" markdown="1">
Let $$s$$ be a statement, and $$\sigma$$, $$\sigma'$$ be states. We say that
the execution of $$s$$ starting in state $$\sigma$$ __terminates__ in state
$$\sigma'$$, denoted $$\langle s, \sigma \rangle \Downarrow \sigma'$$, whenever
$$\langle s, \sigma \rangle \rightarrow^{*} \langle \texttt{skip}, \sigma'
\rangle$$. (That is, whenever there exists a finite complete trace from
configuration $$\langle s, \sigma \rangle$$ to a configuration
$$\langle \texttt{skip}, \sigma' \rangle$$.)
</div>

Given this definition, we can __derive__ inference rules that characterise
relation $$\Downarrow$$, although we do not prove their correctness here.

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

</div>

Rules of this form are often known as the "big-step" semantics of a language.
Instead of a sequence of short derivations—each of which represents a single
computation step—bit-step semantics represent the entire computation as a
single derivation.

Try writing a derivation for the execution of the factorial programme with
variable \texttt{n} initially holding value $2$.

Because these rules allow us to consider a "whole" computation, they will be
useful in defining the semantics of blocks (where a "whole" block needs
evaluated in a state, parts of which get reset to their initial values at the
end) and procedures (where it is useful to not have to worry about where to go
when the procedure call terminates). These are possible to do in a "small-step"
style (this is essentially what your compiler does!), but take a lot more work.
