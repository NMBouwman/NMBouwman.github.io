---
layout: post
title:  "Binary operations on Braille"
date:   2025-02-10 21:49:00 +0100
categories: blogs
mathjax: true
---
{% include script.html %}

<em>This post contains Unicode Braille symbols. If these are not showing, this might be due to the font used by your device.</em>

<img title="Braille binary" alt="Braille binary" src="/images/braille_binary_PET_XOR.png">

Braille is a language that has been fascinating me for a long time. Through its elementary grid-like symbols, it enables people with visual impairments to read by touch rather than sight. Many languages have their own type of Braille, adapted to accomodate for specific letters and symbols. Besides its regular and useful functions, however, I have found Braille to be a particularly fun cipher. Though not difficult to crack (frequency analysis will do the trick), the signs are just unrevealing enough to act as an excellent communication method between friends. They are also sufficiently simple to hide in puzzles or images. I have encountered a Braille cipher or puzzle regularly in the Dutch General Intelligence and Security Service's (AIVD) yearly <a href="https://www.aivd.nl/onderwerpen/aivd-kerstpuzzel">Christmas puzzle collection (Dutch)</a>, and I would say that by now I can confidently write in Unified English Braille (Grade 1, at the very least).

Yet somehow, this little language keeps finding ways to pique my interest. I want to talk about a little project of mine that started out as a funny discovery, but eventually taught me about search trees in computer science, and taught me the word "snirt".

<br>

<h3>A language of zeros and ones</h3>

There is something about the Braille language that strongly appeals to me. The signs are extremely simple; they only have six dots, each of which can be either raised or not. This binary feature of Braille is what allows us to assign a unique sign to every letter of the alphabet, with plenty of configurations to spare. In particular, there are $$2^6=64$$ combinations and only $$26$$ letters. Depending on the type of Braille, the rest of the symbols are used for special characters, groups of letters, abbreviatons and more.

