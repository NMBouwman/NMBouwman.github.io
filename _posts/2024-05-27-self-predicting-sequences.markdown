---
layout: post
title:  "Self-predicting sequences"
date:   2024-05-27 16:47:49 +0200
categories: blogs
mathjax: true
---
{% include script.html %}

<p style="text-align: center;"><i>Is there a number sequence that predicts at the \( n \)-th term in how many steps it will show an \( n \)?</i></p>

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

We will first look at \\( Q_0 \\). Recall the idea for the sequence: \\( Q_n \\) describes after how many steps we see the number \\( n \\) in the sequence. So after \\( Q_0 \\) steps, we should find a \\( 0 \\). Let's think about this for a second; if \\( Q_n=0 \\) anywhere further up in the sequence, it tells us that after <i>zero</i> steps we should see the number \\( n \\). But taking zero steps means staying in the same spot! We cannot have both a \\( 0 \\) and an \\( n \\) in the same position. Therefore \\( n \\) has no choice but to equal \\( 0 \\). Turning the argument around, \\( Q_0 \\) needs to be \\( 0 \\) because if it were anything else, it would place an impossible \\( 0 \\) somewhere else in the table!

If this line of reasoning is at all confusing, please feel invited to place any number at \\( Q_0 \\) and follow the gimmick of the sequence that after \\( Q_n \\) steps we find the number \\( n \\).

&nbsp;

$$
\begin{array}{c|c|c|c|c|c|c|c|c|c|c|c|c|c|c|c|c|c}
n & 0 & 1 & 2 & 3 & 4 & 5 & 6 & 7 & 8 & 9 & 10 & 11 & 12 & 13 & 14 & 15 & \cdots\\
\hline
Q_n & 0 & . & . & . & . & . & . & . & . & . & . & . & . & . & . & . &
\end{array}
$$

&nbsp;

Now that \\( Q_0 \\) is in place, what could \\( Q_1 \\) be? It surely cannot be a \\( 0 \\) due to the contradiction we saw above. Let's try \\( Q_1 = 1 \\). This quickly escalates into a whole list of numbers we can fill in:

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

An interesting pattern emerges, which some readers will recognise as the Fibonacci sequence. This sequence, commonly denoted \\( F_n \\), starts with the numbers \\( 0, 1, 1, 2, 3, 5, 8, 13, 21, 34, \cdots \\). Its defining property is that each term is the sum of the two terms prior to it: \\( F_n = F_{n-1} + F_{n-2} \\) (for \\( n \geq 2 \\)).

To see why we encounter these numbers in our self-predicting sequence \\( Q_n \\), it might help to look at its own defining relation. \\( Q_n \\) tells us after how many steps we will see an \\( n \\) in the sequence. In other words, at position \\( n + Q_n \\), the sequence shows the number \\( n \\) itself. Adding these two will bring us to the next position \\( (n + Q_n) + n\\), where we see the number \\( n + Q_n \\). Notice that we keep adding the last two numbers to arrive at the new position and simply carry the last number along. This is the exact same principle as the Fibonacci sequence. Since we put \\( Q_1=1 \\), we start out similar to the Fibonacci terms \\( 1,1,2,3,\cdots \\) and cause the rest of the sequence to unroll.

Let's look at the next blank entry in the table: \\( n=4 \\). Of course, we cannot fill in any number that points us to a position that is already occupied by another number. This means we cannot enter a \\( 1 \\) or a \\( 4 \\), because the positions \\( n=5 \\) and \\( n=8 \\) are already filled in. However, the numbers \\( 2 \\) and \\( 3 \\) seem valid, and we will try entering a \\( 2 \\) to see what happens. It's not a surprise that we again uncover a whole Fibonacci-esque subsequence of terms:

&nbsp;

$$
\begin{array}{c|c|c|c|c|c|c|c|c|c|c|c|c|c|c|c|c|c}
n & 0 & 1 & 2 & 3 & 4 & 5 & 6 & 7 & 8 & 9 & 10 & 11 & 12 & 13 & 14 & 15 & \cdots\\
\hline
Q_n & 0 & 1 & 1 & 2 & 2 & 3 & 4 & . & 5 & . & 6 & . & . & 8 & . & . &
\end{array}
$$

&nbsp;

So far, so good. However, can we be sure that the two subsequences never overlap? After all, our table only extends up to \\( n=15 \\) but no further. Luckily, we can prove that these sequences stay out of each other's ways, under one condition we'll get to in a second.

&nbsp;

Let's consider a position \\( n \\) that lies between some \\( m \\) and the next term of its sequence \\( m + Q_m \\):

$$
\begin{equation}
  m < n < m + Q_m.
\end{equation}
$$

If we choose for \\( Q_n \\) a number such that \\( Q_m \leq Q_n \leq m \\), then we can add these inequalities to get

$$
\begin{equation}
  m+Q_m < n+Q_n < (m+Q_m) + m,
\end{equation}
$$

which tells us that the position of the next term of the \\(n\\)-sequence falls neatly between those of the \\(m\\)-sequence. The numbers we carry over correspond to the initial positions \\(m\\), \\(n\\) and \\(m+Q_m\\) and are also strictly different. Therefore, the following term of the \\(n\\)-sequence will also lie between those of the \\(m\\)-sequence:

$$
\begin{equation}
  (m+Q_m) + m < (n+Q_n) + n < \left((m+Q_m) + m\right) + (m + Q_m).
\end{equation}
$$

This pattern repeats indefinitely, so the subsequences will never collide.

There's a subtle but very important choice we made here, which is that if at some position \\( n \\) we are allowed to pick a value \\( Q_n \\), we should choose a number that either lies between its neighbours or equals one of them.  We did exactly this with \\( n=4 \\), where the term \\( Q_4=2 \\) equals its lower neighbour \\( Q_3=2 \\). Following the reasoning above, the subsequences will not coincide anywhere.

&nbsp;

Now that we understand how \\( Q_n \\) behaves, and with one rule in our pocket which we can apply to make sure none of the subsequences overlap, we are free to fill in the rest of the table. The options for \\( n=7 \\) are bounded by its neighbouring terms, so we can choose either a \\( 4 \\) or a \\( 5 \\). In fact, let's always go for the lowest allowed number in an attempt to find the "lowest" self-predicting sequence. In this case, that means \\( Q_7=4 \\). More generally: each time we have a free choice, we can simply let \\( Q_n \\) equal \\( Q_{n-1} \\):

&nbsp;

$$
\begin{array}{c|c|c|c|c|c|c|c|c|c|c|c|c|c|c|c|c|c}
n & 0 & 1 & 2 & 3 & 4 & 5 & 6 & 7 & 8 & 9 & 10 & 11 & 12 & 13 & 14 & 15 & \cdots\\
\hline
Q_n & 0 & 1 & 1 & 2 & 2 & 3 & 4 & 4 & 5 & 5 & 6 & 7 & 7 & 8 & 9 & 9 &
\end{array}
$$

&nbsp;

And there we go! We have constructed a sequence with our desired property by following some simple rules. Of course, we made a few arbitrary choices in our construction process. Can we do better? Let's see if we can find a formula for our sequence.

<h3>From sequence to formula</h3>


$$
\begin{equation}
  Q_n = \left\lfloor \frac{n}{\phi}+\frac{1}{2} \right\rfloor
\end{equation}
$$
