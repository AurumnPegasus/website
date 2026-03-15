---
title: "Connect Four, Part 1"
date: 2020-11-22T12:00:06+09:00
tags:
  - game
  - paper
---

<p>This blog contains explanation of the paper by Victor Allis called <a href="http://www.informatik.uni-trier.de/~fernau/DSL0607/Masterthesis-Viergewinnt.pdf">&lsquo;A Knowledge-based Approach of Connect-Four&rsquo;</a></p>
<h2 id="introduction">Introduction<a href="#introduction" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h2>
<p>Connect-Four is a game for two players. Both have <!-- raw HTML omitted -->21 identical coins<!-- raw HTML omitted -->. In the standard form of the game, one set of coins is yellow, and the other is red. You play the game on a vertical, rectangular board consisting of <!-- raw HTML omitted -->7<!-- raw HTML omitted --> vertical columns of <!-- raw HTML omitted -->6<!-- raw HTML omitted --> rows each. Each time a player puts a coin down, it falls to the lowest unoccupied block in that column. Players make a move in turns.</p>
<p>If a player connects four coins either horizontally, vertically or diagonally, they win. Occupying each of the 7x6 blocks such that no other move is possible, and ensuring that there is no winning player, entails the draw condition.</p>
<p>Now, some definitions to make referencing the board easier. The 7 columns are labelled &lsquo;a&rsquo; through &lsquo;g&rsquo;, while the rows are numbered 1 through 6. In this way, the lowest square in the middle column is called d1. For convenience&rsquo;s sake, we are taking the <strong>first player as White (W) and second as Black (B) (similar to Chess).</strong></p>
<h2 id="approaching-the-game">Approaching the Game<a href="#approaching-the-game" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h2>
<p>Before you show off your excellent techniques, we need to first prove that the dumb approach does not work. It is easy to see that the number of possible positions is at most $3^{42} (\geq 10^{20}).$ This upper bound is a very crude one and can be brought into better proportions. For this purpose, a program was written in the C programming language, which is mentioned later. For the standard, <!-- raw HTML omitted -->7 x 6<!-- raw HTML omitted --> board, the program saw an upper bound of $7.1* 10^{13}$.</p>
<p>So, too many possibilities, significantly less accuracy, brute force is not an efficient approach. A player cannot win just by instantiating each possible board and trying to follow it. You are a human after all, not a computer 😛</p>
<h2 id="some-possible-boards">Some Possible Boards<a href="#some-possible-boards" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h2>
<p>Let&rsquo;s start small. Consider a board of <!-- raw HTML omitted -->n columns<!-- raw HTML omitted -->, but only <!-- raw HTML omitted -->2 rows<!-- raw HTML omitted -->. My claim is <strong>that Black will never lose a game on this board.</strong> Even if n were to be practical infinity.</p>
<p>This is how B should play:</p>
<ol>
<li>Pair up all the n rows in groups of 2. If n is odd let the nth row be alone.</li>
<li>If W plays in row 1, play in row 2 (pair). If W plays in row 2, play in row 1.</li>
<li>If W plays in row N, play in row N.</li>
</ol>
<p>This will always result in a draw, since only way W could win is if it were to get its coin in 4 consecutive rows side by side. This is prevented by B.</p>
<p>Another solved board is with <!-- raw HTML omitted -->2n rows<!-- raw HTML omitted --> (even) and <!-- raw HTML omitted -->columns ≤ 6<!-- raw HTML omitted -->. This strategy could also be used for ensuring B always draws.</p>
<ol>
<li>If W plays in columns 1, 2, 5 or 6, B plays in the same column.</li>
<li>If W plays for the first time in column 3 or 4, B plays in the other column.</li>
<li>Otherwise, if W plays in column 3 or 4, and B can still play in it, B plays in the same column</li>
<li>If W plays in column 3 or 4, and B can not play in it, B plays in the other column.</li>
</ol>
<p>Since B never allows a vertical 4 for W, that is out of the question. For horizontal 4 at row 1, B ends up occupying at least 1 of the two bottom columns, hence denying that for W as well. For any other horizontal 4, it is only possible in odd rows for W. But in column 3 and 4, B ends up having at least 1 in either of the two in all odd rows. Diagonal is not possible at all since W will have all coins at odd rows in column 1, 2, 5 and 6.</p>
<h2 id="threats">Threats<a href="#threats" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h2>
<h4 id="useless-threats">Useless Threats<a href="#useless-threats" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h4>
<figure>
    <img src="https://i.imgur.com/r60hfG3.png"/> 
</figure>

