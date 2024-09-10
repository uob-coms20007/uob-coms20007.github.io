---
layout: math
title: Concrete Syntax
mathjax: true
nav_order: 0
parent: Prologue
---

# Strings and Concrete Syntax

{% include defn-strings.liquid %}

For example, $\mathsf{hello}$ is a string over the alphabet $$\{\mathsf{a},\mathsf{b},\mathsf{c},\ldots,\mathsf{y},\mathsf{z}\}$$, but $\mathsf{Hello}$ is not.  Another string over the same alphabet is $\mathsf{abbbbb}$ and another is $\epsilon$.  Strings over the alphabet $$\{▽,△\}$$ include $\mathord{\bigtriangleup}\mathord{\bigtriangleup}\mathord{▽}$ and $\bigtriangledown$.  Strings over the alphabet $$\{\mathsf{oh},\mathsf{really}\}$$ in which each letter is one of these two strings, include $\mathsf{oh}\mathsf{really}$ and $\mathsf{really}\mathsf{oh}\mathsf{really}$, but not $\mathsf{ohr}$.

{% include defn_language.liquid %}

Now we consider some operations on strings.

{% include defn_substring.liquid %}

For example, $aab$ is a substring of $aaabbb$ but it's not a substring of $abab$.  On the other hand $a$ is a substring of both (typically, we are not too concerned about distinguishing between $a$ the letter and $a$ the string consisting of a single letter).  We also have that $\epsilon$ is a substring of any string (over any alphabet).

{% include defn_length.liquid %}

{% include defn_concat.liquid %}

## Concrete Syntax

When we speak of the *(concrete) syntax of a language* (e.g. a programming language) we typically mean a description of which strings constitute a valid sentence in the language, or more specifically for us, a valid program.  For example, the syntax of the C programming language tells us that the only the first of these two strings is a valid C program:

{% include ex_c_programs.liquid %}

By the way, we will typically consider blocks of text like those above to be strings - we won't usually care about "whitespace", that is: space characters or newlines.

## Grammars

The way that syntax is usually described is using a *context-free grammar*.  Such a grammar allows us to define a set of strings (e.g. the set of valid C programs) using a bunch of rules that stipulate how such strings can be derived.  

Let's forget about C programs now and look to something simpler.  The following grammar defines a set of simple arithmetic expressions, built out of natural numbers, addition ($+$), subtraction ($-$), multiplication ($*$) and parentheses:

{% include ex_s_grammar_arith.liquid %}

where $n$ stands for any natural number $0, 1, 2, ...$ and so on.  Each line of the grammar is called a *production rule* (or just *rule* or *production* for short).  This grammar has 7 productions.  I have labelled each with a number from 1 to 7, but this is purely to make it easier to explain what is going on, these labels are not formally part of the grammar.  The $$::=$$ symbol divides the rule into two parts, the *left-hand side* or *head* of the rule and the *right-hand side* or *body* of the rule.  The left-hand side of each rule comprises a single symbol, here either $A$, $F$ or $L$ which are called the *non-terminal* symbols.  If a rule has a non-terminal $$X$$ as its left-hand side, then we say the rule is a $$X$$-rule.  All the other symbols occurring on right-hand side apart from the non-terminal symbols are called the *terminal* symbols.  Here, the terminal symbols are $$+$$, $$-$$, $$*$$, the left and right parentheses and every natural number $$n$$.  The idea of the grammar is that it is a collection of rules for generating strings built over the terminal symbols - i.e. the set of terminal symbols is exactly the alphabet of the strings we are considering.

The way the grammar generates strings is as follows.  First, one of the non-terminal symbols is designated as the *start symbol* which is one of the non-terminals.  I didn't mention it yet, but in this grammar we have $$A$$ as the start symbol (in general whoever specifies the grammar must tell you what is the start symbol).  We begin with the start symbol $$A$$ and replace it with any of the right-hand sides of the corresponding rules, i.e. since the start symbol is $$A$$, we are looking at $$A$$-rules.  We have free choice over which rule to use.  Then we *rewrite* the start symbol $$A$$ into a string by repeatedly replacing non-terminals by their right-hand sides.  To start with, let's replace our only non-terminal $$A$$ by the right-hand side of the rule (2), namely $$F+A$$.  We write this up as:

$$
  A \to F + A
$$

Now we consider $$F+A$$.  We are allowed to replace any of the non-terminals in the given expression by one of their right-hand sides - we get to choose which non-terminal to replace and what to replace it with (within the constraints of the set of available rules).  So let's replace $$F$$ by $$L$$ using rule (4).  We now have:

$$
  A \to F + A \to L + A
$$

Next, let's choose to replace $$L$$ by the number $$3$$ according to rule (6).  The symbol $3$ is a terminal symbol.  

$$
 A \to F + A \to L + A \to 3 + A
$$

