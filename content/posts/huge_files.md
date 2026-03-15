---
title: "Reversing Huge Files"
date: 2021-09-13T12:00:06+09:00
tags:
  - system
---

<p>This blog contains an explanation and tutorial of how to reverse very large files in C</p>
<h2 id="introduction">Introduction<a href="#introduction" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h2>
<p>To understand requiring a different method to read and reverse huge files, one must first understand the standard process. Your system loads the program you want to execute into the RAM, and since you are reading the whole file at once, it loads it into the RAM. Now, in standard systems, RAM is limited to about 4-8 GB; hence, it cannot load the whole file into the RAM at once.
A simple work-around to this is not to load the file into the RAM at all. If you read everything directly from the disk storage, your program would execute, but it will become excruciatingly slow.</p>
<h2 id="problem">Problem<a href="#problem" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h2>
<p>The problem here is that we are given three values: path to the file, number of parts we want to divide the text into and the block we want to reverse. Just for a simple explanation, we will assume that the text length is perfectly divisible by the number of parts given.</p>
<h2 id="solution">Solution<a href="#solution" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h2>
<p>To solve this, instead of trying to either load the whole text into the RAM or not load it at all, we will load it block by block.  The text file will remain in the disk, but we will move the cursor so that it loads just X amount of characters into the RAM each time.</p>
<h4 id="setup">Setup<a href="#setup" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h4>
<p>First, we read the given arguments and get our source and destination files ready. It is important to know what each of the flags here do. For a better understanding, refer to its <a href="https://man7.org/linux/man-pages/man2/open.2.html">man page.</a></p>
<div class="highlight"><pre class="chroma"><code class="language-C" data-lang="C">
<span class="c1">// O_RDONLY: Read Only  
</span><span class="c1"></span><span class="kt">int</span> <span class="n">source</span> <span class="o">=</span> <span class="n">open</span><span class="p">(</span><span class="n">value</span><span class="p">[</span><span class="mi">1</span><span class="p">],</span> <span class="n">O_RDONLY</span><span class="p">);</span>

