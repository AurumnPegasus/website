---
title: "Connect Four, Part 2"
date: 2020-11-22T12:00:06+09:00
tags:
  - game
  - paper
---

<p>This blog contains explanation of the paper by Victor Allis called <a href="http://www.informatik.uni-trier.de/~fernau/DSL0607/Masterthesis-Viergewinnt.pdf">&lsquo;A Knowledge-based Approach of Connect-Four&rsquo;</a>
This is a 2 part series exploring the paper, and I would suggest going through <a href="">&lsquo;first part&rsquo;</a> if you haven&rsquo;t already.</p>
<h2 id="-strategies">🕵️‍♂️ Strategies<a href="#-strategies" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h2>
<p>A formal definition is given of the nine rules which are used to refute potential threats of the opponent. These can be only applied by the player in control of the Zugzwang. These are always applied in the places opponent has to move.</p>
<h4 id="claimeven">Claimeven<a href="#claimeven" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h4>
<p>This makes direct use of the fact that player in control of Zugzwang can get all even unclaimed positions, giving odd positions to the other.</p>
<figure>
    <img src="https://i.imgur.com/ScthNh3.png"/> 
</figure>

<p><strong>B</strong> is in control of Zugzwang here since <strong>W doesn&rsquo;t have any threat.</strong> Here, if B were to use claimeven, he could end up getting a draw, since any threat of W will need to have a coin in even row, which will not be possible.</p>
<h4 id="baseinverse">Baseinverse<a href="#baseinverse" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h4>
<p>This is based on the logic that a player cannot play two directly playable moves in one turn. Therefore once the opponent has made the move, controller of Zugzwang can still cover the other position.</p>
<figure>
    <img src="https://i.imgur.com/Sqgzm1C.png"/> 
</figure>

<p>Here, if W cannot play in a1 and b1 at the same time. That means if W plays in a1, B plays in b1, and vice versa. Thus the threat is nullified.</p>
<h4 id="verticle">Verticle<a href="#verticle" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h4>
<p>Similar to Baseinverse, this is based on the fact that a player cannot play two directly playable moves in a single column in one turn. Depending on the opponent, the player controlling the Zugzwang can either play second or first in the column to prevent a verticle 4.</p>
<h4 id="aftereven">Aftereven<a href="#aftereven" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h4>
<p>Aftereven uses a special side-effect of the usage of one or more Claimevens. If a group can be completed by the controller of zugzwang, then they can complete the whole board using claim even, and then complete that group:</p>
<figure>
    <img src="https://i.imgur.com/U8HBV2W.png"/> 
</figure>

<p>Here, <strong>B</strong> can use aftereven to complete the group at either <!-- raw HTML omitted -->b2<!-- raw HTML omitted --> or <!-- raw HTML omitted -->f2<!-- raw HTML omitted -->. Here, then <strong>B</strong> can use claimeven to finally force <strong>W</strong> to play in either <!-- raw HTML omitted -->b1<!-- raw HTML omitted --> or <!-- raw HTML omitted -->f1<!-- raw HTML omitted -->. This is called as aftereven, where you can form a group and win by using claimevens.</p>
<h4 id="lowinverse">Lowinverse<a href="#lowinverse" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h4>
<p>Lowinverse is based on the fact that two odd numbers when summed give an even number. Normally, controller of Zugzwang will play lowest even square of the column containing odd number of empty squares. But when we have two columns (doesn&rsquo;t have to be consecutive) having odd number of empty squares, this will force the opponent to play first. In such a way controller of zugzwang gets to take the odd square above the opponent.</p>
<h4 id="highinverse">Highinverse<a href="#highinverse" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h4>
<p>This is based on the same principle as that of lowinverse:</p>
<figure>
    <img src="https://i.imgur.com/479IQjs.png"/> 
</figure>