Finally, we'll replace our only remaining non-terminal $$A$$ by $$F$$ using rule (1), replace this new $$F$$ by $L * F$ using rule (5), the $L$ of this expression by $$2$$ using rule (6), the $F$ by $L$ using rule (4) and this $L$ by $6$ again using rule (6).  Doing so we obtain (writing over several lines for clarity):

$$
  \begin{array}{rcl}
  A &\to& F + A\\ 
  &\to& L + A\\ 
  &\to& 3 + A\\ 
  &\to& 3 + L * F\\ 
  &\to& 3 + 2 * F\\ 
  &\to& 3 + 2 * L\\ 
  &\to& 3 + 2 * 6.  
  \end{array}
$$

This final expression is a string over the terminal symbols and so it cannot be further rewritten.  This signals the end of the process.  We say that the string $$3+2*6$$ is *derivable* from the above grammar and the sequence of expressions above, separated by each $\to$, is its *derivation*.

The string $$3 * (42 + 6)$$ is also derivable using this grammar.  You must believe me on this because I can provide the evidence -- the derivation.  See if you can work out which rule was used in each case:

$$
  \begin{array}{rcl}
    A &\to& F\\
      &\to& L * F\\
      &\to& L * L\\
      &\to& L * (A)\\
      &\to& L * (F + A)\\
      &\to& L * (L + A)\\
      &\to& L * (L + F)\\
      &\to& L * (L + L)\\
      &\to& 3 * (L + L)\\
      &\to& 3 * (42 + L)\\
      &\to& 3 * (42 + 6)
  \end{array}
$$

Notice that once a terminal symbol is introduced during a derivation, it can never be removed again -- there is no way to replace a terminal symbol by something else, only a non-terminal symbol.  Hence, if a derivation introduces a terminal symbol during some step, then that terminal symbol must occur in the derived string (the end result of the derivation, the final expression which includes no non-terminals).  For example, the derivation above introduces the terminal symbol $$*$$ on line 2 and, sure enough, $$*$$ appears in the corresponding place in the derived string $$3 * (42 + 6)$$.

On the other hand, $$6++$$ is not a derivable string according to this grammar.  This is because it is impossible to derive $$6++$$ by following the grammar rules.  For now, you will have to just take my word on this, it should become clearer as we explore the grammar a bit more.

First, let's agree a standard convention.  Let's write all the rules with the same non-terminal on the left on a single line, by combining the right-hand sides using the *alternative* symbol, written $$\mid$$.

$$
  \begin{array}{rcl}
  A &::=& F \mid F + A \mid F - A \\
  F &::=& L \mid L * F\\
  L &::=& n \mid (A)
  \end{array}
$$

To be clear, this is just a more compact way of writing exactly the same grammar as above. There are still 7 productions, but they have been written more concisely. 

Whenever you are presented with a grammar, you should try to understand the role of each of the non-terminals.  Either someone will tell you their role, or you will have to experiment with deriving strings in order to make a guess.  Since I gave you this grammar, let me tell you what I had in mind.  

  * The start symbol $$A$$ is straightforward, it is meant to be the non-terminal used for deriving arithmetic expressions.  What I mean by this is that any sequence of replacements starting at $$A$$ will end in an arithmetic expression as we commonly know them.
  * The non-terminal $$L$$ is meant to represent *atomic arithmetic expressions*, *atomic* in the sense of being indivisible.  What I mean by this is that any sequence of replacements starting at $$L$$ will end in either a number literal or a parenthesized arithmetic expression.
  * The non-terminal $$F$$ is meant to represent *factor expressions*, that is, sequences of multiplications of atomic arithmetic expressions.

## Turtle Graphics

Part of the exercises for weeks 2, 3 and 4 involves implementing a parser, interpreter and compiler for a very simple graphics language.  This language is described by the following grammar, in which $n$ stands for any natural number:

{% include defn_turtle_s_grammar.liquid %}

A program $P$ in this language is just a sequence of commands $C$ for drawing a picture.  The basic commands are:

$$
  \begin{array}{r|l}
    \kw{up} & \text{take the pen off the page (stop drawing when moving)}\\
    \kw{down} & \text{put the pen on the page (start drawing when moving)}\\
    \kw{fd}(n) & \text{move forward $n$ units}\\
    \kw{left}(n) & \text{turn left (anticlockwise) $n$ degrees}\\
    \kw{right}(n) & \text{turn right (clockwise) $n$ degrees}\\
  \end{array}
$$

The idea is to imagine a little creature standing on a blank page of size 500x500 units, holding a pen.  A program is a sequence of commands which are instructions to the creature on how to move about the page and when to lower and raise the pen.  When the pen is lowered, the creature drags it along the page, which results in drawn lines.  The creature is traditionally a turtle.

The turtle initially starts in the lower left corner of the paper facing due East.  Consequently, the following program will draw a square of side 200 units, anchored at the bottom left corner of the page:

{% include ex_turtle_square.liquid %}

We know that this is a program in the turtle graphics language because we can carry out the following derivation:

