---
title: "Longest Substring Without Repeating Characters"
date: 2021-11-01T10:28:11+05:30
archived: true
tags:
  - leetcode
  - string
---

<p>Hey, so the whole idea of me posting leetcode solutions is not because I think I can explain better than thousands of others out there. Its just a place where I write down my thoughts and explanations so that I remember different techniques I worked through :)</p>
<h2 id="problem">Problem<a href="#problem" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h2>
<p><strong>Link</strong>: <a href="https://leetcode.com/problems/longest-substring-without-repeating-characters/">https://leetcode.com/problems/longest-substring-without-repeating-characters/</a></p>
<p><strong>Statement</strong>: Given a string <code>s</code>, find the length of the longest substring without repeating characters.</p>
<p><strong>Example</strong>:</p>
{{< highlight text >}}
Input: s = "abcabcbb"
Output: 3
Explanation: The answer is "abc", with the length 3. 
{{< /highlight >}}

<p><strong>Constraints</strong>:</p>
{{< highlight text >}}
0 <= s.length <= 5*1e4
{{< /highlight >}}<p><strong>Note</strong>: It is important to see that the question asks for substring (the characters must be continous) and not subsequence.</p>
<h2 id="solution-1-binary-search--linear-search">Solution 1: Binary Search + Linear Search<a href="#solution-1-binary-search--linear-search" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h2>
<p><strong>Time Complexity: O(\(nlogn\))</strong></p>
<p>So I will just explain the solution as nicely as possible. I wont give code for this since I believe you should write it yourself for practice (there is no sense if once just googles code for a given problem). Although, for questions which are difficult I would probably provide some snippets to enable understanding.</p>
<p><strong>Thought Process</strong>: Well, we need to find length of longest substring but I cannot go do O(\(n^2\)). O(\(n\)) seems nice but complicated, I will leave that for now. So, looking at the string I see that it would be nice if I could just linearly search through all of them and see which one is the answer, but issue is that I cannot search through all <code>n</code> possibilities that way.
So, I only search through selected values of <code>n</code> so that the complexity does not rise too much. Now, one thing I know is if there is no answer for <code>x</code> length, there is no answer for any length <code>&gt;x</code>. So, boom, this looks like a job for binary search.</p>
<p><strong>Explanation</strong>
So, we are doing binary search to find the required length of substring, and then doing linear search to confirm the condition (all characters must be unique). Naturally, if there exists even 1 substring which satisfies this condition, we say that the x satisfies the condition.
Now, lets take our example of <code>abcabcbb</code>. Our first <code>x=4</code>, for which the answer is:
<figure>
    <img src="https://i.imgur.com/VoRoQdT.png"/> 
</figure>
</p>
<p>Now, our search range is between <code>0 -&gt; 3</code>. Our next <code>x=1</code>, and it is obvious that satisfies condition. So now our search range is between <code>1 -&gt; 3</code>, and our next x is <code>x=2</code>. Again, its obvious this satisfies the condition. So our final range is between <code>2 -&gt; 3</code>. Now, lets check for <code>x=3</code>.
<figure>
    <img src="https://i.imgur.com/NtMC8E2.png"/> 
</figure>
</p>
<p>As we can see, our answer is 3.</p>
<h2 id="solution-2-pointer-with-dictionary">Solution 2: Pointer with Dictionary<a href="#solution-2-pointer-with-dictionary" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h2>
<p><strong>Time Complexity: O(\(n\))</strong></p>
<p><strong>Thought Process</strong>: So we need O(\(n\)). One thing you must note is that when we are the ones trying to find the answer (as humans, unless you are a bot, I guess you are also welcome here? idk) is that we go linearly from left to right, and whenever a repeating character comes up, we cut off from where it previously occured, and continue. Neat. This will be our algorithm.</p>
<p><strong>Explanation</strong>: Consider a dictionary <code>m</code> and a left pointer. Whenever I get a new character never seen before, I insert it into the dictionary with the index where it occured as its value. Whenever I see a character already in the dictionary, I make note of the current substring, and then update the value of <code>key</code> in dictionary with <code>value = i</code>. Simple enough?</p>
<figure>
    <img src="https://i.imgur.com/ZMThPq6.png"/> 
</figure>

<p>A couple of things we do need to note is that
a) Well, what about the end?: Simple enough, we add a line which goes from my left pointer (I will tell you the use of this soon) and current index. This will be my last substring which satisfies the condition
b) Uh, what if I get something like <code>xyyx</code>, where I have to cutoff after the first index. Fret not, this has been taken care of. We just make sure that any index I consider has to be after left at all costs.</p>
<figure>
    <img src="https://i.imgur.com/nXyuRDi.png"/> 
</figure>

<p>So, in this case I feel a bit of code is important.</p>
{{< highlight python >}}
# Left is simply an int which remembers where my substring is starting from
# m is my dictionary
# sollen stores the max answer (initialised to 0)
for i in range(len(s)):
    # check if character exists 
    if s[i] in m:
       # if it does, then check if it occurs before left
       # (before it was cutoff)
       if m[s[i]] < left:
          # if it was before it was cutoff, then my substring is valid 
          # I just need to update its value
          m[s[i]] = i
       else:
          # If not, then I need to cut my substring now
          # Store the length of substring
          sollen = max(sollen, i - left)
          # Update my left pointer to next occurance of repeated character
          left = m[s[i]] + 1
          # Update value of repeated character
          m[s[i]] = i

# Check if the last substring might be the answer?          
sollen = max(sollen, len(s) - left)
return sollen
{{< /highlight >}}
