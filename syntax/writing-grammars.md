---
layout: math
title: Writing Grammars
mathjax: true
nav_order: 3
parent: Syntax
---

## Mathematical Notation

Some notation for discussing context free grammars a bit more precisely.

<div class="defn" markdown="1">
A __Context Free Grammar (CFG)__ consists of four components:
* An alphabet of __terminal__ symbols, which we shall usually write as $\Sigma$ (capital letter sigma)
* A finite, non-empty set of __non-terminal__ symbols, disjoint from the terminals, which we shall usually write as $\mathcal{N}$ (caligraphic letter N)
* A finite set of __production rules__, which we shall usually write as $\mathcal{R}$ (caligraphic letter R)
* A designated non-terminal from $\mathcal{N}$, called the _start symbol_, which we will usually write as $S$.
</div>

We will adopt the following conventions.  We will use lowercase letters from the start of the alphabet, $a$, $b$, $c$ and so on, as metavariables for terminal symbols.  We will use uppercase letters as metavariables for non-terminal symbols.  

If you think back to the previous example, you will notice that a key part of the grammar is manipulating sequences of terminals and non-terminals, like $$\ff \andop (L \orop L)$$.  One could think of these as strings over the alphabet $\Sigma \cup \mathcal{N}$, but I would rather reserve the word "string" for the objects (over $\Sigma$ only) that the grammar derives.  So instead, we shall refer to these kind of sequences as _sentenial forms_.

{: .defn }
A __sentential form__ is just a finite sequence of terminals (from $\Sigma$) and nonterminals (from $\mathcal{N}$).

According to this view, we will reserve metavariables $u$, $v$, $w$ and so on for strings (as we have done so far), and introduce metavariables $\alpha$, $\beta$, $\gamma$ and so on to stand for arbitrary sentential forms.

We use metavariables when we want to state something that involves a string, or a sentential form that we don't want to specify concretely.  For example, we might say, "for all strings $u$, $\|u^2
| \geq \|u\|$", or "given a sentential form of shape $\alpha 0 \beta$ (i.e. containing at least one letter $0$),...".  In other words we use them as ordinary mathematical variables in our reasoning about strings, or sentential forms.  However, in mathematical logic and programming language theory, we are often talking about strings (or sentential forms) that come from languages that have their own notion of variable.  For example, if we want to discuss strings that are C programs, then the language of C programs has its own notion of variable, namely the variables in the program.  So, to not confuse these different kinds of variables: the variables that are in the language we are studying, e.g. C program variables, and the variables we use in the language we write when we are reasoning about them (this is sometimes called the *metalanguage*); it is standard to refer to the latter as *metavariables* (variables of the metalanguage).  

However, to make the situation even more confusing, but much more convenient, we will by some abuse use the metavariables for terminals and nonterminals also as the names of particular terminals and non-terminals when their names don't matter.  For example, we might say "Consider a grammar for generating strings over the alphabet $$\{a,b\}$$,...".  This can either be read as an alphabet over two unspecified terminal symbols represented by the (meta-)variables $a$ and $b$, or it can be read as an alphabet over the two particular lowercase letters $a$ and $b$ from the Latin alphabet - it almost always doesn't matter which way you want to read it.

<div class="defn" markdown="1">
The __production rules__ $\mathcal{R}$ of a CFG consisting of components $$(\Sigma,\mathcal{N},\mathcal{R},S)$$ each have shape:

$$
  X \longrightarrow \alpha
$$

where $X$ is a nonterminal from $\mathcal{N}$ and $\alpha$ is a sentenial form over $\Sigma$ and $\mathcal{N}$.
</div>

Here's another example of a context free grammar which is particularly important to this unit, it is the syntax of a little idealised imperative programming language called *While*.

<div class="defn" markdown="1">
The syntax of the __While Programming Language__ is given by the CFG with:
- Terminal symbols: $0,1,\ldots{},9$, $'$, $a,b,\ldots,z,A,B,\ldots,Z$, $\andop$, $\orop$, $!$, $+$, $*$, $-$, $\leq$, $=$, $\leftarrow$, $;$.  
- Nonterminal symbols: $S$, $B$, $A$, $D$, $E$, $L$, $U$, $M$, $V$, $N$
- Production rules:

    $$
      \begin{array}{rcl}
        S &\longrightarrow& \mathsf{skip} \mid V \leftarrow A \mid S ; S \mid \mathsf{if}\ B\ \mathsf{then}\ S\ \mathsf{else}\ S \mid \mathsf{while}\ B\ \mathsf{do}\ S \mid \{\ S\ \}\\
        B &\longrightarrow& \tt \mid \ff \mid A \leq A \mid A = A \mid \mathop{!}B \mid B \andop B \mid B \orop B \mid (B)\\
        A &\longrightarrow& V \mid N \mid A + A \mid A - A \mid A * A \mid (A)\\
        D &\longrightarrow& 0 \mid 1 \mid \cdots{} \mid 9\\
        E &\longrightarrow& D\ E \mid \epsilon\\
        L &\longrightarrow& a \mid b \mid \cdots{} \mid z \\
        U &\longrightarrow& A \mid B \mid \cdots{} \mid Z \mid ' \\
        M &\longrightarrow& L\ M \mid U\ M \mid \epsilon\\
        V &\longrightarrow& L\ M\\
        N &\longrightarrow& D\ E
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

