---
layout: obs
title: Strings
mathjax: true
nav_order: 1
parent: Prerequesites
grand_parent: Part I
---

# Strings

An __alphabet__ is any finite set, whose members are called __symbols__ (equivalently: __letters__ or __characters__).  We typically use $\Sigma$ to denote a generic alphabet and $a,b,c,d$ to denote its letters.

A __string__ (equivalently: __word__) over an alphabet $\Sigma$ is a finite sequence of characters from $\Sigma$.  The sequence may be empty, and we write the __empty string__ as $\epsilon$.  We typically use $u,v,w,x,y,z$ to denote a generic string.

The set of all strings over the alphabet $\Sigma$ is written $\Sigma^*$.

We will refer to a set of strings as a __language__.

The __length__ of a string $w$, written $\|w\|$, is just the number of characters in the string.  That is, if $x = a_1\cdots{}a_k$ then $\|x\| = k$.

A string $w$ is said to be a __substring__ of a string $v$ just if $w$ appears consecutively in $v$.

Given strings $x$ and $y$, we write $xy$ for the string obtained by __concatenating__ $y$ to the end of $x$.  That is, if $x = x_1\cdots{}x_k$ and $y = y_1 \cdots{} y_m$ then $xy = x_1\cdots{}x_k y_1 \cdots{} y_m$.

We write $w^k$ for the __$k$-fold concatenation__ of $w$ with itself, i.e. the word $\underbrace{ww\cdots{}w}_{\text{$k$-times}}$.
