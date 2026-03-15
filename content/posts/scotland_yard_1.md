---
title: "Scotland Yard: Part 1"
date: 2020-11-22T12:00:06+09:00
tags:
  - game
  - paper
---

<p>This blog contains an explanation of the paper <a href="https://citeseerx.ist.psu.edu/viewdoc/download;jsessionid=A042C241BA640121A461FBF99CD93FEC?doi=10.1.1.76.9583&amp;rep=rep1&amp;type=pdf">&lsquo;The complexity of Scotland Yard&rsquo;</a> by Merlijn Sevenster.</p>
<p>This is part 1 of a two part blog. Part 1 explains the game, and lays out the foundations required for formalisation of the game. It also lists out the various assumptions we are going to consider in the game.</p>
<p>The blog was originally published on <a href="https://captains-mistress.github.io/scotlandyard">&lsquo;GameLab&rsquo;</a> for our course project. The website also contains additional information about heuristics used in Scotland Yard, and similar analysis for other board games.</p>
<h2 id="-introduction">📖 Introduction<a href="#-introduction" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h2>
<p>Scotland yard is a popular board game, named after Scotland Yard, the headquarters of London&rsquo;s Metropolitan Police Service. It is played by 2-6 players in which one of the players is the criminal (Mr. X) and the remaining are detectives. As is intuitive, the objective of the game for the detectives is to catch the criminal, while the objective of the criminal is to run away from its pursuers for 22 rounds. <strong>It is an asymmetric board game since the players do not have the same goal.</strong></p>
<h2 id="-rules">📜 Rules<a href="#-rules" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h2>
<p>Let&rsquo;s take a look at the rules once, before we jump into analysing the game.</p>
<h4 id="movement-of-players">Movement of players:<a href="#movement-of-players" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h4>
<p>Each detective and the criminal is assigned a pawn to mark their position on the board. There are a <strong>total of 199 positions on the Scotland Yard board</strong>. Each position can represent 1-3 stations, a taxi, a bus, and an underground route, which is marked by a number and the colour of the station it represents.</p>
<p><strong>Every station on the map can be reached with the taxi</strong> (yellow). However, the distance that you can travel is short: You can only move (along the yellow line) to the next station.</p>
<p>The bus (turquoise) <strong>only drives from stations with a turquoise semi-circle</strong>; a bus will take you a little further than the taxi (along the bus line).</p>
<p>The underground (red) <strong>travels along the red line and covers the furthest distances the quickest</strong>. However, there are only a few underground stations (stations with a red inner rectangle) on the map.</p>
<p>All playing pieces can only be moved to unoccupied stations. <strong>If there are no unoccupied stations for Mister X to travel to, he has lost the game. Mister X also loses if one of the detectives moves to the station where Mister X is located.</strong></p>
<figure>
    <img src="https://i.imgur.com/AonWaMh.png"/> 
</figure>

