---
layout: math
mathjax: true
parent: "Slides"
title: 9. A C Statement
nav_order: 9
---

$$
  \begin{align*}
    & \textit{statement}\\
    &\to \textit{selection-statement} \\
    &\to \textbf{switch (}\textit{expression}\ \textbf{)}\ \textit{statement}\\
    &\;\;\vdots{}\\
    &\to \textbf{switch (}x\textbf{)}\ \textit{statement}\\
    &\to \textbf{switch (}x\textbf{)}\ \textbf{if (}\textit{expression}\textbf{)}\ \textit{statement}\ \textbf{else}\ \textit{statement}\\
    &\;\;\vdots{}\\
    &\to \textbf{switch (}x\textbf{)}\ \textbf{if (}0\textbf{)}\ \textit{statement}\ \textbf{else}\ \textit{statement}\\
    &\to \textbf{switch (}x\textbf{)}\ \textbf{if (}0\textbf{)}\ \textit{labeled-statement}\ \textbf{else}\ \textit{statement}\\
    &\to \textbf{switch (}x\textbf{)}\ \textbf{if (}0\textbf{)}\ \textbf{case}\ \textit{constant-expression}:\ \textit{statement}\ \textbf{else}\ \textit{statement}\\
    &\to \textbf{switch (}x\textbf{)}\ \textbf{if (}0\textbf{)}\ \textbf{case}\ 0:\ \textit{statement}\ \textbf{else}\ \textit{statement}\\
    &\to \textbf{switch (}x\textbf{)}\ \textbf{if (}0\textbf{)}\ \textbf{case}\ 0:\ \textit{statement}\ \textbf{else}\ \textit{labeled-statement}\\
    &\to \textbf{switch (}x\textbf{)}\ \textbf{if (}0\textbf{)}\ \textbf{case}\ 0:\ \textit{statement}\ \textbf{else}\ \textbf{case}\ \textit{constant-expression}:\ \textit{statement}\\
    &\to \textbf{switch (}x\textbf{)}\ \textbf{if (}0\textbf{)}\ \textbf{case}\ 0:\ \textit{statement}\ \textbf{else}\ \textbf{case}\ 1:\ \textit{statement}
  \end{align*}
$$