<p>In lowinverse, we would consider <!-- raw HTML omitted -->c2, c3, d2, d3<!-- raw HTML omitted --> as important. Here we consider <!-- raw HTML omitted -->c4<!-- raw HTML omitted --> and <!-- raw HTML omitted -->d4<!-- raw HTML omitted --> as important too. Highinverse is nothing but a combination of lowinverse and claimeven. What this does is say <strong>W</strong> plays in <!-- raw HTML omitted -->c2<!-- raw HTML omitted -->, then using lowinverse <strong>B</strong> can get <!-- raw HTML omitted -->c3<!-- raw HTML omitted -->. Then <strong>B</strong> (controller of Zugzwang) convert <!-- raw HTML omitted -->d3<!-- raw HTML omitted --> and <!-- raw HTML omitted -->d4<!-- raw HTML omitted --> into a claimeven, to get <!-- raw HTML omitted -->d4<!-- raw HTML omitted -->. In such a way <strong>B</strong> ends up getting <!-- raw HTML omitted -->c3<!-- raw HTML omitted --> and <!-- raw HTML omitted -->d4<!-- raw HTML omitted -->.</p>
<h4 id="baseclaim">Baseclaim<a href="#baseclaim" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h4>
<p>This is a combination of Baseinverse and Claimeven:</p>
<figure>
    <img src="https://i.imgur.com/0w4iKvV.png"/> 
</figure>

<p>Here, W can possibly have 3 threats formed: <!-- raw HTML omitted -->b1-e1, c1-f1, b1-e4<!-- raw HTML omitted -->(diagonal). <strong>B</strong> needs to play in a way to counter this.</p>
<ul>
<li><strong>W</strong> plays in <!-- raw HTML omitted -->b1<!-- raw HTML omitted -->, <strong>B</strong> plays in <!-- raw HTML omitted -->e1<!-- raw HTML omitted --> and then uses Claimeven at <!-- raw HTML omitted -->c1-c2<!-- raw HTML omitted --> to prevent <!-- raw HTML omitted -->b1-e4<!-- raw HTML omitted -->.</li>
<li><strong>W</strong> plays in <!-- raw HTML omitted -->c1<!-- raw HTML omitted -->, <strong>B</strong> plays in <!-- raw HTML omitted -->e1<!-- raw HTML omitted --> and then uses Claimeven at <!-- raw HTML omitted -->b1-c2<!-- raw HTML omitted --> to prevent <!-- raw HTML omitted -->b1-e4<!-- raw HTML omitted -->.</li>
<li><strong>W</strong> plays in <!-- raw HTML omitted -->e1<!-- raw HTML omitted -->, <strong>B</strong> plays in <!-- raw HTML omitted -->c1<!-- raw HTML omitted --> and then uses Claimeven at <!-- raw HTML omitted -->b1-c2<!-- raw HTML omitted --> to prevent <!-- raw HTML omitted -->b1-e4<!-- raw HTML omitted -->.</li>
</ul>
<p>In such ways <strong>B</strong> can nullify all of <strong>W</strong>&lsquo;s threats.</p>
<h4 id="before">Before<a href="#before" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h4>
<p>This is a combination of Claimeven and Vertical:</p>
<figure>
    <img src="https://i.imgur.com/EuOTX0t.png"/> 
</figure>

<p>Here <!-- raw HTML omitted -->b4-e1<!-- raw HTML omitted --> is the Before group. Since <!-- raw HTML omitted -->b4<!-- raw HTML omitted --> and <!-- raw HTML omitted -->e1<!-- raw HTML omitted --> are still empty, this means it works for all groups needing both <!-- raw HTML omitted -->b5<!-- raw HTML omitted --> and <!-- raw HTML omitted -->e2 (b5-e2)<!-- raw HTML omitted -->. Here, before uses the squares <!-- raw HTML omitted -->b4-b5<!-- raw HTML omitted --> and <!-- raw HTML omitted -->e1-e2<!-- raw HTML omitted -->. As soon as <!-- raw HTML omitted -->b4<!-- raw HTML omitted --> is played, <!-- raw HTML omitted -->b5<!-- raw HTML omitted --> is played, and same with <!-- raw HTML omitted -->e1-e2<!-- raw HTML omitted -->. This will ensure <strong>B</strong> completing <!-- raw HTML omitted -->b4-e1<!-- raw HTML omitted --> or preventing <strong>W</strong>&lsquo;s <!-- raw HTML omitted -->b5-e2<!-- raw HTML omitted -->. In both cases <!-- raw HTML omitted -->b5-e2<!-- raw HTML omitted --> is a useless threat.</p>
<p>It basically means that if there is a before group, the opponent cannot claim all the unclaimed squares in the threat column.</p>
<h4 id="special-before">Special Before<a href="#special-before" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h4>
<figure>
    <img src="https://i.imgur.com/39qjBJM.png"/> 
