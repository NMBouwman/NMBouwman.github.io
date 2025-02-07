---
layout: post
title:  "Binary operations on Braille"
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

It struck me that, since each dot in a Braille sign is essentially just a $$0$$ or a $$1$$, we can compare letters elementwise. That is, we look at two Braille signs, but only compare them one dot at a time. If we perform binary operations between two letters, can we get another letter out of it? There are a couple of operations to consider:

- AND (&#8743;): The output is a \\(1\\) if and only if both input dots are a \\(1\\);
- OR (&#8744;): The output is a \\(1\\) if at least one of the input dots is a \\(1\\);
- XOR (&#8891;): The output is a \\(1\\) if and only if exactly one of the input dots is a \\(1\\).

Each of these binary operations has its associated opposite (NAND, NOR, XNOR) by negating the output.

[add binary tables]

For our purposes, an OR comparison is maybe not that interesting, since it will result in a sign with at least as many dots as the input letters. Most of the signs in Braille contain three or four dots, so we want to avoid an abundance of dots in our results. Similarly, applying an AND operation to a pair of letters will always result in a sign with an equal or lower amount of dots. A more interesting candidate is the XOR operation: we might end up with completely different signs depending on the exact arrangements of dots within the input letters!

Let's play around with this idea. Take for example the letter P (&#10255;). We can make another letter by XORing this with the letter E (&#10257;). This essentially cancels the top-left dot and adds the middle-right one, forming the letter T (&#10270;). The letters E, P and T therefore form what I call a <em>triplet</em>. In fact, the order of these three letters doesn't matter: applying the XOR operation between T and E returns P again. 
<p style="text-align: center;"><i>Check for yourself that this is true! Does this property hold for every triplet? And does it hold for each binary operation?</i></p>

The existence of these seemingly arbitrary triplets did not feel obvious at all when I was playing around with these operations, but it did raise some questions:

- Can we XOR two words in Braille to make a third word?
- If so, what is the longest word length we can find a triplet for?

<h3>The hunt for XOR triplets</h3>

Admittedly, there are multiple versions of Braille we could use to answer these questions. In English, the most widely used type of Braille is Unified English Braille (UEB) Grade 2, which contains many signs for abbreviations and groups of letters. For example, the ST is written as &#10252; and AND as &#10287;. With WITH written like &#10302;, the word WITHSTAND is neatly reduced to &#10302;&#10252;&#10287;. You can imagine that this makes the goal of comparing words elementwise very tricky, since it first requires us to exactly figure out how a word is spelled using the contractions. Instead, I decided to stick with Grade 1 UEB, which simply substitutes each letter of a word with its corresponding single-letter sign. Although it reduces the amount of potential triplets, this version was more likely to withstand (&#10298;&#10250;&#10270;&#10259;&#10252;&#10270;&#10241;&#10269;&#10265;) my search.

The table below shows all XOR combinations of two letters in Grade 1 UEB. As expected, it is perfectly symmetric, because the order of the inputs does not matter. In fact, there is a certain sixfold symmetry due to the triplet property shown earlier. It is interesting that the I, J, S and T form a triplet with every letter in the first and second decade.

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

The next step is to compare every word in the English dictionary with every other word in the English dictionary to check if the result happens to be a word in the English dictionary. Right, that's going to take a while. Since we want to compare the words letter by letter, we can restrict ourselves to words of equal length. However, this restriction is of little use; there are over $$XXXX$$ words of nine letters, so the method described above would take at the very least $$XXXX^3$$ operations. Indeed, my poor laptop could not handle anything beyond four-letter words when I implemented this brute-force way in Python. It is clearly necessary to construct a way to compare words faster, for example by eliminating pairs of words that contain letters that do not form triplets. But how can we do this without comparing the words entirely?

Thanks to a friend of mine, I stumbled upon the concept of search trees in computer science. Search trees are structures that are used to store and retrieve information efficiently by starting with a root node and branching off by certain rules. For example, a set of numbers ($$3$$, $$5$$, $$9$$, $$10$$, $$13$$, $$19$$) could be sorted by picking $$9$$ as a root and branching off to the left for smaller numbers and to the right for larger numbers. Then, the node for the right brach can be chosen to be $$13$$, which will split the two remaining large numbers to opposing sides. The structure could then look like the one in the image below. If each number is associated with some information, then the location of this information is easily retrievable by comparing the number with the nodes and tracing the tree down.

[image tree]

Another type of search tree is a prefix tree, also called a <em>trie</em>. A trie splits words into prefixes, which construct words at the bottom of the trie. For example, the words WORD and WORK have a common prefix WOR-, so they trace they same path down the trie (W-, WO-, WOR-) until they split off at the final node. It is then possible to add an "end" node, which indicates that a valid word has been reached. The prefix WORK- can then split off into WORK (end) and WORKS-, which continues the trie. This is shown in the image below. Since we already split the dictionary into words of equal length in advance, we will not bother with end nodes but simply go down the entire tree.

[image trie]

Tries provide the perfect solution to our problem. We first construct a trie containing all words in the English dictionary of a certain length. The root node is an empty string, branching out into $$26$$ leaves; A- to Z-. The strategy is then to perform all three searches at the same time:

<!--ADROP - PINTE - SHIFT
APHIS - PEDRO - STING
ARGIL - PIEND - SNIRT
BIFID - CRAFT - INIAL
TINGI - ENROL - PRISM-->