There are quite a number of nonterminals, but there is a sense (which we will make more precise later) that the first three are really doing all the heavy lifting in the description of this language.  The latter seven non-terminals are just describing two things: the shape of numbers and the shape of variable names (also known as identifiers).

Note: there is nothing special about the particular letters used for the nonterminals, we could have used alternative names and we would still consider it essentially the same description of the While programming language.  For example, if we replaced everywhere in the rules the letter N by the letter P.

Some remarks on the notation:
  * $\mathsf{skip}$ is the "do nothing" statement, it has no effect when executed
  * $V \leftarrow A$ is an assignment statement: assign the value described by the arithmetic expression derived from $A$ to the variable whose name derives from $V$.
  * { $S$ } are just brackets for statements (as parentheses are to expressions)
  * We use $;$ to combine two statements by executing them in sequence, rather than as a terminator for every (atomic) statement, so e.g. $x \leftarrow 2; y \leftarrow 3$ is a perfectly good statement derivable from $S$.

### Derivation

The process of deriving a string from the grammar, step by step, is made precise using the derivation relation $\alpha \to \beta$ (this is a shorter arrow than the one that we use to separate to the two sides of the rules; ideally we would use a more visually distinctive arrow, like $\Rightarrow$, but this takes twice as long to draw on the board in lectures).

<div class="defn" markdown="1">
The __one-step derivation relation__ is a binary relation on sentential forms with two sentential forms $\alpha$ and $\beta$ related, written $$\alpha \to \beta$$, just if $\alpha$ is of shape $\alpha_1 X \alpha_2$ and there is a production rule $X \longrightarrow \gamma$ and $\beta$ is exactly $\alpha_1 \gamma \alpha_2$.  

We write $\alpha \to^* \beta$, and say __$\beta$ is derivable from $\alpha$__ (or __$\alpha$ derives $\beta$__) just if $\beta$ can be derived from $\alpha$ in any (finite) number of steps, including zero steps.

Finally, we say that a word $w$ is in the \emph{language of a grammar}, and write $w \in L(G)$ just if $S \to^* w$.
</div>

In other words, we write $\alpha \to \beta$ just if $\beta$ arises by replacing exactly one occurrence of a nonterminal $X$ in $\alpha$ by its right-hand side according to one of the rules of the grammar.

Some common ways in which this notation will be used:
- $S \to^* u$ means that the grammar, starting from the start symbol $S$ (which is a sentential form of length 1), derives a string (of terminals) $u$.  Here the sentential form at the end of some number of derivation steps is asserted to be a string (i.e. a sentential form containing no non-terminals).
- $S \to^* \alpha$ means that sentential form $\alpha$ is derivable in the grammar.  $\alpha$ may or may not be a string, so without further information it could be that $\alpha$ represents a kind of intermediate point in the derivation which is not yet at an end.

For example, in the grammar above we have the following:

- $S \to \mathsf{while}\ B\ \mathsf{do}\ S$

    This is justified by the definition if we take $\alpha_1$ to be $\epsilon$, $X$ to be $S$, $\alpha_2$ to be $\epsilon$ and $\gamma$ to be $\mathsf{while}\ B\ \mathsf{do}\ S$.

- $\mathsf{while}\ B\ \mathsf{do}\ S \to \mathsf{while}\ \tt\ \mathsf{do}\ S$ 

    This is justified by the definition if we take $\alpha_1$ to be $\mathsf{while}$, $X$ to be $B$, $\alpha_2$ to be the sentential form $\mathsf{do}\ S$ and $\gamma$ as $\tt$.

- $\mathsf{while}\ \tt\ \mathsf{do}\ S \to \mathsf{while}\ \tt\ \mathsf{do}\ \mathsf{skip}$

    This is justified by the definition if we take $\alpha_1$ to be $\mathsf{while}\ \tt\ \mathsf{do}$, $X$ to be $S$, $\alpha_2$ to be $\epsilon$ and $\gamma$ to be $\mathsf{skip}$.

If we put these together in order:

$$
  \begin{align*}
    S &\to \mathsf{while}\ B\ \mathsf{do}\ S \\
      &\to \mathsf{while}\ \tt\ \mathsf{do}\ S\\
      &\to \mathsf{while}\ \tt\ \mathsf{do}\ \mathsf{skip} 
  \end{align*}
