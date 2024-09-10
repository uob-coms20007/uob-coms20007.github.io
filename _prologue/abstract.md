---
layout: math
title: Abstract Syntax
mathjax: true
nav_order: 1
parent: Prologue
---

# Trees and Abstract Syntax

In this unit, programs are the objects of study, as integers are the basic objects of study in number theory and reals in analysis.  So, given their importance, we need a good representation of programs that makes reasoning about them as easy and convenient as possible.  

We could represent programs using their concrete syntax, i.e. as a string, as in the previous lecture.  However, the concrete syntax contains a lot of information which is not essential to understanding the fundamental structure of the program.  For example, consider the same while loop expressed in the concrete syntax of various programming languages.  The first is C, do you know any of the others?

{% include ex_concrete_loops.liquid %}

  Each has its own syntactic peculiarities, some use braces, some parentheses, some use the keyword "do" and some don't.  However, the essential information is the same:

  1. It's a while loop.
  2. There is a loop guard (which is checking if $x$ is greater than 0).
  3. There is a loop body (which decrements x and increments y).

  I mean that this is the essential information in the sense that if you forget what the loops look like in their concrete syntax but I tell you points 1-3, then you can recreate the concrete syntax in whichever language you are interested in.  For example, if I tell you points 1-3 above, and then ask you to write the loop in C, then you can reconstruct (modulo whitespace and unnecessary bracketing) the first piece of concrete syntax above.  On the other hand, if I omitted any one of 1-3, say 2, then you could not possibly know what the loop looked like when written in C in the original concrete syntax.

In this unit, we will not concern ourselves with these little quirks of concrete syntax.  Instead we will represent programs by *abstract syntax trees* (ASTs).  ASTs are *abstract* in the sense of not containing so many irrelevant details such as whether we surround the body of the while loop in braces or not.  They are trees in the sense of, well, being trees rather than strings.

For example, we can represent the arithmetic expression $3 + (4 * 2)$ by the following abstract syntax tree:
```text
    +
   / \
  3   *
     / \
    4   2
```
and, we can represent the while loop by the following AST:
```text
          while
        /       \
       >        seq
      / \      /   \
     x   0   assn   assn 
             / \     / \
            x   -   y   + 
               / \     / \
              x   1   y   1
```
If we look just at the root of this tree, it gives us the essential information from the concrete syntax above: namely that we are dealing with a while loop, and that the while loop consists of two ingredients, the guard (first child) and the body (second child).  You don't need to understand any more about this for now, but we will discuss it in more detail when we look at While programs later.

A nice feature of the tree representation is that you can easily see which is the *toplevel* operator, which is the entity at the root of the whole tree.  In theory the entity at the root of the tree does not have any more significance than any other node in the tree, but in practice it is usually the most important.  For example, by just knowing what is at the root of the tree, one can often immediately infer what kind of syntactic object the tree represents.  For example, consider the following two trees in which I have not told you the exact details of the two subtrees on the left and right.  

```text
      &&           +
      / \         / \
     ?   ?       ?   ?
```

I hope that you can guess that the first is the AST of a Boolean expression (i.e. it should evaluate to True or False when executed) and the the second is the AST of an arithmetic expression (i.e. it will probably evaluate to some kind of number).  Generally, we will process ASTs starting from the root and working our way down.

## Tree grammars

We can use grammars to describe languages of abstract syntax trees, in which case they are called *tree grammars*.  For this to work, we just need to remember to say what kind of *tree constructors* the terminal symbols correspond to, by giving their arity.

{% include defn_arity.liquid %}

For example: 
  * The arity of each arithmetic operators $\*$, $+$ and $-$ is 2.  
  * The arity of Boolean conjunction ("and") is 2, and the arity of Boolean negation ("not") is 1.  
  * The arity of any natural number is 0 -- these symbols require no inputs and they will always be leaves of a syntax tree.
  * The arity of the Boolean values *true* and *false* are each 0.
  * The arity of "while" (as a symbol in the abstract syntax) is 2 -- two pieces of data are required to represent a proper while loop: the guard and the loop body. 

In a string grammar, such as those we saw in the lecture on concrete syntax, in each production rule we replaced a non-terminal symbol by its right-hand-side, which was some string.  In a tree grammar, each production will replace a non-terminal by a tree.

{% include defn_tree_grammar.liquid %}

For example, abstract syntax trees for arithmetic expressions can be described by the following grammar, with single non-terminal $A$ and where $n$ stands for any integer:

