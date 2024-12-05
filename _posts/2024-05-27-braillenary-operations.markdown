---
layout: post
title:  "Braillenary operations"
date:   2024-05-27 16:47:49 +0200
categories: blogs
mathjax: true
---
{% include script.html %}

<p style="text-align: center;"><i>Can we make valid words by applying the XOR operation to pairs of words in braille?</i></p>

<h3>&#10259;&#10257;&#10247;&#10247;&#10261;</h3>

Braille is a language that has fascinated me for a long time.

$$
\begin{array}{c|c|c|c|c|c|c|c|c|c|c|c|c|c|c|c|c|c|c|c|c|c|c|c|c|c|c}
  & A & B & C & D & E & F & G & H & I & J & K & L & M & N & O & P & Q & R & S & T & U & V & W & X & Y & Z \\
\hline
A &   &   &   &   &   & I & J &   & F & G &   &   &   &   &   & S & T &   & P & Q &   &   &   &   &   &   \\
\hline
B &   &   & I & J &   &   &   &   & C & D &   &   & S & T &   &   &   &   & M & N &   &   &   &   &   &   \\
\hline
C &   & I &   &   &   &   &   & J & B & H &   & S &   &   &   &   &   & T & L & R &   &   &   &   &   &   \\
\hline
D &   & J &   &   &   &   &   & I & H & B &   & T &   &   &   &   &   & S & R & L &   &   &   &   &   &   \\
\hline
E &   &   &   &   &   & J & I &   & G & F &   &   &   &   &   & T & S &   & Q & P &   &   &   &   &   &   \\
\hline
F & I &   &   &   & J &   &   &   & A & E & S &   &   &   & T &   &   &   & K & O &   &   &   &   &   &   \\
\hline
G & J &   &   &   & I &   &   &   & E & A & T &   &   &   & S &   &   &   & O & K &   &   &   &   &   &   \\
\hline
H &   &   & J & I &   &   &   &   & D & C &   &   & T & S &   &   &   &   & N & M &   &   &   &   &   &   \\
\hline
I & F & C & B & H & G & A & E & D &   &   & P & M & L & R & Q & K & O & N &   &   &   & X &   & V &   &   \\
\hline
J & G & D & H & B & F & E & A & C &   &   & Q & N & R & L & P & O & K & M &   &   &   & Y &   &   & V &   \\
\hline
K &   &   &   &   &   & S & T &   & P & Q &   &   &   &   &   & I & J &   & F & G &   &   &   &   &   &   \\
\hline
L &   &   & S & T &   &   &   &   & M & N &   &   & I & J &   &   &   &   & C & D &   &   & Y &   & W &   \\
\hline
M &   & S &   &   &   &   &   & T & L & R &   & I &   &   &   &   &   & J & B & H &   &   &   &   &   &   \\
\hline
N &   & T &   &   &   &   &   & S & R & L &   & J &   &   &   &   &   & I & H & B &   & W & V &   &   &   \\
\hline
O &   &   &   &   &   & T & S &   & Q & P &   &   &   &   &   & J & I &   & G & F &   &   &   &   &   &   \\
\hline
P & S &   &   &   & T &   &   &   & K & O & I &   &   &   & J &   &   &   & A & E &   &   & Z &   &   & W \\
\hline
Q & T &   &   &   & S &   &   &   & O & K & J &   &   &   & I &   &   &   & E & A & W &   & U &   &   &   \\
\hline
R &   &   & T & S &   &   &   &   & N & M &   &   & J & I &   &   &   &   & D & C &   &   & X & W &   &   \\
\hline
S & P & M & L & R & Q & K & O & N &   &   & F & C & B & H & G & A & E & D &   &   &   &   &   &   &   &   \\
\hline
T & Q & N & R & L & P & O & K & M &   &   & G & D & H & B & F & E & A & C &   &   &   &   &   &   &   &   \\
\hline
U &   &   &   &   &   &   &   &   &   &   &   &   &   &   &   &   & W &   &   &   &   &   & Q &   &   &   \\
\hline
V &   &   &   &   &   &   &   &   & X & Y &   &   &   & W &   &   &   &   &   &   &   &   & N & I & J &   \\
\hline
W &   &   &   &   &   &   &   &   &   &   &   & Y &   & V &   & Z & U & X &   &   & Q & N &   & R & L & P \\
\hline
X &   &   &   &   &   &   &   &   & V &   &   &   &   &   &   &   &   & W &   &   &   & I & R &   &   &   \\
\hline
Y &   &   &   &   &   &   &   &   &   & V &   & W &   &   &   &   &   &   &   &   &   & J & L &   &   &   \\
\hline
Z &   &   &   &   &   &   &   &   &   &   &   &   &   &   &   & W &   &   &   &   &   &   & P &   &   &  
\end{array}
$$

ADROP - PINTE - SHIFT
APHIS - PEDRO - STING
ARGIL - PIEND - SNIRT
BIFID - CRAFT - INIAL
TINGI - ENROL - PRISM




