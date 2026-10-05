---
layout: math
mathjax: true
parent: "Slides"
title: 36. Parsing Table
nav_order: 36
---

<table class="pure-table pure-table-bordered" border="1">
  <thead><tr id="llTableHead"><th>Nonterminal</th><th>def</th><th>ident</th><th>(</th><th>)</th><th>=</th><th>var</th><th>,</th><th>$</th></tr></thead>
  <tbody id="llTableRows"><tr></tr>
  <tr><td nowrap="nowrap">Cmd</td><td nowrap="nowrap">Cmd ::= def ident ( ExpList ) = Exp</td><td nowrap="nowrap">Cmd ::= Exp</td><td nowrap="nowrap"></td><td nowrap="nowrap"></td><td nowrap="nowrap"></td><td nowrap="nowrap">Cmd ::= Exp</td><td nowrap="nowrap"></td><td nowrap="nowrap">Cmd ::= ''</td></tr>
  <tr><td nowrap="nowrap">Exp</td><td nowrap="nowrap"></td><td nowrap="nowrap">Exp ::= ident ( ExpList )</td><td nowrap="nowrap"></td><td nowrap="nowrap"></td><td nowrap="nowrap"></td><td nowrap="nowrap">Exp ::= var</td><td nowrap="nowrap"></td><td nowrap="nowrap"></td></tr>
  <tr><td nowrap="nowrap">ExpList</td><td nowrap="nowrap"></td><td nowrap="nowrap">ExpList ::= Exp ExpList'</td><td nowrap="nowrap"></td><td nowrap="nowrap">ExpList ::= ''</td><td nowrap="nowrap"></td><td nowrap="nowrap">ExpList ::= Exp ExpList'</td><td nowrap="nowrap"></td><td nowrap="nowrap"></td></tr>
  <tr><td nowrap="nowrap">ExpList'</td><td nowrap="nowrap"></td><td nowrap="nowrap"></td><td nowrap="nowrap"></td><td nowrap="nowrap">ExpList' ::= ''</td><td nowrap="nowrap"></td><td nowrap="nowrap"></td><td nowrap="nowrap">ExpList' ::= , Exp ExpList'</td><td nowrap="nowrap"></td></tr></tbody>
</table>
