---
layout: math
title: Problems
mathjax: true
---

## Decision Problems

A __decision problem__ is a specification of a computational problem whose input is a string and whose output is a decision: YES or NO.  We will usually write decision problems as follows:

PROBLEM
 * *Given*:  a string $$x$$ over some alphabet     
 * *Decide*: if $$x$$ satisfies some property $$P$$

An __instance__ of a decision problem is simply a concrete example of the Given, e.g. the particular string "hello".  A __yes instance__ of a decision problem is an instance for which the decision should be YES.  A __no instance__ is an instance for which the decision should be NO.

We will typically identify decision problems with their set of yes instances:
$$
PROBLEM = \{x | P(x) \}
$$
and this is how decision problems are typically presented in Sipser's book.

## Decidability

If there is an algorithm that solves a decision problem $$P$$, in the sense that it computes the correct decision for every possible instance (in finite time), then we say that $$P$$ is __decidable__.  Otherwise we say that $$P$$ is __undecidable__.

## Undecidable Problems

The following are problems that we have shown to be undecidable during this unit:
* The Acceptance Problem for Turing machines (Week 6, Lecture 1).
* The Halting Problem for Turing machines (Week 6, Lecture 2).
* The Equivalence Problem for Turing machines (Week 6, Lecture 2).
* The theory of True Arithmetic (Week 7, Lecture 1).
* The problem of whether a Turing machine ever tries to move it's head off the left edge of the tape (Problem Sheet 4).
* The problem of whether a Turing machine has a useless state (Problem Sheet 4).
* The problem of whether two Integer predicates definable in Haskell are equal (Problem Sheet 4).
