---
layout: math
title: LL(1) Grammars
mathjax: true
nav_order: 5
parent: Syntax
---


# LL(1) Grammars for Predictive Parsing

## Motivation

There are lots of methods that have been designed for parsing strings according to a grammar, but we will be studying just one, which happens to be one of the most widely used in practice.  This method is called predictive parsing, and many of the programming languages you regularly use are parsed by some variant on a predictive parser.

Suppose we have a grammar such as this simple grammar for Boolean expressions:

$$
  B \longrightarrow B \andop B \mid B \orop B \mid \tt \mid \ff \mid \tm{id} \mid (B)
$$

If you fix a string (the string you want to parse) then you can try to check whether or not it is in the language described by the grammar by constructing a derivation.  However, a key difficulty is that, from the very start, there are choices - which production rule should we use? 

For example, consider trying to derive the string $\tt \orop \ff \andop \tt$.  We start from $B$, but which rule should we pick?  Should we pick the first rule $$B \longrightarrow B \andop B$$ because the string contains the terminal $\andop$?  Or maybe we should pick the second rule $$B \longrightarrow B \orop B$$ because the string contains the terminal $\orop$?  

Often it can require some human insight in order to make the right choice, and this is not a good basis for an efficient algorithm.  

The idea of predictive parsing is that we limit ourselves to parsing grammars for which making these choices is easy.  

_LL(1)_ grammars have the property that, at every step of the derivation, which rule we choose is completely determined by a combination of the leftmost non-terminal in the current sentential form (this is the first L in "LL(1)"), and a fixed size prefix of the remaining input string, say the single leftmost character (this is the second L and the 1).

The following grammar may look strange, but it is actually equivalent to the previous one, in the sense that they generate the same languages, except that this one makes the end-of-input symbol explicit as a terminal symbol.  It is traditional to use \\$ for this - so the first rule of the grammar says that a string derivable from $D$ consists of a prefix derivable from $C$, a suffix derivable from $D'$ and that's it, i.e. we next meet the end-of-input marker.  The start symbol of the new grammar is $D$:

$$
  \begin{array}{lcl}
    D &\longrightarrow& C\ D'\ $ \\
    D' &\longrightarrow& \orop C\ D' \mid \epsilon \\
    C &\longrightarrow& A\ C' \\
    C' &\longrightarrow& \andop A\ C' \mid \epsilon \\
    A &\longrightarrow& \tt \mid \ff \mid \tm{id} \mid (D)
  \end{array}
$$

We'll get into how one can come up with a grammar like this in the next lecture, but for now I hope you can simply accept it is an alternative way to describe this language of simple Boolean expressions.  

Although it is arguably less readable, this grammar has the advantage that it is LL(1), i.e. for any input string in the language, we can construct a derivation in which the choice of production at each step is completely determined by the leftmost non-terminal in the sentential form and the first character of the remaining input.  Let's try to derive $\tt \orop \ff \andop \tt\ $$.

We start from the start symbol $D$ and immediately we have no choice, we must derive:

$$
  D \to C\ D' \$
$$

Next, we consider the leftmost non-terminal in the sentential form we have reached.  Again we have no choice since there is only one rule, so we must derive:

$$
  C\ D'\ $ \to A\ C'\ D'\ $
$$

Next, we consider the leftmost non-terminal again, which is now $A$.  Here there are three productions to choose from so we also consider the first letter of the remaining input.  The first letter of the input is $\tt$ so there is really only one applicable choice, namely the production $A \longrightarrow \tt$:

$$
  A\ C'\ D'\ $ \to \tt\ C'\ D'\ $
$$

In other words the production was (uniquely) determined once we knew the nonterminal we are trying to derive from and the first letter of the input.

Now derived that first letter (token) $\tt$, so we remove it from the input and continue to try to derive the remainder $\orop \ff \andop \tt$.  The next leftmost non-terminal is $C'$ and here we have two productions available for $C'$.  So we consult the first letter of the remaining input, which is $\orop$.  Of the two possible production rules for $C'$, only one can potentially derive a string starting with $\orop$ (the other must derive strings starting with $\andop$). So the rule $C' \longrightarrow \epsilon$ is uniquely determined:

$$
  \tt\ C\ D'\ $ \to \tt\ D'\ $
$$

Now the leftmost non-terminal is $D'$ and again we have two choices, but a glance at the first letter $\orop$ of the remaining input tells us that the first of the two rules for $D'$ is the correct one, and we derive:

$$
  \tt\ D'\ $ \to \tt\ \orop\ C\ D'\ $