<span class="c1">// O_WRONLY: Write Only
</span><span class="c1">// O_CREAT: Create the file if it does not exist
</span><span class="c1">// O_TRUNC: Overwrite the file
</span><span class="c1"></span><span class="kt">int</span> <span class="n">destination</span> <span class="o">=</span> <span class="n">open</span><span class="p">(</span><span class="s">&#34;output.txt&#34;</span><span class="p">,</span> <span class="n">O_WRONLY</span> <span class="o">|</span> <span class="n">O_CREAT</span> <span class="o">|</span> <span class="n">O_TRUNC</span><span class="p">,</span> <span class="mo">0600</span><span class="p">);</span>

<span class="kt">int</span> <span class="n">total_parts</span> <span class="o">=</span> <span class="o">*</span><span class="n">value</span><span class="p">[</span><span class="mi">2</span><span class="p">]</span> <span class="o">-</span> <span class="mi">48</span><span class="p">;</span>
<span class="kt">int</span> <span class="n">index</span> <span class="o">=</span> <span class="o">*</span><span class="n">value</span><span class="p">[</span><span class="mi">3</span><span class="p">]</span> <span class="o">-</span> <span class="mi">48</span><span class="p">;</span>
</code></pre></div><h4 id="main-code">Main Code<a href="#main-code" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h4>
<p>The star of our show is <a href="https://man7.org/linux/man-pages/man2/lseek.2.html">lseek.</a> It is a system call which repositions the cursor as we want (without loading it into the RAM). Before going into the main logic, we need to define some variables.</p>
<div class="highlight"><pre class="chroma"><code class="language-C" data-lang="C">  <span class="c1">// in the source file, go the end of file and offset by 0
</span><span class="c1"></span>  <span class="n">off_t</span> <span class="n">source_size</span> <span class="o">=</span> <span class="n">lseek</span><span class="p">(</span><span class="n">source</span><span class="p">,</span> <span class="mi">0</span><span class="p">,</span> <span class="n">SEEK_END</span><span class="p">);</span>

  <span class="c1">// block size
</span><span class="c1"></span>  <span class="kt">long</span> <span class="n">limit</span> <span class="o">=</span> <span class="mi">1024</span><span class="p">;</span>

  <span class="c1">// Length of each part of source file
</span><span class="c1"></span>  <span class="kt">long</span> <span class="n">each_part</span> <span class="o">=</span> <span class="n">source_size</span><span class="o">/</span><span class="n">total_parts</span>

  <span class="c1">// Start and ending position of part we need to reverse
</span><span class="c1"></span>  <span class="kt">long</span> <span class="n">start</span> <span class="o">=</span> <span class="p">(</span><span class="n">index</span> <span class="o">-</span> <span class="mi">1</span><span class="p">)</span><span class="o">*</span><span class="n">each_part</span><span class="p">;</span>
  <span class="kt">long</span> <span class="n">end</span> <span class="o">=</span> <span class="p">(</span><span class="n">index</span> <span class="o">-</span><span class="mi">1</span><span class="p">)</span><span class="o">*</span><span class="n">each_part</span> <span class="o">+</span> <span class="n">each_part</span>

  <span class="c1">// Arrays for storing read and reversed part
</span><span class="c1"></span>  <span class="kt">char</span> <span class="o">*</span><span class="n">r</span> <span class="o">=</span> <span class="p">(</span><span class="kt">char</span> <span class="o">*</span><span class="p">)</span> <span class="n">malloc</span><span class="p">(</span><span class="n">limit</span><span class="p">);</span>
  <span class="kt">char</span> <span class="o">*</span><span class="n">rev</span> <span class="o">=</span> <span class="p">(</span><span class="kt">char</span> <span class="o">*</span><span class="p">)</span> <span class="n">malloc</span><span class="p">(</span><span class="n">limit</span><span class="p">);</span>
</code></pre></div><p>Now, onto the main logic (which is surprisingly small)</p>
<div class="highlight"><pre class="chroma"><code class="language-C" data-lang="C">  <span class="k">for</span><span class="p">(</span><span class="kt">long</span> <span class="n">i</span><span class="o">=</span><span class="n">start</span><span class="o">+</span><span class="n">limit</span><span class="p">;</span> <span class="n">i</span><span class="o">&lt;</span><span class="n">end</span><span class="p">;</span> <span class="n">i</span><span class="o">+=</span><span class="n">limit</span><span class="p">){</span>

      <span class="c1">// SET the cursor at the position given, which is end - i + start 
</span><span class="c1"></span>      <span class="n">lseek</span><span class="p">(</span><span class="n">source</span><span class="p">,</span> <span class="n">end</span> <span class="o">-</span><span class="n">i</span> <span class="o">+</span> <span class="n">start</span><span class="p">,</span> <span class="n">SEEK_SET</span><span class="p">);</span>

      <span class="c1">// Read limit number of characters from source and store it into r
</span><span class="c1"></span>      <span class="c1">// It is important to note that the cursor moves ahead limit amount of times due to reading
</span><span class="c1"></span>      <span class="n">read</span><span class="p">(</span><span class="n">source</span><span class="p">,</span> <span class="n">r</span><span class="p">,</span> <span class="n">limit</span><span class="p">);</span>

      <span class="c1">// Reverse the read array
</span><span class="c1"></span>      <span class="k">for</span><span class="p">(</span><span class="kt">int</span> <span class="n">j</span><span class="o">=</span><span class="mi">0</span><span class="p">;</span> <span class="n">j</span><span class="o">&lt;</span><span class="n">limit</span><span class="p">;</span> <span class="n">j</span><span class="o">++</span><span class="p">){</span>
          <span class="n">rev</span><span class="p">[</span><span class="n">j</span><span class="p">]</span> <span class="o">=</span> <span class="n">r</span><span class="p">[</span><span class="n">limit</span> <span class="o">-</span><span class="mi">1</span> <span class="o">-</span><span class="n">j</span><span class="p">];</span>
      <span class="p">}</span>

      <span class="c1">// SET the cursor at the position given, which is i - limit - start
</span><span class="c1"></span>      <span class="n">lseek</span><span class="p">(</span><span class="n">destination</span><span class="p">,</span> <span class="n">i</span> <span class="o">-</span><span class="n">limit</span> <span class="o">-</span><span class="n">start</span><span class="p">,</span> <span class="n">SEEK_SET</span><span class="p">);</span>

      <span class="c1">// Write the number of characters from rev into destination
</span><span class="c1"></span>      <span class="c1">// It is important to note that the cursor moves ahead limit amount of times due to writing
</span><span class="c1"></span>      <span class="n">write</span><span class="p">(</span><span class="n">destination</span><span class="p">,</span> <span class="n">rev</span><span class="p">,</span> <span class="n">limit</span><span class="p">);</span>

      <span class="k">if</span><span class="p">(</span><span class="n">i</span> <span class="o">+</span><span class="n">limit</span> <span class="o">&gt;=</span> <span class="n">each_part</span><span class="p">){</span>
          <span class="c1">// This part is left as an excercise, think why this is needed and what will come here
</span><span class="c1"></span>          <span class="c1">// Also think about better ways to structure the loop so that you do not require this :p
</span><span class="c1"></span>      <span class="p">}</span>
  <span class="p">}</span>
</code></pre></div>
