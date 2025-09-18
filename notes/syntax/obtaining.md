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

The definition of the Brischeme language from the coursework sheet Appendix A is not LL(1).  Here it is:

$$
  \begin{array}{rcl}
    \nt{Prog} &\Coloneqq& \nt{Form}^*\\[4mm]
    \nt{Form} &\Coloneqq& \nt{SExpr}\\[2mm]
              &\mid& (\ \tm{define}\ \tm{ident}\ \nt{SExpr}\ )\\[4mm]
    \nt{SExpr} &\Coloneqq& \tm{literal}\\[2mm] 
                &\mid& \tm{ident}\\[2mm]
                &\mid& (\ \nt{SExpr}\ \nt{SExpr}^*\ )\\[2mm]
                &\mid& (\ \tm{primop}\ \nt{SExpr}^*\ )\\[2mm]
                &\mid& (\ \tm{lambda}\ \tm{(}\ \tm{ident}^*\ \tm{)}\ \nt{SExpr}\ )
  \end{array}
$$

If we were to construct the parse table $T$, we would see that the cell at $T[\nt{SExpr},\,\tm{(}]$ contains several rules.  If the leftmost nonterminal is $\nt{SExpr}$ and the next letter of the input is $\tm{(}$, then clearly, there are three eligible rules:

$$
  \begin{array}{rcl}
    \nt{SExpr} &\Coloneqq& (\ \nt{SExpr}\ \nt{SExpr}^*\ )\\[2mm]
    \nt{SExpr} &\Coloneqq& (\ \tm{primop}\ \nt{SExpr}^*\ )\\[2mm]
    \nt{SExpr} &\Coloneqq& (\ \tm{lambda}\ \tm{(}\ \tm{ident}^*\ \tm{)}\ \nt{SExpr}\ )
  \end{array}
$$

without knowing more about what comes after the left-parenthesis, it's not clear which rule should be chosen.

Fortunately, there is a simple remedy which is to _left factor_ the rules, which involves factoring out the common prefix (in this case '(').  Whenever we have several rules, say $k>1$ in number, for the same nonterminal with a common prefix, say $\alpha$:

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

Thus, we can rephrase the rules for $\nt{SExpr}$ by:

$$
  \begin{array}{rcl}
  \nt{SExpr} &\Coloneqq& \tm{literal}\\[2mm] 
                &\mid& \tm{ident}\\[2mm]
                &\mid& (\ R\\[4mm]
  \nt{R} &\Coloneqq& \nt{SExpr}\ \nt{SExpr}^*\ )\\[2mm]
                &\mid& \tm{primop}\ \nt{SExpr}^*\ )\\[2mm]
                &\mid& \tm{lambda}\ \tm{(}\ \tm{ident}^*\ \tm{)}\ \nt{SExpr}\ )
  \end{array}
$$

Now which of the three rules for $\nt{SExpr}$ is chosen is clearly uniquely determined by the next letter of the input, since each starts with a different terminal symbol.  Similarly, which of the three rules for $R$ is chosen is also uniquely determined because it is easy to see that $\mathsf{First}(\nt{SExpr})$ is different from $\tm{primop}$ and $\tm{lambda}$, which are clearly different from each other.  However, in general, the left factoring process may have to continue into the definition of $R$.

Incidentally, if you look at the LL(1) grammar in Appendix C of the Brischeme coursework, you will see that we also factored out the common suffix of the three rules in $R$.  So that this particular rule is rather more like:

$$
  \nt{SExpr} \Coloneqq (\ R \ )
$$

This is purely an aesthetic choice, it's nice to see that there has to be a matching parenthesis.

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

There are other ways to generate such sequences.  In fact three different grammars come to mind for generating lists of something (think also of defining a list datatype):

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
    S \Coloneqq \tm{stmt}\ (\tm{;}\ \tm{stmt})^*
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

Common prefixes (which can be removed by left factoring) and left recursion are two common problems that prevent grammars for programming languages from being left recursive, and reformulating a grammar with one of these problems is often enough to obtain an LL(1) grammar.  However, there are no guarantees.  The grammar we obtained for Brischeme after left factoring no longer contains any common prefixes or left recursion, but it is still not LL(1).

$$
  \begin{array}{rcl}
  \nt{Prog} &\Coloneqq& \nt{Form}^*\\[4mm]
  \nt{Form} &\Coloneqq& \nt{SExpr}\\[2mm]
            &\mid& (\ \tm{define}\ \tm{ident}\ \nt{SExpr}\ )\\[4mm]
  \nt{SExpr} &\Coloneqq& \tm{literal}\\[2mm] 
                &\mid& \tm{ident}\\[2mm]
                &\mid& (\ R\ )\\[4mm]
  \nt{R} &\Coloneqq& \nt{SExpr}\ \nt{SExpr}^*\\[2mm]
                &\mid& \tm{primop}\ \nt{SExpr}^*\\[2mm]
                &\mid& \tm{lambda}\ \tm{(}\ \tm{ident}^*\ \tm{)}\ \nt{SExpr}
  \end{array}
$$

In particular, if, during parsing, we are attempting to derive a string starting with a left parenthesis $($ from the nonterminal $\nt{Form}$, then the rule to use is not determined: both of the rules for $\nt{Form}$ can derive strings starting with $($.  This can be avoided by introducing a new nonterminal $\nt{CForm}$ and reformulating the rules for $\nt{Form}$:

$$
  \begin{array}{rcl}
    \nt{Form} &\Coloneqq& \tm{ident}\\[2mm] 
              &\mid& \tm{literal}\\[2mm]
              &\mid& (\ \nt{CForm} \ )\\[4mm]
    \nt{CForm} &\Coloneqq& \nt{R} \mid \tm{define}\ \tm{ident}\ \nt{SExpr}
  \end{array}
$$

Essentially, we have inlined the rules for $\nt{SExpr}$ and then performed left factoring; but this required a little bit of thinking -- to understand what the problem is, as identified in the parsing table, and how to solve it -- rather than _only_ following one of the two recipes above.
