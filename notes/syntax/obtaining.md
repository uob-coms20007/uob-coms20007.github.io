---
layout: math
title: 7. Obtaining LL(1)
mathjax: true
nav_order: 7
parent: Syntax
---

$$
\newcommand{\andop}{\mathrel{\&\!\&}}
\newcommand{\orop}{\mathrel{\|}}
\newcommand{\ff}{\mathsf{false}}
\newcommand{\tt}{\mathsf{true}}
\newcommand{\tm}[1]{\mathsf{#1}}
\newcommand{\nt}[1]{\mathit{#1}}
$$

# Obtaining Grammars Suitable for Predictive Parsing

Now we know how to identify whether or not a grammar is LL(1), and how to implement parsing.  However, typically the most straightforward descriptions of syntax by context-free grammars do not turn out to be LL(1).  Consider, for example, the simple one-line description of Boolean expressions:

$$
  B \Coloneqq B \andop B \mid B \orop B \mid \tt \mid \ff \mid (B)
$$

This is a very direct description of Boolean expressions, but it is not the case that which rule we need to use when parsing a given string is determined uniquely by the combination of the left-most nonterminal in the sentential form and the next letter of the input, i.e. the parsing table will have multiple rules in the same cell.

There is no simple recipe to transform a given grammar such as the one above into an LL(1) grammar.  Indeed, not all context-free grammars are can be written as an LL(1) grammar.  Nevertheless, in practice it is often possible to start with an arbitrary CFG and obtain an LL(1) grammar by rephrasing certain problematic features. 

## Left Factoring

Consider the following grammar (fragment) of an imperative programming language.  In the part we will be interested in there are six terminal symbols:

$$
  \tm{if} \qquad \tm{(} \qquad \tm{)} \qquad \tm{\{} \qquad \tm{\}} \qquad \tm{else}
$$

In this fragment there are several non-terminals but we will only really interested in the nonterminal $$\nt{IfStmt}$$, which derives if-then and if-then-else statements.  For this there are two grammar productions:

$$
  \begin{array}{rcl}
    \nt{IfStmt} &\Coloneqq& \tm{if}\ \tm{(}\ \nt{Expr}\ \tm{)}\ \tm{\{} \nt{Stmts}\ \tm{\}}\\
                &\mid&  \tm{if}\ \tm{(}\ \nt{Expr}\ \tm{)}\ \tm{\{} \nt{Stmts}\ \tm{\}}\ \tm{else}\ \tm{\{}\ \nt{Stmts}\ \tm{\}}
  \end{array}
$$

Even if we put the rest of the grammar (not shown) to one side, this fragment consisting of two production rules is already not LL(1).  Suppose we are trying to derive the valid string 

$$
\tm{if}\ ( x > 2 )\ \{ x = 1 \}
$$

and the leftmost non-terminal (which we want to replace, in order to derive this string) is $\nt{IfStmt}$.  If this really were part of an LL(1) grammar, then we would know which of the two production rules to choose for replacing $\nt{IfStmt}$ simply by looking at the leftmost terminal symbol of the string that we are aiming to derive.  However, only looking at the leftmost terminal symbol, here $\tm{if}$, is not enough information to know which rule to pick.  Only knowing that the first terminal of the string is $\tm{if}$ doesn't tell us whether we are aiming to derive an if-then statement, or whether we are actually aiming to derive an if-then-else statement.  

If we built the parsing table $T$ for this grammar, we would see that the cell at $T[\nt{IfStmt},\tm{if}]$ contains both of these production rules, and is thus not LL(1).  

Fortunately, this kind of obstacle to LL(1)-ness is easy to overcome.  A simple remedy is to _left factor_ the rules, which involves factoring out the common prefix of the sentential forms on the right-hand-sides.  In this case, this common prefix is:

$$
  \tm{if}\ \tm{(}\ \nt{Expr}\ \tm{)}\ \tm{\{} \nt{Stmts}\ \tm{\}}
$$

In general, whenever we have several rules, say $k>1$ in number, for the same nonterminal with a common prefix, say $\alpha$:

$$
    X \Coloneqq \alpha\ \beta_1 \mid \alpha\ \beta_2 \mid \cdots{} \mid \alpha\ \beta_k 
$$

Then we can express the same derived strings by introducing a new non-terminal, say $R$, to represent the possible "rest" of the rule that comes after the common prefix:

$$
  \begin{array}{rcl}
    X &\Coloneqq& \alpha\ R\\
    R &\Coloneqq& \beta_1 \mid \beta_2 \mid \cdots{} \mid \beta_k
  \end{array}
$$

Note: it may be there are also $X$-rules that do not share the common prefix, and these can be safely ignored, they do not participate in the transformation.

Thus, we can rephrase the rules for $\nt{IfStmt}$ by:

$$
  \begin{array}{rcl}
    \nt{IfStmt} &\Coloneqq& \tm{if}\ \tm{(}\ \nt{Expr}\ \tm{)}\ \tm{\{} \nt{Stmts}\ \tm{\}}\ \nt{ElseOpt}\\[2mm]
    \nt{ElseOpt} &\Coloneqq&  \tm{else}\ \tm{\{}\ \nt{Stmts}\ \tm{\}} \mid \epsilon
  \end{array}
$$

Now, there is no longer any choice for which rule to pick when replacing $\nt{IfStmt}$ - there is only one rule.  Moreover, assuming the rest of the grammar presents no further obstacle, we can choose between the two possible rules of $\nt{ElseOpt}$ simply by looking to see if the next terminal symbol is $\tm{else}$ or not.  In general, it may be that  performing one left-factoring transformation fixes an immediate obstacle to LL(1)-ness, but also reveals a new one, so you may have to repeat the left-factoring process several times until all common prefixes are factored out.

## Left Recursion

A second type of problematic grammar construction concerns recursive rules.  Consider the following grammar for sequences of statements.

$$
  S \Coloneqq \tm{stmt} \mid S \mathrel{;} S
$$

Here I use a terminal symbol to $\tm{stmt}$ to represent programming language statements because their internal structure is not important to the example.  Now suppose I am trying to generate a given string starting from $S$.  Is the rule I should use uniquely determined by the first terminal symbol of the input?  The first rule starts with a specific terminal symbol, so it looks promising.  However, the second rule is a problem, because the right-hand side of this rule starts with the nonterminal $S$ again.  Consequently, it will _always_ be eligible as a choice whenever the next terminal symbol is in $\first(S)$.

For example, suppose the input string starts with the terminal $\tm{stmt}$.   At first sight it might seem like the production $S \Coloneqq \tm{stmt}$ is the correct rule to use, and if the complete input string was just that one terminal symbol, then it would be.  However, the input may instead continue as $\tm{stmt}; \tm{stmt}$ and this case we should have chosen the rule $S \Coloneqq S;S$ instead.  

This phenomenon, where some nonterminal $X$ has a production in which $X$ is again the first symbol on the right-hand side, is called _left recursion_ and it is typically a problem for obtaining an LL(1) grammar.  That's because the first set of (the RHS of) a left recursive rule for $X$ always contains all of $\first(X)$.

The resolution is to rewrite the relevant productions using only _right recursion_, that is, where the nonterminal $X$ occurs at the end of the right-hand side instead of the start.

Clearly, the grammar above derives strings that are finite non-empty sequences of assignment statements punctuated by semicolons:

$$
  \tm{stmt}; \tm{stmt}; \cdots{} \tm{stmt}
$$

There are other ways to generate such sequences.  In fact three different grammars come to mind for generating non-empty lists of something (think also of defining a non-empty list datatype):

- The ``append'' approach: every list is either (a) a singleton list consisting of one item, or (b) obtained by appending two smaller lists together.  This is the style of definition we have above, with $\tm{;}$ playing the role of the append operator.

    $$
      S \Coloneqq \tm{stmt} \mid S \mathrel{;} S
    $$

- The ``snoc'' approach: every list is either (a) a singleton list consisting of one item, or (b) a smaller list onto the end of which we have inserted a new item.

    $$
      S \Coloneqq \tm{stmt} \mid S\ \tm{;}\ \tm{stmt}
    $$

- The ``cons'' approach: every list is either (a) a singleton list consisting of one item, or (b) a smaller list onto the _front_ of which we have inserted a new element.
    
    $$
      S \Coloneqq \tm{stmt} \mid \tm{stmt}\ \tm{;}\ S
    $$

I hope you can convince yourself that each of these grammars derives exactly the same set of non-empty finite sequences of $\tm{stmt}$ separated by semicolons.  So, in principle, we could use any of them to define our language.  However, if we want the grammar to be LL(1), then only the last version will do, because it avoids left recursion.

You may notice that the _right-recursive_ ("cons") approach is exactly the pattern we noted when discussing that CFGs can express sequencing, back in lecture 3.  Using the notation introduced there we can write the above grammar as:

  $$
    S \Coloneqq \tm{stmt}\ [\tm{;}\ \tm{stmt}]^*
  $$

### Left Recursion Elimination

Here's a slightly more complicated example.  The same principle applies, but it is less easy to see.  Consider again the grammar of Boolean expressions from above: 

$$
  B \Coloneqq B \andop B \mid B \orop B \mid \tt \mid \ff \mid (B)
$$

This involves two left recursive productions, $B \longrightarrow B \andop B$ and $B \longrightarrow B \orop B$.

The kinds of strings these production rules generate are all Boolean expressions, e.g.

$$
  \tt \andop (\ff \orop tt) \andop ((\ff \orop \tt) \andop \ff) 
$$

Personally, I find it unintuitive to think of such expressions as some kind of recursively generated sequence.  However, it is not so difficult to do just that if we introduce a couple of new nonterminals to clarify the structure a bit.  

Let's start by putting all the non-left recursive rules together into a separate production, say for a new nonterminal $A$:

$$
  \begin{array}{rcl}
    B &\Coloneqq& B \andop B \mid B \orop B \mid A\\
    A &\Coloneqq& \tt \mid \ff \mid (B)
  \end{array} 
$$

Hopefully it's clear to you that this revised grammar generates the same sequences (but also suffers from the same left-recursion problem).

Next, let's left-factor the new $B$ productions, to lift out the common $B$ prefix of the first two rules:

$$
  \begin{array}{rcl}
    B &\Coloneqq& B\ R \mid A\\
    R &\Coloneqq& \tm{\andop}\ B \mid \tm{\orop}\ B\\
    A &\Coloneqq& \tt \mid \ff \mid (B)\\
  \end{array} 
$$

Again, the nonterminal B of the new grammar derives exactly the same language as in the old grammar, and still suffers from left recursion.  However, now we can see the clearly the kind of sequences that $B$ is recursively generating: each such sequence is either (a) a singleton $A$ or (b) an additional item $R$ added to the end of a smaller list.  If we only consider the rules for $B$, forgetting the other rules for a moment, then the sentential forms we can derive are sequences of $R$ starting with an $A$:

$$
  \begin{array}{l}
    A\\
    A\ R\\
    A\ R\ R\\
    A\ R\ R\ R\\
    \quad\vdots{}
  \end{array}
$$

It is easy to generate such sequences using our notation for sequences:

$$
  B \Coloneqq A\ R^*
$$

Since this generates exactly the same sentential forms as the $B$ rules $$B \Coloneqq B\ R \mid A$$ above, we can replace those two rules in the grammar by the rule $$B \Coloneqq A\ R^*$$ without changing the language defined.

$$
  \begin{array}{rcl}
    B &\Coloneqq& A\ R^*\\
    R &\Coloneqq& \tm{\andop}\ B \mid \tm{\orop}\ B\\
    A &\Coloneqq& \tt \mid \ff \mid (B)\\
  \end{array} 
$$

and now the grammar contains no left recursion.  In fact, if we were to build the parse table, we would see that it is LL(1).

## No Guarantees

Common prefixes (which can be removed by left factoring) and left recursion are two common problems that prevent grammars for programming languages from being left recursive, and reformulating a grammar with one of these problems is often enough to obtain an LL(1) grammar.  However, there are no guarantees - in particular, not every context-free grammar has an equivalent presentation as an LL(1) grammar, some languages expressible by CFGs are inherently not LL(1).  Even if your language does have an equivalent presentation as an LL(1) grammar, it may require more than simply applying the above two rules in order to obtain it.  A simple example can be seen in this fragment of the [Go grammar](https://go.dev/ref/spec#Expression_statements):

$$
  \begin{array}{rcl}
  \nt{SimpleStmt} &\Coloneqq& \nt{EmptyStmt} \mid \nt{ExpressionStmt} \mid \nt{SendStmt} \mid \nt{IncDecStmt} \mid \nt{Assignment} \mid \nt{ShortVarDecl}\\[2mm]
  \nt{ExpressionStmt} &\Coloneqq& \nt{Expression}\\[2mm]
  \nt{SendStmt} &\Coloneqq& \nt{Channel}\ \tm{\leftarrow}\ \nt{Expression}\\[2mm]
  \nt{Channel} &\Coloneqq& \nt{Expression}
  \end{array}
$$

Suppose we are trying to derive a string like $\tm{getChannel()}\ \tm{\leftarrow}\ \tm{3}$ starting from the nonterminal $\nt{SimpleStmt}$.  Just looking at the leftmost terminal symbol - here an identifier $\tm{getChannel}$ for a function name - isn't enough on its own to determine whether we are deriving an $\nt{ExpressionStmt}$ or a $\nt{SendStmt}$ since both start with an expression.  Yet, there isn't and _immediate_ left-factoring problem or left-recursion problem, as described above.  

However, there is a kind of indirect left-factoring problem.  If we reformulate the grammar a bit, then we can see it more clearly.  Suppose we just inline the rules for $\nt{ExpressionStmt}$, $\nt{SendStmt}$ and $\nt{Channel}$ - clearly this will not change the language of derivable strings:

$$
\begin{array}{rcl}
\nt{SimpleStmt} &\Coloneqq& \nt{EmptyStmt}\\[2mm] 
&\mid& \nt{Expression} \\[2mm]
&\mid& \nt{Expression}\ \tm{\leftarrow}\ \nt{Expression} \\[2mm]
&\mid& \nt{IncDecStmt} \\[2mm]
&\mid& \nt{Assignment} \\[2mm]
&\mid& \nt{ShortVarDecl}
\end{array}
$$

Now one can see there is really a left-factoring problem, and one can perform the transformation described above to eliminate it, although this is not what the Go language writers chose to do.