$$

then this justifies the statement: 

$$S \to^* \mathsf{while}\ \tt\ \mathsf{do}\ \mathsf{skip}$$

and this justifies us to claim that $\mathsf{while}\ \tt\ \mathsf{do}\ \mathsf{skip}$ is in the language of While programs (i.e. *is a While program*).

## Design of Context-Free Grammars

We discuss the following examples over alphabet $$\{0,1\}$$ (most are from Sipser Ex. 2.4), which shows some of the common patterns when formulating a CFG.

***

$$
  \{ w \mid \text{$w$ is any word over $\{0,1\}$}\}
$$

A grammar for this language is:

$$
  S \longrightarrow 0\ S \mid 1\ S \mid \epsilon
$$

Each derivation step consists of either extending the sentential form by any choice of digit 0 or 1, or ending the string with epsilon.  Thus, any possible word can be derived.

***

$$
  \{ w \mid \text{$w$ contains at least three letter 1} \}
$$

A grammar for this language is:

$$
  \begin{array}{rcl}
    S &\longrightarrow& A\ 1\ A\ 1\ A\ 1\ A\\
    A &\longrightarrow& 0\ A \mid 1\ A \mid \epsilon
  \end{array}
$$

A word derivable from S must include 3 1s, because there is only one rule possible which introduces them.  Around the 1s you may put any words you like because we only require "at least" 3, so the rules for $A$ are exactly the same as in the previous example.  If you wanted to express the language of words with _exactly_ 3 1s, then you can remove the $A \longrightarrow 1\ A$ production, so that the words allowed between the 1s cannot contain 1.

***

$$
  \{ w \mid \text{$w$ starts and ends with the same symbol} \}
$$

A grammar for this language is:

$$
  \begin{array}{rcl}
    S &\longrightarrow& 0 A 0 \mid 1 A 1 \mid 0 \mid 1 \mid \epsilon
    A &\longrightarrow& 0\ A \mid 1\ A \mid \epsilon
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
    S &\longrightarrow& X\ S\ X \mid 0\\
    X &\longrightarrow& 0 \mid 1
  \end{array}
$$

When we use the first production rule for $S$ we always add two terminal symbols onto our sentential form (we replace $S$ by $XSX$ and each $X$ derives exactly one terminal symbol).  Thus, using this first rule preserves the parity (even or odd) of the length of our sentential form.  In the base case we allow for $S$ being replaced by $0$ which is a length 1 string - i.e. odd.  So, $S$ must derive odd length strings.  In each sentential form derivable from $S$, except for the last, there will be exactly one occurrence of $S$ and it will be the exact middle of the string.  Hence, when we finally replace this $S$ by 0 we guarantee that $0$ must be the middle letter of the derived string.

***

$$
  \{ w \mid \text{$w$ is a palindrome (i.e. the same word when reversed)}\}
$$

A grammar for this string is:

$$
  S \longrightarrow 0\ S\ 0 \mid 1\ S\ 1 \mid 0 \mid 1 \mid \epsilon
$$

Here again, the middle of each sentential form (of length > 1) will be $S$ and using the first two rules ensures that the letter $k$ positions to the left of this mid-point will be the same as the letter $k$ positions to the right (for any $k$ appropriate to the length of the sentential form).  Hence, a derivable string is certain to be a palindrome.

***

$$
  \{uv \mid |u| = |v| \text{ and } u \neq v^R\}
$$

Here $v^R$ is the reverse of $v$.  A grammar for this language is:

$$
  \begin{array}{rcl}  
    S &\longrightarrow& X\ S\ X \mid T\\
    T &\longrightarrow& 0\ U\ 1 \mid 1\ U\ 0\\
    U &\longrightarrow& X\ U\ X \mid \epsilon
  \end{array}
$$

A word in this language can be cut down the middle, and the substring on the left is different from the reverse of the substring on the right.  In other words, there is some letter, say $k$ places to the left of the midpoint which is different from the letter $k$ places to the right of the midpoint.  

The grammar encodes this in the following way.  Starting from $S$, using the first rule, we derive an odd length sentential form with $S$ at the midpoint and we allow free choice over the letters to the left and right of the midpoint.  They may match or not.  At some point the derivation must choose to exit this process by replacing $S$ by $T$.  Starting from $T$ we are forced to add a mismatching pair of letters to the sentential form.  After applying either the first or second rule for $T$, we will have an odd length sentential form with $U$ at its midpoint and we are sure that the letters one place to the left and right of the midpoint are different (all the other letters may or may not be the same).  Finally, we return to the original process of adding two letters at a time which are free to match or not match and finally we terminate the string by replacing $U$ with the empty string to obtain an even length string which is guaranteed to have a mismatching pair of letters $k$ places to the left and right of the midpoint for some value of $k$.