However, aside from their simple binary nature, the signs in Braille are far from trivial. Rather than making use of binary counting, the letters A through J have their own little configurations in the top four dots. The rest of the letters in the alphabet are constructed by repeating the ten signs with successively one and two dots filled in on the bottom row. These repetitions are called decades. An interesting exception is the letter $$\text{W}$$ (&#10298;), which has a sign from the fourth decade assigned to it.

It struck me that each dot in a Braille sign is essentially just a $$1$$ (raised) or a $$0$$ (not raised). This means we can compare letters "bitwise": we look at two Braille signs, but only compare them one dot at a time. There are a couple of operations we can consider to compare two dots:

- AND (&#8743;): The output is a \\(1\\) if and only if both input dots are a \\(1\\);
- OR (&#8744;): The output is a \\(1\\) if at least one of the input dots is a \\(1\\);
- XOR (&#8891;): The output is a \\(1\\) if and only if exactly one of the input dots is a \\(1\\).

Each of these binary operations has an associated negated operation (NAND, NOR, XNOR). If we perform binary operations between two letters, can we get another letter out of it?

For our purposes, an OR comparison is maybe not that interesting, since it will result in a sign with at least as many dots as the input letters. Most of the signs in Braille contain three or four dots, so we want to avoid an abundance of dots in our results. Similarly, applying an AND operation to a pair of letters will always result in a sign with an equal or lower amount of dots. These two operations could also easily give an output that is exactly the same as either of the inputs. A more interesting candidate is the XOR operation: not only might we end up with completely different signs depending on the exact arrangements of dots within the input letters, it is <em>impossible</em> for an output letter to be identical to one of the inputs!

<p style="text-align: center;">
<img title="XOR table" alt="XOR table" src="/images/braille_binary_XOR_table.png" width="400"></p>

Let's play around with this idea. Take for example the letter $$\text{P}$$ (&#10255;). We can make another letter by XORing this with the letter $$\text{E}$$ (&#10257;). This essentially cancels the top-left dot and adds the middle-right one, forming the letter $$\text{T}$$ (&#10270;). The letters $$\text{E}$$, $$\text{P}$$ and $$\text{T}$$ therefore form what I call a <em>triplet</em>. In fact, the order of these three letters doesn't matter: applying the XOR operation between $$\text{T}$$ and $$\text{E}$$ returns $$\text{P}$$ again. 
<p style="text-align: center;"><i>Check for yourself that this is true! Does this property hold for every triplet? And does it hold for each binary operation?</i></p>

The existence of these seemingly arbitrary triplets did not feel obvious at all when I was playing around with these operations, but it did raise some questions:

- Can we XOR two words in Braille to make a third word?
- If so, what is the longest word length we can find a triplet for?

<br>

<h3>The hunt for XOR triplets</h3>

Admittedly, there are multiple versions of Braille we could use to answer these questions. In English, the most widely used type of Braille is Unified English Braille (UEB) Grade 2, which contains many signs for abbreviations and groups of letters. For example, the $$\text{ST}$$ is written as &#10252; and $$\text{AND}$$ as &#10287;. With $$\text{WITH}$$ written like &#10302;, the word $$\text{WITHSTAND}$$ is neatly reduced to the three signs &#10302;&#10252;&#10287;. You can imagine that this makes the goal of comparing words bitwise very tricky, since it first requires us to exactly figure out how a word is spelled using the contractions. Instead, I decided to stick with Grade 1 UEB, which simply substitutes each letter of a word with its corresponding single-letter sign. Although it reduces the amount of potential triplets, this version was more likely to withstand (&#10298;&#10250;&#10270;&#10259;&#10254;&#10270;&#10241;&#10269;&#10265;) my search.

The table below shows all XOR combinations of two letters in Grade 1 UEB. As expected, it is perfectly symmetric, because the order of the inputs does not matter. In fact, there is a certain sixfold symmetry due to the triplet property shown earlier, but of course this is difficult to showcase in a table. It is notable that the $$\text{I}$$, $$\text{J}$$, $$\text{S}$$ and $$\text{T}$$ form a triplet with every letter in the first and second decade. We see that $$\text{I}$$ and $$\text{J}$$ connect letters within the two decades, while $$\text{S}$$ and $$\text{T}$$ connect letters from the first decade to the second. This makes sense, given that these pairs of letters are equal aside from a raised bottom left dot. Interestingly, there are <em>no</em> other letters that connect the first two decades to one another. We also see several letters scattered on the bottom and on the right; upon closer inspection, this appears to be entirely caused by the irregular structure of the $$\text{W}$$. All letters from the third decade are connected to $$\text{W}$$ by some letter from the second decade. Lastly, note that the vowels $$\text{A}$$, $$\text{E}$$ and $$\text{O}$$ all form a triplet with $$\text{I}$$ (as mentioned before), but not with any other vowels. This will become important soon.

<br>

<p style="text-align: center;">
<img title="XOR table" alt="XOR table" src="/images/braille_binary_triplet_table.svg" width="700"></p>

<!--
<p style="text-align:center; font-size:12px;">
$$
\begin{array}{c|c|c|c|c|c|c|c|c|c|c|c|c|c|c|c|c|c|c|c|c|c|c|c|c|c|c}
  & \text{A} & \textrm{B} & \text{C} & \text{D} & \text{E} & \text{F} & \text{G} & \text{H} & \text{I} & \text{J} & \text{K} & \text{L} & \text{M} & \text{N} & \text{O} & \text{P} & \text{Q} & \text{R} & \text{S} & \text{T} & \text{U} & \text{V} & \text{W} & \text{X} & \text{Y} & \text{Z} \\
\hline
\text{A} &   &   &   &   &   & \text{I} & \text{J} &   & \text{F} & \text{G} &   &   &   &   &   & \text{S} & \text{T} &   & \text{P} & \text{Q} &   &   &   &   &   &   \\
\hline
\text{B} &   &   & \text{I} & \text{J} &   &   &   &   & \text{C} & \text{D} &   &   & \text{S} & \text{T} &   &   &   &   & \text{M} & \text{N} &   &   &   &   &   &   \\
\hline
\text{C} &   & \text{I} &   &   &   &   &   & \text{J} & \text{B} & \text{H} &   & \text{S} &   &   &   &   &   & \text{T} & \text{L} & \text{R} &   &   &   &   &   &   \\
\hline
\text{D} &   & \text{J} &   &   &   &   &   & \text{I} & \text{H} & \text{B} &   & \text{T} &   &   &   &   &   & \text{S} & \text{R} & \text{L} &   &   &   &   &   &   \\
\hline
\text{E} &   &   &   &   &   & \text{J} & \text{I} &   & \text{G} & \text{F} &   &   &   &   &   & \text{T} & \text{S} &   & \text{Q} & \text{P} &   &   &   &   &   &   \\
\hline
\text{F} & \text{I} &   &   &   & \text{J} &   &   &   & \text{A} & \text{E} & \text{S} &   &   &   & \text{T} &   &   &   & \text{K} & \text{O} &   &   &   &   &   &   \\
\hline
\text{G} & \text{J} &   &   &   & \text{I} &   &   &   & \text{E} & \text{A} & \text{T} &   &   &   & \text{S} &   &   &   & \text{O} & \text{K} &   &   &   &   &   &   \\
\hline
\text{H} &   &   & \text{J} & \text{I} &   &   &   &   & \text{D} & \text{C} &   &   & \text{T} & \text{S} &   &   &   &   & \text{N} & \text{M} &   &   &   &   &   &   \\
\hline
\text{I} & \text{F} & \text{C} & \text{B} & \text{H} & \text{G} & \text{A} & \text{E} & \text{D} &   &   & \text{P} & \text{M} & \text{L} & \text{R} & \text{Q} & \text{K} & \text{O} & \text{N} &   &   &   & \text{X} &   & \text{V} &   &   \\
\hline
\text{J} & \text{G} & \text{D} & \text{H }& \text{B} & \text{F} & \text{E} & \text{A} & \text{C} &   &   & \text{Q} & \text{N} & \text{R} & \text{L} & \text{P} & \text{O} & \text{K} & \text{M} &   &   &   & \text{Y} &   &   & \text{V} &   \\
\hline
\text{K} &   &   &   &   &   & \text{S} & \text{T} &   & \text{P} & \text{Q} &   &   &   &   &   & \text{I} & \text{J} &   & \text{F} & \text{G} &   &   &   &   &   &   \\
\hline
\text{L} &   &   & \text{S} & \text{T} &   &   &   &   & \text{M} & \text{N} &   &   & \text{I} & \text{J} &   &   &   &   & \text{C} & \text{D} &   &   & \text{Y} &   & \text{W} &   \\
\hline
\text{M} &   & \text{S} &   &   &   &   &   & \text{T} & \text{L} & \text{R} &   & \text{I} &   &   &   &   &   & \text{J} & \text{B} & \text{H} &   &   &   &   &   &   \\
\hline
\text{N} &   & \text{T} &   &   &   &   &   & \text{S} & \text{R} & \text{L} &   & \text{J} &   &   &   &   &   & \text{I} & \text{H} & \text{B} &   & \text{W} & \text{V} &   &   &   \\
\hline
\end{array}
$$
$$
\begin{array}{c|c|c|c|c|c|c|c|c|c|c|c|c|c|c|c|c|c|c|c|c|c|c|c|c|c|c}
& \text{A} & \textrm{B} & \text{C} & \text{D} & \text{E} & \text{F} & \text{G} & \text{H} & \text{I} & \text{J} & \text{K} & \text{L} & \text{M} & \text{N} & \text{O} & \text{P} & \text{Q} & \text{R} & \text{S} & \text{T} & \text{U} & \text{V} & \text{W} & \text{X} & \text{Y} & \text{Z} \\
\hline
\text{O} &   &   &   &   &   & \text{T} & \text{S} &   & \text{Q} & \text{P} &   &   &   &   &   & \text{J} & \text{I} &   & \text{G} & \text{F} &   &   &   &   &   &   \\
\hline
\text{P} & \text{S} &   &   &   & \text{T} &   &   &   & \text{K} & \text{O} & \text{I} &   &   &   & \text{J} &   &   &   & \text{A} & \text{E} &   &   & \text{Z} &   &   & \text{W} \\
\hline
\text{Q} & \text{T} &   &   &   & \text{S} &   &   &   & \text{O} & \text{K} & \text{J} &   &   &   & \text{I} &   &   &   & \text{E} & \text{A} & \text{W} &   & \text{U} &   &   &   \\
\hline
\text{R} &   &   & \text{T} & \text{S} &   &   &   &   & \text{N} & \text{M} &   &   & \text{J} & \text{I} &   &   &   &   & \text{D} & \text{C} &   &   & \text{X} & \text{W} &   &   \\
\hline
\text{S} & \text{P} & \text{M} & \text{L} & \text{R} & \text{Q} & \text{K} & \text{O} & \text{N} &   &   & \text{F} & \text{C} & \text{B} & \text{H} & \text{G} & \text{A} & \text{E} & \text{D} &   &   &   &   &   &   &   &   \\
\hline
\text{T} & \text{Q} & \text{N} & \text{R} & \text{L} & \text{P} & \text{O} & \text{K} & \text{M} &   &   & \text{G} & \text{D} & \text{H} & \text{B} & \text{F} & \text{E} & \text{A} & \text{C} &   &   &   &   &   &   &   &   \\
\hline
\text{U} &   &   &   &   &   &   &   &   &   &   &   &   &   &   &   &   & \text{W} &   &   &   &   &   & \text{Q} &   &   &   \\
\hline
\text{V} &   &   &   &   &   &   &   &   & \text{X} & \text{Y} &   &   &   & \text{W} &   &   &   &   &   &   &   &   & \text{N} & \text{I} & \text{J} &   \\
\hline
\text{W} &   &   &   &   &   &   &   &   &   &   &   & \text{Y} &   & \text{V} &   & \text{Z} & \text{U} & \text{X} &   &   & \text{Q} & \text{N} &   & \text{R} & \text{L} & \text{P} \\
\hline
\text{X} &   &   &   &   &   &   &   &   & \text{V} &   &   &   &   &   &   &   &   & \text{W} &   &   &   & \text{I} & \text{R} &   &   &   \\
\hline
\text{Y} &   &   &   &   &   &   &   &   &   & \text{V} &   & \text{W} &   &   &   &   &   &   &   &   &   & \text{J} & \text{L} &   &   &   \\
\hline
\text{Z} &   &   &   &   &   &   &   &   &   &   &   &   &   &   &   & \text{W} &   &   &   &   &   &   & \text{P} &   &   &  
\end{array}
$$
</p> -->

<br>

The next step is to compare every word in the English dictionary with every other word in the English dictionary to check if the result happens to be a word in the English dictionary. Right, that's going to take a while. Since we want to compare the words letter by letter, we can restrict ourselves to words of equal length. However, this restriction is of little use; there are over $$32400$$ words of nine letters, so the method described above would take at the very least $$32400^3 \approx 34000000000000$$ operations, multiplied by nine for the word length. Indeed, my poor laptop could not handle anything beyond four-letter words when I implemented this brute-force search in Python. It is clearly necessary to construct a way to compare words faster, for example by eliminating pairs of words that contain letters that do not form triplets. But how can we do this without comparing the words in their entirety?

<br>

<h3>Trying tries</h3>

Thanks to a friend of mine, I stumbled upon the concept of search trees in computer science. Search trees are structures that are used to store and retrieve information efficiently. The trees start with a root node on top and branch off by certain rules. For example, a set of numbers ($$3$$, $$5$$, $$9$$, $$10$$, $$13$$, $$19$$) could be turned into a search tree as follows: our rule will be that each node contains up to two branches, such that smaller numbers are always placed to the left and larger numbers to the right. We could pick $$9$$ as a root and branch off to $$5$$ on the left side and $$13$$ on the right side. The tree structure that follows then looks like the one on the left in the image below. If each number is associated with some information, then the location of this information is easily retrievable by comparing the number with the nodes. It now only takes up to three comparisons to reach the location of any number, rather than up to six if we were to compare against each element in the set.

Another type of search tree is a prefix tree, also called a <em>trie</em>. A trie splits words into prefixes, which construct words as we traverse down the trie. For example, the words <strong>WORD</strong> and <strong>WORK</strong> have a common prefix <strong>WOR-</strong>, so they trace they same path down the trie (<strong>W-</strong>, <strong>WO-</strong>, <strong>WOR-</strong>) until they finally split off into separate branches. It is then possible to add an "end" node, which indicates that a valid word has been reached. The prefix <strong>WORD-</strong> can then split off into <strong>WORD (end)</strong> and <strong>WORDS-</strong>, which continues the trie. This is shown on the right side in the image below. Since we already split the dictionary into words of equal length in advance, we will not bother with end nodes but simply go down the entire tree.

<img title="Trie example" alt="Trie example" src="/images/braille_binary_search_trees.png">

Tries provide the perfect solution to our problem: they allow us to perform all three searches through the dictionary at once. First, we construct a trie containing all words in the English dictionary of a certain length. We use an empty string for the root node, which branches out into $$26$$ leaves <strong>A-</strong> to <strong>Z-</strong>. The strategy is as follows:

- Traverse each branch down to the very bottom, to form <strong>word 1</strong>;
- For each <strong>word 1</strong>, loop through the branches of the root node to start constructing <strong>word 2</strong>;
- Check whether XORing the first letter of <strong>word 1</strong> with the first letter of <strong>word 2</strong> gives a valid letter as output;
- If the output is valid, save the output branch to construct <strong>word 3</strong> and loop over the branches of the first letter of <strong>word 2</strong> to pick a second letter;
- Check whether XORing the second letter of <strong>word 1</strong> with the second letter of <strong>word 2</strong> gives a letter that exists as a branch of the first letter of <strong>word 3</strong>;
- If it is, then repeat the above two steps and dive deeper into <strong>word 2</strong> and <strong>word 3</strong>. If it is not, then stop the process and find the next <strong>word 1</strong>.

I implemented this method in Python, making use of the `nltk.corpus.words`{:.python} dictionary from the nltk library. Since words with hyphens were a tricky case, I removed these from my analysis completely. The code only took around four seconds to run for nine-letter words; all other word lengths took less time to search through! To XOR the letters in Braille, I used somewhat of a shortcut: instead of comparing each dots separately, I considered the Braille signs as six digit binary numbers. The trie contained these numbers as nodes (in decimal, for simplicity), which made it incredibly easy to XOR the letters and to check whether the output letter existed as a branch for <strong>word 3</strong>.

<p style="text-align: center;">
<img title="XOR table" alt="XOR table" src="/images/braille_binary_T_number.png" width="600"></p>

<br>

<h3>Results</h3>

The answer is convincing: yes! We can absolutely XOR two words in Braille and get a valid word out. For two, three and four-letter words, there was a surprising amount of triplets. My code gave $$31$$, $$192$$ and $$96$$ hits, respectively. Of course, many of these words are vague or uncommon, but some of them are certainly worth appreciating. Let's look at a few noteworthy triplets.

<h4>Three letters</h4>

For three letters, I quite liked these triplets:
<p style="text-align: center;">
$$\text{AGE} \; \veebar \; \text{PET} \; = \; \text{SIP}$$
$$\text{ALE} \; \veebar \; \text{FIT} \; = \; \text{IMP}$$
$$\text{ART} \; \veebar \; \text{FIB} \; = \; \text{INN}$$
$$\text{EGG} \; \veebar \; \text{PSI} \; = \; \text{TOE}$$
</p>

<h4>Four letters</h4>

For four letters, I had a laugh at this rather inappropriate triplet:
<p style="text-align: center;">
$$\text{ARSE} \; \veebar \; \text{PIPI} \; = \; \text{SNAG}$$
</p>
And what about something a little more international?
<p style="text-align: center;">
$$\text{ANGO} \; \veebar \; \text{FIJI} \; = \; \text{IRAQ}$$
</p>
Or this lizard falling off a tropical tree:
<p style="text-align: center;">
$$\text{DROP} \; \veebar \; \text{HISS} \; = \; \text{INGA}$$
</p>


<h4>Five letters</h4>

Five-letter triplets turned out particularly interesting: I found a total of <em>five</em> five-letter triplets. Here is the complete list:

<p style="text-align: center;">
$$\text{ADROP} \; \veebar \; \text{PINTE} \; = \; \text{SHIFT}$$
$$\text{APHIS} \; \veebar \; \text{PEDRO} \; = \; \text{STING}$$
$$\text{ARGIL} \; \veebar \; \text{PIEND} \; = \; \text{SNIRT}$$
$$\text{BIFID} \; \veebar \; \text{CRAFT} \; = \; \text{INIAL}$$
$$\text{ENROL} \; \veebar \; \text{PRISM} \; = \; \text{TINGI}$$
</p>

It is interesting that the vast majority of triplets contain an $$\text{I}$$, but it makes sense: $$\text{I}$$ and $$\text{J}$$ form the most triplets with other letters, and $$\text{I}$$ is the only vowel that forms a triplet with the vowels $$\text{A}$$, $$\text{E}$$ and $$\text{O}$$. Since each (proper) word requires <em>some</em> vowel, it is inevitable that the $$\text{I}$$ shows up in at least one of the words.

Here are the definitions of the words, according to the <a href="https://www.merriam-webster.com/">Merriam-Webster dictionary</a>:

- \\( \text{ADROP:} \\) <em>A substance (as lead) believed essential to evolving the philosophers' stone.</em>
- \\( \text{APHIS:} \\) <em>Any of a genus (Aphis) of aphids.</em>
  - Aphid: <em>Any of numerous very small soft-bodied homopterous insects (superfamily Aphidoidea) that suck the juices of plants.</em>
- \\( \text{ARGIL:} \\) <em>Clay, especially potter's clay.</em>
- \\( \text{BIFID:} \\) <em>Divided into two equal lobes or parts by a median cleft.</em>
- \\( \text{CRAFT:} \\) <em>An occupation, trade, or activity requiring manual dexterity or artistic skill. [and more definitions]</em>
- \\( \text{ENROL:} \\) <em>To insert, register, or enter in a list, catalog, or roll.</em>
- \\( \text{INIAL:} \\) [From Wiktionary] <em>Relating to the inion.</em>
  - Inion: <em>A small protuberance on the external surface of the back of the skull near the neck; the external occipital protuberance.</em>
- \\( \text{PEDRO:} \\) <em>Dom; name of two emperors of Brazil.</em>
- \\( \text{PIEND:} \\) <em>Arris.</em>
  - Arris: <em>The sharp edge or salient angle formed by the meeting of two surfaces, especially in moldings.</em>
- \\( \text{PINTE:} \\) [From Wiktionary] <em>A French-Canadian term for an Imperial quart, equal to \\( 1/4 \\) of an Imperial gallon, \\(2\\) Imperial pints, \\(40\\) Imperial ounces or \\(1.136\\) liters.</em>
- \\( \text{PRISM:} \\) <em>A polyhedron with two polygonal faces lying in parallel planes and with the other faces parallelograms. [and more definitions]</em>
- \\( \text{SHIFT:} \\) <em>To change the place, position, or direction of. [and more definitions]</em>
- \\( \text{SNIRT:} \\) <em>(Chiefly Scottish) An unsuccessfully suppressed snort of laughter.</em>
  - I found this one particularly funny.
- \\( \text{STING:} \\) <em>To prick painfully. [and more definitions]</em>
- \\( \text{TINGI:} \\) [From Wiktionary] <em>A Brazilian tree, Magonia pubescens, whose seeds yield soap.</em>

And with that, I can also answer the second question: there are <em>no</em> triplets with words longer than five letters. The maximum word length is therefore five, and all the triplets for this length are given above.

<br>

<h3>Dotting the \(\text{I}\)'s</h3>

The patterns in Braille have surprisingly complex and fun properties. By interpreting this intriguing little language of dots as binary numbers, we can perform binary operations on each dot separately and combine two words to form one that is entirely different. When I thought of this little idea, I had no idea that it would teach me about search trees, or that the result would be so satisfying. All because of what I now consider the most important letter in Braille: the $$\text{I}$$ (&#10250;). Without it, there would not have been a single (proper) triplet of words. Of course, our final result is of little direct importance, even if it makes a good fun fact to tell at room parties. However, the triplet search itself was definitely worth the patience. Perhaps sometime in the future, I'll come back to this little project to include groupsigns in the analysis. I can't yet tell whether I will find more triplets in Grade 2 Braille, but at the very least they exist in Grade 1. Doesn't that feel like a sign?

&#10270;&#10259;&#10241;&#10269;&#10245;&#10254;&#10240;&#10251;&#10261;&#10263;&#10240;&#10263;&#10257;&#10241;&#10265;&#10250;&#10269;&#10267; !


