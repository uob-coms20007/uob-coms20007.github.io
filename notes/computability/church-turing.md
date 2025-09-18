---
layout: math
title: The Church-Turing thesis
nav_order: 9
mathjax: true
parent: Computability
---

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