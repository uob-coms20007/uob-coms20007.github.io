---
layout: math
parent: Transition Systems
mathjax: true
title: Examples
---

# Examples

## Traffic Light

The standard UK traffic light sequence is: red; red and amber together; green; amber and then red again.

The __traffic light example__ transition system consists of the following data:
  * The set of configurations is \{ [`⬤`{: style="color:red"} `⬤` `⬤`], [`⬤`{: style="color:red"} `⬤`{: style="color:orange"} `⬤`], [`⬤` `⬤` `⬤`{: style="color:green"}], [`⬤` `⬤`{: style="color:orange"} `⬤`]  \}
  * The transition relation is given by the following listing of transitions:

    {: style="list-style-type:none; display:list-item" }
    - [`⬤`{: style="color:red"} `⬤` `⬤`] $\tr$ [`⬤`{: style="color:red"} `⬤`{: style="color:orange"} `⬤`]
    - [`⬤`{: style="color:red"} `⬤`{: style="color:orange"} `⬤`] $\tr$ [`⬤` `⬤` `⬤`{: style="color:green"}]
    - [`⬤` `⬤` `⬤`{: style="color:green"}] $\tr$ [`⬤` `⬤`{: style="color:orange"} `⬤`]
    - [`⬤` `⬤`{: style="color:orange"} `⬤`] $\tr$ [`⬤`{: style="color:red"} `⬤` `⬤`]

## Circuit

The following circuit implements a 2-bit counter with reset line and an output.

![circuit](../assets/circuit.svg)

The __circuit example__ transition system consists of the following data:
  * The configurations are all 4-tuples of bits $(x,q_0,q_1,y)$.
  * The transition relation $(x,q_0,q_1,y) \tr (x',q_0',q_1',y')$ can be tabulated as follows:

    $$
      \begin{array}{|cccc|cccc|}
      \hline
      x & q_1 & q_0 & y & x' & q_1' & q_0' & y'\\
      \hline
      0 & 0 & 0 & \_ & \_ & 0 & 1 & 0 \\
      0 & 0 & 1 & \_ & \_ & 1 & 0 & 0 \\
      0 & 1 & 0 & \_ & \_ & 1 & 1 & 0 \\
      1 & 0 & 0 & \_ & \_ & 0 & 0 & 0 \\
      1 & 0 & 1 & \_ & \_ & 0 & 0 & 0 \\
      1 & 1 & 0 & \_ & \_ & 0 & 0 & 0 \\
      \_ & 1 & 1 & \_ & \_ & 0 & 0 & 1 \\
      \hline
      \end{array}
    $$

    Here, for brevity, we use $\\_$ to indicate that the entry can be $0$ or $1$, so the first row above should be read as shorthand for the following four rows:

    $$
    \begin{array}{|cccc|cccc|}
      0 & 0 & 0 & 0 & 0 & 0 & 1 & 0 \\
      0 & 0 & 0 & 0 & 1 & 0 & 1 & 0 \\
      0 & 0 & 0 & 1 & 0 & 0 & 1 & 0 \\
      0 & 0 & 0 & 1 & 1 & 0 & 1 & 0 \\
    \end{array}
    $$

## While Program

Consider the following *While* program:

```
  x := x ٭ x + 1
  while ¬(x = 0) do
      x := x - 2
      y := 2 ٭ y - 1
  print (100 / (y - x))
```

The __while program example__ transition system consists of the following data:
  * The configurations are the set $\\{1,2,3,4,5\\} \times \mathbb{Z} \times \mathbb{Z}$.
  * The transition relation has $(\mathit{pc}, x, y) \tr (\mathit{pc}', x', y')$ just if any of the following is true:
      - $\mathit{pc} = 1$, $\mathit{pc}'=2$, $x'=x*x+1$ and $y'=y$
      - $\mathit{pc} = 2$, $x=0$, $\mathit{pc}'=5$, $x'=x$ and $y'=y$
      - $\mathit{pc} = 2$, $x \neq 0$, $\mathit{pc}'=3$, $x'=x$ and $y'=y$
      - $\mathit{pc} = 3$, $\mathit{pc}'=4$, $x' = x-2$ and $y'=y$
      - $\mathit{pc} = 4$, $\mathit{pc}'=2$, $x' = x$ and $y' = 2 *y-1$


## Chameleons

An island is inhabited by red, blue and green chameleons.  Whenever two chameleons of different colours meet, they both change colour to become the third colour.  For example, when a red and blue chameleon meet, they both turn green; when a blue and a green meet, they both turn red.

The __chameleons example__ transition system consists of the following data:
  * The configurations consist of all possible triples $(r,b,g)$ of natural numbers.
  * The transition relation has $(r, b, g) \tr (r',b',g')$ just if any one of the following is true:
      - $r' = r+2$ and $b' = b-1$ and $g' = g-1$
      - $r' = r-1$ and $b' = b+2$ and $g' = g-1$
      - $r' = r-1$ and $b' = b-1$ and $g' = g+2$
