---
layout: math
mathjax: true
parent: "Slides"
title: 18. Token Grammar
nav_order: 18
---

Part of the While grammar for Statements and Arithmetic Expressions:

$$
\begin{array}{rcl}
  S &\longrightarrow& V \leftarrow A \mid \cdots{}\\
  A &\longrightarrow& V \mid N \mid A + A \mid \cdots{}\\[6mm]
  L &\longrightarrow& a \mid b \mid \cdots{} \mid z \\
  U &\longrightarrow& A \mid B \mid \cdots{} \mid Z \mid '\\
  M &\longrightarrow& L\ M \mid U\ M \mid \epsilon\\
  V &\longrightarrow& L\ M\\
  D &\longrightarrow& 0 \mid 1 \mid \cdots{} \mid 9\\
  E &\longrightarrow& D\ E \mid \epsilon\\
  N &\longrightarrow& D\ E
\end{array}
$$

***

Modified grammar to account for lexing stage:

$$
  \begin{array}{rcl}
  \mathit{S} &\longrightarrow& \mathsf{id}\ \mathsf{assn}\ \mathit{A} \mid \cdots{}\\
  \mathit{A} &\longrightarrow& \mathsf{id} \mid \mathsf{num} \mid \mathit{A}\ \mathsf{plus}\ \mathit{A} \mid \cdots{}
  \end{array}
$$