</figure>

<p>We use <!-- raw HTML omitted -->d2-g2<!-- raw HTML omitted --> as the before group. This can contain claim evens at <!-- raw HTML omitted -->f1-f2<!-- raw HTML omitted --> and <!-- raw HTML omitted -->g1-g2<!-- raw HTML omitted --> and vertical at <!-- raw HTML omitted -->e2-e3<!-- raw HTML omitted -->. We need to use baseinverse to solve <!-- raw HTML omitted -->a1-d1<!-- raw HTML omitted -->, which would give <strong>W</strong> a possibility of <!-- raw HTML omitted -->b1-e4<!-- raw HTML omitted -->. To combat this, we can use claimeven to get <!-- raw HTML omitted -->e4<!-- raw HTML omitted -->. This claimeven, however, is conflicting with vertical at <!-- raw HTML omitted -->e2-e3<!-- raw HTML omitted -->.</p>
<p>The only reason <strong>B</strong> needs to play <!-- raw HTML omitted -->e3<!-- raw HTML omitted --> is to prevent <!-- raw HTML omitted -->d3-g3<!-- raw HTML omitted -->. So <strong>B</strong> can play <!-- raw HTML omitted -->d3<!-- raw HTML omitted --> as well. If <strong>W</strong> were to play at <!-- raw HTML omitted -->d3<!-- raw HTML omitted --> before, then <strong>B</strong> should immediately get <!-- raw HTML omitted -->e2<!-- raw HTML omitted --> to continue with the Before play. Therefore to play a Special Before, we need a before group <!-- raw HTML omitted -->(d2-g2)<!-- raw HTML omitted --> with one of the empty squares as directly playable <!-- raw HTML omitted -->(e2)<!-- raw HTML omitted -->. Furthermore, we need another playable square <!-- raw HTML omitted -->(d3)<!-- raw HTML omitted -->.</p>
<h4 id="combination">Combination<a href="#combination" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h4>
<figure>
    <img src="https://i.imgur.com/0aqQXCz.png"/> 
</figure>