<h4 id="tickets">Tickets<a href="#tickets" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h4>
<p>Each detective receives ticket cards that allow him to across the board. At the start of the game, each detective gets <strong>4 underground tickets, 8 bus tickets, and 11 taxi tickets, and each detective receives 5 black tickets and 2 double move tickets.</strong></p>
<p>In each round, after <strong>a detective</strong> has used up a ticket to travel to another position, <strong>they cannot use them again</strong>, however, this ticket is now available for Mr. X&rsquo;s use. If a detective no longer has any tickets or can&rsquo;t move from his current station with the tickets he has left, they have to sit out.</p>
<p>A black ticket allows Mr.X to hide the route he used and also travel by ferry (a route only he is allowed), and a double move ticket allows him to make two moves to two different stations in one round.</p>
<h4 id="initial-starting-position">Initial Starting Position<a href="#initial-starting-position" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h4>
<p>To determine each player’s starting position, a set of start cards marked D and X are shuffled separately and each detective selects from the D cards and places their playing piece on the respective position. Mr. X picks an X card but doesn’t reveal his position to the detectives or place his playing piece on the board.</p>
<h4 id="gameplay">Gameplay<a href="#gameplay" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h4>
<p>In total, <strong>22 rounds are played</strong>. In each round, Mr. X first makes their move, concealing his new position from the remaining players, and writes down the station he moved to on a paper, hiding the station with the ticket they used. (The detectives can see which mode of transport Mr.X has used.)</p>
<p>When a detective makes a move, the used-up ticket is placed in the general draw pile where Mr. X gets his tickets (<strong>so Mr. X basically has an unlimited number of tickets</strong>).</p>
<h4 id="moving-mr-x">Moving Mr. X<a href="#moving-mr-x" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h4>
<p>Each turn Mr. X conceals his move. However, there are special moves (<strong>3rd, 8th,13th, 18th, and 24th moves</strong>) where Mr. X must surface. He has to reveal his current position before moving to a new station, which gives detectives the chance to co-ordinate and corner the criminal!</p>
<h4 id="moving-the-detectives">Moving the Detectives<a href="#moving-the-detectives" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h4>
<p>The detectives use their tickets to move around the board. If they run out of tickets or don&rsquo;t have the required ticket to move out of a station, they must sit out of the game. Detectives can&rsquo;t trade tickets among themselves and all their remaining tickets have to be visible to Mr. X, so he can see the remaining means of transportation they have left.</p>
<h4 id="winning-the-game">Winning the Game<a href="#winning-the-game" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h4>
<p>Mr. X wins if -</p>
<ul>
<li>He is able to move around the board for 22 rounds without being caught.</li>
</ul>
<p>The detectives win if -</p>
<ul>
<li>They corner Mr. X (he has no stations to go to where a detective is not present)</li>
<li>They move to a station where Mr. X is currently</li>
</ul>
<p>Now that we understand how to play, let&rsquo;s dive into different aspects of the game!</p>
<h2 id="-objective">🎯 Objective<a href="#-objective" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h2>
<p>The goal of this page is to analyze the game Scotland Yard.</p>
<p>We start off by venturing into proofs for <strong>Scotland Yard being a PSPACE problem and the similarities between Scotland Yard and a game of perfect information</strong>. It is easy to feel daunted by these claims, trust me I felt it too. To make it easier, we remove all the layers of abstraction from the game first. We convert the game into a problem of Groups, Graphs, and Sets.</p>
<p>It is understandable if you think it still is going to be tough. We ensure that you will understand what these jargons are and how they interact with the game itself. We firstly introduce Games with the viewpoint of perfect and imperfect information. Then we connect Scotland Yard to that idea and remove all the layers of abstraction. Once that is done, we proceed with proofs.</p>
<h2 id="-laying-the-foundation">🏗️ Laying the Foundation<a href="#-laying-the-foundation" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h2>
<p>To properly explain some concepts, we need to define some terms:</p>
<p><strong>Extensive Games</strong>: Games that allow the representation of various key aspects. These aspects include a set of players, each player&rsquo;s moves, their decisions, the information (possibly imperfect) about a player, and their payoffs for all possible outcomes. Essentially, they are games that can be represented with a game (decision) tree.</p>
<p><strong>Perfect Information Game</strong>: In a perfect information game, a player has complete information about all events which have previously occurred in the game.</p>
<p><strong>Imperfect Information Game</strong>: Games which have some aspects of the game hidden are called imperfect information game.</p>
<p>It is easily understandable why Scotland Yard comes under the bracket of the extensive game with imperfect information. For it to be an extensive game, it should formally represent each and every aspect of the game, which is the moves and mode of commutation each player uses. Even if the moves are hidden, they are definite and are represented. Since the moves are hidden, it is impossible for the detectives to know which route the criminal has taken, which makes it an imperfect information game.</p>
<p>Now, let&rsquo;s look into some abstraction or representations.</p>
<p>Any win-loss game $G$ with perfect information can be represented as a 4-tuple
$$G = &lt; N, H, P, U&gt;$$</p>
<ul>
<li>\( N \): Represents the number of total players.</li>
<li>\(H\):  Is a set of histories. A history (\(h\)) represents a given state of the board at some point in time. Every \(h = &lt;a_1, a_2, . . a_p &gt; \), where \(a_i\) is an action. Each history is an ordered list of actions.</li>
<li>\(h&rsquo;\) = \(&lt;h, a'&gt;\) represents the immediate successor of \(h\), where \(h&rsquo;\) = \(&lt; a_1, a_2 . . . a_p, a'&gt;\). There are two types of history, terminal (\(Z\)) and non-terminal (\(H-Z\)). Terminal represents an end condition, after which no other action can be taken. A history becomes terminal when a player wins.</li>
<li>\(P\): Is the player function. It assigns to each non-terminal history a particular player. Formally, we define it as \(P:\) { \(H - Z\) } \(\rightarrow N\). We say that a history \(h\) belongs to \(P(h)\), essentially when the last action in the set of actions that is \(h\) is made by the player.</li>
<li>\(U\): Is the utility function, assigning each terminal history to a player. (the player has won the game). The formal definition would be \(U: Z \rightarrow N\)</li>
</ul>
<p>Given \(G\) defined as above, a function \(S\) is called a strategy for a player \(\space i \in N i∈N\) if it maps for every history \(h\) belonging to \(i\) to an action \(A(h)\).</p>
<p>An extensive game with imperfect information extends a game with perfect information. To represent the former, all you need is to add is an Information function in the original tuple.</p>
<p>$$G = &lt; N, H, P, \langle \mathcal{I}_i \rangle _{i \in N}, U &gt;$$</p>
<p>The only difference in this is that \(\mathcal{I}_i\) carries information sets for each \(i \in N\). \(\mathcal{I}_i =\) {\(I_1, I_2, . . . I_q\)}. where each \(I\) represents a set of histories, there having been \(q\) rounds of the game played so far. Each \(I\) basically is a set of histories (or state changes of the board) of that round (till \(i\) makes an action again). Intuitively, an extensive game with imperfect information models the situation in which player \(i\) knows that some history \(h \in I \in \mathcal{I}_i\) has happened, but there are unable to tell h apart from the other histories in \(I\).</p>
<p>In simple terms, they know other players have made a move based on the last action they took, but are not completely sure of the previous actions the player took.</p>
<p>A function \(S\) is called a strategy for a player \(i\) in \(G\) if it maps every information partition \(I \in \mathcal{I}_i\) belonging to \(i\) onto action in \(A(I)\)</p>
<h4 id="assumptions-for-mathematical-modelling">Assumptions for Mathematical Modelling<a href="#assumptions-for-mathematical-modelling" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h4>
<p>For convenience, there are some assumptions which have been taken.</p>
<ol>
<li><strong>There is only 1 mode of transport</strong>, that is Taxi. The same method described as follows can be easily translated with more modes of transport.</li>
<li>A player will have \(k\) amount of tickets of Taxi, where \(k\) = number of rounds.</li>
<li>There are only two players, Detective and Mr X.</li>
<li>Only 1 player will be controlling all detectives.</li>
<li>Value of \(k\) will be always \(\leq |V|\), where \(V\) is the number of nodes in the graph.</li>
<li>We have used digraph to represent the game board.</li>
<li><strong>Mr. X will always play the first move in each round.</strong></li>
<li>Mr. X will be considered to be caught IF AND IF ONLY it is on a node occupied by a detective at the END of the round (after detectives have moved).</li>
<li>Mr X will win if and if only the game goes on for \(&gt;k\) rounds, otherwise Detectives have won.</li>
</ol>
<h2 id="-references">📝 References<a href="#-references" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h2>
<ul>
<li>
<p>P. Nijssen and M. H. M. Winands, &ldquo;Monte Carlo Tree Search for the Hide-and-Seek Game Scotland Yard,&rdquo; in IEEE Transactions on Computational Intelligence and AI in Games, vol. 4, no. 4, pp. 282-294, Dec. 2012, doi: 10.1109/TCIAIG.2012.2210424.</p>
</li>
<li>
<p>Sevenster, Merlijn. (2006). The complexity of Scotland Yard. Journal of Pharmacology and Experimental Therapeutics - J PHARMACOL EXP THER.</p>
</li>
</ul>
