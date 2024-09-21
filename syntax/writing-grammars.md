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

If you think back to the previous example, you will notice that a key part of the grammar is manipulating sequences of terminals and non-terminals, like $$\ff \andop (L \orop L)$$.  One could think of these as strings over the alphabet $\Sigma \cup \mathcal{N}$, but I would rather reserve the word "string" for the objects (over $\Sigma$ only) that the grammar derives.  So instead, we shall refer to these kind of sequences as __sentenial forms__.

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
- Nonterminal symbols: $S$, $A$, $V$, $B$, $D$, $E$, $L$, $U$, $M$, $N$
- Production rules:

    $$
      \begin{array}{rcl}
        S &\longrightarrow& \mathsf{skip} \mid V \leftarrow A \mid S ; S \mid \mathsf{if}\ B\ \mathsf{then}\ S\ \mathsf{else}\ S \mid \mathsf{while}\ B\ \mathsf{do}\ S \mid \{\ S\ \}\\
        B &\longrightarrow& \tt \mid \ff \mid A \leq A \mid A = A \mid \mathop{!}B \mid B \andop B \mid B \orop B \mid (B)\\
        A &\longrightarrow& V \mid N \mid A + A \mid A - A \mid A * A \mid (A)\\
        D &\longrightarrow& 0 \mid 1 \mid \cdots{} \mid 9\\
        E &\longrightarrow& D\ E \mid \epsilon\\
        L &\longrightarrow& a \mid b \mid \cdots{} \mid z \mid \_ \\
        U &\longrightarrow& A \mid B \mid \cdots{} \mid Z \\
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
* $L$: single lowercase letters, or the underscore
* $U$: single uppercase letters
* $M$: possibly empty sequences of letters and underscores
* $V$: variable names (identifiers)
* $N$: numerals

There are quite a number of nonterminals, but there is a sense (which we will make more precise later) that the first three are really doing all the heavy lifting in the description of this language.  The latter seven non-terminals are just describing two things: the shape of numbers and the shape of variable names (also known as identifiers).

Note: there is nothing special about the particular letters used for the nonterminals, we could have used alternative names and we would still consider it essentially the same description of the While programming language.  For example, if we replaced everywhere in the rules the letter N by the letter P.

### Derivation

The process of deriving a string from the grammar, step by step, is made precise using the derivation relation $\alpha \to \beta$ (this is a shorter arrow than the one that we use to separate to the two sides of the rules; ideally we would use a more visually distinctive arrow, like $\Rightarrow$, but this takes twice as long to draw on the board in lectures).

<div class="defn" markdown="1">
The __one-step derivation relation__ is a binary relation on sentential forms with two sentential forms $\alpha$ and $\beta$ related, written $$\alpha \to \beta$$, just if $\alpha$ is of shape $\alpha_1 X \alpha_2$ and there is a production rule $X \longrightarrow \gamma$ and $\beta$ is exactly $\alpha_1 \gamma \alpha_2$.  

We write $\alpha \to^* \beta$, and say __$\beta$ is derivable from $\alpha$__ (or __$\alpha$ derives $\beta$__) just if $\beta$ can be derived from $\alpha$ in any (finite) number of steps, including zero steps.

Finally, we say that a word $w$ over $\Sigma$ is in the __language of a grammar__ $(\Sigma,\mathcal{N},\mathcal{R},S)$ just if $S \to^* w$.
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

We discuss the following examples over alphabet $$\{0,1\}$$ (most are from Sipser Ex. 2.4), which shows some of the common patterns when formulating a CFG:

$$
  \{ w \mid \text{$w$ is any word over $\{0,1\}$}\}
$$

$$
  \{ w \mid \text{$w$ contains at least three letter 1} \}
$$

$$
  \{ w \mid \text{$w$ starts and ends with the same symbol} \}
$$

$$
  \{ w \mid \text{the length of $w$ is odd and its middle letter is $0$}\}
$$

$$
  \{ w \mid \text{$w$ is a palindrome (i.e. the same word when reversed)}\}
$$

$$
  \emptyset
$$

$$
  \{uv \mid |u| = |v| \text{ and } u \neq v\}
$$