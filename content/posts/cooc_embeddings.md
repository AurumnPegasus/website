---
title: "Co-Occurance Matrix"
date: 2021-09-13T12:00:06+09:00
tags:
  - ml
  - nlp
---

<p>This blog contains an explanation and tutorial to create word embeddings via co-occurance matrix and svd</p>
<h2 id="introduction">Introduction<a href="#introduction" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h2>
<p>A computer cannot understand the language of humans. It can do a lot by understanding where and how words occur in a sentence, but it still cannot understand the semantics of a word. To map the human semantic space into something which a computer can understand, compute and work with, Word-Embedding Space is created. This is an n-dimensional vector space where each vector within it represents a word. Words similar in meaning are close together, and words that are opposites are farther apart.</p>
<h2 id="problem">Problem<a href="#problem" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h2>
<p>The problem here is that we are given a huge .json file which contains bunch of reviews. Our goal is to create a word embedding space by it so that we can represent all the words and their meaning via n-dimensional vectors</p>
<h2 id="preparing-data">Preparing Data<a href="#preparing-data" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h2>
<p>Data preparation for the task is quite simple, we will extract the reviews and tokenize them. We will <strong>not</strong> be removing stop words or lemmatizing since all high frequency words are important.</p>
<p>First, we read the reviews from given json file. I have used <a href="https://pypi.org/project/ijson/">ijson</a>, which is a library which helps in iteratively parse huge json files (instead of loading the whole file to the RAM). I have also used <a href="https://spacy.io/">spacy</a> for tokenizing since its one of the best at it.</p>
<p>The below code is simple to understand, I have created a class which creates our vocabulary, and tokenizes the sentences. There are various methods to do this but I felt this was a really cool and modular way to achieve this!</p>
<div class="highlight"><pre class="chroma"><code class="language-Python" data-lang="Python"><span class="kn">import</span> <span class="nn">ijson</span>
<span class="kn">import</span> <span class="nn">spacy</span>