$$

We cross off $\orop$ from the remaining input, and try to derive the rest $\ff\ \andop\ \tt$.   Since the grammar is LL(1) and the given string indeed belongs to the language of this grammar, we can continue this strategy and we are guaranteed to construct a leftmost derivation which required no human insight at all:

$$
  \begin{align*}
       & \tt \orop\ C\ D'\ $\\
    \to\ & \tt \orop\ A\ C'\ D'\ $\\
    \to\ & \tt \orop\ \ff\ C'\ D'\ $\\
    \to\ & \tt \orop\ \ff\ \andop\ A\ C'\ D'\ $\\
    \to\ & \tt \orop\ \ff\ \andop\ \tt\ C'\ D'\ $\\
    \to\ & \tt \orop\ \ff\ \andop\ \tt\ D'\ $\\
    \to\ & \tt \orop\ \ff\ \andop\ \tt\ $
  \end{align*}
$$

This is something we can turn into an algorithm, and a very efficient and simple algorithm at that.

## Structure of an LL(1) Grammar

We'll see a nice way to implement this algorithm in the next lecture, but for now I want to get a better (i.e. more precise) handle on what makes a grammar LL(1).  To do this we have to consider three key properties of non-terminals $X$ in a given grammar, say with terminal symbols drawn from $\Sigma$ and start symbol $S$, called $\nullable(X)$, $\first(X)$ and $\follow(X)$. 

<div class="defn" markdown=1>
$\nullable(X)$ is a proposition indicating whether or not the empty string is derivable from $X$, i.e. 

$$
  \nullable(X) \;\text{iff}\; X \to^* \epsilon
$$
</div>

For example, in the grammar above whose only non-terminal is $$B$$, $$\nullable(B)$$ is false, since the empty string is not derivable.


<div class="defn" markdown=1>
$\first(X)$ is the set of terminal symbols that start sentential forms derivable from $X$, i.e. 

$$
  \first(X) = \{ a \in \Sigma \mid \exists \beta.\, X \to^* a\beta \}
$$ 
</div>

For example, in the grammar above whose only non-terminal is $$B$$, $$\first(B) = \{\tt, \ff, \tm{id}, ( \}$$ because $B$ derives Boolean expressions which can only begin with one of those terminal symbols.

<div class="defn" markdown=1>
$\follow(X)$ is the set of terminal symbols that can appear immediately following non-terminal $X$ in a derivable sentential form, i.e.

$$
  \follow(X) = \{ a \in \Sigma \mid \exists \alpha \beta.\: S \to^* \alpha X a \beta \}
$$
</div>

For example, in the grammar above whose only non-terminal is $$B$$, $$\follow(B) = \{),\andop,\orop\}$$, as can be seen by the following witnessing derivations, respectively:

