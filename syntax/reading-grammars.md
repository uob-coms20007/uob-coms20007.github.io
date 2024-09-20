---
layout: math
title: Reading Grammars
mathjax: true
nav_order: 2
parent: Syntax
---

# Context Free Grammars

## Concrete Syntax

When we speak of the *(concrete) syntax of a language* (e.g. a programming language) we typically mean a description of which strings constitute a valid sentence in the language, or more specifically for us, a valid program.  For example, the syntax of the C programming language tells us that the only the first of these two strings is a valid C program:

{% include ex_c_programs.liquid %}

By the way, we will typically consider blocks of text like those above to be strings - we won't usually care about "whitespace", that is: space characters or newlines.

## Grammars

The way that syntax is usually described is using a *context-free grammar*.  Such a grammar allows us to define a set of strings (e.g. the set of valid C programs) using a bunch of rules that stipulate how such strings can be derived.  

Let's forget about C programs for now and look to something simpler.  The following grammar defines a set of simple Boolean expressions, built out of true, false, conjunction ($\andop$), disjunction ($\orop$), and parentheses:

$$
  \begin{array}{rrcl}
  (1) & B &\longrightarrow& F \\
  (2) & B &\longrightarrow& F \orop B \\ 
  (3) & F &\longrightarrow& L \\
  (4) & F &\longrightarrow& L \andop F\\
  (5) & L &\longrightarrow& \tt \\ 
  (6) & L &\longrightarrow& \ff\\
  (7) & L &\longrightarrow& (B)
  \end{array}
$$

Each line of the grammar is called a *production rule* (or just *rule* or *production* for short).  This grammar has 7 productions.  I have labelled each with a number from 1 to 7, but this is purely to make it easier to explain what is going on, these labels are not formally part of the grammar.  The long right arrow, $$\longrightarrow$$, divides the rule into two parts, the *left-hand side* or *head* of the rule and the *right-hand side* or *body* of the rule.  The left-hand side of each rule comprises a single symbol, here either $A$, $F$ or $L$ which are called the *non-terminal* symbols.  If a rule has a non-terminal $$X$$ as its left-hand side, then we say the rule is a $$X$$-rule.  All the other symbols occurring on right-hand side apart from the non-terminal symbols are called the *terminal* symbols.  Here, the terminal symbols are $$\tt$$, $$\ff$$, $$\andop$$, $\orop$, and the left and right parentheses.  The idea of the grammar is that it is a collection of rules for generating strings built over the terminal symbols - i.e. the set of terminal symbols is exactly the alphabet of the strings we are considering.

The way the grammar generates strings is as follows.  First, one of the non-terminal symbols is designated as the *start symbol* which is one of the non-terminals.  I didn't mention it yet, but in this grammar we have $$B$$ as the start symbol (in general whoever specifies the grammar must tell you what is the start symbol).  We begin with the start symbol $$B$$ and replace it with any of the right-hand sides of the corresponding rules, i.e. since the start symbol is $$B$$, we are looking at $$B$$-rules.  We have free choice over which rule to use.  Then we *rewrite* the start symbol $$B$$ into a string by repeatedly replacing non-terminals by their right-hand sides.  To start with, let's replace our only non-terminal $$B$$ by the right-hand side of the rule (2), namely $$F \orop B$$.  We write this up as:

$$
  B \to F \orop B
$$

Now we consider $$F \orop B$$.  We are allowed to replace any of the non-terminals in the given expression by one of their right-hand sides - we get to choose which non-terminal to replace and what to replace it with (within the constraints of the set of available rules).  So let's replace $$F$$ by $$L$$ using rule (3).  We now have:

$$
  B \to F \orop B \to L \orop B
$$

Next, let's choose to replace $$L$$ by the terminal symbol $$\tt$$, according to rule (5).  

$$
 B \to F \orop B \to L \orop B \to \tt \orop B
$$

Finally, we'll replace our only remaining non-terminal $$B$$ by $$F$$ using rule (1), replace this new $$F$$ by $L \andop F$ using rule (4), the $L$ of this expression by $$\ff$$ using rule (6), the $F$ by $L$ using rule (4) and this $L$ by $\ff$ again using rule (6).  Doing so we obtain (writing over several lines for clarity):

