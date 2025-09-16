---
layout: math
title: 3. Writing Grammars
mathjax: true
nav_order: 3
parent: Syntax
---

# Writing Grammars

There are two parts to this lecture: notation in order to write about grammars precisely, and some patterns that are useful when writing new grammars (i.e. designing grammars).

## Mathematical Notation

It will be useful to agree some notation for discussing context free grammars a bit more precisely.

<div class="defn" markdown="1">
A __Context Free Grammar (CFG)__ consists of four components:
* An alphabet of __terminal__ symbols. We shall use $a$, $b$, $c$ and other lowercase letters as metavariables for terminals.
* A finite, non-empty set of __nonterminal__ symbols, disjoint from the terminals.  We use uppercase letters $S$, $T$, $X$ and so on as metavariables for nonterminals.
* A finite set of __production rules__.  Each production rule has shape $X \Coloneqq \alpha$, where $\alpha$ is a sequence of terminals and non-terminals that we refer to as the _right-hand side_ of the rule.
* A designated non-terminal called the _start symbol_, which we will usually write as $S$.
</div>

If you think back to the previous example, you will notice that a key part of the grammar is manipulating sequences of terminals and nonterminals, like $$\ff \andop (L \orop L)$$.  One could think of these as strings over the alphabet that consists of the terminals the nonterminals combined, but I would rather reserve the word _string_ only for sequences of terminals.  (Possibly) mixed sequences of terminals and nonterminals are known as _sentenial forms_.

{: .defn }
A __sentential form__ is just a finite sequence of letters, each of which is either a  terminal or a nonterminal.  We use lowercase greek letters $\alpha$, $\beta$, $\gamma$ and so on as metavariables for sentential forms.

We will continue to reserve metavariables $u$, $v$, $w$ and so on for strings (of terminals only).

### Metavariables
We use metavariables when we want to state something that involves a string, or a sentential form that we don't want to specify concretely.  For example, we might say, "for all strings $u$, $$ \abs{u^2} \geq \abs{u} $$", or "given a sentential form of shape $\alpha 0 \beta$ (i.e. containing at least one letter $0$),...".  In other words we use them as ordinary mathematical variables in our reasoning about strings, or sentential forms.

The reason they are called _meta_ variables is because, in mathematical logic and programming language theory, we are often talking about strings (or sentential forms) that come from languages that have their own internal notion of variable.  For example, if we want to discuss strings that are C programs, then the language of C programs has its own notion of variable, namely the variables in the program.  So, to not confuse these different kinds of variables: the variables that are in the language we are studying, e.g. C program variables, and the variables we use in the language we write when we are reasoning about them (this is sometimes called the *metalanguage*); it is standard to refer to the latter as *metavariables* (variables of the metalanguage).  

<!-- However, to make the situation even more confusing, but much more convenient, we will by some abuse use the metavariables for terminals and nonterminals also as the names of particular terminals and non-terminals when their names don't matter.  For example, we might say "Consider a grammar for generating strings over the alphabet $$\{a,b\}$$,...".  This can either be read as an alphabet over two unspecified terminal symbols represented by the (meta-)variables $a$ and $b$, or it can be read as an alphabet over the two particular lowercase letters $a$ and $b$ from the Latin alphabet - it almost always doesn't matter which way you want to read it. -->

<!-- <div class="defn" markdown="1">
The __production rules__ $\mathcal{R}$ of a CFG consisting of components $$(\Sigma,\mathcal{N},\mathcal{R},S)$$ each have shape:

$$
  X \Coloneqq \alpha
$$

where $X$ is a nonterminal from $\mathcal{N}$ and $\alpha$ is a sentenial form over $\Sigma$ and $\mathcal{N}$.
</div> -->

<!-- A CFG that is important to the first part of this unit is the grammar of the language _Brischeme_.  