$$
  \begin{array}{rcl}
    P &\to& C;\;P\\
      &\to& \kw{down};\;P\\
      &\to& \kw{down};\;C;\;P\\
      &\to& \kw{down};\;\kw{fd}(200);\;P\\
      &\to& \kw{down};\;\kw{fd}(200);\;C;\;P\\
      &\to& \kw{down};\;\kw{fd}(200);\;\kw{left}(90);\;P\\
      &\vdots&\\
      &\to& \kw{down};\; \kw{fd}(200);\; \kw{left}(90);\; \kw{fd}(200);\; \kw{left}(90);\; \kw{fd}(200);\; \kw{left}(90);\; \kw{fd}(200);\; P\\
      &\to& \kw{down};\; \kw{fd}(200);\; \kw{left}(90);\; \kw{fd}(200);\; \kw{left}(90);\; \kw{fd}(200);\; \kw{left}(90);\; \kw{fd}(200);\; C\\
      &\to& \kw{down};\; \kw{fd}(200);\; \kw{left}(90);\; \kw{fd}(200);\; \kw{left}(90);\; \kw{fd}(200);\; \kw{left}(90);\; \kw{fd}(200);\; \kw{up}
  \end{array}
$$

<!-- Suppose it were possible to derive $$6++$$, then there must be a derivation as evidence.   Let's think about what that derivation would look like.  It has to start at $A$ and then there are three possible next steps it could go down:

$$
 A \to F \qquad A \to F + A \qquad A \to F - A
$$

The third of these choices would introduce the terminal symbol $-$, but this does not appear in the derived expression $$6++$$, so the derivation can't possibly  -->

<!-- We will use concatenation and variables to describe the shape of strings abstractly, just as we use arithmetic operators and variables to describe numbers abstractly.  For example, to say that a string $u$ over the alphabet $$\{a,b\}$$ contains at least two occurrences of $a$, we can say that:

> $u$ is of shape $w_1aw_2aw_3$  

More precisely, with this form of words we are saying that _there exist_ strings $$w_1,w_2,w_3 \in \{a,b\}^*$$ such that $u = w_1 a w_2 a w_3$ (i.e. $u$ is exactly the concatenation of $w_1$ followed by an $a$ followed by $w_2$ followed by an $a$ followed by $w_3$).  Every string over this alphabet that contains at least two $a$ can be decomposed in this way (there may be several ways, generally), for example:

| String | Decomposition as $w_1aw_2aw_3$ |
| $bbabab$ | $w_1 = bb$, $w_2 = b$, $w_3 = b$ |
| $abbaa$  | $w_1 = \epsilon$, $w_2 = bba$, $w_3 = \epsilon$ |
| $aaabbb$  | $w_1 = \epsilon$, $w_2 = \epsilon$, $w_3 = abbb$ |

Compare this with how you would say that an integer $n$ is even: _$n$ is of shape $2*m$ (for some $m$)_.

We will often use this in combination with set notation, to describe all strings of a certain shape.  For example:
  * $$\{w_1 a w_2 a w_3 \mid w_1, w_2, w_3 \in \{a,b\}^* \}$$ - the set of all strings over $$\{a,b\}$$ containing at least two $a$
  * $$\{w_1 a w_2 a w_3 \mid w_1, w_2, w_3 \in \{b\}^* \}$$ - the set of all strings over $$\{a,b\}$$ containing exactly two $a$
  * $$\{xy \mid x,y \in \{0,1\}^* \ \text{and}\ \lvert x \rvert = \lvert y \rvert \}$$ - the set of all strings over $$\{0,1\}$$ that are of even length.
  * $$\{ 0u01u10u0 \mid u \in \{0,1\}^* \}$$ - the set of all strings over $\{0,1\}$ that are formed by repeating a substring three times consecutively, with the first wrapped in 0s, the second in 1s and the third in 0s

Note! Do not confuse, e.g. $$\{w_1 a w_2 a w_3 \mid w_1, w_2, w_3 \in \{a,b\}^* \}$$ with $$\{w_1 a w_2 a w_3\}$$: the latter is a singleton set, it contains the string $$w_1 a w_2 a w_2$$ as its only element (and for that to make sense we must have introduced $w_1$, $w_2$ and $w_3$ already).  E.g.

> Suppose, $w_1 = a$, $w_2 =aa$ and $w_3 = aaa$.  Then my favourite set is $$\{w_1aw_2aw_3\}$$ because its only element is $aaaaaa$, which is just so completely brilliant.

You may rightly ask why we bother with these descriptions: isn't "the set of all strings over $$\{a,b\}$$ containing at least two $a$" already clear enough?  Well, you are right, it is, and I will often simply write an English language description like that when it is clear enough.  However, (a) sometimes English on its own is not clear enough (see the last bullet point example above) and (b) the idea of decomposing a string with respect to concatenation is a key idea of Regular Expressions, so it is good to see it more generally here first.

{% include defn_language.liquid %} -->