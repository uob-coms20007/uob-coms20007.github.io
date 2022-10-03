---
layout: math
title: Regex Semantics
nav_order: 3
mathjax: true
parent: Regular Expressions
---

# Operational Semantics of Regular Expressions

We already explained in [Regex Syntax](syntax.html) how to understand a regular expression in terms of the strings that are matched by each of its pieces.  For example, we said that an expression $a$ matches the string consisting just of $a$ and $R + S$ will match every string that is either matched by $R$ or matched by $S$.  We are now going to give an alternative way to understand a regular expression in terms of how the string matching can be computed, one letter at a time.  

The understanding we already have is already good enough for the purpose of designing regular expressions as string matchers, and I recommend you use the knowledge you already have when asked to construct a regular expression that matches a certain class of strings.  So why bother with an alternative perspective?  We introduce the following, *operational semantics*, for a few different reasons.  
  * It will give us an analysis of regular expressions that allows us to explore their computational properties more easily.
  * It will give us some insight into how we could implement a regular expression language ourselves on a computer. 
  * It will be a useful prelude to the more complex operational semantics of the general-purpose imperative programming language that we will see in a few weeks time.

*Operational semantics* is a way of giving meaning to programs by explaining how they will compute, *step-by-step*.  

There are two main questions one has to ask when giving an operational semantics for a programming language: 
  * If I took a snapshot of the computation at a particular instant in time, what would it look like?  This is the notion of *configuration* (it is sometimes also called the "state of the computation", but we will use "state" in more specific ways later).
  * What is involved in a making single computational step?  In other words, how does a program get from one configuration to another?  This notion is sometimes called *transition* (but *step* works just as well).

We shall the describe the computation of regular expressions as an evolution of the regular expression itself, in which it is consuming a string given to it as input, letter-by-letter.  So, as the regular expression makes a step it changes into a new expression, possibly consuming a letter of its input as it does so. For example, we will see that the regular expression $$ab^*$$ can make a step to $$b^*$$ whist consuming letter $a$ from the front of its input.

{% include defn_configuration.liquid %}

Given a certain configuration $R$ we are going to say what are the possible steps that $R$ can make based on its syntactical shape.  For example, if $R$ is of shape $a \cdot{} S$, i.e. it is a concatenation consisting of the letter $a$ followed by some regular expression $S$, then the only step that it should be allowed to make is to evolve to $S$ whilst simultaneously reading letter $a$ from its input.

If a regular expression $$R$$ is allowed to make a step by (possibly) consuming a single input $\ell$ and to become $S$ then we write:
$$
  R \xrightarrow{\ell} S
$$

We will allow $\ell$ to be either a single letter from the associated alphabet, or $\epsilon$ - meaning it makes the step without consuming any input.  For example, we will have $$ab^* \xrightarrow{a} b^*$$, i.e. $$ab^*$$ can make a step whilst consuming $a$, and after the step it now behaves like $b^*$.  Similarly, we will have $$(a + \epsilon) \xrightarrow{\epsilon} a$$, i.e. $a + \epsilon$ can make a step without consuming input, and afterwards it behaves like $a$ (in this step, the computation is choosing which branch it will take).

All the valid transitions/steps that can a regular expression can make will be defined by a system of rules.  Each of the rules is given in the following format:

{% include rule_format.liquid %}

The transitions listed above the line are called the *hypotheses* or *premises* of the rule.  Any condition to the side is called the *side condition* and the single transition that appears under the line is called the *conclusion*.  The rule name is just an identifier we can use when we talk amongst ourselves about the rules.

{% include rule_reading.liquid %}

So the rules are there to help you deduce which transitions are valid, based on which other ones are already known to be valid.  Some of the most basic rules will not have any premises and so you can know they are valid in an absolute sense, and they form a kind of base case to the whole activity.

{% include defn_transition.liquid %}

Each rule is a kind of scheme that allows you to deduce the validity of many different transitions, via its *instantiations*.  An instantiation of a rule is simply a consistent replacement of each of the variables by concrete objects of the appropriate kind.  For example, suppose we are interested in regular expressions over $$\{a,b,c\}$$, then some instantiations of rule (R2) are:
* $$(ab^* + b) \xrightarrow{\epsilon} ab^*$$, by replacing $R$ by $ab^*$ and $S$ by $b$.
* $$(a + c + b) \xrightarrow{\epsilon} a + c$$, by replacing $R$ by $a+c$ and $S$ by $b$.
* $$(abc + (b + \epsilon)) \xrightarrow abc$$, by replacing $R$ by $abc$ and $S$ by $b + \epsilon$.

However, $$(a + c + b) \xrightarrow{\epsilon} c + b$$ is *not* an instantiation of (R2) since we are not consistent in how we have replaced $R$ and $S$ (here, recalling our conventions on parentheses, $R$ on the left is replaced by $$a+c$$, but $R$ on the right was replaced by $c+b$).  Similarly, $$a + b \xrightarrow{a} a$$ is not an instantiation since the label, $\epsilon$, is not a variable and hence is not eligible to be replaced.

In rule (R1) the only variable is $a$ which, if you recall correctly, is the variable we use to stand for letters.  Hence, there is exactly one instantiation of (R1) for each letter of the alphabet.