```text
                     |    +    |     -     |    *
    A   ::=     n    |   / \   |    / \    |   / \
                     |  A   A  |   A   A   |  A   A  
```

i.e. there are 4 productions: one replacing A by any integer $n$, one replacing A by a tree rooted at + with children A and A, one replacing A by a tree rooted at - with children A and A, and one replacing A by a tree rooted at * with children A and A.

For example, the arithmetic expression AST from above can be derived and therefore we know that it is a valid tree according to this grammar.
```text
  A  ->    +    ->    +    ->    +    ->    +    ->    +  
          / \        / \        / \        / \        / \
         A   A      3   A      3   *      3   *      3   *
                                  / \        / \        / \
                                 A   A      4   A      4   2
```


## Writing ASTs inline

One disadvantage to the tree representation is that it is rather unwieldy to write on lined paper or in ASCII.

Hence, whenever we introduce some new kinds of AST we will agree some conventions for writing down the trees "inline".  In fact, we will nearly always write the trees this way.  For a start we will use parentheses to describe which subexpressions correspond to subtrees.  So we will write $$3 + (4 * 6)$$ to describe the tree:

```text
      +
     / \
    3   *
       / \
      4   6
```

and we will write $$(3 + (6 * 6)) - 2$$ for the tree:
```text
        -
      /   \
     +     2
   /   \   
  3     *
       / \
      6   6
```

and maybe write $$\mathsf{while}\ (x > 0)\ \mathsf{do}\ (x := x - 1; y := y + 1)$$ for:
```text
          while
        /       \
       >        seq
      / \      /   \
     x   0   assn   assn 
             / \     / \
            x   -   y   + 
               / \     / \
              x   1   y   1
```
(we will be more specific when we introduce While programs later).

But then also we will get pretty tired of writing so many parentheses, so we will agree some conventions that allow us to drop some of them whilst remaining unambiguous.

### Precedence

Syntax conventions tell us how to overcome problems that arise when not fully parenthesizing the inline description of a syntax tree.  The first is precedence, which can be illustrated by the following arithmetic expression which is missing parenthesis:
  
  $$
  3 + 4 * 2
  $$

It is not immediately clear which AST we mean because we did not put in all the necessary parentheses in order to determine the tree structure. We could mean either:

  <script type="text/tikz">
  \begin{tikzpicture}
    \node(0){+}
      child{node{3}}
      child{node{*}
        child{node{4}}
        child{node{2}}
      };
  \end{tikzpicture}
  </script>

corresponding to $3 + (4\*2)$, in other words, that $4\*2$ is the second input of $+$, or we might mean:

  <script type="text/tikz">
  \begin{tikzpicture}
    \node(0){*}
      child{node{+}
        child{node{3}}
        child{node{4}}
      }
      child{node{2}};
  \end{tikzpicture}
  </script>

corresponding to $(3+4) \* 2$, in which $3+4$ is the first argument of $\*$.  Probably you have already come across this problem already at school when doing arithmetic, and there we learn that the convention adopted by most of the world is that:

> multiplication ($\*$) binds *tighter* than addition ($+$)

Another way to say this is that "multiplication has higher precedence than addition".  One way to remember how this works is to think of $3 + 4 * 2$ as describing a battle between the two operators ($+$) and ($\*$), with both of them trying to claim the $4$ as their own -- the $4$ is both in the second argument position of $+$ (i.e. it occurs immediately to the right of $+$) and in the first argument position of $\*$ (i.e. it occurs immediately to the left of ($\*$)).  Who wins this battle?  The operator that binds tightest wins (the operator with the highest precedence, of the two), and, if we agree to the modern convention, this means $*$ wins and the underlying syntax tree is the first of those above.  

Precedence of a set of operators is often described as a partial order induced by a precedence numbering.  Each operator is assigned a natural number which is a representation of its precedence.  If one operator has a higher precedence than another, it is said to bind more tightly.  For our small sample of arithmetic, the standard convention could be represented by the following table:

$$
  \begin{array}{r|l}
    * & 20 \\
    + & 10 \\
    - & 10 \\
  \end{array}
$$

So, since the number 20 is larger than the number 10, it follows that $*$ binds more tightly than $+$.  the particular choice of numbers themselves are typically arbitrary, but it is useful in practice to not use consecutive numbers, because we may later decide to extend our language with a new operator $\oplus$ whose precedence lies strictly between $+$ and $\*$, and then we don't need to change all the numbers in the existing precedence table, we can just insert $\oplus$ at precedence $15$.

