---
layout: math
title: Strings
nav_order: 1
mathjax: true
parent: Syntax
---

# Strings and Languages

{: .defn }
An __alphabet__ is any finite set, whose members are called __letters__ (equivalently: *symbols* or *characters*).  We typically use $\Sigma$ to denote a generic alphabet and $a,b,c,d$ as variables that stand for the letters.

{: .defn }
A __string__ (equivalently: *word*) over an alphabet $\Sigma$ is a finite sequence of characters from $\Sigma$.  The sequence may be empty, and we write the __empty string__ as $\epsilon$.  We typically use $u,v,w,x,y,z$ as variables that stand for a string.

For example, $\mathsf{hello}$ is a string over the alphabet $$\{\mathsf{a},\mathsf{b},\mathsf{c},\ldots,\mathsf{y},\mathsf{z}\}$$, but $\mathsf{Hello}$ is not.  Another string over the same alphabet is $\mathsf{abbbbb}$ and another is $\epsilon$.  Strings over the alphabet $$\{▽,△\}$$ include $\mathord{\bigtriangleup}\mathord{\bigtriangleup}\mathord{▽}$ and $\bigtriangledown$.  Strings over the alphabet $$\{\mathsf{oh},\mathsf{really}\}$$ in which each letter is one of these two strings, include $\mathsf{oh}\mathsf{really}$ and $\mathsf{really}\mathsf{oh}\mathsf{really}$, but not $\mathsf{ohr}$.

{: .defn }
The set of __all strings over the alphabet $\Sigma$__ is written $\Sigma^*$, this always includes $\epsilon$.  We will refer to a set of strings as a __language__.

Now we consider some operations on strings.

{: .defn }
A string $w$ is said to be a __substring__ of a string $v$ just if $w$ appears consecutively in $v$.

For example, $aab$ is a substring of $aaabbb$ but it's not a substring of $abab$.  On the other hand $a$ is a substring of both (typically, we are not too concerned about distinguishing between $a$ the letter and $a$ the string consisting of a single letter).  We also have that $\epsilon$ is a substring of any string (over any alphabet).

{: .defn }
The __length__ of a string $w$, written $\|w\|$, is just the number of characters in the string.  That is, if $x = a_1\cdots{}a_k$ then $\|x\| = k$.

{: .defn }
Given strings $x$ and $y$, we write $xy$ for the string obtained by __concatenating__ $y$ to the end of $x$.  That is, if $x = a_1\cdots{}a_k$ and $y = b_1 \cdots{} b_m$ then $xy = a_1\cdots{}a_k b_1 \cdots{} b_m$.  We write $w^k$ for the __$k$-fold concatenation__ of $w$ with itself, i.e. the word $\underbrace{ww\cdots{}w}_{\text{$k$-times}}$.

We will use concatenation and variables to describe the shape of strings abstractly, just as we use arithmetic operators and variables to describe numbers abstractly.  For example, to say that a string $u$ over the alphabet $$\{a,b\}$$ contains at least two occurrences of $a$, we can say that:

> $u$ is of shape $w_1aw_2aw_3$  

More precisely, with this form of words -- "of shape" -- we are saying that _there exist_ strings $$w_1,w_2,w_3 \in \{a,b\}^*$$ such that $u = w_1 a w_2 a w_3$ (i.e. $u$ is exactly the concatenation of $w_1$ followed by an $a$ followed by $w_2$ followed by an $a$ followed by $w_3$).  Every string over this alphabet that contains at least two $a$ can be decomposed in this way (there may be several ways, generally), for example:

| String | Decomposition as $w_1aw_2aw_3$ |
| $bbabab$ | $w_1 = bb$, $w_2 = b$, $w_3 = b$ |
| $abbaa$  | $w_1 = \epsilon$, $w_2 = bba$, $w_3 = \epsilon$ |
| $aaabbb$  | $w_1 = \epsilon$, $w_2 = \epsilon$, $w_3 = abbb$ |

Compare this with how you would say that an integer $n$ is even: _$n$ is of shape $2*m$ (for some $m$)_.

We will often use this in combination with set notation, to describe languages of strings that all have a certain shape.  For example:
  * $$\{w_1 a w_2 a w_3 \mid w_1, w_2, w_3 \in \{a,b\}^* \}$$ - the set of all strings over $$\{a,b\}$$ containing at least two $a$
  * $$\{w_1 a w_2 a w_3 \mid w_1, w_2, w_3 \in \{b\}^* \}$$ - the set of all strings over $$\{a,b\}$$ containing exactly two $a$
  * $$\{xy \mid x,y \in \{0,1\}^* \ \text{and}\ \lvert x \rvert = \lvert y \rvert \}$$ - the set of all strings over $$\{0,1\}$$ that are of even length.
  * $$\{ 0u01u10u0 \mid u \in \{0,1\}^* \}$$ - the set of all strings over $\{0,1\}$ that are formed by repeating a substring three times consecutively, with the first wrapped in 0s, the second in 1s and the third in 0s
  * $$\{ uv \mid u,v \in \{0,1\}^* \ \text{and}\  #_0(u) = #_1(v) \}$$
  * $$\{ 0^ku0^k \mid u \in \{0,1\}^* \ \text{and}\ k > 0 \}$$

Note! Do not confuse, e.g. $$\{w_1 a w_2 a w_3 \mid w_1, w_2, w_3 \in \{a,b\}^* \}$$ with $$\{w_1 a w_2 a w_3\}$$: the latter is a singleton set, it contains the string $$w_1 a w_2 a w_2$$ as its only element (and for that to make sense we must have introduced $w_1$, $w_2$ and $w_3$ already).  E.g.

> Suppose, $w_1 = a$, $w_2 =aa$ and $w_3 = aaa$.  Then my FAVOURITE set is $$\{w_1aw_2aw_3\}$$, which I just ADORE because its ONE AND ONLY element is $aaaaaa$.

<!-- You may rightly ask why we bother with these descriptions: isn't "the set of all strings over $$\{a,b\}$$ containing at least two $a$" already clear enough?  Well, you are right, it is, and I will often simply write an English language description like that when it is clear enough.  However, (a) sometimes English on its own is not clear enough (see the last bullet point example above) and (b) the idea of decomposing a string with respect to concatenation is a key idea in formal languages, so it is good to see it more generally here first. -->