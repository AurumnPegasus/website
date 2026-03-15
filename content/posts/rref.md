---
title: "Reduced Row Echelon Form"
date: 2021-10-13T12:00:06+09:00
tags:
  - math
  - ml
---

<p>The blog explains how to find reduced row-echelon form of a matrix in python</p>
<h2 id="introduction">Introduction<a href="#introduction" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h2>
<p>A matrix being in a row-echelon form means that <a href="https://en.wikipedia.org/wiki/Gaussian_elimination">Gaussian elimination</a> has operated on the rows. A matrix is said to be in column-echelon form if its transpose is in row-echelon form. The specific conditions of RE are:</p>
<ul>
<li>All rows consisting of only zeros are at the bottom</li>
<li>The leading coefficient (pivot) of a non zero row is always to the right of the leading coefficient of the row above it</li>
</ul>
<p>RREF (Reduced Row-Echelon Form) has 2 more conditions:</p>
<ul>
<li>Matrix must be in RE form</li>
<li>All pivots must be 1</li>
<li>Each column containing a leading 1 has zeros everywhere else.</li>
</ul>
<p>One might achieve the RREF via elementary row operations, and here is a code for that!</p>
<h2 id="code">Code<a href="#code" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h2>
<p>Essentially, we want to do this:</p>
<div class="highlight"><pre class="chroma"><code class="language-Python" data-lang="Python"><span class="k">def</span> <span class="nf">rref</span><span class="p">(</span><span class="n">matrix</span><span class="p">):</span>
  <span class="n">dimension</span> <span class="o">=</span> <span class="n">matrix</span><span class="o">.</span><span class="n">shape</span>
  <span class="n">numrows</span> <span class="o">=</span> <span class="n">dimension</span><span class="p">[</span><span class="mi">0</span><span class="p">]</span>
  <span class="n">numcols</span> <span class="o">=</span> <span class="n">dimension</span><span class="p">[</span><span class="mi">1</span><span class="p">]</span>
  <span class="n">lead</span> <span class="o">=</span> <span class="mi">0</span> <span class="c1"># or pivot</span>
  <span class="k">for</span> <span class="n">r</span> <span class="ow">in</span> <span class="nb">range</span><span class="p">(</span><span class="n">numrows</span><span class="p">):</span>
      <span class="c1"># &lt;END: if you reach last column&gt;</span>

      <span class="c1"># &lt;LOOP: to find next pivot&gt;</span>

      <span class="c1"># &lt;CONVERT: convert all other rows based on pivot&gt;</span>
</code></pre></div><p>Now, lets write code for each one by one. The first part is END, which is essentially that if you dont have any more columns, it means that the matrix is already in a RREF state, hence return.</p>
<div class="highlight"><pre class="chroma"><code class="language-Python" data-lang="Python"><span class="c1"># END:</span>
<span class="k">if</span> <span class="n">lead</span> <span class="o">&gt;=</span> <span class="n">numcols</span><span class="p">:</span>
  <span class="k">print</span><span class="p">(</span><span class="bp">self</span><span class="o">.</span><span class="n">matrix</span><span class="p">)</span>
  <span class="k">return</span>
</code></pre></div><p>The loop parts requires you to go through each column in the row to determine if the number is suitable to be a pivot. If you find none, then go to the next column! (this means current column was all zeros and must be put at last). If there are no pivots at all to be found, that means you have already reached RREF</p>
<div class="highlight"><pre class="chroma"><code class="language-Python" data-lang="Python"><span class="c1"># LOOP:</span>
<span class="n">index</span> <span class="o">=</span> <span class="n">r</span>
<span class="k">while</span> <span class="n">matrix</span><span class="p">[</span><span class="n">index</span><span class="p">][</span><span class="n">lead</span><span class="p">]</span> <span class="o">=</span> <span class="mi">0</span><span class="p">:</span>
  <span class="n">index</span> <span class="o">+=</span> <span class="mi">1</span>
  <span class="k">if</span> <span class="n">numrows</span> <span class="o">==</span> <span class="n">index</span><span class="p">:</span>
      <span class="c1"># No pivots in my row, all zeros</span>
      <span class="n">lead</span> <span class="o">+=</span> <span class="mi">1</span>
      <span class="k">if</span> <span class="n">lead</span> <span class="o">==</span> <span class="n">numcols</span><span class="p">:</span>
          <span class="c1"># No pivots at all</span>
          <span class="k">print</span><span class="p">(</span><span class="n">matrix</span><span class="p">)</span>
          <span class="k">return</span>

