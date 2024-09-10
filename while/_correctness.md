---
layout: math
title: Correctness
nav_order: 4
mathjax: true
parent: The While Language
---

# Correctness of While programmes

But with proper language semantics, we can go a lot further than just proving
(or trying to prove—I must keep foreshadowing how bad things are about to get)
liveness and safety. In addition to proving that a programme doesn't do the
wrong thing, we can also try to prove that a programme does the right thing (at
least in a subset of its possible initial states).

This is known as _correctness_ (or functional correctness). There are typically
two versions: _partial_ correctness, which requires that all executions that
terminate are correct; and _total_ correctness, which requires that all
executions terminate and are correct. You will likely be able to draw parallels
with both of these properties during Alex's lectures on computability.

But we're not going to go there yet, we are instead going to stick with our
friend $\texttt{euclid}$ to try and prove that it is (partially) correct.

## Correctness for simple While programmes

$$
\begin{aligned}
& \texttt{q \(\leftarrow\) 0;}\\
& \texttt{r \(\leftarrow\) a;}\\
& \texttt{while (b <= r)}\\
& \left\lfloor~~\begin{aligned}
  & \texttt{r \(\leftarrow\) r - b;}\\
  & \texttt{q \(\leftarrow\) q + 1}
  \end{aligned}\right.
\end{aligned}
$$

We are interested in proving that, whenever some state $\sigma$ is such that $0
< \sigma(\texttt{b})$ and we have some state $\sigma'$ such that $\left\langle
\texttt{euclid}, \sigma \right\rangle \Downarrow \sigma'$, then $\sigma'$ is
such that the following condition holds. (Since it is a condition that holds at
the end of an execution, we call it a _postcondition_.)

$$\sigma(\texttt{a}) = \sigma(\texttt{b}) \cdot \sigma'(\texttt{q}) + \sigma'(\texttt{r})$$

