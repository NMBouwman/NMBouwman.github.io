---
layout: post
title:  "Self-predicting integer sequences"
date:   2025-02-25 16:47:49 +0200
categories: blogs
mathjax: true
published: false
---
{% include script.html %}


Whenever I meet up with a certain good friend of mine, we usually have something mathematical to talk about. A little mathematical microfixation, if you will. Around two years ago, we were looking into the properties of some fun number sequences.

On a late train ride back home, I was trying to think of interesting ideas for integer sequences. I enjoyed the idea of a sequence that in a way describes itself. My first thought was to make a sequence that at each position $$n$$ gives the amount of times the number $$n$$ appears in the sequence. However, I realized that such a sequence would run into issues soon enough: if the value at a large index $$N$$ is another large number $$M$$, then the number $$N$$ would appear $$M$$ times, which in turn means all the positions with a value $$N$$ would have to appear $$N$$ times themselves! This already occupies $$N \cdot M$$ places in the sequences, which is far more than the amount of available places up to position $$N$$. (For an infinite sequence we might get away with pushing numbers further and further ahead, but this does not feel like a clean method to construct a sequence.) If instead we fix most large positions of the sequence to a small number like $$0$$ or $$1$$, then these would appear an infinite amount of times; also not a great option. Moreover, I realized I'd previously encountered a similar idea regarding self-describing numbers instead of sequences, where the consecutive digits of the number describe the amounts of $$0$$s, $$1$$s, $$2$$s etc. An example is the number $$1210$$, which contains one $$0$$, two $$1$$s, one $$2$$ and no $$3$$s.
<p style="text-align: center;"><i>There are exactly seven such self-describing numbers. Can you find a pattern to construct them? What does this say about the first digit of an infinite self-describing sequence?</i></p>

My second idea proved to be more interesting: is there an integer sequence that describes at position $$n$$ in <em>how many steps</em> we encounter the number $$n$$? We could call this a self-predicting sequence, since it "predicts" when we will see a given number. A couple of ambiguities arise here: can we repeat numbers? Can the sequence refer to any position where a certain number appears, or does it describe the amount of steps until the very next occurence? I will address and make suitable choices for these questions while we build the sequence from the ground up.

<br>

<h3>Constructing the sequence</h3>
Let's start with an empty table:

<br>

<!--$$
\begin{array}{c|c|c|c|c|c|c|c|c|c|c|c|c|c|c|c|c|c}
n & 0 & 1 & 2 & 3 & 4 & 5 & 6 & 7 & 8 & 9 & 10 & 11 & 12 & 13 & 14 & 15 & \cdots\\
\hline
Q_n & . & . & . & . & . & . & . & . & . & . & . & . & . & . & . & . &
\end{array}
$$-->

<img title="Empty table for the self-predicting sequence." alt="" src="/images/self_predicting_seq_1.png">

<br>

This table will contain the first $$15$$ elements of our self-predicting sequence. The top row shows the position $$n$$ and the bottom row shows the corresponding element in the sequence, which I will call $$Q_n$$.

To start, we can consider $$Q_0$$. This number should describe after how many steps we see a $$0$$ in the sequence. Let's think about what happens when a $$0$$ appears in the sequence: if at position $$n$$ we see $$Q_n = 0$$, it tells us that after <i>zero</i> steps we should see the number $$n$$. But taking zero steps means we stay at the same position! We cannot have both a $$0$$ and an $$n$$ in the same position, unless this $$n$$ in question is $$0$$ itself. This forces $$Q_0$$ to be $$0$$; any other number would require a $$0$$ to appear later on the sequence, which leads to a contradiction.

<br>

<!--$$
\begin{array}{c|c|c|c|c|c|c|c|c|c|c|c|c|c|c|c|c|c}
n & 0 & 1 & 2 & 3 & 4 & 5 & 6 & 7 & 8 & 9 & 10 & 11 & 12 & 13 & 14 & 15 & \cdots\\
\hline
Q_n & 0 & . & . & . & . & . & . & . & . & . & . & . & . & . & . & . &
\end{array}
$$-->

