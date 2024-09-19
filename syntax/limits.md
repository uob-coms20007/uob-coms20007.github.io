---
layout: math
title: Limitations
mathjax: true
nav_order: 9
parent: Syntax
---


$$
\newcommand{\andop}{\mathrel{\&\!\&}}
\newcommand{\orop}{\mathrel{\|}}
$$

In this lecture, I want to look briefly at the limitations of context-free grammars.  I told you, in the first week, that grammars alone are not enough to describe the valid programs in a particular programming language.  They cannot, for example, enforce that variables are declared before they are used, or (in general) ensure that programs are well-typed.  In this lecture I aim to convince you of the first of these.

To do this, it will be helpful to introduce a new view on grammar derivations.  If you recall, derivations justify whether a string is in the language described by a certain grammar.

## Parse Trees

I want to start by looking at an alternative mechanism for defining which strings are in the language of a grammar, called a _parse tree_.  Parse trees are a bit less straightforward compared with derivations (they are trees rather than sequences), but they have an advantage in revealing more directly how a particular letter (terminal symbol) was arrived at in the derived string.

{:defn}
A __parse tree__ is an ordered tree, whose internal vertices are non-terminals, whose leaves are terminals and in which _symbols_ $\alpha_1 \cdots \alpha_n$ are the children of a non-terminal labelled-node $X$ only if $X \longrightarrow \alpha_1\ldots\alpha_n$ is a production rule of the grammar.

Note: in the above definition I am stipulating that each $\alpha_i$ is a single symbol (either a terminal or a non-terminal).

{:defn}
Theorem: A string $u$ is in the language of a grammar, just if there exists a parse tree whose root is the start symbol of the grammar and whose leaves, read in left-to-right order, spell out $u$.

Consider the following grammar for a simple kind of Boolean expressions:

$$
  B \longrightarrow B \andop B \mid B \orop B \mid (B) \mid \mathsf{true} \mid \mathsf{false}
$$

We can show that $\tt \orop \tt \andop \ff$ is derivable in this grammar, by building a parse tree satisfying the above conditions (i.e. rooted at $B$ and whose leaves spell out the word).  We start by putting down the root of the tree, which is the start symbol (and only nonterminal in this grammar) $B$.

Now, we can choose any production to apply to this $B$.  Suppose we choose $B \longrightarrow B \andop B$.  Rather than making a step by replacing $B$ by its right-hand side, instead we place the right-hand side of the production as the children of $B$.  Each symbol is a child node and the tree is ordered, so the children need to be in the same left-to-right order.

We proceed by choosing any of the nonterminals at the leaves of this tree and adding children in the same way, according to some appropriate production rule.  For example, we can choose the leftmost leaf and add child nodes labelled $B$, $\orop$ and $B$, corresponding to the production $B \longrightarrow B \orop B$.

If, at some point, we run out of nonterminals in the leaves, then we have constructed a _parse tree_ and this parse tree witnesses the membership of the string spelled out in its leaves in the language described by the grammar.

Another example: we can use a parse tree to show that $\tt \andop (\ff \orop \ff)$ is in the grammar.

## Context-Free Languages Can be Pumped

I now want to develop in you some intuitions about an important characteristic of the languages that are describable by context-free grammars - the _context-free languages_.  

Much of the power of context-free grammars comes from the fact that rules in a grammar can be recursive.  If you didn't have any recursive rules in your grammar, then you can only describe a finite language, which is not very interesting.

The recursive rules in a CFG are, in some ways, quite like recursive procedures in a programming language -- as we saw, for an LL(1) grammar, we implement recursive rules directly as recursive procedures.  However, in a programming language, usually when you have a recursive procedure the recursion is tightly controlled by some input to the procedure and the procedure is forced to terminate when the input has some specified form, the base case.  For example, in a recursive implementation of the factorial function, the recursion will terminate once the input becomes 0.

However, context-free grammars have no mechanism by which we can control their recursive rules.  There isn't any possibility to restrict the number of recursive "calls" (rule applications) when deriving strings.  The consequence is that, if we have a grammar in which you can use a certain number of recursive rule applications to derive some part of your string, then it is possible to use any other number of recursive rule applications to derive longer and longer strings by repeating that part, whether you want that or not.

To see this, consider I give you the following parse tree, but I _don't_ give you the grammar for which it is a parse tree, and I ask: what other words are derivable (equivalently, for which there is a parse tree) in this (unknown) grammar.

