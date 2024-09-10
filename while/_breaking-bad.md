---
layout: math
title: Liveness and Safety
nav_order: 3
mathjax: true
parent: The While Language
---

# Liveness, Safety and Breaking Bad

The While language is powerful (you will see exactly how powerful with Alex).
The While language is also simple and boring. These are good properties to have
when trying to reason about programming languages and about programmes written
in them.

We've done a bit of reasoning about the language itself in the last problem
sheet. We're now going to reason a bit about programmes.

The most interesting concrete thing that having formal semantics for the
language enables us to do is to reason about the _safety_ and _correctness_ of
programmes.

Safety, the topic of this lecture, is the most basic property we can consider:
given a programme, we want to identify the initial states in which that
programme has a meaning. (More formally, given $s \in \mathcal{S}$, find
$\phi \subseteq \mathsf{State}$ such that,
for any $\sigma \in \phi$, there is $\sigma' \in \mathsf{State}$ and
$\left\langle s, \sigma \right\rangle \Downarrow \sigma'$.)[^safetynote]

## Safety for simple While programmes

Let's consider the following programme, which we decide to call
$\texttt{euclid}$ because we are people of culture.

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

There is an obvious predicate $\phi$ such that any initial state in $\phi$ will
lead to a terminating execution of $\texttt{euclid}$: $\phi = \emptyset$. It is
not a very interesting example, however.

Much less obviously, there also exists at least one initial state that does not
lead to a terminating execution of $\texttt{euclid}$. For example, here are two:

$$
\sigma_0 =
\left[
\begin{align*}
\texttt{a} & \mapsto 42\\
\texttt{b} & \mapsto 0
\end{align*}
\right]
$$

$$
\sigma_1 = 
\left[
\begin{align*}
\texttt{a} & \mapsto 97\\
\texttt{b} & \mapsto -72
\end{align*}
\right]
$$

How can we convince ourselves that executing $\texttt{euclid}$ in these two
states does indeed not terminate? How can we then delimit (or approximate) the
set of states that do lead to terminating executions of $\texttt{euclid}$? If
we approximate this set of states, in which direction should we do so? (For
example, our first approximation above—a coarse underapproximation, was not
particularly useful at first glance. But would the equally coarse
overapproximation $\phi = \mathsf{State}$ be more useful?)

### Reasoning about non-termination

Let us try and show that there is no state $\sigma'$ such that
$\left\langle \texttt{euclid}, \sigma_0 \right\rangle \Downarrow \sigma'$.

By contradiction, let us assume such a $\sigma'$ exists. Then it must be that
there exists a big step derivation for the following.
$$\left\langle \texttt{euclid}, \sigma_0 \right\rangle \Downarrow \sigma'$$

That derivation must necessarily be rooted as follows.

$$
\begin{prooftree}
\AxiomC{}
\LeftLabel{\(\rlnm{BAss}\)}
\UnaryInfC{\(\left\langle \texttt{q \(\leftarrow\) 0}, \sigma_0 \right\rangle
           \Downarrow \left[\begin{aligned}
                            \texttt{a} & \mapsto 42\\
                            \texttt{b} & \mapsto 0\\
                            \texttt{q} & \mapsto 0
                      \end{aligned}\right]\)}
\AxiomC{}
\LeftLabel{\(\rlnm{BAss}\)}
\UnaryInfC{\(\left\langle \texttt{r \(\leftarrow\) a},
                          \left[\begin{aligned}
                                \texttt{a} & \mapsto 42\\
                                \texttt{b} & \mapsto 0\\
                                \texttt{q} & \mapsto 0
                                \end{aligned}
                          \right]
            \right\rangle \Downarrow
              \left[\begin{aligned}
                    \texttt{a} & \mapsto 42\\
                    \texttt{b} & \mapsto 0\\
                    \texttt{q} & \mapsto 0\\
                    \texttt{r} & \mapsto 42
                    \end{aligned}
              \right]\)}
\AxiomC{D}
\LeftLabel{\(\rlnm{BSeq}\)}
\BinaryInfC{\(\left\langle \begin{aligned}
                           & \texttt{r \(\leftarrow\) a;}\\
                           & \texttt{while (b <= r)}\\
                           & \left\lfloor~~
                               \begin{aligned}
                               & \texttt{r \(\leftarrow\) r - b;}\\
                               & \texttt{q \(\leftarrow\) q + 1}
                               \end{aligned}
                             \right.
                           \end{aligned},
                           \left[\begin{aligned}
                                 \texttt{a} & \mapsto 42\\
                                 \texttt{b} & \mapsto 0\\
                                 \texttt{q} & \mapsto 0
                                 \end{aligned}
                           \right]
              \right\rangle \Downarrow \sigma'\)}
\LeftLabel{\(\rlnm{BSeq}\)}
\BinaryInfC{\(\left\langle \texttt{euclid}, \sigma_0 \right\rangle \Downarrow \sigma'\)}
\end{prooftree}
$$

In the above $D$ stands for some derivation of

$$
\left\langle \begin{aligned}
              & \texttt{while (b <= r)}\\
              & \left\lfloor~~
                  \begin{aligned}
                  & \texttt{r \(\leftarrow\) r - b;}\\
                  & \texttt{q \(\leftarrow\) q + 1}
                  \end{aligned}
                \right.
              \end{aligned},
              \left[\begin{aligned}
                    \texttt{a} & \mapsto 42\\
                    \texttt{b} & \mapsto 0\\
                    \texttt{q} & \mapsto 0\\
                    \texttt{r} & \mapsto 42
                    \end{aligned}
              \right]
 \right\rangle \Downarrow \sigma'
$$


Let us focus, then, on the shape of this derivation $D$—once again, assuming it
exists.

$$
\begin{prooftree}
\AxiomC{\(⟦\texttt{b <= r}⟧^{\mathcal{B}}(\ldots) = \top\)}
\AxiomC{\(\vdots\)}
\UnaryInfC{\(\left\langle
              \begin{aligned}
                \texttt{r \(\leftarrow\) r - b;}\\
                \texttt{q \(\leftarrow\) q + 1}
              \end{aligned},
              \left[\begin{aligned}
                    \texttt{a} & \mapsto 42\\
                    \texttt{b} & \mapsto 0\\
                    \texttt{q} & \mapsto 0\\
                    \texttt{r} & \mapsto 42
                    \end{aligned}
              \right]
 \right\rangle \Downarrow
              \left[\begin{aligned}
                    \texttt{a} & \mapsto 42\\
                    \texttt{b} & \mapsto 0\\
                    \texttt{q} & \mapsto 1\\
                    \texttt{r} & \mapsto 42
                    \end{aligned}
              \right]\)}
\AxiomC{\(\ldots\)}
\AxiomC{\(\vdots\)}
\AxiomC{\(\vdots\)}
\UnaryInfC{\(\left\langle \begin{aligned}
              & \texttt{while (b <= r)}\\
              & \left\lfloor~~
                  \begin{aligned}
                  & \texttt{r \(\leftarrow\) r - b;}\\
                  & \texttt{q \(\leftarrow\) q + 1}
                  \end{aligned}
                \right.
              \end{aligned},
              \left[\begin{aligned}
                    \texttt{a} & \mapsto 42\\
                    \texttt{b} & \mapsto 0\\
                    \texttt{q} & \mapsto 2\\
                    \texttt{r} & \mapsto 42
                    \end{aligned}
              \right]
 \right\rangle \Downarrow \sigma'\)}
\TrinaryInfC{\(\left\langle \begin{aligned}
              & \texttt{while (b <= r)}\\
              & \left\lfloor~~
                  \begin{aligned}
                  & \texttt{r \(\leftarrow\) r - b;}\\
                  & \texttt{q \(\leftarrow\) q + 1}
                  \end{aligned}
                \right.
              \end{aligned},
              \left[\begin{aligned}
                    \texttt{a} & \mapsto 42\\
                    \texttt{b} & \mapsto 0\\
                    \texttt{q} & \mapsto 1\\
                    \texttt{r} & \mapsto 42
                    \end{aligned}
              \right]
 \right\rangle \Downarrow \sigma'\)}
\TrinaryInfC{\(\left\langle \begin{aligned}
              & \texttt{while (b <= r)}\\
              & \left\lfloor~~
                  \begin{aligned}
                  & \texttt{r \(\leftarrow\) r - b;}\\
                  & \texttt{q \(\leftarrow\) q + 1}
                  \end{aligned}
                \right.
              \end{aligned},
              \left[\begin{aligned}
                    \texttt{a} & \mapsto 42\\
                    \texttt{b} & \mapsto 0\\
                    \texttt{q} & \mapsto 0\\
                    \texttt{r} & \mapsto 42
                    \end{aligned}
              \right]
 \right\rangle \Downarrow \sigma'\)}
\end{prooftree}
$$

Note that, in the successive starting states along the rightmost branch:
+ the value of $\texttt{b}$ remains constant at $0$; (this is always the
  case: $\texttt{b}$ never appears on the left-hand side of an assignment!)
+ the value of $\texttt{r}$ remains constant at $42$.

If the loop terminates, it must be that there exists a state $\sigma_{\infty}$
such that:
+ $\sigma_{\infty}(\texttt{b}) = 0$;
+ $\sigma_{\infty}(\texttt{r}) = 42$; and
+ $⟦\texttt{b <= r}⟧^{\mathcal{B}}(\sigma_{\infty}) = \bot$.

This is a contradiction, and it must therefore be that there is no state
$\sigma'$ such that
$\left\langle \texttt{euclid}, \sigma_0 \right\rangle \Downarrow \sigma'$.

In this piece of reasoning, we identified a _loop invariant_: a
predicate over states that holds on the state component of all configurations
of the form

$$
\left\langle \begin{aligned}
              & \texttt{while (b <= r)}\\
              & \left\lfloor~~
                  \begin{aligned}
                  & \texttt{r \(\leftarrow\) r - b;}\\
                  & \texttt{q \(\leftarrow\) q + 1}
                  \end{aligned}
                \right.
              \end{aligned}, \sigma
\right\rangle
$$

in the (hypothetical) derivation $D$. This invariant is
$I_0 = \left\\{ \sigma \in \mathsf{State} \mid \sigma(\texttt{b}) = 0 \wedge \sigma(\texttt{r}) = 42 \right\\}$.

Attempting the same exercise starting from state $\sigma_1$ would yield an invariant
$I_1 = \left\\{ \sigma \in \mathsf{State} \mid \sigma(\texttt{b}) = -72 \wedge 92 \leq \sigma(\texttt{r})\right\\}$.

In both cases, the important thing to note is that:
+ $I_0 \subseteq \left\\{ \sigma \in \mathsf{State} \mid \sigma(\texttt{b}) \leq \sigma(\mathbb{r}) \right\\}$; and
+ $I_1 \subseteq \left\\{ \sigma \in \mathsf{State} \mid \sigma(\texttt{b}) \leq \sigma(\mathbb{r}) \right\\}$.

In other words, any state in $I_0$ (or $I_1$)—that is, all the states in
which the loop itself is ever executed, are such that the loop condition holds.
But it is important to note that the predicate
$\left\\{ \sigma \in \mathsf{State} \mid \sigma(\texttt{b}) \leq \sigma(\texttt{r}) \right\\}$
itself is _not_ a general loop invariant of the programme.

So we've seen that finding a property that is invariant by the loop, initially
implied by the initial state, and implies the loop condition allows us to
demonstrate that a particular configuration does not lead to a termination
execution. Generalising slightly—and considering now non-trivial conditions on
the initial state—we can use the technique above to show that execution does
not terminate when the initial state is in

$$
\bar{\phi} = \{ \sigma \in \mathsf{State} \mid \sigma(\texttt{b}) \leq 0 \wedge \sigma(\texttt{b}) \leq \sigma(\texttt{a}) \}
$$

How can we be sure execution terminates for all other initial states?

### Reasoning about termination

Loop invariants are useful—we haven't even seen the half of them—but it's hard
to see how they will help us prove termination. If the loop condition is an
invariant, then the loop never terminates. But if the negation of the loop
condition is an invariant, then the loop is never executed in the first place.
This is not where we want to be.

Let's look, now, at a terminating execution, but considering some _abstract_
initial state. We'll consider now only the execution of the loop (without the
programme's preamble). We'll also ignore the value of $\texttt{q}$ in our
derivation since it is clearly irrelevant to termination. We'll also ignore the
second disjunct of $\phi$. Starting the execution in any state $\sigma$ such
that $\sigma(\texttt{a}) \leq \sigma(\texttt{b})$ clearly and obviously leads
to immediate termination of the programme's execution, since we never enter the
loop.

$$
\begin{prooftree}
\AxiomC{\(⟦\texttt{b <= r}⟧^{\mathcal{B}}(\sigma) = \top\)}
\AxiomC{\(\vdots\)}
\UnaryInfC{\(\left\langle
              \begin{aligned}
              & \texttt{r \(\leftarrow\) r - b;}\\
              & \texttt{q \(\leftarrow\) q + 1}
              \end{aligned},
              \sigma
 \right\rangle \Downarrow
   \sigma[\texttt{r} \mapsto \sigma(\texttt{r}) - \sigma(\texttt{b}), \ldots]\)}
\AxiomC{\(\ldots\)}
\AxiomC{\(\vdots\)}
\AxiomC{\(\vdots\)}
\UnaryInfC{\(\left\langle \begin{aligned}
              & \texttt{while (b <= r)}\\
              & \left\lfloor~~
                  \begin{aligned}
                  & \texttt{r \(\leftarrow\) r - b;}\\
                  & \texttt{q \(\leftarrow\) q + 1}
                  \end{aligned}
                \right.
              \end{aligned},
   \sigma[\texttt{r} \mapsto \sigma(\texttt{r}) - 2\cdot\sigma(\texttt{b}), \ldots]
 \right\rangle \Downarrow \sigma'\)}
\TrinaryInfC{\(\left\langle \begin{aligned}
              & \texttt{while (b <= r)}\\
              & \left\lfloor~~
                  \begin{aligned}
                  & \texttt{r \(\leftarrow\) r - b;}\\
                  & \texttt{q \(\leftarrow\) q + 1}
                  \end{aligned}
                \right.
              \end{aligned},
   \sigma[\texttt{r} \mapsto \sigma(\texttt{r}) - \sigma(\texttt{b}), \ldots]
 \right\rangle \Downarrow \sigma'\)}
\TrinaryInfC{\(\left\langle \begin{aligned}
              & \texttt{while (b <= r)}\\
              & \left\lfloor~~
                  \begin{aligned}
                  & \texttt{r \(\leftarrow\) r - b;}\\
                  & \texttt{q \(\leftarrow\) q + 1}
                  \end{aligned}
                \right.
              \end{aligned},
   \sigma
 \right\rangle \Downarrow \sigma'\)}
\end{prooftree}
$$

Each time we start a new execution of the loop, and thanks to the fact that the
loop body gets executed between any two successive loop iterations, we do so in
a state where the value of $\texttt{r}$ is the old value of $\texttt{r}$ minus
the (constant) value of $\texttt{b}$.

When $\texttt{b}$ is non-negative, this observation gives us a _loop variant_
$V(\sigma) = \sigma(\texttt{r})$. A loop variant is an integer-valued function
of the state whose value is both:
+ strictly decreased by the loop's body; and
+ lower-bounded by some constant integer.

The existence of a valid loop variant implies termination: it is impossible to
continue decreasing any integer value indefinitely while also staying
lower-bounded.

## Ways in which While programmes cannot go wrong

As mentioned, the While language is designed to be the most boring interesting
language.[^joke] As such there are very many ways in which real programmes in real
programming languages go wrong that we don't consider here.

- Python programmes can go wrong when you add an integer to a string, or when
  you call a method an object does not implement. Some programming languages
  catch such ways of "going wrong" using _type systems_, which you can learn
  about in Types and Lambda Calculus next year. While programme cannot go wrong
  in this way because their _syntax_ separates boolean from arithmetic
  expressions, and enforces that only arithmetic values get stored in the
  state.

- Programmes written in most programming languages "go wrong" when you divide
  by 0. While programmes cannot go wrong in this way because we don't have
  division operations.

- Programmes written in most imperative languages "go wrong" when they read
  from an invalid memory location. While programmes cannot go wrong in this way
  because all (syntactically valid) variable names can always be read from or
  written to.

- Programmes written in C (and most other languages) can "go wrong" if a signed
  integer arithmetic operation overflows the type's capacity. While programmes
  cannot go wrong in this way because they operate in some magical state that
  can store mathematical integers.

- Operating systems may force programmes who misuse memory to go wrong
  (independently of the language's semantics and whether a particular programme
  has a meaning).

The manner of the going wrong varies from "noisily fails" (arithmetic
exceptions on division by 0) to "quietly does something you do not want the
programme to do, like output a cryptographic secret to the network" (buffer
overflows, ...). Understanding what it means for a programme in your chosen
language to go wrong, and what happens in each case is _important_. Using
(static analysis) tools to help you check that a programme is not going to go
wrong is part of modern software engineering practice in large agile companies.

It is also worth noting that in some settings, non-termination does not count
as "going wrong": it is entirely OK to want the software that drives Air
Traffic Control in the UK to continue working as long as the computer it runs
on is powered and functional; it is also OK to want a web server to stay up.
But those systems are written in languages that have something the While
language does not: some way of interacting with the world around it other than
by terminating in a particular state. A While programme that does not terminate
is just a very inefficient heater.

[^safetynote]: It is worth noting that, because the only way While programmes can go
    wrong is by not terminating, what we call safety here is, in fact called
    _liveness_ in the general programme verification literature.

[^joke]: Does that, then, make it more interesting than some other random
    interesting language? This is not as deep a question as it may seem.
