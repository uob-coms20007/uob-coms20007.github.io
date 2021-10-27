---
layout: math
title: Concrete syntax for the While language
nav_order: 1
mathjax: true
parent: The While Language
---

# Lexical categories

*Reserved keywords*:
- <tt>if</tt>
- <tt>then</tt>
- <tt>else</tt>
- <tt>while</tt>
- _Boolean constants_ (<tt>BoolLit</tt>)
  - <tt>true</tt>
  - <tt>false</tt>

*Reserved symbols*:
- <tt>:=</tt>
- <tt>(</tt>
- <tt>)</tt>
- <tt>{</tt>
- <tt>}</tt>
- <tt>+</tt>
- <tt>-</tt>
- <tt>\*</tt>
- _Comparison operators_ (<tt>CompOp</tt>)
  - <tt>=</tt>
  - <tt>\<=</tt>

*Variable identifiers* (<tt>Id</tt>):
- Any string (other than reserved keywords) matched by regular expression $\texttt{[a-z][a-zA-Z0-9]}^*$.

*Integer literals* (<tt>IntLit</tt>):
- Any string matched by regular expression $\texttt{[0-9]}^+$.

Whitespace (including newlines and tabs) has no syntactic or semantic meaning, serving only to separate keywords and variable identifiers.

# Concrete Syntax (Context-Free Grammar)

In the following, terminals are denoted in <tt>typewriter font</tt>, non-terminals are single-letter capitals in $\mathit{math}\ \mathit{font}$.

## Arithmetic Expressions ($E$)

The following context-free grammar defines the concrete syntax for expressions ($E$), terms ($T$) and ($F$).

$$
\begin{xalignat}{3}
E  & \rightarrow E\ \texttt{+}\ T    & T  & \rightarrow T\ \texttt{*}\ F    & F  & \rightarrow\ \texttt{Id}\\
E  & \rightarrow E\ \texttt{-}\ T    & T  & \rightarrow F                   & F  & \rightarrow\ \texttt{IntLit}\\
E  & \rightarrow T                   &    &                                 & F  & \rightarrow\ \texttt{-}\ \texttt{IntLit}\\
   &                                 &    &                                 & F  & \texttt{(}\ E\ \texttt{)}
\end{xalignat}
$$

## Boolean Expression ($B$)

The following context-free grammar defines the concrete syntax for boolean expressions ($B$), clauses ($C$) and atoms ($A$).

$$
\begin{xalignat}{3}
B  & \rightarrow B\ \mathtt{\wedge}\ C    & C  & \rightarrow \texttt{!}\ A  & A  & \rightarrow\ \texttt{BoolLit}\\
B  & \rightarrow C                        & C  & \rightarrow A              & A  & \rightarrow\ E\ \texttt{CompOp}\ E\\
   &                                      &    &                            & A  & \rightarrow \texttt{(}\ B\ \texttt{)}
\end{xalignat}
$$

## Statements ($S$)

The following context-free grammar defines the concrete syntax for statements ($S$) and single instructions ($I$). Note that the empty statement is a statement.

$$
\begin{xalignat}{2}
S  & \rightarrow I\ S      & I  & \rightarrow \texttt{Id}\ \texttt{:=}\ E                       \\
S  & \rightarrow \epsilon  & I  & \rightarrow \texttt{if}\ B\ \texttt{then}\ I \texttt{else}\ I \\
   &                       & I  & \rightarrow \texttt{while}\ B\ I                              \\
   &                       & I  & \rightarrow \texttt{\{}\ S\ \texttt{\}}
\end{xalignat}
$$

## Programs

A While program is simply a While statement. Our implementation expects it to be followed by the end of the file, for parsing to be considered complete.

Formally, we would normally add a rule $P \rightarrow S \texttt{\square}$, where $\texttt{\square}$ is a special terminal denoting the end-of-file token.
