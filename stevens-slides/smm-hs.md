---
layout: math
mathjax: true
parent: "Steven's Slides"
title: SMM Program
nav_order: 30
---

~~~ haskell
digits = [0..9]

sendMoreMoney =
  do s <- digits
     e <- digits \\ [s]
     n <- digits \\ [s,e]
     d <- digits \\ [s,e,n]
     m <- digits \\ [s,e,n,d]
     o <- digits \\ [s,e,n,d,m]
     r <- digits \\ [s,e,n,d,m,o]
     y <- digits \\ [s,e,n,d,m,o,r]
     guard (y == (d + e) `mod` 10)
     let c1 = (d + e) `quot` 10
     guard (e == (n + r + c1) `mod` 10)
     let c2 = (n + r + c1) `quot` 10
     guard (n == (e + o + c2) `mod` 10)
     let c3 = (e + o + c2) `quot` 10
     guard (o == (s + m + c3) `mod` 10)
     guard (m == (s + m + c3) `quot` 10)
     return (s,e,n,d,m,o,r,e,m,o,n,e,y)
~~~