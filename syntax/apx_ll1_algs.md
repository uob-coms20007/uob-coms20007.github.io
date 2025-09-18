---
layout: math
title: A. LL(1) Calculations
mathjax: true
nav_order: 9
parent: Syntax
---


# Appendix A: Calculating Nullable, First and Follow

## (NOT ASSESSED)

_This material is not assessed, look at it only for interest._

In each case, the idea will be to iteratively compute approximations $\nullable[X]$, $\first[X]$ and $\follow[X]$ to the three properties, each iteration refining the approximation until eventually they exactly coincide with $\nullable(X)$, $\first(X)$ and $\follow(X)$ respectively.  

So, I will use the square brackets versions to represent an approximation which we will iteratively modify (think of them as mutable arrays indexed by nonterminals), and the round brackets (parentheses) version is the true value of the map at each nonterminal (according to the mathematical definition above).  

 ***

### Calculating Nullable

<!-- Start by initialising the approximation $\nullable[X]$ to false for each non-terminal $X$. -->

<!-- Next, it is useful to define a helper function $\nullable_s(\alpha)$ which is true if a given sentential form is nullable according to the current approximation: -->

To calculate $\nullable(X)$ for each non-terminal $X$, first set $\nullable[X]$ to false for each $X$, then repeatedly perform the following iteration:

* For each production $X \Coloneqq \alpha$: 
    * $\nullable[X] := \nullable[X] \vee \nullable_s(\alpha)$

until a complete iteration produces no change to $\nullable[X]$ for any $X$ - we say that we have reached a fixed point.  At this point, $\nullable[X]$ will be true iff $X$ can derive the empty string.

#### Example
To calculate $\nullable(B)$ for the grammar above we start by setting $\nullable[B]$ to false, then we repeatedly iterate through all the productions, checking if their RHS is a nullable sentential form with respect to the current approximation $\nullable[B] = \mathsf{false}$.  In each case, the right-hand side of the production is not $\epsilon$ and does not consist entirely of nullable non-terminals (this must be so because every right-hand side has a terminal symbol in it).  Therefore, inside the body of the loop we will never change $\nullable[B]$, and thus we have reached a fixed point after just one complete iteration, i.e. when we exited the loop over productions, $\nullable$ was the same as when we started the loop.  

This is correct, since indeed $B$ is not nullable, i.e. the approximation that we built $\nullable[B]$ is the same as the correct nullability of $B$, namely $\nullable(B)$.

### Example
Suppose we want to calculate $\nullable$ for the following grammar:

$$
  \begin{array}{rcl}
    S &\Coloneqq& 0S0 \mid 1S1 \mid T \\
    T &\Coloneqq& \# \mid \epsilon
  \end{array}
$$

First set $\nullable[X]$ to false for every nonterminal $X$ in the grammar.

| Nonterminal | Nullable[-] |
| S | false |
| T | false |

Then we perform a complete execution of the loop over productions.  For $S \Coloneqq 0S0$ the RHS is clearly not nullable since it contains two terminal symbols 0.  Similarly for $S \Coloneqq 1S1$.  For $S \Coloneqq T$ we must ask whether $T$ is nullable _according to our current approximation_, i.e. the current value of $\nullable[T]$.  If we consult the table above, $\nullable[T]$ is currently false, so therefore the RHS of the production $S \Coloneqq T$ is _not_ nullable.  Next we consider $T \Coloneqq \\#$, the RHS is a terminal symbol, $\\#$, so it is not nullable.  Finally we consider $T \Coloneqq \epsilon$.  Here, the RHS is immediately nullable, so we set $\nullable[T]$ to true (technically, we set it to false $\vee$ true, but this is just the same as true).  Hence, at the end of the loop our approximation is:

| Nonterminal | Nullable[-] |
| S | false |
| T | true |