<img title="Empty table for the self-predicting sequence." alt="" src="/images/self_predicting_seq_2.png">

<br>

Now that $$Q_0$$ is in place, what could $$Q_1$$ be? It surely cannot be a $$0$$, as we stated just now. Let's try $$Q_1 = 1$$. In one step, we should then encounter a $$1$$, so $$Q_2 = 1$$. This in turn tells us that in one step, we should see the number $$2$$, so $$Q_3 = 2$$. Placing a single number seemingly escalates into a whole list of numbers we can fill in:

<br>

<!--$$
\begin{array}{c|c|c|c|c|c|c|c|c|c|c|c|c|c|c|c|c|c}
n & 0 & 1 & 2 & 3 & 4 & 5 & 6 & 7 & 8 & 9 & 10 & 11 & 12 & 13 & 14 & 15 & \cdots\\
\hline
Q_n & 0 & 1 & 1 & 2 & . & 3 & . & . & 5 & . & . & . & . & 8 & . & . &
\end{array}
$$-->

<img title="Empty table for the self-predicting sequence." alt="" src="/images/self_predicting_seq_3.png">

<br>

An interesting pattern emerges, which some may recognise as the <em>Fibonacci sequence</em>, commonly denoted as $$F_n$$. This sequence has the property that each term is the sum of the two numbers prior to it: $$F_n = F_{n-1} + F_{n-2}$$ for $$n \geq 2$$. It starts with the numbers $$0, 1, 1, 2, 3, 5, 8, 13, 21, 34, \cdots$$.

To see why we encounter these numbers in our self-predicting sequence $$Q_n$$, it might help to look at its own defining relation. $$Q_n$$ tells us after how many steps we will see an $$n$$ in the sequence. In other words, at position $$n + Q_n$$, the sequence shows the number $$n$$ itself. Applying the same reasoning at this new position tells us that at position $$(n + Q_n) + n$$, we see the number $$n + Q_n$$. Notice that we keep adding the last two numbers to arrive at the new position. This follows the exact same principle as the Fibonacci sequence! Since we put $$Q_1 = 1$$, we start out exactly like the original Fibonacci terms $$1, 1, 2, 3, \cdots$$ and cause the rest of the sequence to unroll.

Time for me to address the first ambiguity, then. Placing a $$1$$ at $$Q_1$$ already leads to two consecutive ones to show up in our sequence. If we really want to avoid repeating numbers, we would have to start with a higher number, only to encounter a $$1$$ later on. For now, we will allow for repetition of numbers. As we will see, this will lead to a beautiful monotonically increasing sequence, rather than one that looks more like an unstable rollercoaster.

The next blank entry in the table is $$n = 4$$. Of course, we cannot fill in any number that points us to a position that is already occupied by another number. This means we cannot enter a $$1$$ or a $$4$$, because the positions $$n = 5$$ and $$n = 8$$ are already filled in. However, the numbers $$2$$ and $$3$$ seem valid. We will enter the smallest number: a $$2$$. It's not a surprise that we again uncover a whole Fibonacci-esque subsequence of terms:

<br>

<!--$$
\begin{array}{c|c|c|c|c|c|c|c|c|c|c|c|c|c|c|c|c|c}
n & 0 & 1 & 2 & 3 & 4 & 5 & 6 & 7 & 8 & 9 & 10 & 11 & 12 & 13 & 14 & 15 & \cdots\\
\hline
Q_n & 0 & 1 & 1 & 2 & 2 & 3 & 4 & . & 5 & . & 6 & . & . & 8 & . & . &
\end{array}
$$-->

<img title="Empty table for the self-predicting sequence." alt="" src="/images/self_predicting_seq_4.png">

<br>

So far, so good. However, can we be sure that the two Fibonacci subsequences never overlap? After all, our table only extends up to $$n = 15$$, so we can't see what happens further down the sequence. Perhaps the subsequences collide somewhere thousands of steps into the sequence! Luckily, we can prove that these sequences stay out of one another's ways, under one important condition.

<br>

<h4>Proof</h4> 

