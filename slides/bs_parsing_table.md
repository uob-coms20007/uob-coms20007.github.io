---
layout: math
mathjax: true
parent: "Slides"
title: 36. Brischeme Table
nav_order: 36
---

<table class="pure-table pure-table-bordered" border="1">
  <thead>
    <tr id="llTableHead"><th></th><th>eof</th><th>(</th><th>)</th><th>define</th><th>ident</th><th>literal</th><th>lambda</th><th>primop</th></tr></thead>
  <tbody id="llTableRows"><tr></tr><tr><td nowrap="nowrap">Prog</td><td nowrap="nowrap">Prog ::= eof</td><td nowrap="nowrap">Prog ::= Form Prog</td><td nowrap="nowrap"></td><td nowrap="nowrap"></td><td nowrap="nowrap">Prog ::= Form Prog</td><td nowrap="nowrap">Prog ::= Form Prog</td><td nowrap="nowrap"></td><td nowrap="nowrap"></td></tr><tr></tr><tr><td nowrap="nowrap">Form</td><td nowrap="nowrap"></td><td nowrap="nowrap">Form ::= ( CForm )</td><td nowrap="nowrap"></td><td nowrap="nowrap"></td><td nowrap="nowrap">Form ::= Atom</td><td nowrap="nowrap">Form ::= Atom</td><td nowrap="nowrap"></td><td nowrap="nowrap"></td></tr><tr></tr><tr><td nowrap="nowrap">CForm</td><td nowrap="nowrap"></td><td nowrap="nowrap"></td><td nowrap="nowrap"></td><td nowrap="nowrap">CForm ::= define ident SExpr</td><td nowrap="nowrap"></td><td nowrap="nowrap"></td><td nowrap="nowrap">CForm ::= Expr</td><td nowrap="nowrap">CForm ::= Expr</td></tr><tr></tr><tr><td nowrap="nowrap">Atom</td><td nowrap="nowrap"></td><td nowrap="nowrap"></td><td nowrap="nowrap"></td><td nowrap="nowrap"></td><td nowrap="nowrap">Atom ::= ident</td><td nowrap="nowrap">Atom ::= literal</td><td nowrap="nowrap"></td><td nowrap="nowrap"></td></tr><tr></tr><tr><td nowrap="nowrap">IdentList</td><td nowrap="nowrap"></td><td nowrap="nowrap"></td><td nowrap="nowrap">IdentList ::= ε</td><td nowrap="nowrap"></td><td nowrap="nowrap">IdentList ::= ident IdentList</td><td nowrap="nowrap"></td><td nowrap="nowrap"></td><td nowrap="nowrap"></td></tr><tr></tr><tr><td nowrap="nowrap">SExpr</td><td nowrap="nowrap"></td><td nowrap="nowrap">SExpr ::= ( Expr )</td><td nowrap="nowrap"></td><td nowrap="nowrap"></td><td nowrap="nowrap">SExpr ::= Atom</td><td nowrap="nowrap">SExpr ::= Atom</td><td nowrap="nowrap"></td><td nowrap="nowrap"></td></tr><tr></tr><tr><td nowrap="nowrap">SExprList</td><td nowrap="nowrap"></td><td nowrap="nowrap">SExprList ::= SExpr SExprList</td><td nowrap="nowrap">SExprList ::= ε</td><td nowrap="nowrap"></td><td nowrap="nowrap">SExprList ::= SExpr SExprList</td><td nowrap="nowrap">SExprList ::= SExpr SExprList</td><td nowrap="nowrap"></td><td nowrap="nowrap"></td></tr><tr></tr><tr><td nowrap="nowrap">Expr</td><td nowrap="nowrap"></td><td nowrap="nowrap"></td><td nowrap="nowrap"></td><td nowrap="nowrap"></td><td nowrap="nowrap"></td><td nowrap="nowrap"></td><td nowrap="nowrap">Expr ::= lambda ( IdentList ) SExpr</td><td nowrap="nowrap">Expr ::= primop SExprList</td></tr>
  </tbody>
</table>