(A note: this is partial correctness: we _assume_ that $\sigma'$ exists.)

As we did with safety, let's check some simple derivations. First, let's
consider the initial state

$$\sigma = \left[\begin{aligned} \texttt{a} & \mapsto 0\\ \texttt{b} & \mapsto 1 \end{aligned}\right]$$

$$
\begin{prooftree}
\AxiomC{}
\LeftLabel{\((\mathsf{BAss})\)}
\UnaryInfC{\(\left\langle \texttt{q \(\leftarrow\) 0}, \sigma \right\rangle \Downarrow \sigma[\texttt{q} \mapsto 0]\)}
\AxiomC{}
\LeftLabel{\((\mathsf{BAss})\)}
\UnaryInfC{\(\left\langle \texttt{r \(\leftarrow\) a}, \sigma[\texttt{q} \mapsto 0] \right\rangle \Downarrow \sigma[\texttt{q} \mapsto 0; \texttt{r} \mapsto 0]\)}
\AxiomC{\(⟦\texttt{b <= r}⟧^{\mathcal{B}}(\ldots) = \bot\)}
\LeftLabel{\((\mathsf{BWhile}_{\bot})\)}
\UnaryInfC{\(\left\langle \texttt{while (b <= r) ...}, \sigma[\texttt{q} \mapsto 0; \texttt{r} \mapsto 0] \right\rangle \Downarrow \sigma[\texttt{q} \mapsto 0; \texttt{r} \mapsto 0]\)}
\LeftLabel{\((\mathsf{BSeq})\)}
\BinaryInfC{\(\left\langle \texttt{r \(\leftarrow\) a; ...}, \sigma[\texttt{q} \mapsto 0] \right\rangle \Downarrow \sigma[\texttt{q} \mapsto 0; \texttt{r} \mapsto 0]\)}
\LeftLabel{\((\mathsf{BSeq})\)}
\BinaryInfC{\(\left\langle \texttt{euclid}, \sigma \right\rangle \Downarrow \sigma[\texttt{q} \mapsto 0; \texttt{r} \mapsto 0]\)}
\end{prooftree}
$$

Already, we observe that—whatever the initial values of $\texttt{a}$ and
$\texttt{b}$ in the initial state $\sigma$, the state after the programme's
preamble $\texttt{q} \leftarrow \texttt{0; r} \leftarrow \texttt{a}$ will always be
$\sigma_0 = \sigma[\texttt{q} \mapsto 0; \texttt{r} \mapsto \sigma(\texttt{a})]$, and it is clearly the case that
$\sigma(\texttt{a}) = \sigma(\texttt{b}) \cdot \sigma_0(\texttt{q}) + \sigma_0(\texttt{r}) = \sigma(\texttt{b}) \cdot 0 + \sigma(\texttt{a})$.

So our desired postcondition holds after the preamble—and therefore also at the
end of an execution that does not enter the loop.

Let's consider now the shape of a derivation that _does_ enter the loop, for
example when starting from the following initial state.

$$\sigma = \left[\begin{aligned} \texttt{a} & \mapsto 11\\ \texttt{b} & \mapsto 5 \end{aligned}\right]$$

$$
\begin{prooftree}
\AxiomC{}
\LeftLabel{\((\mathsf{BAss})\)}
\UnaryInfC{\(\left\langle \texttt{q \(\leftarrow\) 0}, \sigma \right\rangle \Downarrow \sigma[\texttt{q} \mapsto 0]\)}
\AxiomC{}
\LeftLabel{\((\mathsf{BAss})\)}
\UnaryInfC{\(\left\langle \texttt{r \(\leftarrow\) a}, \sigma[\texttt{q} \mapsto 0] \right\rangle \Downarrow \sigma[\texttt{q} \mapsto 0; \texttt{r} \mapsto 11]\)}
\AxiomC{\(⟦\texttt{b <= r}⟧^{\mathcal{B}}(\ldots) = \top\)}
\AxiomC{...}
\AxiomC{...}
\LeftLabel{\((\mathsf{BWhile}_{\top})\)}
\TrinaryInfC{\(\left\langle \texttt{while (b <= r) ...}, \sigma[\texttt{q} \mapsto 0; \texttt{r} \mapsto 11] \right\rangle \Downarrow \sigma[\texttt{q} \mapsto 2; \texttt{r} \mapsto 1]\)}
\LeftLabel{\((\mathsf{BSeq})\)}
\BinaryInfC{\(\left\langle \texttt{r \(\leftarrow\) a; ...}, \sigma[\texttt{q} \mapsto 0] \right\rangle \Downarrow \sigma[\texttt{q} \mapsto 2; \texttt{r} \mapsto 1]\)}
\LeftLabel{\((\mathsf{BSeq})\)}
\BinaryInfC{\(\left\langle \texttt{euclid}, \sigma \right\rangle \Downarrow \sigma[\texttt{q} \mapsto 2; \texttt{r} \mapsto 1]\)}
\end{prooftree}
$$

Focusing on the rightmost branch, and already abstracting slightly, each
"layer" (except the last one) in the derivation will have the following form,
with $\sigma_{i + 1} = \sigma_i[\texttt{q} \mapsto \sigma_i(\texttt{q}) + 1;
\texttt{r} \mapsto \sigma_i(\texttt{r}) - \sigma_i(\texttt{b})]$.

$$
\begin{prooftree}
\AxiomC{\(⟦\texttt{b <= r}⟧^{\mathcal{B}}(\ldots) = \top\)}
\AxiomC{...}
\UnaryInfC{\(\left\langle \texttt{r \(\leftarrow\) r - b; q \(\leftarrow\) q + 1}, \sigma_i \right\rangle \Downarrow \sigma_{i + 1}\)}
\AxiomC{...}
\UnaryInfC{\(\left\langle \texttt{while (b <= r) ...}, \sigma_{i + 1} \right\rangle \Downarrow \sigma'\)}
\LeftLabel{\((\mathsf{BWhile}_{\top})\)}
\TrinaryInfC{\(\left\langle \texttt{while (b <= r) ...}, \sigma_i \right\rangle \Downarrow \sigma'\)}
\end{prooftree}
$$

At this stage, we can observe that if
$\sigma_i(\texttt{a}) = \sigma_i(\texttt{b}) \cdot \sigma_i(\texttt{q}) + \sigma_i(\texttt{r})$,
then also
$\sigma_{i + 1}(\texttt{a}) = \sigma_{i + 1}(\texttt{b}) \cdot \sigma_{i + 1}(\texttt{q}) + \sigma_{i + 1}(\texttt{r}) = \sigma_i(\texttt{b}) \cdot (\sigma_i(\texttt{q}) + 1) + \sigma_i(\texttt{r}) - \sigma_i(\texttt{b})$.

In a situation like this one, we say that the predicate $I = \left\\{ \sigma
\mid \sigma(\texttt{a}) = \sigma(\texttt{b}) \cdot \sigma(\texttt{q}) +
\sigma(\texttt{r}) \right\\}$ is _invariant_ by the loop's body, or that $I$ is
a loop invariant: when we enter the loop in a state $\sigma \in I$, then all
further loop iterations will be executed in a state in $I$.

We also know (from our previous observation) that our desired postcondition
holds in $\sigma_0$—so we have an invariant that is initially true, and holds
whenever the loop condition is evaluated—including when it evaluates to $\bot$
and the loop exits. However, this invariant $I$ is _insufficient_ to prove
that our desired postcondition holds. Indeed, our desired postcondition relates
the final values of $\texttt{q}$ and $\texttt{r}$ to the _initial_ values of
$\texttt{a}$ and $\texttt{b}$.

In order to conclude, we must _strengthen_ our invariant, and consider the
following predicate (family) instead:

$$
I'_{a_0, b_0} = \left\{ \sigma \mid \sigma(\texttt{a}) = \sigma(\texttt{b}) \cdot \sigma(\texttt{q}) + \sigma(\texttt{r}) \wedge \sigma(\texttt{a}) = a_0 \wedge \sigma(\texttt{b}) = b_0\right\}
$$

Clearly, instantiating $a_0$ and $b_0$ with the actual initial values of
$\texttt{a}$ and $\texttt{b}$ make $I'$ sufficient to conclude, and it is easy
to check that the predicate strengthened thus still holds on loop entry, and is
still invariant by the loop body (since the loop body does not modify the value
of $\texttt{a}$ and $\texttt{b}$).

## Going beyond

Now, this conclusion itself is not particularly useful: as we initially noted,
the postcondition holds even if we do not enter the loop, and the programme
that always stops there is not a particularly correct division algorithm.

However, going much beyond where we are at this point would require a lot more
time, spent studying _Floyd-Hoare logic_—a set of derivation rules that
_abstract_ the semantics in a way similar to what we did on a single programme
above and enable reasoning over imperative programmes for safety, correctness
and liveness. Some variants also consider security properties.

There are alternative ways of reasoning on programmes: _symbolic execution_ is
particularly useful to find bugs (as opposed to proving their absence) and to
demonstrate how those bugs are exercised; _abstract interpretation_ is a
particularly useful framework to define and prove correct static analyses aimed
at proving safety. All these formal tools (and more!) are used at scale at
large and mature software companies such as Microsoft Research, Meta and
AWS—all of which also have in-house research teams working on developing new
languages and new verification tools.