Let's consider a position $$n$$ that lies between some $$m$$ and the next term of its sequence $$m + Q_m$$:

$$
\begin{equation}
  m < n < m + Q_m.
\end{equation}
$$

If we choose for $$Q_n$$ a number such that $$Q_m \leq Q_n \leq m$$, then the next terms in the two sequences are found by adding these inequalities:

$$
\begin{equation}
  m+Q_m < n+Q_n < (m+Q_m) + m,
\end{equation}
$$

which tells us that the position of the next term of the $$n$$-sequence falls neatly between those of the $$m$$-sequence.

<br>

<img title="Empty table for the self-predicting sequence." alt="" src="/images/self_predicting_seq_proof.png">

<br>

We end up in a similar situation as what we started with; the new term in the $$n$$-sequence lies somewhere between two terms of the $$m$$-sequence, and their corresponding values are strictly increasing. Therefore, the following term of the $$n$$-sequence will again fall between the next terms of the $$m$$-sequence:

$$
\begin{equation}
  (m+Q_m) + m < (n+Q_n) + n < \left((m+Q_m) + m\right) + (m + Q_m).
\end{equation}
$$

This pattern repeats indefinitely, so the subsequences will never collide. Note that $$n$$ and $$m$$ can be arbitrary numbers appearing anywhere in any (but not the same) sequence. As long as $$Q_m \leq Q_n \leq m$$, there will be no overlap. In most cases, the two direct neighbours of $$n$$ will belong to two different sequences. To satisfy the requirement above for both neighbour-sequences simultaneously, $$Q_n$$ is forced to either lie between its adjacent numbers or to equal one of them. In other words, any element $$Q_n$$ of our self-predicting sequence satisfies (for $$n \geq 1$$):

$$
\begin{equation}
  Q_{n-1} \leq Q_n \leq Q_{n+1}.
\end{equation}
$$

While this rule is not necessary to build our self-predicting sequence, it is a safe and simple method to avoid any overlap. It also makes the sequence <em>monotonically increasing</em>, which means the terms only grow or stay the same, but never decrease. We already applied this rule for $$n = 4$$, where the term $$Q_4 = 2$$ was set equal to its lower neighbour $$Q_3 = 2$$. Following the reasoning above, the subsequences will not coincide anywhere.

&nbsp;

Now that we understand how $$Q_n$$ behaves, and with one rule in our pocket to make sure none of the subsequences overlap, we are free to fill in the rest of the table. The options for $$n = 7$$ are bounded by its neighbouring terms, so we can choose either a $$4$$ or a $$5$$. Finally, I'll bring up the second ambiguity: let us restrict the sequence to predict the very next appearance of the number. In other words, this means that when we encounter a blank space, we enter a number that is <em>lower</em> than any number already filled in further down the sequence. We will see that there are always two options for a blank space, so we can only enter the lowest of the two which is $$Q_n = Q_{n-1}$$. Returning to the sequence we have so far, our only option becomes $$Q_7 = 4$$. With this extra rule, we can complete our sequence:

&nbsp;

<!--$$
\begin{array}{c|c|c|c|c|c|c|c|c|c|c|c|c|c|c|c|c|c}
n & 0 & 1 & 2 & 3 & 4 & 5 & 6 & 7 & 8 & 9 & 10 & 11 & 12 & 13 & 14 & 15 & \cdots\\
\hline
Q_n & 0 & 1 & 1 & 2 & 2 & 3 & 4 & 4 & 5 & 5 & 6 & 7 & 7 & 8 & 9 & 9 &
\end{array}
$$-->

<img title="Empty table for the self-predicting sequence." alt="" src="/images/self_predicting_seq_5.png">

&nbsp;

And there we go! We have constructed a sequence that predicts itself, by only following some simple rules. Of course, producing the table up to a large position can take quite some time and effort. Can we do better than this?

<br>

<h3>From sequence to formula</h3>


$$
\begin{equation}
  Q_n = \left\lfloor \frac{n}{\phi}+\frac{1}{2} \right\rfloor
\end{equation}
$$
