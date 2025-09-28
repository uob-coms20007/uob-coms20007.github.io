---
layout: math
mathjax: true
parent: "Slides"
title: 7. Linguistics
nav_order: 7
---

$$
  \begin{array}{rcl}
    \bnfnt{SENTENCE} &\longrightarrow& \bnfnt{NOUN-PHRASE}\ \bnfnt{VERB-PHRASE}\\
    \bnfnt{NOUN-PHRASE} &\longrightarrow& \bnfnt{CMPLX-NOUN} \mid \bnfnt{CMPLX-NOUN}\ \bnfnt{PREP-PHRASE}\\
    \bnfnt{VERB-PHRASE} &\longrightarrow&  \bnfnt{CMPLX-VERB} \mid \bnfnt{CMPLX-VERB}\ \bnfnt{PREP-PHRASE}\\
    \bnfnt{PREP-PHRASE} &\longrightarrow& \bnfnt{PREP}\ \bnfnt{CMPLX-NOUN}\\
    \bnfnt{CMPLX-NOUN} &\longrightarrow& \bnfnt{ARTICLE}\ \bnfnt{NOUN}\\
    \bnfnt{CMPLX-VERB} &\longrightarrow& \bnfnt{VERB} \mid \bnfnt{VERB}\ \bnfnt{NOUN-PHRASE}\\
    \bnfnt{ARTICLE} &\longrightarrow& \textrm{a} \mid \textrm{the} \\
    \bnfnt{NOUN} &\longrightarrow& \textrm{boy} \mid \textrm{girl} \mid \textrm{flower}\\
    \bnfnt{VERB} &\longrightarrow& \textrm{touches} \mid \textrm{likes} \mid \textrm{sees}\\
    \bnfnt{PREP} &\longrightarrow& \textrm{with}
  \end{array} 
$$