$$
  \begin{array}{rcl}
  B &\to& F \orop B\\ 
  &\to& L \orop B\\ 
  &\to& \tt \orop B\\ 
  &\to& \tt \orop L \andop F\\ 
  &\to& \tt \orop \ff \andop F\\ 
  &\to& \tt \orop \ff \andop L\\ 
  &\to& \tt \orop \ff \andop \ff.  
  \end{array}
$$

This final expression is a string over the terminal symbols and so it cannot be further rewritten.  This signals the end of the process.  We say that the string $$\tt \orop \ff \andop \ff$$ is *derivable* from the above grammar and the sequence of expressions above, separated by each $\to$, is its *derivation*.

The string $$\ff \andop (\tt \orop \ff)$$ is also derivable using this grammar.  You must believe me on this because I can provide the evidence -- the derivation.  See if you can work out which rule was used in each case:

$$
  \begin{array}{rcl}
    B &\to& F\\
      &\to& L \andop F\\
      &\to& L \andop L\\
      &\to& L \andop (B)\\
      &\to& L \andop (F \orop B)\\
      &\to& L \andop (L \orop B)\\
      &\to& L \andop (L \orop F)\\
      &\to& L \andop (L \orop L)\\
      &\to& \ff \andop (L \orop L)\\
      &\to& \ff \andop (\tt \orop L)\\
      &\to& \ff \andop (\tt \orop \ff)
  \end{array}
$$

Notice that once a terminal symbol is introduced during a derivation, it can never be removed again -- there is no way to replace a terminal symbol by something else, only a non-terminal symbol.  Hence, if a derivation introduces a terminal symbol during some step, then that terminal symbol must occur in the derived string (the end result of the derivation, the final expression which includes no non-terminals).  For example, the derivation above introduces the terminal symbol $$\andop$$ on line 2 and, sure enough, $$\andop$$ appears in the corresponding place in the derived string $$\ff \andop (\tt \orop \ff)$$.

On the other hand, $$\tt \orop \orop$$ is not a derivable string according to this grammar.  This is because it is impossible to derive $$\tt \orop \orop$$ by following the grammar rules.  For now, you will have to just take my word on this, it should become clearer as we explore the grammar a bit more.

