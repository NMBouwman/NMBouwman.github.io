---
layout: post
title:  "Braillenary operations"
date:   2024-05-27 16:47:49 +0200
categories: blogs
mathjax: true
---
{% include script.html %}

Braille is a language that has been fascinating me for a long time. Through its elementary grid-like symbols, it enables people with visual impairments to read by touch rather than sight. Many languages have their own type of Braille, adapted to accomodate for specific letters and symbols. Besides its regular and useful functions, however, I have found Braille to be a particularly fun cipher. Though not difficult to crack (frequency analysis will do the trick), the signs are just unrevealing enough to act as an excellent communication method between friends. They are also sufficiently simple to hide in puzzles or images. I have encountered a Braille cipher or puzzle regularly in the Dutch General Intelligence and Security Service's (AIVD) yearly <a href="https://www.aivd.nl/onderwerpen/aivd-kerstpuzzel">Christmas puzzle collection (Dutch)</a>, and I would say that by now I can confidently write in Unified English Braille (Grade 1, at the very least).

Yet somehow, this little language keeps finding ways to pique my interest. I want to talk about a little project of mine that started out as a funny discovery, but eventually taught me about search trees in computer science, and taught me the word "snirt".

<h3>A language of zeros and ones</h3>

There is something about the Braille language that strongly appeals to me. The signs are extremely simple; they only have six dots, each of which can be either raised or not. This binary feature of Braille is what allows us to assign a unique sign to every letter of the alphabet, with plenty of configurations to spare. In particular, there are $$2^6=64$$ combinations and only $$26$$ letters. Depending on the type of Braille, the rest of the symbols are used for special characters, groups of letters, abbreviatons and more.

However, aside from their simple binary nature, the signs in Braille are far from trivial. Rather than making use of binary counting, the letters A through J have their own little configurations in the top four dots. The rest of the letters in the alphabet are constructed by repeating the ten signs with successively one and two dots filled in on the bottom row. These repetitions are called decades. An interesting exception is the letter W (&#10298;), which has a sign from the fourth decade assigned to it.

Since each dot in a Braille sign is essentially just a $$0$$ or a $$1$$, we can compare letters elementwise. Now, if we perform binary operations between two letters, can we get another letter out of it? An OR comparison is maybe not that interesting, since this will result in a sign with at least as many dots as the input letters. Most of the signs contain two or three dots, so we want to avoid an abundance of dots in our results. Similarly, applying an AND operation to a pair of letters will always result in a sign with an equal or lower amount of dots. A more interesting candidate is the XOR operation: we might end up with completely different signs depending on the exact arrangements of dots within the input letters!

[add binary tables]

Let's play around with this idea. Take for example the letter P (&#10255;). We can make another letter by XORing this with the letter E (&#10257;). This essentially cancels the top-left dot and adds the middle-right one, forming the letter T (&#10270;)! In fact, the order of these three letters doesn't matter: applying the XOR operation between T and E returns P again.
<p style="text-align: center;"><i>Check for yourself that this is true! Does this property hold for every triplet?</i></p>

The existence of these seemingly arbitrary triplets didn't feel obvious at all when I was playing around with these operations, but it did raise some questions:
<p style="text-align: center;"><ul>
<li>Can we XOR two words in Braille to make a third word?</li>
<li>If so, what is the longest word length we can find a triplet for?</li>
</ul></p>

<h3>The hunt for XOR triplets</h3>

<p style="text-align:center;">
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
</p>

<!--ADROP - PINTE - SHIFT
APHIS - PEDRO - STING
ARGIL - PIEND - SNIRT
BIFID - CRAFT - INIAL
TINGI - ENROL - PRISM-->




