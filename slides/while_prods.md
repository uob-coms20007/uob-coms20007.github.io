---
layout: math
mathjax: true
parent: "Slides"
title: 13. While Lang
nav_order: 13
---

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
        U &\longrightarrow& A \mid B \mid \cdots{} \mid Z \mid '\\
        M &\longrightarrow& L\ M \mid U\ M \mid \epsilon\\
        V &\longrightarrow& L\ M\\
        N &\longrightarrow& D\ E
      \end{array}
    $$

- Start symbol: $S$
</div>