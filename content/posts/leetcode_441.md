---
title: "Arranging Coins"
date: 2021-11-05T08:31:41+05:30
tags:
  - daily
  - math
---

<p>So this is probably the simplest weekly question out there <em>if</em> you know the math required. I will just write down my thoughts so I know how I approached the problem later on, and not what the solution is per se.</p>
<h2 id="problem">Problem:<a href="#problem" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h2>
<p><strong>Link</strong>: <a href="https://leetcode.com/problems/arranging-coins/">https://leetcode.com/problems/arranging-coins/</a></p>
<p><strong>Statement</strong>: You have n coins, and you want to build a staircase with these coins. The staircase consists of k rows, where ith row has exactly i coins. The last row may be incomplete. Given the integer n, return the number of complete rows of staircase you will build.</p>
<p><strong>Example</strong>:</p>
{{< highlight text >}}
Input: n = 8
Output: 3
Explanation:
1st row: 1 coin 
2nd row: 2 coins
3rd row: 3 coins
4th row: 2 coins

as can be seen, row 1, row 2 and row 3 are the only complete rows
{{< /highlight >}}

<p><strong>Constraints</strong>:</p>
{{< highlight text >}}
1 <= n <= 2^31 - 1
{{< /highlight >}}<figure>
    <img src="https://i.imgur.com/SsWiiKs.png"/> 
</figure>

<h2 id="solution">Solution<a href="#solution" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h2>
<p><strong>Time Complexity: O(\(1\))</strong></p>
<p><strong>Thought Process</strong>: I think the first thing one must learn via this problem is how to determine complexity based on constraints. See, here the largest value of n can go upto 2^31, which, frankly, we do not know how much it is just by looking at it. So a cool conversion trick can be used :p, just remember this one thingy</p>
<p>$$2^{10} = 1024 \approx 1000 = 10^3$$</p>
<p>Using this, we see that \( 2^{31} \approx {(10^3)}^3 = 10^9 \). Now, we know that for our computer, the linear time limit is <strong>\(10^7-10^8\)</strong>, so our solution has to be either O(\(logN\)) or O(\(1\)). Well, the constant time solution was the first which came to my mind.</p>
<p><strong>Explanation</strong>: So, lets take a general case where we have <code>x</code> coins which forms <code>r</code> rows, and we need to determine what this <code>r</code> is.
We know that each row takes i coins, and we can sum them up to get the total number of coins required to stack r rows. Here, we just get the total number of coins, but we havent, yet, taken into consideration the condition where <code>last row need not be full</code>.
<figure>
    <img src="https://i.imgur.com/Oo9TBAx.png"/> 
</figure>
</p>
<p>Here, a formula is used for summation of natural numbers. The proof of the formula is simple enough, and I am writing it down in case one does not understand it:
<figure>
    <img src="https://i.imgur.com/6M2GXDw.png"/> 
</figure>
</p>
<p>Using this formula, we get our answer. But first, lets factor in the condition of last row need not be full, we just make the formula less than or equal to x, since any value less than that is cool as well in our case. :</p>
<figure>
    <img src="https://i.imgur.com/8TI422Y.png"/> 
</figure>

<p>The formula is the last part of the answer, but we just need to use a bit of common sense to realise that the number of coins cannot be negative, nor can it be a decimal value. Hence we just take the int value of the answer and boom, hence proved :p.</p>
<h2 id="code">Code:<a href="#code" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h2>
{{< highlight python >}}
import math
class Solution:
    def arrangeCoins(self, n: int) -> int:
        ## equation: r^2 + r - 2n <= 0
        ## quadratic formula: (-b + sqrt(b^2 - 4ac))/2
        ## ans: (-1 + sqrt(1 + 8n))/2
        return (int)((-1 + math.sqrt(1 + 8*n))/2)
{{< /highlight >}}