<span class="c1"># Swap if I had to go to the next column</span>
<span class="n">matrix</span><span class="p">[[</span><span class="n">index</span><span class="p">,</span> <span class="n">r</span><span class="p">],</span> <span class="p">:]</span> <span class="o">=</span> <span class="n">matrix</span><span class="p">[[</span><span class="n">r</span><span class="p">,</span> <span class="n">index</span><span class="p">],</span> <span class="p">:]</span>
</code></pre></div><p>I have my pivot now <code>matrix[r][lead]</code>, which I need to convert to 1 via elementary row operations. Once that is done, everything else in the column needs to be converted to 0s as well</p>
<div class="highlight"><pre class="chroma"><code class="language-Python" data-lang="Python"><span class="c1"># CONVERT:</span>

<span class="c1"># R = R/k</span>
<span class="n">matrix</span><span class="p">[</span><span class="n">r</span><span class="p">]</span> <span class="o">=</span> <span class="n">matrix</span><span class="p">[</span><span class="n">r</span><span class="p">]</span> <span class="o">/</span> <span class="n">matrix</span><span class="p">[</span><span class="n">r</span><span class="p">][</span><span class="n">lead</span><span class="p">]</span>

<span class="k">for</span> <span class="n">i</span> <span class="ow">in</span> <span class="nb">range</span><span class="p">(</span><span class="n">numrows</span><span class="p">):</span>
  <span class="k">if</span> <span class="n">i</span> <span class="o">!=</span> <span class="n">r</span><span class="p">:</span>
      <span class="c1"># I = I - cR</span>
      <span class="n">matrix</span><span class="p">[</span><span class="n">i</span><span class="p">]</span> <span class="o">=</span> <span class="n">matrix</span><span class="p">[</span><span class="n">i</span><span class="p">]</span> <span class="o">-</span> <span class="n">matrix</span><span class="p">[</span><span class="n">i</span><span class="p">][</span><span class="n">lead</span><span class="p">]</span><span class="o">*</span><span class="n">matrix</span><span class="p">[</span><span class="n">r</span><span class="p">]</span>
</code></pre></div><h2 id="walkthrough">🚶Walkthrough:<a href="#walkthrough" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h2>
<div class="highlight"><pre class="chroma"><code class="language-text" data-lang="text">matrix: array([[ -12,  12,   2],
               [ 12, -12,  -2],
               [  2,  -2, -17]])
------------------------------------------------------------------------

Iteration 1:
lead = 0, i = 0, r = 0

After Swapping:
matrix: array([[ -12,  12,   2],
               [ 12, -12,  -2],
               [ 2,  -2, -17]])

Dividing by -12:
matrix: array([[ 1,  -1,   0],
               [ 12, -12,  -2],
               [ 2,  -2, -17]])

matrix[i] = [ 12 -12  -2] - 12*[ 1 -1  0]
matrix[i] = [ 12 -12  -2] - 12*[ 1 -1  0]

After Iteration 1:
matrix: array([[ 1,  -1,   0],
               [ 0,   0,  -2],
               [ 0,   0, -17]])


------------------------------------------------------------------------

Iteration 2:
lead = 1, i = 1, r = 1

After Swapping:
array([[ 1,  -1,   0],
       [ 0,   0,  -2],
       [ 0,   0, -17]])