First, let's agree a standard convention.  Let's write all the rules with the same non-terminal on the left on a single line, by combining the right-hand sides using the *alternative* symbol, written $$\mid$$ (sorry that it's a bit similar to our disjunction operator).

$$
  \begin{array}{rcl}
  B &\longrightarrow& F \mid F \orop B\\
  F &\longrightarrow& L \mid L \andop F\\
  L &\longrightarrow& \tt \mid \ff \mid (B)
  \end{array}
$$

To be clear, this is just a more compact way of writing exactly the same grammar as above. There are still 7 productions, but they have been written more concisely. 

Whenever you are presented with a grammar, you should try to understand the role of each of the non-terminals.  Either someone will tell you their role, or you will have to experiment with deriving strings in order to make a guess.  Since I gave you this grammar, let me tell you what I had in mind.  

  * The start symbol $$B$$ is meant to represent possibly trivial *disjunctions*.  What I mean by this is that any sequence of replacements starting at $$B$$ will end in a disjunction expression, but it may be a trivial disjunction of one element, such as $\tt$ (here we are viewing $tt$ as a disjunction of size one).  It is through this allowance of trivial disjunctions that we are able to generate all possible Boolean expressions over the truth values, disjunction and conjunction - if our Boolean expression is actually, say, a conjunction then it can be viewed as a trivial disjunction of one element, the element being a conjunction.
  * The non-terminal $$F$$ is meant to represent possibly trivial *conjunctions*.  What I mean by this is that any sequence of replacements starting at $$F$$ will derive a conjunction expression, though it may be a trivial conjunction of one element.
  * The non-terminal $$L$$ is meant to represent *atomic Boolean expressions*, *atomic* in the sense of being indivisible.  What I mean by this is that any sequence of replacements starting at $$L$$ will end in either a literal or a parenthesized Boolean expression

Computer Scientists are not the only ones to use context-free grammars, they are also used by linguists to describe the phrase structure of natural language.  The following example is taken from Sipser (Section 2.1):

$$
  \begin{array}{rcl}
    \bnfnt{SENTENCE} &\longrightarrow& \bnfnt{NOUN-PHRASE}\ \bnfnt{VERB-PHRASE}\\
    \bnfnt{NOUN-PHRASE} &\longrightarrow& \bnfnt{CMPLX-NOUN} \mid \bnfnt{CMPLX-NOUN}\ \bnfnt{PREP-PHRASE}\\
    \bnfnt{VERB-PHRASE} &\longrightarrow&  \bnfnt{CMPLX-VERB} \mid \bnfnt{CMPLX-VERB}\ \bnfnt{PREP-PHRASE}\\
    \bnfnt{PREP-PHRASE} &\longrightarrow& \bnfnt{PREP}\ \bnfnt{CMPLX-NOUN}\\
    \bnfnt{CMPLX-NOUN} &\longrightarrow& \bnfnt{ARTICLE}\ \bnfnt{NOUN}\\
    \bnfnt{CMPLX-VERB} &\longrightarrow& \bnfnt{VERB} \mid \bnfnt{VERB}\ \bnfnt{NOUN-PHRASE}\\
    \bnfnt{ARTICLE} &\longrightarrow& \textrm{a} \mid \textrm{the} \\
    \bnfnt{NOUN} &\longrightarrow& \textrm{boy} \mid \textrm{girl} \mid \textrm{flower}\\
    \bnfnt{VERB} &\longrightarrow& \textrm{touches} \mid \textrm{likes} \mid \textrm{sees}\\
    \bnfnt{PREP} &\longrightarrow& \textrm{with}
  \end{array} 
$$

Here, the non-terminals are written as capitilised words written in angle brackets, which is a popular format for writing grammars called *Backus-Naur Form* (BNF).  The terminal symbols are 'a', 'the', 'boy', 'girl', 'flower', 'touches', 'likes', 'sees' and 'with'.  Words (sequences of terminal symbols) generated by this grammar include: 

- a boy sees
- the boy sees a flower
- a girl with a flower likes the boy

For each, there is a derivation.  For example:

$$
  \begin{align*}
    &\bnfnt{SENTENCE}\\
    &\to \bnfnt{NOUN-PHRASE}\ \bnfnt{VERB-PHRASE}\\
    &\to \bnfnt{CMPLX-NOUN}\ \bnfnt{VERB-PHRASE}\\
    &\to \bnfnt{ARTICLE}\ \bnfnt{NOUN}\ \bnfnt{VERB-PHRASE}\\
    &\to \textrm{a}\ \bnfnt{NOUN}\ \bnfnt{VERB-PHRASE}\\
    &\to \textrm{a}\ \textrm{boy}\ \bnfnt{VERB-PHRASE}\\
    &\to \textrm{a}\ \textrm{boy}\ \bnfnt{CMPLX-VERB}\\
    &\to \textrm{a}\ \textrm{boy}\ \bnfnt{VERB}\\
    &\to \textrm{a}\ \textrm{boy}\ \textrm{sees}
  \end{align*}
$$


## Grammars in the Wild

If you look up your favourite programming language, you can probably find a grammar that describes its (concrete) syntax.  There is no standard notation for grammars, so you will need to look carefully at the description of the grammar to understand the particular format.  The grammar for the C programming language hosted at [microsoft.com](https://learn.microsoft.com/en-us/cpp/c-language/phrase-structure-grammar?view=msvc-170) uses: 

- italicised words for nonterminals, 
- bold words for terminals, 
- colon as the separator between the left and right hand sides of production rules
- new lines to separate alternatives

By the way, in practice it is a good idea to use words for nonterminals like this (rather than just a single capital letter as we do) because then you can use the word to describe the kind of syntax that the nonterminal derives.  However, it is a bit less convenient for mathematical reasoning.

For example, assuming that $0$ is an expression (can be derived from the nonterminal *expression*) we can derive the following valid C statement form:

$$
  \begin{align*}
    & \textit{statement}\\
    &\to \textit{selection-statement} \\
    &\to \textbf{switch (}\textit{expression}\ \textbf{)}\ \textit{statement}\\
    &\;\;\vdots{}\\
    &\to \textbf{switch (}0\textbf{)}\ \textit{statement}\\
    &\to \textbf{switch (}0\textbf{)}\ \textbf{if (}\textit{expression}\textbf{)}\ \textit{statement}\ \textbf{else}\ \textit{statement}\\
    &\;\;\vdots{}\\
    &\to \textbf{switch (}0\textbf{)}\ \textbf{if (}0\textbf{)}\ \textit{statement}\ \textbf{else}\ \textit{statement}\\
    &\to \textbf{switch (}0\textbf{)}\ \textbf{if (}0\textbf{)}\ \textit{labeled-statement}\ \textbf{else}\ \textit{statement}\\
    &\to \textbf{switch (}0\textbf{)}\ \textbf{if (}0\textbf{)}\ \textbf{case}\ \textit{constant-expression}:\ \textit{statement}\ \textbf{else}\ \textit{statement}\\
    &\to \textbf{switch (}0\textbf{)}\ \textbf{if (}0\textbf{)}\ \textbf{case}\ 0:\ \textit{statement}\ \textbf{else}\ \textit{statement}\\
    &\to \textbf{switch (}0\textbf{)}\ \textbf{if (}0\textbf{)}\ \textbf{case}\ 0:\ \textit{statement}\ \textbf{else}\ \textit{labeled-statement}\\
    &\to \textbf{switch (}0\textbf{)}\ \textbf{if (}0\textbf{)}\ \textbf{case}\ 0:\ \textit{statement}\ \textbf{else}\ \textbf{case}\ \textit{constant-expression}:\ \textit{statement}\\
    &\to \textbf{switch (}0\textbf{)}\ \textbf{if (}0\textbf{)}\ \textbf{case}\ 0:\ \textit{statement}\ \textbf{else}\ \textbf{case}\ 1:\ \textit{statement}
  \end{align*}
$$

Since it is possible to derive $x = 2;$ and $x = 3;$ from the nonterminal *statement*, skipping some more steps gives us the valid C program statement (recall that we don't care about whitespace at this time - space characters, carriage returns and newlines):

```c
  switch (0) 
    if (0) 
      case 0: x=2; 
    else 
      case 1: x=3;
```

The semantics of this statement, how it executes, is a little counterintuitive, because we naturally expect that a conditional statement whose guard is constantly false should be equivalent to just executing it's else branch.   However, this ability to interlace switch statements with other statements (like the conditional statement in this case) can be useful, cf. Duff's Device.

It's possible in C because every *statement* can be a *labeled-statement*, which can be one of the cases in the switch.  This is not allowed in, for example, Java.  

Consider the grammar for Java programs hosted at [oracle.com](https://docs.oracle.com/javase/specs/jls/se7/html/jls-18.html). According to this grammar, the statement part of a switch is not just any old statement but a *SwitchBlockStatementGroups*, and it is only through this particular non-terminal that we can ultimately derive something equivalent to C's *labeled-statement*.

Something that both the C and Java syntax is an assignment statement (actually, in both of these languages assignment to (mutation of) variables is considered an expression, but this is bad form).  In C we have:
  
$$
  \begin{align*}
    & \textit{statement}\\
    &\to \textit{expression-statement}\\
    &\to \textit{expression}_{\text{opt}}\textbf{;}
  \end{align*}
$$

Now, the grammar definition explains that it uses a convention whereby a nonterminal symbol that occurs on the RHS of a rule and is subscripted with the word *opt* is considered optional, in other words we can just drop it from the sentential form.  It is common for grammar writers to use conventions like this in order to write the grammar more concisely: however, we can always express this optional form without the convention by replacing it with a new non-terminal as follows.  The last rule of the grammar we used was:

$$
  \textit{expression-statement} : \textit{expression}_{\text{opt}}\textbf{;}
$$

in which the nonterminal symbol *expression* is considered optional.  It can be equivalently written as the following, completely standard grammar, by introducing a new nonterminal, let's call it *expression-opt*.

$$
  \begin{array}{rcl}
    \textit{expression-statement} : \textit{expression}_{\text{opt}}\textbf{;}\\
    \textit{expression-opt} &:& \textit{expression} \mid \epsilon
  \end{array}
$$

However, the language designers probably felt that, because these optional components occur all over the grammar, it was more readable without the extra nonterminals.  In our case, I want to derive an actual expression, so the derivation can continue as follows:

$$
  \begin{align*}
    &\textit{expression}_{\text{opt}}\textbf{;}\\
    &\to \textit{expression}\textbf{;}\\
    &\to \textit{assignment-expression}\textbf{;}\\
    &\to \textit{unary-expression}\ \textit{assignment-operator}\ \textit{assignment-expression}\textbf{;}\\
    &\to \textit{unary-expression}\ \mathbf{=}\ \textit{assignment-expression}\textbf{;}\\
    &\to \textit{postfix-expression}\ \mathbf{=}\ \textit{assignment-expression}\textbf{;}\\
    &\to \textit{primary-expression}\ \mathbf{=}\ \textit{assignment-expression}\textbf{;}\\
    &\to \textit{identifier}\ \mathbf{=}\ \textit{assignment-expression}\textbf{;}
  \end{align*}
$$

To find out what an *identifier* can look like (which strings can derive from this nonterminal), we need to consult the *lexical grammar*.  The lexical grammar is just another part of the overall syntax grammar, separated out for conceptual reasons that we will come to in a later lecture.  According to the production rules for an identifier, a single letter or underscore constitutes an identifier, let's suppose we choose $x$.

$$
  \begin{align*}
    &\textit{identifier}\ \mathbf{=}\ \textit{assignment-expression}\textbf{;}\\
    &\to \textit{identifier-nondigit}\ \mathbf{=}\ \textit{assignment-expression}\textbf{;}\\
    &\to \textit{identifier-nondigit}\ \mathbf{=}\ \textit{assignment-expression}\textbf{;}\\
    &\to \textit{nondigit}\ \mathbf{=}\ \textit{assignment-expression}\textbf{;}\\
    &\to x\ \mathbf{=}\ \textit{assignment-expression}\textbf{;}
  \end{align*}
$$

Then, through some laborious derivation we can see that an *assignment-expression* can also just be a constant, like $3$, so we obtain the valid C statement:

```c
  x = 3;
```

What I want to point out with this example is that the grammar gives us free choice over the identifier -- in particular we are *not* required to choose only from those identifiers that are already declared "earlier" in the program.  If you follow the only the rules of the grammar, then the following is a C program:

```c
  int main() {
    x = 3;
  }
```
But, of course, this is not a valid C program.  The problem is that a production rules of a context-free grammar, say the rule $\textit{nondigit} : x$, can be applied regardless of what is around it, for example, whether or not there is an earlier declaration of $x$.  In other words, the process of applying a rule is ignorant of the context, hence the name *context-free* grammar.  So this kind of error, a use-before-declaration error, will be caught by the compiler in a different phase.  Later, we will do some mathematics to prove that context-free grammars cannot express this kind of relationship between an identifier and its point of declaration.

Another typical kind of error that cannot generally be prevented by the design of the syntax grammar is the following.  Let's take a look at the syntax of Haskell this time, from [haskell.org](https://www.haskell.org/onlinereport/syntax-iso.html).  In this grammar, they use -> to separate the left and right hand side of rules, and they use \| to separate alternative productions for the same non-terminal.  According to the grammar an expression *exp* can be a function application *fexp*, so we can start a derivation:

$$
  \begin{align*}  
    & \textit{exp}\\
    &\to \textit{fexp}\\
    &\to \lbrack\textit{fexp}\rbrack\ \textit{aexp}
  \end{align*}  
$$

Here we note that this grammar uses square brackets [ ] to denote an optional component (equivalent to the subscript opt).

$$
  \begin{align*}  
    &[\textit{fexp}]\ \textit{aexp}\\
    &\to \textit{fexp}\ \textit{aexp}\\
    &\to [\textit{fexp}]\ \textit{aexp}\ \textit{aexp}\\
    &\to \textit{aexp}\ \textit{aexp}\\
    &\to \textit{literal}\ \textit{aexp}\\
    &\to \textit{integer}\ \textit{aexp}\\
    &\to \textit{decimal}\ \textit{aexp}\\
    &\to \textit{digit}\{\textit{digit}\}\ \textit{aexp}
  \end{align*}  
$$

Here the grammar uses a convention that a non-terminal enclosed in braces { } denotes a component that may occur any number of times (including zero).  Here, we choose to use it zero times.

$$
  \begin{align*}
    &\textit{digit}\{\textit{digit}\}\ \textit{aexp}\\
    &\to \textit{digit}\ \textit{aexp}\\
    &\to \textit{ascDigit}\ \textit{aexp}\\
    &\to 3\ \textit{aexp}\\
    &\to 3\ \textit{gcon}\\
    &\to 3\ ()\\
  \end{align*}
$$

So, according to the grammar a syntactically correct expression is `3()`, i.e. calling 3 as if it was a function.  This is an example of a type error, and they too are an example of an error that is generally not preventable through grammar design.  Type systems are a large and important subject in mathematics and in computer science that we won't touch on in this unit, but which you can learn more about in the third and fourth year units *Types and Lambda Calculus* and *Advanced Topics in Programming Languages*.
