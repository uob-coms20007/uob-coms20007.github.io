---
layout: math
mathjax: true
parent: "Slides"
title: 35. Brischeme Grammar
nav_order: 35
---

$$
    \begin{array}{rcl}
      \nt{Prog} &\Coloneqq& \nt{Form}\ \nt{Prog} \mid \tm{eof} \\[4mm]
      \nt{Form} &\Coloneqq& \nt{Atom}\\
      \nt{Form} &\mid& \tm{(}\ \nt{CForm}\ \tm{)}\\[4mm]
      \nt{CForm} &\Coloneqq& \nt{Expr}\\
      \nt{CForm} &\mid& \tm{define}\ \tm{ident}\ \nt{SExpr}\\[4mm]
      \nt{Atom} &\Coloneqq& \tm{literal} \mid \tm{ident}\\[4mm]
      \nt{IdentList} &\Coloneqq& \tm{ident}\ \nt{IdentList} \mid \epsilon\\[4mm]
      \nt{SExpr} &\Coloneqq& \nt{Atom}\\  
      \nt{SExpr} &\mid& \tm{(}\ \nt{Expr}\ \tm{)}\\[4mm]
      \nt{SExprList} &\Coloneqq& \nt{SExpr}\ \nt{SExprList} \mid \epsilon\\[4mm]
      \nt{Expr} &\Coloneqq& \tm{lambda}\ \tm{(}\ \tm{IdentList}\ \tm{)}\ \nt{SExpr}\\
      \nt{Expr} &\mid& \tm{primop}\ \nt{SExprList}\\
      \nt{Expr} &\mid& \nt{SExpr}\ \nt{SExprList}\\
    \end{array}
$$