Since this is different from our approximation at the start of the loop, we must go through the whole process again, with this new starting point.  So, let us consider each production in turn.  For $S \Coloneqq 0S0$ and $S \Coloneqq 1S1$, it is still the case that their RHS are not nullable - this will never change because they literally have terminal symbols on their RHS.  However, for $S \Coloneqq T$, the nullability of $T$ depends on the current approximation and now our approximation says that $T$ _is_ nullable (i.e. $T$ can derive the empty string), so it follows $S$ is nullable too because we can derive the empty string from $S$ by choosing this production and then deriving the empty string from $T$.  Hence, we update our approximation to set $\nullable[S]$ to true.  We should then consider the two productions for $T$, however, this does not change anything.  Hence, our new approximation is:

| Nonterminal | Nullable[-] |
| S | true |
| T | true |

Since this is different from the previous approximation, we should now go through the loop again to check that nothing further changes.  However, in this case, I know that the approximation cannot change any more due to an iteration of the loop because the assignment in the body of the loop can only change nullability from false to true, but not from true to false.  So, another run through of the loop will not change the approximation and we have reached a fixed point and so the above is the correct definition of $\nullable$ for this grammar.

***

### Calculating First

<!-- Start by initialising $\first[X]$ to the empty set for each non-terminal $X$. -->

<!-- Next, it is useful to define a helper function $\first_s(\alpha)$ which gives the set of terminals that can start a sentential form derivable from $\alpha$, according to the current approximation: -->

To calculate $\first(X)$ for each non-terminal $X$, first set $\first(X)$ to the empty set $\emptyset$ for each $X$.  Then repeatedly perform the following iteration:

* For each production $X \Coloneqq \alpha$: 
    * $\first[X] := \first[X] \cup \first_s(\alpha)$

until a complete iteration produces no change to $\first[X]$ for any $X$, i.e. we have reached a fixed point.  Then $\first[X]$ will be exactly $\first(X)$.

#### Example
For example, we can calculate $\first(B)$ for the grammar above as follows.  First set $\first[B]$ to $\emptyset$.  This is the first approximation:

| Nonterminal | First[-] |
| B | $\emptyset$ |

Then we execute the loop, examining each production in turn.  

The first production is $B \Coloneqq B \andop B$.  We must compute $\first_s(B \andop B)$ with respect to our current approximation $\first[B]$.  The definition for $\first_s$ above says that, since $B$ is not nullable, the terminal symbols that can start a sentential form derived from $B \andop B$ are just the terminal symbols that can start a sentential form derived from $B$, i.e. $\first(B)$.  We haven't yet computed the final value of $\first(B)$, so we use our approximation instead.  According to our approximation (in the table above this paragraph), $\first[B]$ is the empty set.  Hence, we union the current approximation $\first[B]$ with the empty set, and of course, this changes nothing.   

