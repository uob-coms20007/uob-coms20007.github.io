---
layout: math
title: While Eats Itself
nav_order: 7
mathjax: true
parent: Computability
---


# Gödel numberings

We have now shown how to put the following things in bijection with the natural
numbers:
[integers](https://uob-coms20007.github.io/notes/computability/encoding.html#integers-vs-natural-numbers),
[pairs of
naturals](https://uob-coms20007.github.io/notes/computability/encoding.html#encoding-pairs-of-numbers),
and [lists of
naturals](https://uob-coms20007.github.io/notes/computability/encoding.html#encoding-lists).

In the problem sheet, you were also asked to show how to put binary trees in
bijection with natural numbers.

In previous lectures we discussed that While programs themselves may be
represented as an [Abstract Syntax Tree
(AST)](https://uob-coms20007.github.io/notes/syntax/ast.html).
Thus, it is not a stretch of the imagination to imagine that we may encode
While ASTs as natural numbers.

As While's chief data type is that of integers, an encoding of this sort
means that _we will be able to compute functions that modify or act on While
programs within While itself_. This sounds somewhat paradoxical, but it
isn't.

Encodings of "systems" (logical systems, programming languages, etc.) in
themselves are often called **Gödel numberings**. They were first proposed by
the great logician [Kurt
Gödel](https://en.wikipedia.org/wiki/Kurt_G%C3%B6del) in the proof of his celebrated
[incompleteness
theorems](https://en.wikipedia.org/wiki/G%C3%B6del%27s_incompleteness_theorems).

Let $\textbf{Stmt}$ be the set of Abstract Syntax Trees of While. For the rest
of this unit we assume that we have a Gödel numbering

$$
  \ulcorner - \urcorner : \textbf{Stmt} \xrightarrow{\cong} \mathbb{N}
$$

which encodes While programs as natural numbers.

## Uses and abuses of Gödel numberings

Gödel numberings allow us to prove various __impossibility results__ about
logical systems and/or programming languages. All these proofs proceed
through constructions that seem quasi-paradoxical (but, again, are not). The
apparent paradox often implies that the programming language has some
limitation, i.e. is unable to achieve some task (compute a function, decide a
predicate, etc.). We will see such a result next week.

There is a [significant amount of
lore](https://en.wikipedia.org/wiki/G%C3%B6del,_Escher,_Bach) surrounding Gödel
numberings. However, this lore somewhat [overstates their
importance](https://wiki.c2.com/?JeanYvesGirardOnGoedelEscherBach). When
understood intuitively, most impossibility results amount to a statement of the
form  "a Parliament cannot grant itself amnesty by its own vote: it must recruit
some external authority larger than itself." Alternatively, "you cannot fix your
glasses while keeping them on your nose."

## Use-case 1: Code transformations

It may not be entirely clear for what sort of thing one might use a Gödel
numbering. The answer is that they can be used to determine the computability of
_code transformations_.

Let $\textbf{Stmt}$ be the set of all While statements represented as ASTs
(i.e. the mathematical counterpart of the relevant Haskell data type used to
represent While ASTs).

A __code transformation__ is a function $f : \textbf{Stmt} \to \textbf{Stmt}$.

Code transformations may seem close to Haskell's higher-order functions, in
that they take programs as arguments, and return programs (cf.
functions-as-data). However, they are strictly more powerful than that.
Essentially, the only thing a Haskell program with a higher-order argument 
`f :: a -> b` can do is call it at some inputs (see e.g. the definition of
`map`, which calls `f` on every element of a list).

Instead, code transformations are significantly more powerful, as they are
able to arbitrarily construct and deconstruct pieces of syntax. For example,
we can have a code transformation $h : \textbf{Stmt} \to \textbf{Stmt}$ that
is defined by

$$
  h(S) = \begin{cases}
    S; x := 0 & \text{ if $x$ occurs in $S$} \\
    S         & \text{ otherwise}
  \end{cases}
$$

Or we can even define $h' : \textbf{Stmt} \to \textbf{Stmt}$ by

$$
  h'(S) = \begin{cases}
    S                 & \text{ if there is a while loop in $S$ } \\
    \textbf{skip}     & \text{ otherwise}
  \end{cases}
$$

This program transformation depends on the particular _syntactic_
characteristics of $S$ (i.e. whether or not it ever uses the variable $x$).
Modulo the different programming paradigms (functional vs. imperative), no
Haskell code can "examine" its higher-order inputs and tell whether they are
using some character/construct or not.

Thus, a first _raison d'être_ of Gödel numberings is so that we can discuss the
computability of such code transformations. That is, we want to
- write code transformations that map While programs to other While programs,
- use the Gödel numbering to reflect them into functions $\mathbb{N} \to \mathbb{N}$, and
- compute these reflected functions in While itself.

Why we might want to do this last thing will become evident soon.

## Use-case 2: Static analysis

There is a second reason why we might want to consider the use of Gödel
numberings.

If you have been using modern IDEs (e.g. Visual Studio Code, or IntelliJ IDEA)
then you will have noticed that they know an awful lot about the program you are
writing, _even before you run it_.

The art of determining various things about programs without even running them
is called **static analysis**. [Static
analyses](https://en.wikipedia.org/wiki/Static_program_analysis) may be used to
estimate the run-time behaviour of any aspect of a program fragment, including:
* the type of data it will output
* whether it will terminate
* how much time it will take to run
* how much memory it might use
* whether it has any obvious bugs (e.g. dereferencing that causes a Segmentation Fault)
* whether it is correct, according to some given specification of its behaviour
and so on.

Modern IDEs are bundled with built-in program analysers that try to work out
many such features.

Let us think about the program analyser for a minute. What sort of function is it?
In our context, a program analyser can be seen as a function

$$
  A : \textbf{Stmt} \to \mathbb{N}
$$

which takes as input a While program, and outputs e.g. $0$ if it is 'bad' (say,
contains a bug), or $1$ if it is 'good' (does not contain a bug).

As you see, a program analyser is a program that accepts another program as input.

Thus, Gödel numberings can be used to approach the question of whether a program
analysis is computable or not.


Thus, a second _raison d'être_ of Gödel numberings is so that we can discuss the
computability of such code transformations. That is, we want to
- write program analysers that examine While programs
- use the Gödel numbering to reflect them into functions $\mathbb{N} \to \mathbb{N}$, and
- compute these reflected functions in While itself.

Perhaps unsurprisingly, we might soon find out that [we can't always get what we
want](https://www.youtube.com/watch?v=Ef9QnZVpVd8).


# The Universal Function

From this point onwards let us fix a particular variable `x` wrt which our
[definition of
computability](https://uob-coms20007.github.io/reference/computability/functions.html#computes)
is stated.

We define a function

$$ 
  \begin{aligned}
    & U : \mathbf{Stmt} \times \mathbb{N} ⇀ \mathbb{N} \\
    & U(P, n) = ⟦ P ⟧_\texttt{x}(n)
  \end{aligned}
$$

We often call $U$ the __universal function__, in the sense that it is able to
produce the behaviour of every computable function. If a function $f :
\mathbb{N} ⇀ \mathbb{N}$ is computable, then by definition it is equal to $⟦ P
⟧_{\texttt{x}}$ for some While program $P$. It follows that 

$$
  f(n) \simeq ⟦ P ⟧_\texttt{x}(n) \simeq U(P,n)
$$

for all $n \in \mathbb{N}$.

$U$ is partial because these computable functions $⟦ P ⟧_\texttt{x}$ are
partial themselves.

The following is a celebrated result in computability theory:

**Theorem (Kleene)**. The universal function is computable.

Let's unpack what this means. First things first: in order to speak of
computability, we have to encode the domain as natural numbers. The domain
itself is the product of two sets, $\textbf{Stmt}$ and $\mathbb{N}$. The first
can be encoded in the natural numbers through the Gödel numbering $\ulcorner -
\urcorner : \textbf{Stmt} \to \mathbb{N}$. The second is already the natural
numbers. By an exercise in one of the sheets we can then construct a bijection
as the composite function

$$
  \mathbf{Stmt} \times \mathbb{N}
    \xrightarrow{\ulcorner - \urcorner \times \text{id}_\mathbb{N}}
  \mathbb{N} \times \mathbb{N}
    \xrightarrow{\langle -, - \rangle}
  \mathbb{N}
$$

where if $f : A \to B$ and $g : C \to D$ we define 

$$
  \begin{aligned}
    & f \times g : A \times C \to B \times D \\ 
    & (f \times g)(a, c) = (f(a), g(c))
  \end{aligned}
$$

It is not very difficult to prove that if $f$ and $g$ are bijections, then so is
$f \times g$. Moreover, we have previously shown that the composite of two
bijections is a bijection, so the composite function $\langle -, - \rangle \circ
(\ulcorner - \urcorner \times \text{id}_\mathbb{N})$ is a bijection.

It follows that we can reflect this function into the natural numbers using
the Gödel numbering and pairing. 

The theorem says that resultant function is computable!

In lieu of a proof, let us informally sketch what the program that computes this
does.

1. Upon receiving $i$ as an argument, it decodes it as a pair $(e, n) =
   \textsf{split}(i)$.
2. It decodes the first component $e = \ulcorner S \urcorner \in \mathbb{N}$
   into (the AST of) a While program $S$.
3. It simulates the running of the program $S$ on input $n$.

This last step consists of constructing the configuration $\langle S,
[\texttt{x} \mapsto n] \rangle$, followed by computing the (unique) next
configuration of the operational semantics of While. This is repeated until
the simulation reaches a halting configuration $\langle \texttt{skip},
[\texttt{x} \mapsto m] \rangle$. At this point the program halts, returning
$m$ in its output.

That description may seem familiar: it corresponds to what we would nowadays
call an __interpreter__ for a programming language. An interpreter is a
program that loads the source code of another program, and computes the
intended behaviour of that program. One way to write an interpreter is to
follow the operational semantics of a language, as described above. Usually
interpreters do not translate the source code, but merely produce the desired
behaviour directly. The most famous language that is interpreted (and not
compiled) is perhaps
[Python](https://en.wikipedia.org/wiki/Python_(programming_language)).

This theorem of Stephen Kleene states that it is possible to write an
interpreter. Curiously, it was proven long before any actual computers
existed!

The proof of the theorem proceeds by actually constructing the interpreter
itself as a While program. We will not do so, because it is awfully tedious:
it involves unpairing a number of inputs, and pattern-matching on the next
instruction (if the program is of the form $S_1; S_2$, then do the first
instruction of $S_1$, and so on). As While only supports natural numbers, all
this needs to be done numerically, by computing the reflections these
functions! Evidently, that is a formidable task. 

However, it should not be very hard for you to see how to write such an
interpreter in Haskell, using the powerful features of algebraic data types
and pattern-matching. 

For the purposes of this unit we are willing to accept Kleene's theorem at
face value, without an explicit construction. We will also be reasonably
relaxed about the explicit construction of such programs from this point
onwards, and accept
[handwaving](https://en.wikipedia.org/wiki/Hand-waving#In_mathematics_(and_formal_logic,_philosophy,_theoretical_science))
instead.

# The Church-Turing Thesis

We have defined the notion of computability of functions $\mathbb{N} ⇀
\mathbb{N}$ through the language While. 

That very act of definition included an implicit assumptions. In short, We
assumed that While is a _reasonable model of computation_. This assumption
has two parts:
1. It assumes that While is _good enough._ That is, it assumes that While can
   do a lot of things that we expect a computer to be able to do.
2. It assumes that While is _not too good._ That is, it assumes that While
   does not have any abilities above those expected of a usual digital
   computing device.

Because of its similarity to common imperative programming languages (like C
or Java) we are fairly content to accept these two assumptions. One way in
which While is unrealistic is that its variables can store integers of
arbitrary size. However, we are willing to accept that in the name of
simplicity.

We have also seen that it is possible to write an
[interpreter](https://uob-coms20007.github.io/reference/computability/universal.html#interpreter)
for While. Through appropriate Gödel numbering, encoding of pairs, etc. it
was even possible to implement this interpreter in While itself.

However, this last step was not a special feature of While. We could
implement an interpreter for While in Java. We could also implement an
interpreter in Haskell (and if you are adventurous you may have already tried
to do so).

Conversely, there is nothing special about implementing an interpreter for
While. With enough time and tenacy we could implement an interpreter for e.g.
Java as a While program. Or we could implement an interpreter for Haskell,
again in While. The possibilities are endless.

This is not only a practical phenomenon, but also a theoretical observation.
Suppose you take issue with the idea that While is a realistic model of
computation. Suppose further that this incites you to go away and design your
own model of computation, which is meant to embody what we mean by a digital
computater. You will then observe that you will be able to write an
interpreter for that model of computation in While!

It has been empirically observed that this holds for any two _realistic_
models of computation of which we can conceive: each one can be _simulated_
in the other. This observation was made long before the advent of modern
programming languages (like While, C, Java, Haskell, ...).

This statement is often known as the

**Church-Turing thesis**: any two models of computation are equivalent with
respect to the computability of functions $\mathbb{N} ⇀ \mathbb{N}$.

Thus, the act of defining computable functions to be the set

$$
  \mathcal{PR} = \{ f : \mathbb{N} ⇀ \mathbb{N} \mid \text{$f = ⟦ P ⟧_{\texttt{x}}$ for some While program $P$ } \}
$$

of functions that can be computed by a While program is in fact not so
arbitrary. Had we picked another option of the tens that are out there, we
would have gotten the same set of functions. The reason is that any other
model of computation can be simulated as a While program.

The letters $\mathcal{PR}$ stand for __partial recursive functions__. This is
the set of partial functions on $\mathbb{N}$ that is considered computable by
any realistic model of computation.

Indicatively, here is a highly incomplete list of other models of computation:
* [Turing machines](https://www.youtube.com/watch?v=E3keLeMwfHY)
* [Lambda calculus](https://www.bris.ac.uk/unit-programme-catalogue/UnitDetails.jsa?ayrCode=21%2F22&unitCode=COMS30040)
* [Register machines](https://en.wikipedia.org/wiki/Register_machine)
* [Counter machines](https://en.wikipedia.org/wiki/Counter_machine)
* [Post-Turing machines](https://en.wikipedia.org/wiki/Post%E2%80%93Turing_machine)
