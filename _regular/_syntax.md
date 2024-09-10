---
layout: math
title: Regex Syntax
nav_order: 2
mathjax: true
parent: Regular Expressions
---

# Regular Expression Syntax

Regular expressions are a language for describing strings using patterns.

## Formal Syntax

You are already familiar with *arithmetic expressions* which are expressions built from numbers and the operators $+$, $-$, $*$ and $÷$.

{% include defn_regex_syntax.liquid %}

The idea is that $\emptyset$ is a regular expression that matches nothing, $\epsilon$ is a regular expression that matches just the empty string and $a$ is a regular expression that matches the single character $a$.  
Then we can combine regular expressions $R$ and $S$ using *concatenation* $(R \cdot{} S)$ to form a new regular expression that will match strings $w$ that can be broken into two pieces $uv$ with $u$ matched by $R$ and $v$ matched by $S$; or, using *choice* to form a new regular expression $(R + S)$ that will match strings $w$ that are matched either by $R$ or by $S$.  Finally, with a regular expression $R$ already in your hand, you may construct the *Kleene star* $(R^*)$, which will match any word that can be divided into finitely many (possibly 0) consecutive pieces, each of which match $R$.

Some examples of regular expressions over the alphabet $$\{0,1\}$$ are:
* $$((0+1) \cdot{} (0^*))$$ - matches all non-empty strings whose tail consists entirely of zeroes
* $$((0+1)^*)$$ - matches all words over $$\{0,1\}$$
* $$(((0^*) \cdot{} 1) \cdot{} (0^*))$$ - matches all strings that contain exactly one 1
* $$((\epsilon + 1) \cdot{} ((((0 \cdot{} 1) \cdot{} 0) \cdot{} 1))$$ - matches $0101$ or $10101$

This vast quantities of parentheses are needed in order to ensure that our simple description of the syntax of regular expressions above is *unambiguous*.  Informally, this means that there is only one way to understand an expression involving $\emptyset$, $\epsilon$, $+$, $\cdot{}$, $$*$$ and so on as a *regular expression*.  This would not be the case if we simply omitted some of the parentheses.  For example consider the expression $$0 + 1 \cdot{} 0^*$$.  Is this supposed to mean:
* $$((0 + 1) \cdot{} (0^*))$$ - a choice between 0 and 1 followed by any number of zeros?
* Or, $$(0 + (1 \cdot{} (0^*)))$$ - a choice between 0 and the word consisting of 1 followed by any number of zeros?
* Or, $$(((0 + 1) \cdot{} 0)^*)$$ - a choice between 0 and 1, followed by zero, which is all repeated any finite number of times?
* Or, something else, there are other possibilities...

However, to alleviate some of the terrible toil involved in writing quite so many parens, let us allow ourselves to omit some of them and resolve any ambiguities by agreeing a convention:
{% include conv_regex_parens.liquid %}

Finally, let us borrow a trick from arithmetic and agree to suppress the concatenation operator in favour of juxtaposition:
{% include conv_regex_concat.liquid %}

With your agreement then, we will understand the (formerly) problematic $$0 + 1 \cdot{} 0^*$$ unambiguously as $$(0 + (1 \cdot{} (0^*)))$$.  The agreement allows us to write our previous examples more compactly:
* $$(0+1)0^*$$ instead of $$((0+1) \cdot{} (0^*))$$
* $$(0+1)^*$$ instead of $$((0+1)^*)$$
* $$0^*10^*$$ instead of $$(((0^*) \cdot{} 1) \cdot{} (0^*))$$
* $$(\epsilon + 1)0101$$ instead of $$((\epsilon + 1) \cdot{} ((((0 \cdot{} 1) \cdot{} 0) \cdot{} 1))$$

Some other examples of regular expressions over $$\{0,1\}$$ (based on Sipser 1.53):
* $$(0+1)^*1(0+1)^*$$ - matches words containing at least one 1
* $$(0+1)^*001(0+1)^*$$ - matches words containing 001 as a substring
* $$1^*(011^*)^*$$ - matches words in which every occurrence of 0 is followed by at least one 1
* $$0 + 1 + (0(0+1)^*0) + (1(0+1)^*1)$$ - matches words that start and end with the same symbol
* $$((0 + 1)(0 + 1))^*$$ - matches words of even length

## Syntactic Sugar

In practice, when we want to describe more complicated kinds of matching, it will be useful to allow ourselves certain abbreviations that make the regexes more succinct and readable.  
{% include defn_regex_sugar.liquid %}

We will also typically introduce abbreviations as we go, by temporarily defining variables, e.g.
> $$(RR)^*$$ where $R = [abcdef]$

Note: we are *not* extending the language of regular expressions, it will still be the case that every regular expression is a combination of letters of the alphabet, $\emptyset$, $\epsilon$, $+$, $\cdot{}$ and $$*$$, exactly  as we have defined originally.  The abbreviations are simply to make our lives easier as humans when we construct examples of regular expressions.  When we come to analyse them, we can rely on the fact they can all be written using the simple syntax given in the definition at the top.

Some more examples, with the ASCII alphabet:
 * $$(R(R + \epsilon)SR + R(R+\epsilon)S(S+\epsilon))\ SRR$$ where $$R = [ABCD\ldots{}XYZ]$$ and $$S = [0123456789]$$.  This is a description of the UK postcode format (from [ideal-postcodes.co.uk](http://ideal-postcodes.co.uk)).  It could be more precise because it matches strings that are not currently legitimate UK postcodes, such as ZZ1 9UX (there is no such outward code starting ZZ in the UK at present).
 * $$\{\} + \{(R,)^*R\}$$ where $$R = [0123456789]^+$$. A comma separated list of integers, wrapped in braces.
 * With $$R = [0123456789]$$, we have all possible dates in dd/mm format.:

   $$ 
     \begin{array}{ll}&([012]R + 30)/(09 + 04 + 06 + 11)\\ &+\ ([012]R + 3[01])/(01 + 03 + 05 + 07 + 08 + 10 + 12)\\ &+\ ([012]R)/02\end{array}
   $$
   
{% comment %}
## Identities

Let us write $R \equiv S$ just if $R$ and $S$ match exactly the same set of strings.  We *don't* think of them as the same regular expression however: they are different programs that happen to do the same thing.

Some identities concerning regular expressions:
* $$R + \emptyset \equiv R$$
* $$R + S \equiv S + R$$
* $$R + R \equiv R$$
* $$(RS)T \equiv R(ST)$$
* $$R(S + T) \equiv (RS + RT)$$
* $$(S + T)R \equiv (SR + ST)$$
* $$RR^* \equiv R^*R$$
* $$R \emptyset \equiv \emptyset$$
* $$\emptyset R \equiv \emptyset$$
* $$\emptyset^* \equiv \epsilon$$
{% endcomment %}