<p>The threats by <strong>W</strong> in <!-- raw HTML omitted -->row 3,4,5<!-- raw HTML omitted --> are considered to be useless, due to the threat by <strong>B</strong> in row 2. Since W cannot move in <!-- raw HTML omitted -->column 2<!-- raw HTML omitted --> and <!-- raw HTML omitted -->6<!-- raw HTML omitted -->, it has to fill column 7. Even number of rows mean that W will still have the turn when column 7 is filled, meaning B will end up winning.</p>
<p>Whether a threat is useless or useful is dependant on control of Zugzwang (explained later).</p>
<h4 id="odd-even-threat">Odd Even Threat<a href="#odd-even-threat" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h4>
<p>A threat is only useful if a player is forced to play in a row just below that of the threat. Usually that happens when other columns are filled. In such cases, <strong>generally</strong>, W has only odd rows, and B has even rows.</p>
<p>Black has odd threats, and White has even. If they are in the same column, the lower threat will win (as seen above)</p>
<p>Generally speaking, W threat is stronger than that of B. Here is how everything rolls out:</p>
<ul>
<li>W has odd, B has even threat: W will win.</li>
<li>W has even, B has even threat: B will win.</li>
<li>W has even, B has odd threat: Draw.</li>
<li>W has odd, B has odd threat: Draw.
A simple example consisting of multiple threats is:</li>
</ul>
<figure>
    <img src="https://i.imgur.com/HvAXpVf.png"/> 
</figure>

<p>Here, <strong>B has odd threat</strong> at <!-- raw HTML omitted -->c3<!-- raw HTML omitted --> and <strong>W has odd threat</strong> at <!-- raw HTML omitted -->f3<!-- raw HTML omitted -->. Going by the table, it should be a draw. Lets play it out. <strong>W</strong> has to give up its own threat to play the game, which would ideally result in a draw, since <strong>B</strong> will have to give up its threat in <!-- raw HTML omitted -->c<!-- raw HTML omitted -->. But what happens is that <strong>B</strong> gets to create a new threat at <!-- raw HTML omitted -->c2<!-- raw HTML omitted --> due to coin at <!-- raw HTML omitted -->f5<!-- raw HTML omitted -->. This gives B consecutive threats, and thus it wins.</p>
<p>This tells us that <strong>though parity of threats tells us a good amount about the winner, we need to be careful about new threats.</strong></p>
<h2 id="zugzwang">Zugzwang<a href="#zugzwang" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h2>
<p>This is a very basic but powerful concept in Connect 4. <strong>Zugzwang</strong> basically means to force a player to make a move they would rather not make. This is due to the simple rule that they have to make a move, and the constraints of the board.</p>
<h4 id="initial-position">Initial Position<a href="#initial-position" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h4>
<p><strong>W always moves first.</strong> Therefore B always is in a position to play follow up, hence to control the Zugzwang. Suppose B plays follow up from the beginning, you would end up with a board like this:</p>
<figure>
    <img src="https://i.imgur.com/9qvEWpv.png"/> 
</figure>

<p>Though this is an illegal position, it is interesting to note who won first. Since <strong>W</strong> controls whole first row, naturally <strong>W</strong> won the game first. This shows that follow up is not a good strategy at the start of the game for <strong>B</strong>.</p>
<h4 id="other-positions">Other Positions<a href="#other-positions" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h4>
<p>The following board represents a position where W is in control of the Zugzwang:</p>
<figure>
    <img src="https://i.imgur.com/ncuhIkK.png"/> 
</figure>

<p>Here, <strong>W</strong> has an odd threat in <!-- raw HTML omitted -->column a<!-- raw HTML omitted -->. Knowing this, <strong>B</strong> won&rsquo;t play there. Since on the rest of the board, odd number of squares are remaining, whoever plays first here, the opposite will have to play in <!-- raw HTML omitted -->column a<!-- raw HTML omitted -->. Since <strong>W</strong> is playing the first, it can control the zugzwang to force <strong>B</strong> to play at <!-- raw HTML omitted -->a2<!-- raw HTML omitted -->. The only option <strong>B</strong> has to go against the zugzwang and connect its 4 men, but still, it is <strong>W&rsquo;s game to lose.</strong></p>
<p><strong>In this case, B has control of Zugzwang due to even threats</strong></p>
<figure>
    <img src="https://i.imgur.com/rim4YUj.png"/> 
</figure>

<p><strong>B</strong> and <strong>W</strong> ideally do not want to be the first to play in <!-- raw HTML omitted -->b<!-- raw HTML omitted --> or <!-- raw HTML omitted -->f<!-- raw HTML omitted -->. Other than these 2, the total number of boxes remaining is even. This means whoever plays first here, will be the player *forced to play first in either column <!-- raw HTML omitted -->b<!-- raw HTML omitted --> or <!-- raw HTML omitted -->f<!-- raw HTML omitted -->. That means at the end, <strong>W</strong> will have to play in <!-- raw HTML omitted -->b1<!-- raw HTML omitted --> or <!-- raw HTML omitted -->f1<!-- raw HTML omitted -->, thus losing. <strong>B</strong> is in complete control of Zugzwang here and can play follow up</p>
<figure>
    <img src="https://i.imgur.com/7jwwzvm.png"/> 
</figure>

<h2 id="references">References<a href="#references" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h2>
<ul>
<li>Resources by Victor Allis</li>
<li>Alpha Beta Pruning based heuristics</li>
<li>Principles and Tehniques BY Stanford</li>
<li>C4 Numbers by oeis org</li>
<li>Math oriented resources behind Connect-4</li>
</ul>
