---
layout: math
title: Context-Free Grammars
nav_order: 1
mathjax: true
parent: Context-Free Grammars
---

# Context-Free Grammars

A context-free grammar over a set of non-terminals $N$ and a set of terminals $T$ is a _start symbol_ $S \in N$, and a finite set of _production rules_ in $N \times (N \cup T)^*$. (Note that the in fact define a relation.)
We write production rules as $X \rightarrow \gamma$ instead of $(X, \gamma)$.

A context-free grammar $\mathcal{G}$ defines a language $L(\mathcal{G}) \subseteq T^*$ (that is sentences in the language are possibly empty sequences of terminals). The class of languages defined by context-free grammars is the class of _context-free languages_. They are recognised by pushdown automata, which we won't discuss in this unit.

## Parse Trees

In order to check whether a word in $T^*$ is in the language $L(\mathcal{G})$ described by some context-free grammar $\mathcal{G}$, we check whether there exists a _derivation_ of it in the grammar by constructing a _parse tree_.

A parse tree for some grammar $\mathcal{G} = (S, \{ p_1, \ldots, p_n \})$ is a tree whose root is the start symbol $S$, internal nodes are non-terminals in $N$, leaves are terminals in $T$, and for any internal node $X \in N$ with children $n_1, \ldots, n_k$ (for $k \in \mathbb{N}$, if $k = 0$, the internal node is said to have a single child labelled $\epsilon$), there must exist a rule $p_i$ (for some $i \in \{1, \ldots, n\}$) such that $p_i = X \rightarrow n_1\ \ldots n_k$ (or $p_i = X \rightarrow \epsilon$, if $k = 0$).

# Ambiguous Grammars

A grammar is ambiguous if there exists a sentence that has more than one derivation in the grammar.