Dividing by -2:
matrix: array([[ 1,  -1,   0],
               [ 0,   0,   1],
               [ 0,   0, -17]])

matrix[i] = [ 1 -1  0] - 0*[0 0 1]
matrix[i] = [  0   0 -17] - -17*[0 0 1]

After Second Iteration:
matrix: array([[ 1, -1,  0],
               [ 0,  0,  1],
               [ 0,  0,  0]])
------------------------------------------------------------------------

Third Iteration:

since lead &gt;= numcols, exit
</code></pre></div><h2 id="final-code">Final Code:<a href="#final-code" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h2>
<div class="highlight"><pre class="chroma"><code class="language-Python" data-lang="Python"><span class="kn">import</span> <span class="nn">numpy</span> <span class="kn">as</span> <span class="nn">np</span>

<span class="k">class</span> <span class="nc">RREF</span><span class="p">:</span>
  <span class="k">def</span> <span class="fm">__init__</span><span class="p">(</span><span class="bp">self</span><span class="p">,</span> <span class="n">matrix</span><span class="p">):</span>
      <span class="bp">self</span><span class="o">.</span><span class="n">matrix</span> <span class="o">=</span> <span class="n">matrix</span>

  <span class="k">def</span> <span class="nf">rref</span><span class="p">(</span><span class="bp">self</span><span class="p">):</span>
      <span class="n">dimension</span> <span class="o">=</span> <span class="bp">self</span><span class="o">.</span><span class="n">matrix</span><span class="o">.</span><span class="n">shape</span>
      <span class="n">numrows</span> <span class="o">=</span> <span class="n">dimension</span><span class="p">[</span><span class="mi">0</span><span class="p">]</span>
      <span class="n">numcols</span> <span class="o">=</span> <span class="n">dimension</span><span class="p">[</span><span class="mi">1</span><span class="p">]</span>
      <span class="n">lead</span> <span class="o">=</span> <span class="mi">0</span>
      <span class="k">for</span> <span class="n">r</span> <span class="ow">in</span> <span class="nb">range</span><span class="p">(</span><span class="mi">0</span><span class="p">,</span> <span class="n">numrows</span><span class="p">):</span>
          <span class="k">if</span> <span class="n">numcols</span> <span class="o">&lt;=</span> <span class="n">lead</span><span class="p">:</span>
              <span class="k">print</span><span class="p">(</span><span class="bp">self</span><span class="o">.</span><span class="n">matrix</span><span class="p">)</span>
              <span class="k">return</span>

          <span class="n">index</span> <span class="o">=</span> <span class="n">r</span>
          <span class="k">while</span> <span class="bp">self</span><span class="o">.</span><span class="n">matrix</span><span class="p">[</span><span class="n">index</span><span class="p">][</span><span class="n">lead</span><span class="p">]</span> <span class="o">==</span> <span class="mi">0</span><span class="p">:</span>
              <span class="n">index</span> <span class="o">+=</span> <span class="mi">1</span>
              <span class="k">if</span> <span class="n">numrows</span> <span class="o">==</span> <span class="n">index</span><span class="p">:</span>
                  <span class="n">index</span> <span class="o">=</span> <span class="n">r</span>
                  <span class="n">lead</span> <span class="o">+=</span> <span class="mi">1</span>
                  <span class="k">if</span> <span class="n">lead</span> <span class="o">==</span> <span class="n">numcols</span><span class="p">:</span>
                      <span class="k">print</span><span class="p">(</span><span class="bp">self</span><span class="o">.</span><span class="n">matrix</span><span class="p">)</span>
                      <span class="k">return</span>

          <span class="c1"># swap if my row is all 0s</span>
          <span class="bp">self</span><span class="o">.</span><span class="n">matrix</span><span class="p">[[</span><span class="n">index</span><span class="p">,</span> <span class="n">r</span><span class="p">],</span> <span class="p">:]</span> <span class="o">=</span> <span class="bp">self</span><span class="o">.</span><span class="n">matrix</span><span class="p">[[</span><span class="n">r</span><span class="p">,</span> <span class="n">index</span><span class="p">],</span> <span class="p">:]</span>

          <span class="c1"># make my first number = 1</span>
          <span class="bp">self</span><span class="o">.</span><span class="n">matrix</span><span class="p">[</span><span class="n">r</span><span class="p">]</span> <span class="o">=</span> <span class="bp">self</span><span class="o">.</span><span class="n">matrix</span><span class="p">[</span><span class="n">r</span><span class="p">]</span> <span class="o">/</span> <span class="bp">self</span><span class="o">.</span><span class="n">matrix</span><span class="p">[</span><span class="n">r</span><span class="p">][</span><span class="n">lead</span><span class="p">]</span>

          <span class="k">for</span> <span class="n">i</span> <span class="ow">in</span> <span class="nb">range</span><span class="p">(</span><span class="n">numrows</span><span class="p">):</span>
              <span class="k">if</span> <span class="n">i</span> <span class="o">!=</span> <span class="n">r</span><span class="p">:</span>
                  <span class="k">print</span><span class="p">(</span><span class="n">f</span><span class="s2">&#34;Matrix: self.matrix[i] = {self.matrix[i]} - {self.matrix[i][lead]}*{self.matrix[r]}&#34;</span><span class="p">)</span>
                  <span class="bp">self</span><span class="o">.</span><span class="n">matrix</span><span class="p">[</span><span class="n">i</span><span class="p">]</span> <span class="o">=</span> <span class="bp">self</span><span class="o">.</span><span class="n">matrix</span><span class="p">[</span><span class="n">i</span><span class="p">]</span> <span class="o">-</span> <span class="bp">self</span><span class="o">.</span><span class="n">matrix</span><span class="p">[</span><span class="n">i</span><span class="p">][</span><span class="n">lead</span><span class="p">]</span><span class="o">*</span><span class="bp">self</span><span class="o">.</span><span class="n">matrix</span><span class="p">[</span><span class="n">r</span><span class="p">]</span>
          <span class="n">lead</span> <span class="o">+=</span> <span class="mi">1</span>

      <span class="n">ic</span><span class="p">(</span><span class="bp">self</span><span class="o">.</span><span class="n">matrix</span><span class="p">)</span>

<span class="n">m</span> <span class="o">=</span> <span class="n">np</span><span class="o">.</span><span class="n">array</span><span class="p">([</span>
  <span class="p">[</span><span class="o">-</span><span class="mi">12</span><span class="p">,</span> <span class="mi">12</span><span class="p">,</span> <span class="mi">2</span><span class="p">],</span>
  <span class="p">[</span><span class="mi">12</span><span class="p">,</span> <span class="o">-</span><span class="mi">12</span><span class="p">,</span> <span class="o">-</span><span class="mi">2</span><span class="p">],</span>
  <span class="p">[</span><span class="mi">2</span><span class="p">,</span> <span class="o">-</span><span class="mi">2</span><span class="p">,</span> <span class="o">-</span><span class="mi">17</span><span class="p">]</span>
<span class="p">])</span>
<span class="n">r</span> <span class="o">=</span> <span class="n">RREF</span><span class="p">(</span><span class="n">m</span><span class="p">)</span>
<span class="n">r</span><span class="o">.</span><span class="n">rref</span><span class="p">()</span>
</code></pre></div><h2 id="references">References:<a href="#references" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h2>
<ul>
<li><a href="https://en.wikipedia.org/wiki/Row_echelon_form#Reduced_row_echelon_form">Understanding RREF from wikipedia</a></li>
<li><a href="https://rosettacode.org/wiki/Reduced_row_echelon_form">Psuedocode from Rosetta Code, containing implementation in lots of different languages!</a></li>
</ul>