<div class="defn" markdown="1">
The syntax of the _Brischeme_ programming language is given by the CFG with:
  
  - Terminals: $0,1,\ldots{},9$, $a,b,\ldots,z,A,B,\ldots,Z$, $$\_$$, $!$, $?$, $<$, $=$, $+$, $-$, $*$, $/$, $($, $)$, $\tm{define}$, $\tm{lambda}$, $\tm{not}$, $\tm{or}$, $\tm{and}$, $$\tm{\#t}$$, $$\tm{\#f}$$.

  - Nonterminals: $\nt{Prog}$, $\nt{Form}$, $\nt{SExpr}$, $\nt{Ident}$, $\nt{Literal}$, $\nt{Num}$, $\nt{Bool}$, $\nt{Digit}$, $\nt{LChar}$, $\nt{IdChar}$.
  
  - The production rules are:
  
    $$
      \begin{array}{rcl}
        \nt{Prog} &::=& \nt{Form}^*\\[4mm]
        \nt{Form} &::=& \nt{SExpr}\\[2mm]
                  &\mid& (\ \tm{define}\ \nt{Ident}\ \nt{SExpr}\ )\\[4mm]
        \nt{SExpr} &::=& \nt{Literal}\\[2mm] 
                    &\mid& \nt{Ident}\\[2mm]
                    &\mid& (\ \nt{SExpr}\ \nt{SExpr}^*\ )\\[2mm]
                    &\mid& (\ \tm{primop}\ \nt{SExpr}^*\ )\\[2mm]
                    &\mid& (\ \tm{lambda}\ \tm{(}\ \nt{Ident}^*\ \tm{)}\ \nt{SExpr}\ )\\[6mm]
        \nt{Ident} &::=& \nt{LChar}\ \nt{IdChar}^*\\[2mm]
        \nt{LChar} &::=& \tm{a} \mid \tm{b} \mid \cdots{} \mid \tm{z}\\[2mm]
        \nt{IdChar} &::=& a \mid b \mid \cdots{} \mid z \mid A \mid B \mid \cdots{} \mid \tm{Z} \mid \tm{!} \mid \tm{?} \mid \_\\[6mm]
        \nt{Literal} &::=& \nt{Bool} \mid \nt{Num}\\[2mm]
        \nt{Bool} &::=& \tm{\#t} \mid \tm{\#f}\\[2mm]
        \nt{Num} &::=& \nt{Digit}^+\\[2mm]
        \nt{Digit} &::=& 0 \mid 1 \mid \cdots{} \mid 9
      \end{array}
    $$

</div> -->

<!-- Here's another example of a context free grammar which is particularly important to this unit, it is the syntax of a little idealised imperative programming language called *While*.

<div class="defn" markdown="1">
The syntax of the __While Programming Language__ is given by the CFG with:
- Terminal symbols: $0,1,\ldots{},9$, $'$, $a,b,\ldots,z,A,B,\ldots,Z$, $\andop$, $\orop$, $!$, $+$, $*$, $-$, $\leq$, $=$, $\leftarrow$, $;$.  
- Nonterminal symbols: $S$, $B$, $A$, $D$, $E$, $L$, $U$, $M$, $V$, $N$
- Production rules:

    $$
      \begin{array}{rcl}
        S &\Coloneqq& \mathsf{skip} \mid V \leftarrow A \mid S ; S \mid \mathsf{if}\ B\ \mathsf{then}\ S\ \mathsf{else}\ S \mid \mathsf{while}\ B\ \mathsf{do}\ S \mid \{\ S\ \}\\
        B &\Coloneqq& \tt \mid \ff \mid A \leq A \mid A = A \mid \mathop{!}B \mid B \andop B \mid B \orop B \mid (B)\\
        A &\Coloneqq& V \mid N \mid A + A \mid A - A \mid A * A \mid (A)\\
        D &\Coloneqq& 0 \mid 1 \mid \cdots{} \mid 9\\
        E &\Coloneqq& D\ E \mid \epsilon\\
        L &\Coloneqq& a \mid b \mid \cdots{} \mid z \\
        U &\Coloneqq& A \mid B \mid \cdots{} \mid Z \mid ' \\
        M &\Coloneqq& L\ M \mid U\ M \mid \epsilon\\
        V &\Coloneqq& L\ M\\
        N &\Coloneqq& D\ E
      \end{array}
    $$
- Start symbol: $S$
</div>

Informally the "meaning" of the different nonterminals (sometimes called the syntactic classes of the language) are as follows:
* $S$: statements, i.e. code that, when executed, does not return a value but instead has some effect (such as updating the value of a variable)
* $B$: Boolean expressions, i.e. code that, when executed, will return a Boolean (true/false) value
* $A$: arithmetic expressions, i.e. code that, when executed, will return a numerical value
* $D$: single digits
* $E$: possibly empty sequences of digits
* $L$: single lowercase letters, or the prime (apostrophe) symbol '
* $U$: single uppercase letters
* $M$: possibly empty sequences of letters and underscores
* $V$: variable names (identifiers)
* $N$: numerals

There are quite a number of nonterminals, but there is a sense (which we will make more precise later) that the first three are really doing all the heavy lifting in the description of this language.  The latter seven non-terminals are just describing two things: the shape of numbers and the shape of variable names (also known as identifiers). -->

Note: there is nothing special about the particular letters used for the nonterminals, we could have used alternative names and we would still consider it essentially the same description of the While programming language.  For example, if we replaced everywhere in the rules the letter N by the letter P.

<!-- Some remarks on the notation:
  * $\mathsf{skip}$ is the "do nothing" statement, it has no effect when executed
  * $V \leftarrow A$ is an assignment statement: assign the value described by the arithmetic expression derived from $A$ to the variable whose name derives from $V$.
  * { $S$ } are just brackets for statements (as parentheses are to expressions)
  * We use $;$ to combine two statements by executing them in sequence, rather than as a terminator for every (atomic) statement, so e.g. $x \leftarrow 2; y \leftarrow 3$ is a perfectly good statement derivable from $S$. -->

### Derivation
A context free grammar is a device for defining a language (i.e. set) of strings.  A string is in the language defined by a grammar just if we can use the rules of the grammar to _derive_ it.

Derivation is a step-by-step process for transforming one sentential form into another one.  A single step of a derivation, consists in transforming one sentential form $\alpha$ into another sentential form $\beta$ by replacing, in $\alpha$, exactly one occurrence of a nonterminal symbol by its right-hand side according to one of the production rules.

<div class="defn" markdown="1">
The sentential form $\alpha$ can make a **derivation step** to $\beta$, written $\alpha \to \beta$, just if:
  - $\alpha$ has shape $\gamma_1\,X\,\gamma_2$ and $\beta$ has shape $\gamma_1\,\delta\,\gamma_2$
  - and there is a production rule $X \Coloneqq \delta$ in the grammar
</div>

The best way to understand a definition of some new assertion, in this case $\alpha \to \beta$, is to think of it as telling you how to check the assertion.  That is, if I gave you some random sentential forms $\alpha$ and $\beta$, how do we know if we can say $\alpha \to \beta$ ?  How do you go about checking that it is a correct step?

First you would try, by comparing $\alpha$ and $\beta$, to check that the only difference between them is that some nonterminal in $\alpha$ is replaced in $\beta$ by some other bit of sentential form.  A way of saying "the only difference between them" more precisely is to say that both $\alpha$ and $\beta$ start with the same sentential form, lets call it $\gamma_1$, and end with the same sentential form, lets call that $\gamma_2$, but that the bit in between $\gamma_1$ and $\gamma_2$ can be different.  However, it can't be an any old difference.  The thing that changed in $\alpha$ must be a single solitary nonterminal symbol, say $X$, which was changed according to a production rule, say $X \Coloneqq \delta$, whose left-hand side is $X$.  Then it had better be that the bit that is different in $\beta$ is exactly the right-hand side of this production rule, i.e. $\delta$.

<img src="../assets/syntax/step.png" style="max-width:600px;"/>

Note that, since $\epsilon$ is a sentential form - the empty sentential form - the definition allows for any of $\gamma_1$, $\gamma_2$ and $\delta$ to be omitted.  In the following example, we have $\gamma_1 = \gamma_2 = \epsilon$:

$$
  F \to L \andop F
$$

We can chain derivation steps together to form a derivation sequence.

<div class="defn" markdown="1">
A **derivation sequence** is a non-empty sequence of sentential forms $\alpha_1$, $\alpha_2$, ... $\alpha_{k-1}$, $\alpha_k$ in which consecutive elements of the sequence are derivation steps:

$$
  \alpha_1 \to \alpha_2 \to \cdots{} \to \alpha_{k-1} \to \alpha_k
$$
</div>

Often, we don't care about the intermediate steps in a derivation sequence, but only that the end is reachable from the start, for which we have the notation $\alpha_1 \to^* \alpha_k$.

<div class="defn" markdown="1">
A sentential form $\beta$ is **derivable** from $\alpha$, written $\alpha \to^* \beta$ just if there is a derivation sequence starting with $\alpha$ and ending with $\beta$.
</div>

<!-- Some common ways in which this notation will be used:
- $X \to^* u$ means that the grammar, starting from the start symbol $S$ (which is a sentential form of length 1), derives a string (of terminals) $u$.  Here the sentential form at the end of some number of derivation steps is asserted to be a string (i.e. a sentential form containing no non-terminals) because we used the metavariable $u$.
- $X \to^* \alpha$ means that sentential form $\alpha$ is derivable from nonterminal $X$.  $\alpha$ may or may not be a string, so without further information it could be that $\alpha$ represents a kind of intermediate point in the derivation which is not yet at an end. -->

Ultimately, all this machinery is there in order for us to say precisely which strings constitute the language defined by the grammar.

<div class="defn" markdown="1">
The **language of a grammar $G$**, written $L(G)$, is the set of all strings $w$ derivable from the start symbol, i.e. $$\{\, w \mid S \to^* w \,\}$$.
</div>

## Grammars can Express Sequences

Suppose we want to design a grammar to define the language of all possible sequences of digits (0-9), including the empty sequence.  A useful approach is to think of a sequence of digits as a Haskell or OCaml list, which is either empty, or consists of at least one element - the head, a digit, - and a possibly empty sequence of further elements - the tail, itself a sequence of digits.  This approach gives rise to the following grammar:

$$
  \begin{array}{rcl}
    \nt{Digit}  &\Coloneqq& 0 \mid 1 \mid 2 \mid 3 \mid 4 \mid 5 \mid 6 \mid 7 \mid 8 \mid 9\\[2mm]
    \nt{Digits} &\Coloneqq& \nt{Digit}\ \nt{Digits} \mid \epsilon
  \end{array}
$$

For example:

$$
  \begin{array}{rll}
    \nt{Digits} &\to& \nt{Digit}\ \nt{Digits}\\
                &\to& \nt{Digit}\ \nt{Digit}\ \nt{Digits}\\
                &\to& \nt{Digit}\ \nt{Digit}\ \nt{Digit}\ \nt{Digits}\\
                &\to& \nt{Digit}\ \nt{Digit}\ \nt{Digit}\\
                &\to^*& 1\ 2\ 3
  \end{array}
$$

The same pattern works no matter how complex the elements of the sequence can be (as long as they can themselves be described by production rules).  Recall the production rules defining Boolean expressions starting from $\nt{B}$, by adding the following extra rule, we can describe all sequences of them with a nonterminal $\nt{Bs}$:

$$
  \begin{array}{rcl}
    \nt{Bs} &\Coloneqq& \nt{B}\ \nt{Bs} \mid \epsilon
  \end{array}
$$

Suppose we want to describe non-negative integers, that is, _non empty_ sequences of digits.  We can build this out of our description of (possibly empty) sequences of digits by saying that a number is a digit followed by a (possibly empty) sequence of digits:

$$
  \begin{array}{rcl}
    \nt{Digit}  &\Coloneqq& 0 \mid 1 \mid 2 \mid 3 \mid 4 \mid 5 \mid 6 \mid 7 \mid 8 \mid 9\\[2mm]
    \nt{Digits} &\Coloneqq& \nt{Digit}\ \nt{Digits} \mid \epsilon\\[2mm]
    \nt{Number} &\Coloneqq& \nt{Digit}\ \nt{Digits}
  \end{array}
$$

Describing sequences is so useful in the definition of programming languages that often we use some special notation to save us having to use this pattern over and over again.  The notation:
$$
  [\alpha]^*
$$
when used on the right-hand side of a production rule means "any finite sequence of $\alpha$".  We will omit the square brackets if $\alpha$ is just a single symbol.  So, non-negative integers can be defined by:

$$
  \begin{array}{rcl}
    \nt{Digit}  &\Coloneqq& 0 \mid 1 \mid 2 \mid 3 \mid 4 \mid 5 \mid 6 \mid 7 \mid 8 \mid 9\\[2mm]
    \nt{Number} &\Coloneqq& \nt{Digit}\ \nt{Digit}^*
  \end{array}
$$

A non-empty comma-separated list of digits can be described by:

$$
  \begin{array}{rcl}
    \nt{Digit}  &\Coloneqq& 0 \mid 1 \mid 2 \mid 3 \mid 4 \mid 5 \mid 6 \mid 7 \mid 8 \mid 9\\[2mm]
    \nt{DigitList} &\Coloneqq& \nt{Digit}\ [,\ \nt{Digit}]^*
  \end{array}
$$

This new notation doesn't really add anything to grammars, because we can always eliminate any occurrence of $$[\alpha]^*$$ by introducing two new non-terminals, say $X$ and $\nt{Xs}$, and the rules $X \Coloneqq \alpha$ and $\nt{Xs} \Coloneqq X\ \nt{Xs} \mid \epsilon$, and finally replacing $$[\alpha]^*$$ by simply $Xs$.

$$
  \begin{array}{rcl}
    \nt{Digit}  &\Coloneqq& 0 \mid 1 \mid 2 \mid 3 \mid 4 \mid 5 \mid 6 \mid 7 \mid 8 \mid 9\\[2mm]
    \nt{DigitList} &\Coloneqq& \nt{Digit}\ \nt{Xs}\\[2mm]
    \nt{X} &\Coloneqq& ,\ \nt{Digit}\\[2mm]
    \nt{Xs} &\Coloneqq& \nt{X}\ \nt{Xs} \mid \epsilon
  \end{array}
$$

So, if you want to construct a derivation involving the notation $[\alpha]^*$, just remember that it is a shorthand for some non-terminal $\nt{Xs}$ defined as above, and so the only thing we can really do is to say that it derives any finite sequence of $\alpha$ over some number of steps.  For example, we can derive exactly three $\alpha$:

$$
  [\alpha]^* \to^* \alpha\ \alpha\ \alpha
$$

## Other Useful Patterns

We discuss the following examples over alphabet $$\{0,1\}$$ (most are from Sipser Ex. 2.4), which shows some of the common patterns when formulating a CFG.

***

$$
  \{ w \mid \text{$w$ is any word over $\{0,1\}$}\}
$$

A grammar for this language is:

$$
  S \Coloneqq 0\ S \mid 1\ S \mid \epsilon
$$

Each derivation step consists of either extending the current sentential form by any choice of digit 0 or 1, or ending the string with epsilon.  Thus, any possible word can be derived.

Or, using our new notation:

$$
  \begin{array}{rcl}
    S &\Coloneqq& B^*\\
    B &\Coloneqq& 0 \mid 1
  \end{array}
$$

***

$$
  \{ w \mid \text{$w$ contains at least three letter 1} \}
$$

A grammar for this language is:

$$
  \begin{array}{rcl}
    S &\Coloneqq& A\ 1\ A\ 1\ A\ 1\ A\\
    A &\Coloneqq& 0\ A \mid 1\ A \mid \epsilon
  \end{array}
$$

A word derivable from S must include 3 1s, because there is only one rule possible which introduces them.  Around the 1s you may put any words you like because we only require "at least" 3, so the rules for $A$ are exactly the same as in the previous example.  If you wanted to express the language of words with _exactly_ 3 1s, then you can remove the $A \Coloneqq 1\ A$ production, so that the words allowed between the 1s cannot contain 1.

***

$$
  \{ w \mid \text{$w$ starts and ends with the same symbol} \}
$$

A grammar for this language is:

$$
  \begin{array}{rcl}
    S &\Coloneqq& 0 A 0 \mid 1 A 1 \mid 0 \mid 1 \mid \epsilon
    A &\Coloneqq& 0\ A \mid 1\ A \mid \epsilon
  \end{array}
$$

If you look at the shape of words derivable from $S$, they are forced to start and end with the same letter.  For words of length 2 or more, between the first and last letter we allow any substring.

***

$$
  \{ w \mid \text{the length of $w$ is odd and its middle letter is $0$}\}
$$

A grammar for this language is:

$$
  \begin{array}{rcl}
    S &\Coloneqq& X\ S\ X \mid 0\\
    X &\Coloneqq& 0 \mid 1
  \end{array}
$$

When we use the first production rule for $S$ we always add two terminal symbols onto our sentential form (we replace $S$ by $XSX$ and each $X$ derives exactly one terminal symbol).  Thus, using this first rule preserves the parity (even or odd) of the length of our sentential form.  In the base case we allow for $S$ being replaced by $0$ which is a length 1 string - i.e. odd.  So, $S$ must derive odd length strings.  In each sentential form derivable from $S$, except for the last, there will be exactly one occurrence of $S$ and it will be the exact middle of the string.  Hence, when we finally replace this $S$ by 0 we guarantee that $0$ must be the middle letter of the derived string.

***

$$
  \{ w \mid \text{$w$ is a palindrome (i.e. the same word when reversed)}\}
$$

A grammar for this string is:

$$
  S \Coloneqq 0\ S\ 0 \mid 1\ S\ 1 \mid 0 \mid 1 \mid \epsilon
$$

Here again, the middle of each sentential form (of length > 1) will be $S$ and using the first two rules ensures that the letter $k$ positions to the left of this mid-point will be the same as the letter $k$ positions to the right (for any $k$ appropriate to the length of the sentential form).  Hence, a derivable string is certain to be a palindrome.

***

$$
  \{uv \mid |u| = |v| \text{ and } u \neq v^R\}
$$

Here $v^R$ is the reverse of $v$.  A grammar for this language is:

<!-- $$
  \begin{array}{rcl}  
    S &\Coloneqq& X\ S\ X \mid T\\
    T &\Coloneqq& 0\ U\ 1 \mid 1\ U\ 0\\
    U &\Coloneqq& X\ U\ X \mid \epsilon\\
    X &\Coloneqq& 0 \mid 1
  \end{array}
$$ -->

<!-- A word in this language can be cut down the middle, and the substring on the left is different from the reverse of the substring on the right.  In other words, there is some letter, say $k$ places to the left of the midpoint which is different from the letter $k$ places to the right of the midpoint.  

The grammar encodes this in the following way.  Starting from $S$, using the first rule, we derive an odd length sentential form with $S$ at the midpoint and we allow free choice over the letters to the left and right of the midpoint.  They may match or not.  At some point the derivation must choose to exit this process by replacing $S$ by $T$.  Starting from $T$ we are forced to add a mismatching pair of letters to the sentential form.  After applying either the first or second rule for $T$, we will have an odd length sentential form with $U$ at its midpoint and we are sure that the letters one place to the left and right of the midpoint are different (all the other letters may or may not be the same).  Finally, we return to the original process of adding two letters at a time which are free to match or not match and finally we terminate the string by replacing $U$ with the empty string to obtain an even length string which is guaranteed to have a mismatching pair of letters $k$ places to the left and right of the midpoint for some value of $k$. -->