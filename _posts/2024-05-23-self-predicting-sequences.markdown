---
layout: post
title:  "Self-predicting sequences"
date:   2024-05-23 16:47:49 +0200
categories: blogs
mathjax: true
---
{% include script.html %}


<h3>Constructing the sequence</h3>
Let's start with an empty table:

&nbsp;

$$
\begin{array}{c|c|c|c|c|c|c|c|c|c|c|c|c|c|c|c|c|c}
n & 0 & 1 & 2 & 3 & 4 & 5 & 6 & 7 & 8 & 9 & 10 & 11 & 12 & 13 & 14 & 15 & \cdots\\
\hline
Q_n & . & . & . & . & . & . & . & . & . & . & . & . & . & . & . & . &
\end{array}
$$

&nbsp;

a

&nbsp;

$$
\begin{array}{c|c|c|c|c|c|c|c|c|c|c|c|c|c|c|c|c|c}
n & 0 & 1 & 2 & 3 & 4 & 5 & 6 & 7 & 8 & 9 & 10 & 11 & 12 & 13 & 14 & 15 & \cdots\\
\hline
Q_n & 0 & . & . & . & . & . & . & . & . & . & . & . & . & . & . & . &
\end{array}
$$

&nbsp;

What could \\( Q_1 \\) be? It surely cannot be a \\( 0 \\), since this would mean that in zero steps we were to encounter a \\( 1 \\). Let's try \\( Q_1 = 1 \\). This quickly escalates into a range of numbers we can fill in:

&nbsp;

$$
\begin{array}{c|c|c|c|c|c|c|c|c|c|c|c|c|c|c|c|c|c}
n & 0 & 1 & 2 & 3 & 4 & 5 & 6 & 7 & 8 & 9 & 10 & 11 & 12 & 13 & 14 & 15 & \cdots\\
\hline
Q_n & 0 & 1 & 1 & 2 & . & 3 & . & . & 5 & . & . & . & . & 8 & . & . &
\end{array}
$$

&nbsp;

It might be good to take a moment to follow these steps through, one by one. The \\( 1 \\) we entered says:
<p style="text-align: center;">"after \( 1 \) step, we will see a \( 1 \)",</p>
and indeed, we encounter a \\( 1 \\) in position \\( n=2 \\). This in turn tells us:
<p style="text-align: center;">"after \( 1 \) step, we will see a \( 2 \)",</p>
which is also true; we find a \\( 2 \\) in position \\( n=3 \\). The next instruction will then be:
<p style="text-align: center;">"after \( 2 \) steps, we will see a \( 3 \)".</p>
The reader is invited (and encouraged!) to verify the other entries in the list.

&nbsp;

An interesting pattern emerges, which some readers will recognize as the Fibonacci sequence. This sequence, commonly denoted \\( F_n \\), starts with the numbers \\( 0, 1, 1, 2, 3, 5, 8, 13, 21, 34, \cdots \\). Its defining property is that each term is the sum of the two terms prior to it: \\( F_n = F_{n-1} + F_{n-2} \\) (for \\( n \geq 2 \\)).

To see why we encounter these numbers in our self-predicting sequence \\( Q_n \\), it might help to look at its own defining relation. \\( Q_n \\) tells us after how many steps we will see an \\( n \\) in the sequence. In other words, at position \\( Q_n + n \\), the sequence shows the number \\( n \\) itself. Adding these two will bring us to the next position \\( (Q_n + n) + n\\), where we see the number \\( Q_n + n \\). Notice that we keep adding the last two numbers to arrive at the new position and simply carry the last number along. This is the exact same principle as the Fibonacci sequence. Since we put \\( Q_1=1 \\), we start out similar to the Fibonacci terms \\( 1,1,2,3,\cdots \\) and cause the rest of the sequence to unroll.

Let's look at the next blank entry in the table: \\( n=4 \\). Of course, we cannot fill in any number that points us to a position that is already occupied by another number. This means we cannot enter a \\( 1 \\) or a \\( 4 \\), because the positions \\( n=5 \\) and \\( n=8 \\) are already filled in. However, the numbers \\( 2 \\) and \\( 3 \\) seem valid, and we will try entering a \\( 2 \\) to see what happens. It's not a surprise that we again uncover a whole sequence of terms:

&nbsp;

$$
\begin{array}{c|c|c|c|c|c|c|c|c|c|c|c|c|c|c|c|c|c}
n & 0 & 1 & 2 & 3 & 4 & 5 & 6 & 7 & 8 & 9 & 10 & 11 & 12 & 13 & 14 & 15 & \cdots\\
\hline
Q_n & 0 & 1 & 1 & 2 & 2 & 3 & 4 & . & 5 & . & 6 & . & . & 8 & . & . &
\end{array}
$$


$$
\begin{equation}
  Q_n = \left\lfloor \frac{n}{\phi}+\frac{1}{2} \right\rfloor
\end{equation}
$$

