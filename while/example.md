---
layout: math
title: An example While program
nav_order: 3
mathjax: true
parent: The While Language
---

# Factorial in Concrete Syntax

Our brain and fingers write the following sequences of octets (displayed in a form readable by puny humans).

```
r := 1
while (1 <= n) {
  r := r * n
  n := n - 1
}
```
# Factorial as a token stream

The lexer and its FSA turns it into the following sequence of tokens (see the code for the lexer for the regular expressions that match the tokens).

```haskell
[TId "r", TAssign, TInt 1, TWhile, TLParen, TInt 1, TLe, TId "n", TRParen, TLBrace, TId "r", TAssign, TId "r", TStar, TId "n", TId "n", TAssign, Tid "n", TMinus, TInt 1]
```

# Factorial as an Abstract Syntax Tree

Our recursive descent parser recognises the token string as a sentence in the concrete grammar's language _and_ takes action on recognising the most important literals. In our case, we build an Abstract Syntax Tree that represents the factorial program in a structured way. It looks (with somewhat better handwriting) like [this](https://uob-coms20007.github.io/reference/assets/factorial-ast.pdf).

Note that our parser is slightly lazy when parsing sequences of instructions, and always uses ```SSkip``` as a base for lists of instructions. We could instead have put more cleverness in the action that we took when parsing a statement using the grammar rule $S \rightarrow I\ S$, so that instead of always producing ```SSeq (parseI) (parseS)``` we'd only used ```SSeq``` if the second recursive call returns a non-empty tree. Small savings when parsing might turn out to be large savings while making multiple passes over the AST during compilation.