$$
  \begin{align*}
    B &\to^* (B) \quad \text{with $\alpha=($ and $\beta=\epsilon$}\\
    B &\to^* B \andop B \quad \text{with $\alpha=\epsilon$ and $\beta=B$}\\
    B &\to^* B \orop B \quad \text{with $\alpha=\epsilon$ and $\beta=B$}
  \end{align*}
$$

(In this case, all the witnessing sentential forms are obtained after just one step of derivation, but in general it may require many steps.)


If one were to calculate these properties naively, it seems like it would take forever since there are potentially infinitely many derivations to check.  Luckily, they can each be calculated by a simple finite procedure.  For now though, let's just assume we have access to these properties and see how they allow us to analyse a given grammar to see whether or not it is appropriate for us in a predictive parser.

For this purpose, it is useful to extend the definitions of $\nullable(X)$ and $\first(X)$ so that we can ask about the nullability of and the first terminal symbols derivable from, a sentential form $\alpha$, which we shall write as $\nullable_s(\alpha)$ and $\first_s(\alpha)$ respectively.

<div class="defn" markdown=1>
$$
  \nullable_s(\alpha) =
    \begin{cases}
      \mathsf{true} & \text{if $\alpha = \epsilon$}\\
      \mathsf{false} & \text{if $\alpha$ is of shape $a\beta$}\\
      \nullable(X) \wedge \nullable_s(\beta) & \text{if $\alpha$ is of shape $X\beta$}
    \end{cases}
$$
</div>

The idea is that an empty sentential form is, of course, nullable, and a sentential form beginning with a terminal symbol $a$ is, of course, not nullable.  A sentential form beginning with a non-terminal $X$ is a bit more subtle, it is nullable just if $X$ is nullable _and_ the rest of the sentential form is again (recursively) nullable.

<div class="defn" markdown=1>
$$
  \first_s(\alpha) =
    \begin{cases}
      \emptyset & \text{if $\alpha = \epsilon$}\\
      \{a\} & \text{if $\alpha$ is of shape $a\beta$}\\
      \first(X) & \text{if $\alpha$ is of shape $X\beta$ and $\neg \nullable(X)$}\\
      \first(X) \cup \first_s(\beta) & \text{if $\alpha$ is of shape $X\beta$ and $\nullable(X)$}
    \end{cases}
$$
</div>

The idea is that no terminals that can start any sentential form derivable from $\epsilon$.  Only the terminal symbol $a$ can start any sentential form derivable from a sentential form already starting with $a$.  Finally, the terminal symbols that can start a sentential form derivable from a sentential form of shape $X\beta$ -- that is, starting with some non-terminal $X$ -- are those that can start sentential forms derivable from $X$ _and_ if $X$ is nullable, then also those that (recursively) can start forms derivable from $\beta$.


Recall that the key idea of a predictive parser (with lookahead 1) is that it tries to derive a given string starting from the start symbol of the grammar and, at each step in the derivation, _which rule to choose should be completely determined by a combination of the left-most non-terminal and the next letter of the input_.

Therefore, to check if a grammar is appropriate for predictive parsing, we build a table $T$ that describes the possible meaningful choices $T[X,a]$ of grammar rule for each combination of non-terminal $X$ (representing the left-most non-terminal in the current sentential form) and terminal $a$ (representing the next letter of the input).

<div class="defn" markdown="1">
We define the __parsing table__, usually $T$, for a given grammar as a 2d array in which each entry $T[X,a]$ is a set of production rules from the grammar, such that some rule $X \longrightarrow \beta$ is in the set $T[X,a]$ just if, either:
  
  1. $a \in \first_s(\beta)$
  2. or, $\nullable_s(\beta)$ and $a \in \follow(X)$
</div>

To see why this makes sense, suppose we are in the middle of building a derivation for some string $s$ and the next unconsumed/unmatched character is $a$ and the leftmost non-terminal symbol is $X$.  

In other words, we have done some derivation like:

$$
  \begin{align*}
  S &\to \alpha_1\\
    &\to \alpha_2\\
    &\;\;\,\vdots{}\\
    &\to b_1\ b_2\cdots{}b_k\ X\ \beta
  \end{align*}
$$

ending with a sentential form that has some prefix of terminal symbols $b_1\ b_2 \cdots{} b_k$ followed by a non-terminal $X$.  If we've been crossing off letters have already been derived from the input string, then we are left wanting to derive next the letter $a$ and then some remaining substring $u$.

<img src="../assets/syntax/matched-prefix.png" style="max-width:400px;"/>

So, intuitively, there are two possible ways of continuing our derivation in order to derive this next $a$.  

  * We could try to derive a string starting with $a$ using this leftmost non-terminal $X$.  This only makes sense as a strategy if there is a rule $X \longrightarrow \alpha$ that is capable of deriving a string starting with $a$, in other words, if $a \in \first_s(\alpha)$.  This then corresponds to condition 1 above. 

  * Alternatively, we could derive $\epsilon$ using this leftmost non-terminal $X$ and postpone deriving a string starting with $a$ to the remainder of the sentential form $\beta$.  This only makes sense as a strategy if (a) $X$ can derive $\epsilon$ (i.e. $\nullable(X)$) and (b) it is possible to derive a string starting from $a$ using the sentential form following $X$ (i.e. $a \in \follow(X)$).  This corresponds to condition 2 above.

A grammar is suitable for predictive parsing if the associated parsing table $T$ contains _at most one_ rule in each cell.  This way, there is no choice about which rule to pick at any given point either there is no possible rule to apply, or the rule is uniquely determined.

{: .defn }
A grammar whose parsing table contains at most one rule in each cell is called LL(1).

For example, the parsing table for the $B$ grammar above is:

|     | $($ | $)$ | $\andop$ | $\orop$ | $\tt$ | $\ff$ | $\tm{id}$ |
| --- | --- | --- | --- | --- | ---| --- | --- |
| $B$ | $B \longrightarrow (B)$ $B \longrightarrow B \andop B$ $B \longrightarrow B \orop B$ | | | | $B \longrightarrow \tt$ $B \longrightarrow B \andop B$ $B \longrightarrow B \orop B$ | $B \longrightarrow \ff$ $B \longrightarrow B \andop B$ $B \longrightarrow B \orop B$ | $B \longrightarrow \tm{id}$ $B \longrightarrow B \andop B$ $B \longrightarrow B \orop B$

The parsing table for this grammar illustrates it's problems for predictive parsing quite directly.  If the leftmost non-terminal is say, $B$ and the next unmatched character of the input is $($, then we don't know whether to use:
  * $B \longrightarrow (B)$, which would make sense if e.g. the rest of the string is $(\tt \andop \ff)$
  * $B \longrightarrow B \andop B$, which would make sense if e.g. the rest of the string is $(\tt) \andop \ff$
  * or $B \longrightarrow B \orop B$, which would make sense if e.g. the rest of the string is $(\tt) \orop \ff$

On the other hand, I can tell you that the nullable, first and follow properties of the LL(1) version of the grammar above are as follows (I will use a table to display these maps concisely, but don't confuse this presentation with the parsing table):

| Nonterminal | Nullable | First | Follow |
| D | | true, false, id, ( | \$, ) | 
| D' | $\checkmark{}$ | $\orop$ | \$, ) |
| C | | true, false, id, ( | $\orop$, \$, ) |
| C' | $\checkmark{}$ | $\andop$ | $\orop$, \$, ) |
| A | | true, false, id, ( | $\andop$, $\orop$, \$, ) | 


Then we can calculate the associated parsing table as:

| | ( | ) | $\andop$ | $\orop$ | $\tt$ | $\ff$ | $\tm{id}$ | \\$ |
| $D$ | $D \longrightarrow CD'\$$ | | | | $D \longrightarrow CD'\$$ | $D \longrightarrow CD'\$$ | $D \longrightarrow CD'\$$ | |
| $D'$ | | $D' \longrightarrow \epsilon$ | | $D' \longrightarrow \mathord{\orop}\,C\,D'$ | | | | $D' \longrightarrow \epsilon$ |
| $C$ | $C \longrightarrow A\,C'$ | | | | $C \longrightarrow A\,C'$ | $C \longrightarrow A\,C'$ | $C \longrightarrow A\,C'$ | |
| $C'$ | | $C' \longrightarrow \epsilon$ | $C' \longrightarrow \mathord{\andop}\,A\,C'$ | $C' \longrightarrow \epsilon$ | | | | $C' \longrightarrow \epsilon$ |
| $A$ | $A \longrightarrow (D)$ | | | | $A \longrightarrow \tt$ | $A \longrightarrow \ff$ | $A \longrightarrow \tm{id}$ | |

Since the table has at most one entry per cell, we can conclude that this grammar is LL(1), and it will allow us to construct a derivation for any given input string in the language of the grammar without any insight - we just consult the table to see which rule to choose at each step.  

I recommend you redo the derivation for $\tt \orop \ff \andop \tt$ from above simply following the strategy described by the parsing table, rather than relying on any thought of your own.

## Calculating Nullable, First and Follow

In each case, the idea will be to iteratively compute approximations $\nullable[X]$, $\first[X]$ and $\follow[X]$ to the three properties, each iteration refining the approximation until eventually they exactly coincide with $\nullable(X)$, $\first(X)$ and $\follow(X)$ respectively.  

### Calculating Nullable

<!-- Start by initialising the approximation $\nullable[X]$ to false for each non-terminal $X$. -->

<!-- Next, it is useful to define a helper function $\nullable_s(\alpha)$ which is true if a given sentential form is nullable according to the current approximation: -->

To calculate $\nullable(X)$ for each non-terminal $X$, first set $\nullable[X]$ to false for each $X$, then repeatedly perform the following iteration:

* For each production $X \longrightarrow \alpha$: 
    * $\nullable[X] := \nullable[X] \vee \nullable_s(\alpha)$

until a complete iteration produces no change to $\nullable[X]$ for any $X$ - we say that we have reached a fixed point.  At this point, $\nullable[X]$ will be true iff $X$ can derive the empty string.

#### Example
To calculate $\nullable(B)$ for the grammar above we start by setting $\nullable[B]$ to false, then we repeatedly iterate through all the productions.  In each case, the right-hand side of the production is not $\epsilon$ and does not consist entirely of nullable non-terminals (this must be so because every right-hand side has a terminal symbol in it).  Therefore, $\nullable[B]$ is never changed, and we have reached a fixed point after just one complete iteration.  This is correct, since indeed $B$ is not nullable.

### Calculating First

<!-- Start by initialising $\first[X]$ to the empty set for each non-terminal $X$. -->

<!-- Next, it is useful to define a helper function $\first_s(\alpha)$ which gives the set of terminals that can start a sentential form derivable from $\alpha$, according to the current approximation: -->

To calculate $\first(X)$ for each non-terminal $X$, first set $\first(X)$ to the empty set $\emptyset$ for each $X$.  Then repeatedly perform the following iteration:

* For each production $X \longrightarrow \alpha$: 
    * $\first[X] := \first[X] \cup \first_s(\alpha)$

until a complete iteration produces no change to $\first[X]$ for any $X$, i.e. we have reached a fixed point.  Then $\first[X]$ will be exactly $\first(X)$.

#### Example
For example, we can calculate $\first(B)$ as follows.  First set $\first[B]$ to $\emptyset$.  Then we analyse the first production $B \longrightarrow B \andop B$.  However, $\first_s(B)$ is empty, so nothing is added to $\first[B]$, and we proceed to the next production $B \longrightarrow B \orop B$.  Again $\first_s(B)$ is empty, so we proceed onto $B \longrightarrow \tt$.  Now $\first_s(\tt) = \\{\tt\\}$, so we update $\first[B]$ to include $\tt$.  A similar analysis of the next production forces us to update $\first[B]$ to also include $\ff$.  Finally an analysis of the last production, $B \longrightarrow (B)$, gives $\first_s((B)) = \\{(\\}$ and so, at the end of one complete iteration, we have $\first[B] = \\{\tt, \ff, (\\}$.  

Since the approximation was changed in the last iteration, we have to repeat the process.  Reanalysing the first production $B \longrightarrow B \andop B$ now gives $\first_s(B \andop B) = \\{\tt,\ff,(\\}$, but this does not add anything new to $\first[B]$, which remains $\\{\tt,\ff,(\\}$.  Analysis of the second production is similar.  Reanalysing the production $B \longrightarrow \tt$ gives $\first_s(\tt) = \\{\tt\\}$, and again nothing new is added to $\first[B]$.  The remaining three productions are similar, and do not add anything new.  Therefore, at the end of this second complete iteration, $\first[B]$ did not change and we have reached a fixed point.

### Calculating Follow

Start by initialising $\follow[X]$ to the empty set for each non-terminal $X$.  Then repeatedly perform the following nested iteration:

* For each non-terminal $X$:
    * For each occurrence of $X$ on the right-hand side of a production $Y \longrightarrow \alpha X \beta$:
        * $\follow[X] := \follow[X] \cup \first_s(\beta)$
        * if $\nullable_s(\beta)$ then $\follow[X] := \follow[X] \cup \follow[Y]$

until a complete iteration of the outer loop produces no change to $\follow[X]$ for any non-terminal $X$, i.e. we have reached a fixed point.  Then $\follow[X]$ will be exactly $\follow(X)$.

#### Example
To calculate $\follow(B)$ we proceed as follows.  First set $\follow[X]$ to $\emptyset$.  Then we identify all the occurrences of $B$ on the right-hand sides of productions:

1. $B \longrightarrow \underline{B} \andop B$
1. $B \longrightarrow B \andop \underline{B}$
1. $B \longrightarrow \underline{B} \orop B$
1. $B \longrightarrow B \orop \underline{B}$
1. $B \longrightarrow (\underline{B})$

Then we analyse each in turn.  For 1, we have $\first_s(\andop B) = \\{\andop\\}$ and so we add $\andop$ into $\follow[B]$.  For 2, the sentential form after the occurrence of $B$ is just $\epsilon$, and we have $\first_s(\epsilon) = \emptyset$, so nothing is added into $\first[B]$. However, this suffix $\epsilon$ is nullable, so the last clause of the definition applies and we must add all of $\follow[B]$ to $\follow[B]$, but this, of course, makes no change.  The analysis of 3 and 4 is similar, and we end up with $\follow[B] = \{\andop, \orop\}$.  Finally, we analyse 5, where $\first_s(\mathord{)}) = \\{)\\}$, so we are forced to add $)$ to $\follow[B]$.  

Our approximation $\follow[B]$ was changed as a result of analysing the occurrences 1--5, so we are not yet at a fixed point and must reanalyse. Note that any computations of $\first_s$ and $\nullable_s$ will not be different in the reanalysis, so we need only pay attention to the very last clause in the iteration, when $\nullable_s(\beta)$.  Consequently, the analysis of 1, 3 and 5 does not change.  When we reanalyse 2 and 4, where the suffix $\beta$ is $\epsilon$, we are forced to add in all of $\follow[B]$ into $\follow[B]$, but this does not change the contents.  Hence, at the end of the reanalysis, we have achieved a fixed point.