<span class="k">class</span> <span class="nc">Data</span><span class="p">:</span>
  <span class="k">def</span> <span class="fm">__init__</span><span class="p">(</span><span class="bp">self</span><span class="p">):</span>

      <span class="c1"># All the variables in uppercase are globally defined constants</span>
      <span class="bp">self</span><span class="o">.</span><span class="n">path</span> <span class="o">=</span> <span class="n">PATH</span>
      <span class="bp">self</span><span class="o">.</span><span class="n">limit</span> <span class="o">=</span> <span class="n">LIMIT</span>
      <span class="bp">self</span><span class="o">.</span><span class="n">window</span> <span class="o">=</span> <span class="n">WINDOW</span>
      <span class="bp">self</span><span class="o">.</span><span class="n">nlp</span> <span class="o">=</span> <span class="n">spacy</span><span class="o">.</span><span class="n">load</span><span class="p">(</span><span class="n">DATASET</span><span class="p">)</span>
      <span class="bp">self</span><span class="o">.</span><span class="n">word2index</span> <span class="o">=</span> <span class="p">{}</span>
      <span class="bp">self</span><span class="o">.</span><span class="n">index2word</span> <span class="o">=</span> <span class="p">{}</span>
      <span class="bp">self</span><span class="o">.</span><span class="n">vocab</span> <span class="o">=</span> <span class="p">[]</span>
      <span class="bp">self</span><span class="o">.</span><span class="n">index</span> <span class="o">=</span> <span class="mi">0</span>
  
  <span class="k">def</span> <span class="nf">getSentences</span><span class="p">(</span><span class="bp">self</span><span class="p">):</span>
      <span class="k">return</span> <span class="bp">self</span><span class="o">.</span><span class="n">sentences</span>

  <span class="k">def</span> <span class="nf">getVocab</span><span class="p">(</span><span class="bp">self</span><span class="p">):</span>
      <span class="k">return</span> <span class="bp">self</span><span class="o">.</span><span class="n">word2index</span><span class="p">,</span> <span class="bp">self</span><span class="o">.</span><span class="n">index2word</span><span class="p">,</span> <span class="bp">self</span><span class="o">.</span><span class="n">index</span><span class="p">,</span> <span class="bp">self</span><span class="o">.</span><span class="n">vocab</span>

  <span class="k">def</span> <span class="nf">readJSON</span><span class="p">(</span><span class="bp">self</span><span class="p">,</span> <span class="n">limit</span><span class="p">,</span> <span class="n">path</span><span class="p">):</span>

      <span class="c1"># Reading sentences line by line</span>
      <span class="n">sentences</span> <span class="o">=</span> <span class="p">[]</span>
      <span class="n">f</span> <span class="o">=</span> <span class="nb">open</span><span class="p">(</span><span class="n">path</span><span class="p">)</span>
      <span class="k">for</span> <span class="n">item</span> <span class="ow">in</span> <span class="n">ijson</span><span class="o">.</span><span class="n">items</span><span class="p">(</span><span class="n">f</span><span class="p">,</span> <span class="s1">&#39;reviewText&#39;</span><span class="p">,</span> <span class="n">multiple_reviews</span><span class="o">=</span><span class="bp">True</span><span class="p">):</span>
          <span class="n">sentences</span><span class="o">.</span><span class="n">append</span><span class="p">(</span><span class="n">item</span><span class="p">)</span>
          <span class="k">if</span> <span class="nb">len</span><span class="p">(</span><span class="n">sentences</span><span class="p">)</span> <span class="o">==</span> <span class="n">limit</span><span class="p">:</span>
              <span class="k">break</span>
      
      <span class="k">return</span> <span class="n">sentences</span>

  <span class="k">def</span> <span class="nf">addWords</span><span class="p">(</span><span class="bp">self</span><span class="p">,</span> <span class="n">word</span><span class="p">):</span>

      <span class="c1"># Adding words to vocabulary (if they don&#39;t exist in it previously)</span>
      <span class="k">if</span> <span class="n">word</span> <span class="ow">not</span> <span class="ow">in</span> <span class="bp">self</span><span class="o">.</span><span class="n">word2index</span><span class="p">:</span>
          <span class="bp">self</span><span class="o">.</span><span class="n">word2index</span><span class="p">[</span><span class="n">word</span><span class="p">]</span> <span class="o">=</span> <span class="bp">self</span><span class="o">.</span><span class="n">index</span>
          <span class="bp">self</span><span class="o">.</span><span class="n">index2word</span><span class="p">[</span><span class="bp">self</span><span class="o">.</span><span class="n">index</span><span class="p">]</span> <span class="o">=</span> <span class="n">word</span>
          <span class="bp">self</span><span class="o">.</span><span class="n">vocab</span><span class="o">.</span><span class="n">append</span><span class="p">(</span><span class="n">word</span><span class="p">)</span>
          <span class="bp">self</span><span class="o">.</span><span class="n">index</span> <span class="o">+=</span> <span class="mi">1</span>
  
  <span class="k">def</span> <span class="nf">createVocab</span><span class="p">(</span><span class="bp">self</span><span class="p">,</span> <span class="n">sentences</span><span class="p">):</span>

      <span class="c1"># Going line by line and token by token</span>
      <span class="k">for</span> <span class="n">sent</span> <span class="ow">in</span> <span class="n">sentences</span><span class="p">:</span>
          <span class="n">words</span> <span class="o">=</span> <span class="bp">self</span><span class="o">.</span><span class="n">nlp</span><span class="p">(</span><span class="n">sent</span><span class="p">)</span>
          <span class="k">for</span> <span class="n">word</span> <span class="ow">in</span> <span class="n">words</span><span class="p">:</span>
              <span class="bp">self</span><span class="o">.</span><span class="n">addWords</span><span class="p">(</span><span class="n">word</span><span class="o">.</span><span class="n">text</span><span class="p">)</span>

  <span class="k">def</span> <span class="nf">handle</span><span class="p">(</span><span class="bp">self</span><span class="p">):</span>
      <span class="bp">self</span><span class="o">.</span><span class="n">sentences</span> <span class="o">=</span> <span class="bp">self</span><span class="o">.</span><span class="n">readJSON</span><span class="p">(</span><span class="bp">self</span><span class="o">.</span><span class="n">limit</span><span class="p">,</span> <span class="bp">self</span><span class="o">.</span><span class="n">path</span><span class="p">)</span>
      <span class="bp">self</span><span class="o">.</span><span class="n">createVocab</span><span class="p">(</span><span class="bp">self</span><span class="o">.</span><span class="n">sentences</span><span class="p">)</span>
</code></pre></div><h2 id="creating-co-occurance-matrix">Creating Co-Occurance Matrix<a href="#creating-co-occurance-matrix" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h2>
<p>Usually, the limit of numpy matrices are around the region of 1e5*1e5. In some cases, the data might exceed this limit, hence I have used sparse matrices (so that all the cases are covered). Sparse matrices are able to hold larger matrices given that they are sparse in nature (mostly 0s).</p>
<p>I have used <a href="https://docs.scipy.org/doc/scipy/reference/generated/scipy.sparse.lil_matrix.html">lil_matrix</a> to create the sparse matrix, you can also use <a href="https://docs.scipy.org/doc/scipy/reference/generated/scipy.sparse.csr_matrix.html">csr_matrix</a> instead.</p>
<p>I have also used <a href="https://docs.scipy.org/doc/scipy/reference/generated/scipy.sparse.linalg.svds.html">svds</a>, which is a function which returns the Singular Value Decomposition matrices given a matrix and is optimised to work on sparse matrices. You can also use <a href="https://scikit-learn.org/stable/modules/generated/sklearn.decomposition.TruncatedSVD.html">truncated_svd</a> in place of it.</p>
<div class="highlight"><pre class="chroma"><code class="language-Python" data-lang="Python">
<span class="kn">import</span> <span class="nn">numpy</span> <span class="kn">as</span> <span class="nn">np</span>
<span class="kn">from</span> <span class="nn">scipy.sparse.linalg</span> <span class="kn">import</span> <span class="n">svds</span>
<span class="kn">from</span> <span class="nn">scipy.sparse</span> <span class="kn">import</span> <span class="n">lil_matrix</span>

<span class="k">class</span> <span class="nc">Matrix</span><span class="p">:</span>
  <span class="k">def</span> <span class="fm">__init__</span><span class="p">(</span><span class="bp">self</span><span class="p">,</span> <span class="n">sentences</span><span class="p">,</span> <span class="n">nlp</span><span class="p">,</span> <span class="n">word2index</span><span class="p">,</span> <span class="n">index2word</span><span class="p">,</span> <span class="n">index</span><span class="p">,</span> <span class="n">vocab</span><span class="p">):</span>

      <span class="c1"># Window refers to the window size for calculating co-occurances</span>
      <span class="c1"># K refers to the truncated value of svd we will be taking</span>
      <span class="bp">self</span><span class="o">.</span><span class="n">sentences</span> <span class="o">=</span> <span class="n">sentences</span>
      <span class="bp">self</span><span class="o">.</span><span class="n">nlp</span> <span class="o">=</span> <span class="n">nlp</span>
      <span class="bp">self</span><span class="o">.</span><span class="n">window</span> <span class="o">=</span> <span class="n">WINDOW</span>
      <span class="bp">self</span><span class="o">.</span><span class="n">word2index</span> <span class="o">=</span> <span class="n">word2index</span>
      <span class="bp">self</span><span class="o">.</span><span class="n">index2word</span> <span class="o">=</span> <span class="n">index2word</span>
      <span class="bp">self</span><span class="o">.</span><span class="n">index</span> <span class="o">=</span> <span class="n">index</span>
      <span class="bp">self</span><span class="o">.</span><span class="n">vocab</span> <span class="o">=</span> <span class="n">vocab</span>
      <span class="bp">self</span><span class="o">.</span><span class="n">k</span> <span class="o">=</span> <span class="n">K</span>

  <span class="k">def</span> <span class="nf">getEmbeddings</span><span class="p">(</span><span class="bp">self</span><span class="p">):</span>
      <span class="k">return</span> <span class="bp">self</span><span class="o">.</span><span class="n">embeddings</span>

  <span class="k">def</span> <span class="nf">getMatrix</span><span class="p">(</span><span class="bp">self</span><span class="p">):</span>
      <span class="k">return</span> <span class="bp">self</span><span class="o">.</span><span class="n">matrix</span>

  <span class="k">def</span> <span class="nf">wordEmbeddings</span><span class="p">(</span><span class="bp">self</span><span class="p">,</span> <span class="n">word</span><span class="p">):</span>
      <span class="k">return</span> <span class="bp">self</span><span class="o">.</span><span class="n">embeddings</span><span class="p">[</span><span class="bp">self</span><span class="o">.</span><span class="n">word2index</span><span class="p">[</span><span class="n">word</span><span class="p">]]</span>

  <span class="k">def</span> <span class="nf">moveWindow</span><span class="p">(</span><span class="bp">self</span><span class="p">,</span> <span class="n">matrix</span><span class="p">,</span> <span class="n">window</span><span class="p">,</span> <span class="n">sentences</span><span class="p">):</span>

      <span class="c1"># Going window to window to increment all co-occuring words</span>
      <span class="k">for</span> <span class="n">sent</span> <span class="ow">in</span> <span class="n">sentences</span><span class="p">:</span>
          <span class="n">words</span> <span class="o">=</span> <span class="bp">self</span><span class="o">.</span><span class="n">nlp</span><span class="p">(</span><span class="n">sent</span><span class="p">)</span>
          <span class="k">for</span> <span class="n">i</span> <span class="ow">in</span> <span class="nb">range</span><span class="p">(</span><span class="mi">0</span><span class="p">,</span> <span class="nb">len</span><span class="p">(</span><span class="n">words</span><span class="p">)</span> <span class="o">-</span> <span class="n">window</span> <span class="o">+</span> <span class="mi">1</span><span class="p">):</span>
              <span class="k">for</span> <span class="n">j</span> <span class="ow">in</span> <span class="nb">range</span><span class="p">(</span><span class="n">i</span> <span class="o">+</span> <span class="mi">1</span><span class="p">,</span> <span class="n">i</span> <span class="o">+</span> <span class="n">window</span> <span class="o">+</span> <span class="mi">1</span><span class="p">):</span>
                  <span class="k">if</span> <span class="n">j</span> <span class="o">&lt;</span> <span class="nb">len</span><span class="p">(</span><span class="n">words</span><span class="p">):</span>
                      <span class="n">i_index</span> <span class="o">=</span> <span class="bp">self</span><span class="o">.</span><span class="n">word2index</span><span class="p">[</span><span class="n">words</span><span class="p">[</span><span class="n">i</span><span class="p">]</span><span class="o">.</span><span class="n">text</span><span class="p">]</span>
                      <span class="n">j_index</span> <span class="o">=</span> <span class="bp">self</span><span class="o">.</span><span class="n">word2index</span><span class="p">[</span><span class="n">words</span><span class="p">[</span><span class="n">j</span><span class="p">]</span><span class="o">.</span><span class="n">text</span><span class="p">]</span>
                      <span class="n">matrix</span><span class="p">[</span><span class="n">i_index</span><span class="p">,</span> <span class="n">j_index</span><span class="p">]</span> <span class="o">+=</span> <span class="mi">1</span>
                      <span class="n">matrix</span><span class="p">[</span><span class="n">j_index</span><span class="p">,</span> <span class="n">i_index</span><span class="p">]</span> <span class="o">+=</span> <span class="mi">1</span>

      <span class="k">return</span> <span class="n">matrix</span>

  <span class="k">def</span> <span class="nf">getSVD</span><span class="p">(</span><span class="bp">self</span><span class="p">,</span> <span class="n">matrix</span><span class="p">):</span>

      <span class="c1"># Performing Single Value Decomposition on matrix and taking top k columns</span>
      <span class="c1"># The word embeddings is the right matrix, and hence we return only that</span>
      <span class="n">right</span><span class="p">,</span> <span class="n">sigma</span><span class="p">,</span> <span class="n">left_t</span> <span class="o">=</span> <span class="n">svds</span><span class="p">(</span><span class="n">matrix</span><span class="p">,</span> <span class="bp">self</span><span class="o">.</span><span class="n">k</span><span class="p">)</span>
      <span class="k">return</span> <span class="n">right</span>

  <span class="k">def</span> <span class="nf">handle</span><span class="p">(</span><span class="bp">self</span><span class="p">):</span>
      <span class="bp">self</span><span class="o">.</span><span class="n">matrix</span> <span class="o">=</span> <span class="n">lil_matrix</span><span class="p">((</span><span class="bp">self</span><span class="o">.</span><span class="n">index</span><span class="p">,</span> <span class="bp">self</span><span class="o">.</span><span class="n">index</span><span class="p">),</span> <span class="n">dtype</span><span class="o">=</span><span class="n">np</span><span class="o">.</span><span class="n">float</span><span class="p">)</span>
      <span class="bp">self</span><span class="o">.</span><span class="n">matrix</span> <span class="o">=</span> <span class="bp">self</span><span class="o">.</span><span class="n">moveWindow</span><span class="p">(</span><span class="bp">self</span><span class="o">.</span><span class="n">matrix</span><span class="p">,</span> <span class="bp">self</span><span class="o">.</span><span class="n">window</span><span class="p">,</span> <span class="bp">self</span><span class="o">.</span><span class="n">sentences</span><span class="p">)</span>
      <span class="bp">self</span><span class="o">.</span><span class="n">embeddings</span> <span class="o">=</span> <span class="bp">self</span><span class="o">.</span><span class="n">getSVD</span><span class="p">(</span><span class="bp">self</span><span class="o">.</span><span class="n">matrix</span><span class="p">)</span>

</code></pre></div>