<h2 id="-black-and-white">⚫⚪ Black and White<a href="#-black-and-white" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h2>
<h4 id="black">Black<a href="#black" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h4>
<p>We have developed a set of rules which can be used to show that certain potential threats can be refuted. Since some of the rules depend on Zugzwang, it is important that the person who applies them is in control of the Zugzwang.</p>
<p><strong>B</strong> is in control of Zugzwang until <strong>W</strong> creates an odd threat. Till then if <strong>B</strong> just plays using the strategy. If <strong>W</strong> were to create a good threat (odd threat), <strong>B</strong> is no more in control of Zugzwang. Here we observe that no matter what <strong>B</strong> does from here on out, there generally will not be any set of rules which can refute that threat.</p>
<p>From this we can conclude that we do not need to check who controls the Zugzwang for <strong>B</strong> before applying the rules. For if <strong>B</strong> is in control, we can apply the rules, if not, it doesn&rsquo;t matter what <strong>B</strong> does.</p>
<h4 id="white">White<a href="#white" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h4>
<p><strong>W</strong> needs an odd threat to gain control of Zugzwang. Once it has that, he just needs to follow the strategic rules to fill up the rest of the board. If <strong>W</strong> has more than one odd threat, it can choose from which poison to kill <strong>B</strong> from.</p>
<h4 id="victor">Victor<a href="#victor" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h4>
<p>A position in which <strong>W</strong> has to move, can be evaluated as <strong>B</strong> as controller of Zugzwang, and vice versa. For <strong>W</strong> as a controller of Zugzwang, evaluation must be done removing the odd column out of viable options. The evaluation begins with finding all possible instances of the 9 strategic rules.</p>
<p>For each position where any rule is applied, it is seen whether it can solve a problem or not. Each application of one of the rules which solve one or more problems is stored. These are called Solutions. This results in a list of solutions, where each solution is stored as a Struct. Struct consists of fields describing the rule, and the positions involved. Furthermore for each solution we have a list of groups solved by that solution.</p>
<p>We also create a map with problem as the key and list of pointer(s) to the solution as the values. After all this, we need to see which solutions can work together and which cannot. To work this out, solutions are seen as nodes of an undirected graph. If two solutions can&rsquo;t be used simultaneously, they are connected. These connections are stored in an adjacency matrix. To fill it, it is important to know type of solutions and squares involved. Once it is filled, it is a normal square array.</p>
<p>If we see the problems as nodes, too, and we connect a solution and a problem if the solution solves the problem, and no problems are connected, we can solve it as a pure graph problem.</p>
<p>Given are two sets of nodes, S(olutions) and P(roblems). We try to find an allowable (in graph theory: independent) subset C(hosen) of S, with the property that P is contained in B(C) (the set of all neighbours of nodes in C)</p>
<p>It can be solved using a simple backtracking algorithm.:</p>
<div class="highlight"><pre class="chroma"><code class="language-cpp" data-lang="cpp"><span class="kt">void</span> <span class="nf">FindChosenSet</span><span class="p">(</span><span class="n">P</span><span class="p">,</span> <span class="n">S</span><span class="p">)</span> <span class="p">{</span>
    <span class="k">if</span> <span class="p">(</span><span class="n">P</span> <span class="o">==</span> <span class="n">EmptySet</span><span class="p">)</span> <span class="p">{</span>
        <span class="n">Eureka</span><span class="p">();</span> <span class="c1">// We have found a subset C
</span><span class="c1"></span>    <span class="p">}</span> <span class="k">else</span> <span class="p">{</span>
        <span class="n">MostDifficultNode</span> <span class="o">=</span> <span class="n">NodeWithLeastNumberOfNeighbours</span><span class="p">(</span><span class="n">P</span><span class="p">);</span>
        <span class="k">for</span> <span class="p">(</span><span class="k">auto</span> <span class="nl">neighbours</span><span class="p">:</span> <span class="n">MostDifficultNode</span><span class="p">)</span> <span class="p">{</span>
            <span class="n">FindChosenSet</span><span class="p">(</span>
            <span class="n">P</span> <span class="o">-</span> <span class="p">{</span> <span class="n">MostDifficultNode</span> <span class="p">},</span>
            <span class="n">S</span> <span class="o">-</span> <span class="n">AllNeighboursOf</span><span class="p">(</span><span class="n">ChosenNeighbour</span><span class="p">)</span>
            <span class="p">);</span>
        <span class="p">}</span>
    <span class="p">}</span>
<span class="p">}</span>
</code></pre></div><p>If a set of solutions is found for a given position, these <strong>solutions show the plan which has to be followed to play the game</strong> until the desired result (win for White, or at least a draw for Black) is reached.</p>
<h2 id="-food-for-thought">🧠 Food for Thought<a href="#-food-for-thought" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h2>
<p>Lets assume we have an oracle. This oracle cannot predict who will win, but for any given state of board, give the best possible outcome. Let us assume that we play such that for each state of board, we ask for help from the oracle. <strong>Therefore it is a &lsquo;perfect&rsquo; game. In such case, if W wins, does that mean W will always win if it were a perfect game?</strong></p>
<p>The thing is, if for B we were to choose that draw is fine, it will not change any result. Since oracle predicts the best move, if the second scenario gives different result, that would mean Oracle could have chosen the best move, but did not. That is contradictory. That means that whoever will win with the help of the Oracle, is always at an advantage.</p>
<p>Another thought to explore is number of legal ways to arrange $N$ coins in board. For now, we take the board to be the standard $7 \times 6$ board.</p>
<figure>
    <img src="https://i.imgur.com/EVrx7DC.png"/> 
</figure>

<h2 id="-references">📝 References<a href="#-references" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h2>
<ul>
<li>Resources by Victor Allis</li>
<li>Alpha Beta Pruning based heuristics</li>
<li>Principles and Tehniques BY Stanford</li>
<li>C4 Numbers by oeis org</li>
<li>Math oriented resources behind Connect-4</li>
</ul>