With this in mind, let us consider each rule in turn.  We will illustrate some of the more interesting rules using examples over the alphabet $$\{0,1\}$$.
* Rule (R1) says that a regular expression which is itself just a single letter $a$ can consume an $a$ and become $\epsilon$.
* Rules (R2) and (R3) say that a choice of $(R + S)$ can transition to either $R$ or $S$ without consuming any letters.
* Rule (R4) says that a concatenation whose first component is $\epsilon$ can make a step by dropping the $\epsilon$ part, since this only matches only the empty string.  This consumes no input.
* Rule (R5) and (R6) concern concatenations in which the first component is not $\epsilon$.  Rule (R5) says that, if the first component $R$ of the concatenation can itself make a transition to $\epsilon$ on label $\ell$, then the whole concatenation can transition directly to $S$ on label $\ell$.  For example, we know from Rule (R1) that $0 \xrightarrow{0} \epsilon$, so we can deduce using rule (R5) that $01 \xrightarrow{0} 1$.
* Rule (R6) says that if the first component of the concatenation, $R$, can make a step the does not end in $\epsilon$, then that step can be promoted to the whole concatenation.  For example, we know from rule (R2) that $(0 + 1) \xrightarrow{\epsilon} 1$, so we can deduce by rule (R6) that $(0+1)1^* \xrightarrow{\epsilon} 11^*$.
* Rule (R7) and (R8) characterise the Kleene star $$R^*$$ as behaving something like a finite sequence of $R$s (which makes sense, because it matches any word that can be divided into a finite sequence of substrings that each match $$R$$).  Rule (R7) says that the list may be empty, by $$R^*$$ simply degenerating to $\epsilon$, and rule (R8) says that we can unfold the list by revealing the next $R$, to get $$R\cdot{}R^*$$.

Note: rules (R5) and (R6) are the only times you get to apply a rule "inside" an expression; what I mean by this is that you can think that you are making a step as a whole by making a step on a strict subexpression.  By contrast, there is *no* transition $0(0+1) \xrightarrow{\epsilon} 00$, in which we would like to make the transition $(0+1) \xrightarrow{\epsilon} 0$ (which is itself justified, by rule (R3)) *inside* the right-hand component of the concatenation $0(0+1)$.  *This is not allowed by any rule.*  On the other hand, it is very helpful to think of rules (R5) and (R6) precisely as allowing you to make a step as a whole by making a step inside the left(-most) component of a concatenation, and you will use these rules frequently when computing with non-trivial regexes.
 
In practice, we are not interested in deducing random transitions for any old expressions, but we usually have a particular expression in mind and we want to trace out computations involving many steps.  For example:

$$
  \begin{array}{rcl}
  (00 + 1)^* &\xrightarrow{\epsilon}& (00 + 1)(00+1)^*\\ 
             &\xrightarrow{\epsilon}& 00(00+1)^*\\ 
             &\xrightarrow{0}& 0(00+1)^*\\
             &\xrightarrow{0}& (00+1)^*\\
             &\xrightarrow{\epsilon}& (00+1)(00+1)^*\\
             &\xrightarrow{\epsilon}& 1(00+1)^*\\
             &\xrightarrow{1}& (00+1)^*\\
             &\xrightarrow{\epsilon}& \epsilon
  \end{array}
$$

Here, the first transition is justified by rule (R8), and the second by rule (R6) with the step inside justified by (R2).  The third transition is justified by a use of rule (R6) with the step inside justified by (R5), and the step inside that justified by (R1)!  This is because the expression at the start of the transition, when written more explicitly is $((0 \cdot{} 0) \cdot{} (00+1)^*)$.  However, when constructing a transition, it is probably not worth giving thought to which of (R5) and (R6) applies and precisely what is the step inside, and instead view rules (R5) and (R6) simply as forcing all computation to happen in the  left-most component of a large concatenation.

We call a sequence of transitions such as this, a *trace*.

{% include defn_trace.liquid %}

There is no requirement that finite traces come to a full stop in any sense.  For example, $$(00 + 1) \xrightarrow{\epsilon} 00 \xrightarrow{0} 0$$ is a perfectly good trace, even though we could potentially perform more computation.  Also note that a trace may be empty.

{% include defn_complete_trace.liquid %}

A complete finite trace that ends in $\epsilon$ corresponds to matching some word (possibly the empty string).  The word that is matched can simply be read off the transition labels by concatenating them in order.  For example, the sequence of transitions above shows that $$001$$ can be matched by the regex $$(00 + 1)^*$$.

However, before we turn this into a definition, it is useful to be able to abbreviate traces where we only care about the start and the end.

{% include defn_reach.liquid %}

For example, since we have the trace starting $$(00+1)^*$$ and ending $$\epsilon$$ above, we can legitimately write $$(00_1)^* \rreds{001} \epsilon$$.  This gives us a neat way to describe which strings are matched by a regex.

{% include defn_matches.liquid %}

Here is a trace showing that $$0 + 1 + (0(0+1)^*0) + (1(0+1)^*1)$$ matches $010$:

$$
\begin{array}{rcl}
  (0 + 1 + (0(0+1)^*0) + (1(0+1)^*1)) &\rred{\epsilon}& 0 + 1 + (0(0+1)^*0)\\
                                      &\rred{\epsilon}& 0(0+1)^*0\\
                                      &\rred{0}       & (0+1)^*0\\
                                      &\rred{\epsilon}& (0+1)(0+1)^*0\\
                                      &\rred{\epsilon}& 1(0+1)^*0\\
                                      &\rred{1}       & (0+1)^*0\\
                                      &\rred{\epsilon}& 0\\
                                      &\rred{0}       & \epsilon
\end{array}
$$

Note that it actually takes two steps to transition from $$0 + 1 + (0(0+1)^*0) + (1(0+1)^*1)$$ to $$0(0+1)^*0$$.  That is because we agreed the former, more properly written out, is $$(((0 + 1) + (0(0+1)^*0)) + (1(0+1)^*1))$$ so we really have to make two choices before we get to the subexpression we are interested in.  In practice it very rarely matters whether this would take one step or two, so you are unlikely to be penalised if you answer a question by transitioning in one step instead of two because you forgot about some bracketing that doesn't affect what can be matched in any case.

