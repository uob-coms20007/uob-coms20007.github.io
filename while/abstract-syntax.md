---
layout: math
title: Abstract syntax for the While language
nav_order: 2
mathjax: true
parent: The While Language
---

# Abstract Syntax

The abstract syntax of a programming language is a structured representation of programs in the language, which normally simplifies aspects of the concrete syntax. It gives rise to abstract syntax trees, which are operated on by interpreters and compilers.

The type of abstract syntax trees we'll consider for the While language is in file [while/src/WhileAST.hs](https://github.com/uob-coms20007/labcode/blob/main/while/src/WhileAST.hs) of the labcode.

That datatype is a programmatic construct but it describes a few (relatively) simple sets.

- Type ```ABinOp``` describes the set $A = \left\\{\texttt{AAdd}, \texttt{ASub}, \texttt{AMul}\right\\}$.
- Type ```AExpr``` describes the smallest set $E$ such that:
  - For any variable name $\texttt{x}$, the value $\texttt{EVar x}$ is in $E$;
  - For any integer $i \in \mathbb{Z}$, the value $\texttt{EInt}\ i$ is in $E$; and
  - For any $a \in A$, $e_1 \in E$ and $e_2 \in E$, $\texttt{EBinOp}\ a\ e_1\ e_2$ is in $E$.
- Type ```ACompOp``` describes the set $C = \left\\{\texttt{AEq}, \texttt{ALe}\right\\}$.
- Type ```BExpr``` describes the smallest set $B$ such that:
  - The values $\texttt{BBool True}$ and $\texttt{BBool False}$ are in $B$;
  - For any $b \in B$, the value $\texttt{BNot}\ b$ is in $B$;
  - For any $b_1, b_2 \in B$, the value $\texttt{BAnd}\ b_1\ b_2$ is in $B$; and
  - For any $c \in C$ and $e_1, e_2 \in E$, $\texttt{BComp}\ c\ e_1\ e_2$ is in $B$.
- Type ```Stmt``` describes the smallest set $S$ such that:
  - $\texttt{SSkip}$ is in $S$;
  - For any variable name $\texttt{x}$ and any $e \in E$, the value $\texttt{SAssign}\ \texttt{x}\ e$ is in $S$;
  - For any $b \in B$ and any $s_1, s_2 \in S$, the value $\texttt{SIte}\ b\ s_1\ s_2$ is in $S$;
  - For any $b \in B$ and any $s \in S$, the value $\texttt{SWhile}\ b\ s$ is in $S$; and
  - For any $s_1, s_2 \in S$, the value $\texttt{SSeq}\ s_1\ s_2$ is in $S$.

In less formal words, these describe how to construct:
- arithmetic expressions from variable names, integer literals, operators and other expressions (```AExpr```);
- boolean expressions from boolean literals, boolean operators, other boolean expressions, and comparisons between arithmetic expressions (```BExpr```); and
- statements (or programs) from the empty program, single assigment instructions, sequential composition of statements, and while and if statements—which combine boolean expressions with statements (```Stmt```).

## Abstract Syntax Trees

Elements in $S$ are called Abstract Syntax Trees (ASTs). We often represent them with the name of the constructor as nodes. We show the [AST for the factorial program](https://uob-coms20007.github.io/reference/while/example.html#factorial-as-an-abstract-syntax-tree) as an example.

## Denoting Abstract Syntax Linearly: Backus-Naur Form

A program in abstract syntax is really just a tree in the set $S$ defined above. However, when we're writing mathematical definitions down, we (unfortunately) tend to be limited to a linear notation, as opposed to being able to simply write trees down.

In general, this is done using another form of abstract syntax which _looks like_ the concrete syntax, but is simpler—and often omits all syntactic sugar, so we can focus on a core language instead. (We'll see how having more constructions than needed in the core language forces us down really tedious paths for defining and reasoning about semantics and compilation.) We give some examples throughout the video lectures in Week 7.

This abstract syntax is often presented in Backus-Naur Form (or BNF), which is really just another way of presenting context-free grammars, but is typically used to describe abstract syntax instead of the concrete syntax that's used to parse the language in the first place. Below, we give the BNF for the abstract syntax of While programs as we define them. Do note, in particular, that the language remains structured, and parentheses and braces are only added when denoting the programs linearly to represent that structure.
We'll also use semicolons to denote sequential composition for readability, even though our concrete syntax does not need them. (This is because we can use newlines in our concrete programs, whereas the point of the abstract syntax's linear representation is to represent programs on a single line.)

$$
\begin{alignat}{3}
e ::= & n \textrm{ with } n \in \mathbb{Z}                &  b ::= & e = e           & s ::= & \texttt{x} := e                                    \\
      & \textrm{\texttt{x} with \texttt{x} a variable}    &        & e <= e          &       & \texttt{if}\ b\ \texttt{then}\ s\ \texttt{else}\ s \\
      & e + e                                             &        & \texttt{true}   &       & \texttt{while}\ b\ s                               \\
      & e - e                                             &        & \texttt{false}  &       & s\ s                                               \\
      & e * e                                             &        & !b              &       &                                                    \\
      &                                                   &        & b \wedge b      &       &
\end{alignat}
$$