### Association

You will notice that (+) and (-) have been assigned the same precedence.   Consequently, precedence on its own does not help to disambiguate all arithmetic expressions.  The expression $5 - 4 + 1$ could represent either of the trees $5 - (4+1)$ or $(5-4) + 1$.  

The same problem arises when we have the same operator twice consecutively in an expression, such as in $5 + 4 + 1$.  In this case, it doesn't matter to the *evaluation* of $5+4+1$ whether we mean the tree $(5+4)+1$ or the tree $5 + (4+1)$, because both denote $10$; but (i) sometimes it does matter, and (ii) whether or not it matters to the evaluation of the expression, it always matters that we can understand which tree is meant, because we are interested in things other than just evaluation.

For this reason, most languages adopt conventions regarding the *association* of operators.  These conventions tell us where the missing parenthesis should go when we write an expression that includes adjacent operators of the same precedence.

If an operator, or a family of operators (of the same precedence) *associate to the left*, then this means that we should disambiguate by placing parentheses to the left.  The usually agreed convention for arithmetic is that all the operators associate to the left:

{% include ex_left_assoc.liquid %}

In fact, left-association is so common, that we will assume that all operators we use in this unit associate to the left, unless we say otherwise.

If an operator, or a family of operators (of the same precedence) *associate to the right*, then this means that we should disambiguate by placing parentheses to the right.  An example of a right associative operator is the function type constructor ->, as found in e.g. Haskell.

{% include ex_right_assoc.liquid %}

To understand why it makes sense for the function type constructor to associate right, consider that all functions in haskell take exactly one input and return exactly one output.  Although we sometimes think of `map` as taking two inputs, namely a function and a list over which to apply the function, technically this is incorrect.  The `map` function takes a single input of type `a -> b` and returns a single output, which happens to itself be a function of type `[a] -> [b]`.   When we partially apply `map` to some function, e.g. `not :: Bool -> Bool`, this is a perfectly good value `map not` and it has type `map not :: [Bool] -> [Bool]`.  Writing the AST for the type of `map` makes it even clearer that it is a function expecting exactly one function as input and returning exactly one function as output:

  <script type="text/tikz">
  \begin{tikzpicture}
    [level distance=12mm,level/.style={sibling distance=32mm/#1}]
    \node(0){$\to$}
      child{node{$\to$}
        child{node{a}}
        child{node{b}}
      }
      child{node{$\to$}
        child{node{[\ ]} child{node{a}}}
        child{node{[\ ]} child{node{b}}}
      };
  \end{tikzpicture}
  </script>

Note, the type of "lists of a", written `[a]`, is properly understood as the list type constructor `[ ]` applied to the argument `a` (the list type constructor is a unary operator on types - it takes a type `T` and input and returns as output the type of lists of `T`).

Sometimes language designers want to force you to be explicit about which tree you mean, and in this case, they will specify that an operator has *no association*.  In this case, you cannot be lazy about putting in the parentheses, because there is no agreed convention for disambiguating missing ones.

## Trees derived inline

With our notation for writing trees inline, we can rewrite the tree grammar above as:

{% include defn_t_grammar_arith.liquid %}

Here are some derivations.  

Deriving the tree $3 + (4 \* 2)$, which by our conventions can be written equally well as $3 + 4 \* 2$:

$$
  \begin{array}{rcl}
    A &\to& A + A \\
      &\to& 3 + A \\ 
      &\to& 3 + A * A \\
      &\to& 3 + 4 * A \\
      &\to& 3 + 4 * 2 
  \end{array}
$$

Deriving the tree $(5 + 4) \* (3 - 2)$:

$$
  \begin{array}{rcl}
    A &\to& A * A \\
      &\to& (A + A) * A \\
      &\to& (A + A) * (A - A)\\
      &\to& (5 + A) * (A - A)\\
      &\to& (5 + 4) * (A - A)\\
      &\to& (5 + 4) * (3 - A)\\
      &\to& (5 + 4) * (3 - 2)
  \end{array}
$$

Deriving the tree $5 + (4 - (1 + 1))$:

$$
  \begin{array}{rcl}
      A &\to& A + A \\
        &\to& 5 + A \\
        &\to& 5 + (A - A) \\
        &\to& 5 + (4 - A) \\
        &\to& 5 + (4 - (A + A)) \\
        &\to& 5 + (4 - (1 + A)) \\
        &\to& 5 + (4 - (1 + 1))
  \end{array}
$$