We proceed to the next production $B \Coloneqq B \orop B$.  Again we must compute $\first_s(B \orop B)$ according to the current approximation.  As in the previous case, $\first_s(B \orop B)$ is just $\first(B)$, so we consult our approximation $\first[B]$, which is empty.  Hence, when we add the empty set into $\first[B]$ nothing changes and we proceed onto $B \Coloneqq \tt$.  Now $\first_s(\tt) = \\{\tt\\}$, so we update $\first[B]$ to include $\tt$.  A similar analysis of the next production forces us to update $\first[B]$ to also include $\ff$.  By the same process, analysing $B \Coloneqq \tm{id}$ forces us to add $\tm{id}$ to the approximation.  Finally an analysis of the last production, $B \Coloneqq (B)$, gives $\first_s((B)) = \\{(\\}$ and so, at the end of one complete iteration, we have a an updated approximation:

| Nonterminal | First[-] |
| B | true, false, id, ( |

Since this approximation differs from the one before we executed the loop, we have not yet reached a fixed point and we must repeat the process.  Reanalysing the first production $B \Coloneqq B \andop B$ with respect to our current approximation now gives $\first_s(B \andop B) = \first[B] = \\{\tt,\ff,\tm{id},(\\}$, but this does not add anything new to $\first[B]$, which remains $\\{\tt,\ff,\tm{id},(\\}$.  Analysis of the second production is similar.  Reanalysing the production $B \Coloneqq \tt$ gives $\first_s(\tt) = \\{\tt\\}$, and again nothing new is added to $\first[B]$.  The remaining productions are similar, and do not add anything new.  Therefore, at the end of this second complete iteration, $\first[B]$ did not change and we have reached a fixed point.

***

### Calculating Follow

Start by initialising $\follow[X]$ to the empty set for each non-terminal $X$.  Then repeatedly perform the following nested iteration: 

* For each non-terminal $X$:
    * For each occurrence of $X$ on the right-hand side of a production $Y \Coloneqq \alpha X \beta$:
        * $\follow[X] := \follow[X] \cup \first_s(\beta)$
        * if $\nullable_s(\beta)$ then $\follow[X] := \follow[X] \cup \follow[Y]$

until a complete iteration of the outer loop produces no change to $\follow[X]$ for any non-terminal $X$, i.e. we have reached a fixed point.  Then $\follow[X]$ will be exactly $\follow(X)$.

#### Example
To calculate $\follow(B)$ we proceed as follows.  First set $\follow[B]$ to $\emptyset$. Let us also recall the nullable and first properties already computed for $B$ (though these will not change)"

| Nonterminal | Nullable | First | Follow |
| B | false | $\tt$, $\ff$, $\tm{id}$, $($ | \emptyset |

Then we must loop over all occurrences of $B$ on the right-hand-sides of rules.  So let's identify all the occurrences of $B$ on the right-hand sides of productions and what sentential form follows immediately after (i.e. $\beta$ in the description above):

1. $B \Coloneqq \underline{B} \andop B$ (here $\beta = \andop B$)
1. $B \Coloneqq B \andop \underline{B}$ (here $\beta = \epsilon$)
1. $B \Coloneqq \underline{B} \orop B$ (here $\beta = \orop B$)
1. $B \Coloneqq B \orop \underline{B}$ (here $\beta = \epsilon$)
1. $B \Coloneqq (\underline{B})$ (here $\beta = )$)

Then we analyse each in turn.  For 1, we have $\first_s(\andop B) = \\{\andop\\}$ and so we add $\andop$ into $\follow[B]$.  For 2, the sentential form after the occurrence of $B$ is just $\epsilon$, and we have $\first_s(\epsilon) = \emptyset$, so nothing is added into $\first[B]$. However, this suffix $\epsilon$ is nullable, so the last clause of the definition applies and we must add all of $\follow[B]$ to $\follow[B]$, but this, of course, makes no change.  The analysis of 3 and 4 is similar, and we end up with $\follow[B] = \{\andop, \orop\}$.  Finally, we analyse 5, where $\first_s(\mathord{)}) = \\{)\\}$, so we are forced to add $)$ to $\follow[B]$.  Hence our new approximation is:

| Nonterminal | Nullable | First | Follow |
| B | false | $\tt$, $\ff$, $\tm{id}$, $($ | $\andop$, $\orop$, $)$ |

Our approximation $\follow[B]$ was changed as a result of analysing the occurrences 1--5, so we are not yet at a fixed point and must repeat the loop.  Note that any computations of $\first_s$ and $\nullable_s$ will not be different in the reanalysis, so we need only pay attention to the very last clause in the iteration, when $\nullable_s(\beta)$.  Consequently, the analysis of 1, 3 and 5 does not change.  When we reanalyse 2 and 4, where the suffix $\beta$ is $\epsilon$, we are forced to add in all of $\follow[B]$ into $\follow[B]$, but this does not change the contents.  Hence, at the end of the reanalysis, we have achieved a fixed point.

| Nonterminal | Nullable | First | Follow |
| B | false | $\tt$, $\ff$, $\tm{id}$, $($ | $\andop$, $\orop$, $)$ |

### Example

This example is based on one from Appel's "Compiler Construction in ML".
Suppose we want to compute $\follow$ for this grammar:

$$
  \begin{array}{rcl}
    X &\Coloneqq& a \mid Y\\
    Y &\Coloneqq& c \mid \epsilon\\
    Z &\Coloneqq& d \mid X\ Y\ Z
  \end{array}
$$

The $\nullable$ and $\first$ maps for this grammar are:

| Nonterminal | Nullable(-) | First(-) |
| X | true | a c |
| Y | true | c |
| Z | false | a c d |

To calculate $\follow$, we first set our approximation to empty for each nonterminal:

| Nonterminal | Follow[-] |
| X |  |
| Y |  |
| Z |  |

Then we iterate over each nonterminal.  For $X$, we first identify all of the occurrences of $X$ on the RHS of rules, and there is only one $Z \Coloneqq X\ Y\ Z$.  The sentential form that follows this occurrence of $X$ here is $Y\ Z$ and $$\first_s(Y\ Z) = \{ a, c, d \}$$ because $$\first(Y) = \{c\}$$, but $Y$ is nullable, so we must also include $$\first(Z) = \{ a, c, d \}$$.  Hence, according to the first statement in the loop, we add a, c and d to our approximation for $\follow[X]$.  This sentential form, $Y\ Z$, is not nullable (although $Y$ is, $Z$ isn't, so as a whole it cannot derive the empty string), thus the second statement in the loop does not apply.

Next consider $Y$.  There are two occurrences of $Y$ on the RHS of rules: (1) $X \Coloneqq Y$ and (2) $Z \Coloneqq X\ Y\ Z$.  For (1), the sentential form immediately following $Y$ is empty $\epsilon$.  The first set of $\epsilon$ is empty, so the first statement inside the loop adds nothing to the approximation, but $\epsilon$ is certainly nullable, so, according to the second statement, we must add everything from $\follow[X]$ into $\follow[Y]$.  Thus we update $\follow[Y]$ to $$\{a, c, d\}$$.  Now considering occurrence (2), the sentential form immediately following $Y$ is $Z$.  So we add everything from $\first_s(Z) = \first[Z]$ into $\follow[Y]$, but, so far, we have nothing in $\first[Z]$, so there is nothing to add.  Since $Z$ is not nullable, the second statement in the loop does not apply.

Finally, we consider nonterminal $Z$, which occurs only once on the RHS of rules, in $Z \Coloneqq X\ Y\ Z$.  The sentential form immediately following the occurrence is $\epsilon$, so the first statement of the loop adds nothing to $\follow[Z]$.  The second statement also adds nothing, because, although $\epsilon$ is nullable, the statement requires that we add everything from $\follow[Z]$ into $\follow[Z]$, which does nothing.

We have now completed executing the (nested) loop, and the new approximation is:

| Nonterminal | Follow[-] |
| X | a c d |
| Y | a c d |
| Z | |

This approximation differs from the one we entered the loop with, so we have to run again.

We consider non-terminal $X$, which occurs on the RHS of rule $Z \Coloneqq X\ Y\ Z$.  The first statement in the loop requires we add $\first_s(Y\ Z)$ to $\follow[X]$, and $\first_s(Y\ Z)$ is $$\{a, c, d\}$$, so adding these symbols into $\follow[X]$ changes nothing.  Sentential form $Y\ Z$ is not nullable since $Z$ is not nullable, so the second statement does not apply.  

We consider nonterminal $Y$, which occurs on the RHS of rules (1) $X \Coloneqq Y$ and (2) $Z \Coloneqq X\ Y\ Z$.  In (1), we must add $\first_s(\epsilon)$ to $\follow[Y]$, which does nothing and, since $\epsilon$ is nullable, we must add all of $\follow[X]$ to $\follow[Y]$, but this also does nothing, since these sets are already identical.  In (2), we must add $\first_s(Z) = \first[Z]$ to $\follow[Y]$, but this does not add anything new; and, since $Z$ is not nullable, the second statement in the loop does not apply.

Finally, we consider nonterminal $Z$, which occurs on the RHS of rule $Z \Coloneqq X\ Y\ Z$.  The first statement in the loop requires us to add $\first_s(\epsilon)$ to $\follow[Z]$, but this is empty.  The second statement requires us to add $\follow[Z]$ to $\follow[Z]$, but this has no effect.

Hence, the approximation has not changed as a result of executing the loop and we have reached a fixed point.

