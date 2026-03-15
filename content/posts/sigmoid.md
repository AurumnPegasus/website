---
title: "Linear Classifier"
date: 2021-09-21T12:00:06+09:00
tags:
  - ml
---

<p>The blog explains how to create a linear classifier from scratch using MSE loss and Sigmoid Function.</p>
<h2 id="introduction">Introduction<a href="#introduction" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h2>
<p>A classifier is a machine that can differentiate between objects of two or more different classes. We define a Loss Function J as the Mean Squared Error of the actual value vs the predicted value. The machine learns to differentiate between classes by minimising the function J (via gradient descent). For simplicity and explainibility, we have considered two categories and are trying to find a line that divides these two classes.
The explanation below assumes that the reader has a decent understanding of calculus and gradient descent algorithms.</p>
<h2 id="dataset">Dataset<a href="#dataset" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h2>
<p>We artificially generate a dataset using NumPy. Two classes are shown in a two-dimensional plot in red and blue, with class 1 centred around (-5, 3) and class 2 centred around (3, -5). We use the same covariance matrix for both classes.</p>
<figure>
    <img src="https://i.imgur.com/d3hqH4Y.png"/> 
</figure>

<p><br>
Our training dataset consists of 1000 samples, with 500 belonging to class 1 and 500 belonging to class 2. The test dataset consists of 200 samples, with 100 belonging to each class.</p>
<p>First, we get multiple samples from a multivariate normal distribution using NumPy.</p>
<div class="highlight"><pre class="chroma"><code class="language-Python" data-lang="Python"><span class="k">def</span> <span class="nf">getClasses</span><span class="p">(</span><span class="bp">self</span><span class="p">):</span>
  <span class="c1"># Assumed equal amount of samples in each class, you can take anything</span>
  <span class="c1"># covariance matrix = [[12, 0], [0, 12]]</span>

  <span class="c1"># mean_a = [-5, 3]</span>
  <span class="n">train_a</span> <span class="o">=</span> <span class="n">np</span><span class="o">.</span><span class="n">random</span><span class="o">.</span><span class="n">multivariate_normal</span><span class="p">(</span><span class="bp">self</span><span class="o">.</span><span class="n">mean_a</span><span class="p">,</span> <span class="bp">self</span><span class="o">.</span><span class="n">covariance</span><span class="p">,</span> <span class="bp">self</span><span class="o">.</span><span class="n">train_len</span><span class="o">//</span><span class="mi">2</span><span class="p">)</span>
  <span class="n">test_a</span> <span class="o">=</span> <span class="n">np</span><span class="o">.</span><span class="n">random</span><span class="o">.</span><span class="n">multivariate_normal</span><span class="p">(</span><span class="bp">self</span><span class="o">.</span><span class="n">mean_a</span><span class="p">,</span> <span class="bp">self</span><span class="o">.</span><span class="n">covariance</span><span class="p">,</span> <span class="bp">self</span><span class="o">.</span><span class="n">test_len</span><span class="o">//</span><span class="mi">2</span><span class="p">)</span>

  <span class="c1"># mean_b = [3, -5]</span>
  <span class="n">train_b</span> <span class="o">=</span> <span class="n">np</span><span class="o">.</span><span class="n">random</span><span class="o">.</span><span class="n">multivariate_normal</span><span class="p">(</span><span class="bp">self</span><span class="o">.</span><span class="n">mean_b</span><span class="p">,</span> <span class="bp">self</span><span class="o">.</span><span class="n">covariance</span><span class="p">,</span> <span class="bp">self</span><span class="o">.</span><span class="n">train_len</span><span class="o">//</span><span class="mi">2</span><span class="p">)</span>
  <span class="n">test_b</span> <span class="o">=</span> <span class="n">np</span><span class="o">.</span><span class="n">random</span><span class="o">.</span><span class="n">multivariate_normal</span><span class="p">(</span><span class="bp">self</span><span class="o">.</span><span class="n">mean_b</span><span class="p">,</span> <span class="bp">self</span><span class="o">.</span><span class="n">covariance</span><span class="p">,</span> <span class="bp">self</span><span class="o">.</span><span class="n">test_len</span><span class="o">//</span><span class="mi">2</span><span class="p">)</span>

  <span class="k">return</span> <span class="n">train_a</span><span class="p">,</span> <span class="n">test_a</span><span class="p">,</span> <span class="n">train_b</span><span class="p">,</span> <span class="n">test_b</span>
</code></pre></div><p>Once we have samples from the two classes, we append them together (and add 1 for bias) to get train and test dataset.</p>
<div class="highlight"><pre class="chroma"><code class="language-Python" data-lang="Python"><span class="k">def</span> <span class="nf">handle</span><span class="p">(</span><span class="bp">self</span><span class="p">):</span>
  <span class="n">train_a</span><span class="p">,</span> <span class="n">test_a</span><span class="p">,</span> <span class="n">train_b</span><span class="p">,</span> <span class="n">test_b</span> <span class="o">=</span> <span class="bp">self</span><span class="o">.</span><span class="n">getClasses</span><span class="p">()</span>

  <span class="c1"># Assigning 1 label to class A and -1 to class B</span>
  <span class="bp">self</span><span class="o">.</span><span class="n">train_x</span> <span class="o">=</span> <span class="n">np</span><span class="o">.</span><span class="n">concatenate</span><span class="p">((</span><span class="n">np</span><span class="o">.</span><span class="n">concatenate</span><span class="p">((</span><span class="n">train_a</span><span class="p">,</span> <span class="n">train_b</span><span class="p">),</span> <span class="n">axis</span><span class="o">=</span><span class="mi">0</span><span class="p">),</span> <span class="n">np</span><span class="o">.</span><span class="n">asmatrix</span><span class="p">(</span><span class="n">np</span><span class="o">.</span><span class="n">ones</span><span class="p">(</span><span class="bp">self</span><span class="o">.</span><span class="n">train_len</span><span class="p">))</span><span class="o">.</span><span class="n">T</span><span class="p">),</span> <span class="n">axis</span><span class="o">=</span><span class="mi">1</span><span class="p">)</span><span class="o">.</span><span class="n">T</span>
  <span class="bp">self</span><span class="o">.</span><span class="n">train_y</span> <span class="o">=</span> <span class="n">np</span><span class="o">.</span><span class="n">asmatrix</span><span class="p">(</span><span class="n">np</span><span class="o">.</span><span class="n">concatenate</span><span class="p">(</span> <span class="p">(</span><span class="n">np</span><span class="o">.</span><span class="n">ones</span><span class="p">(</span><span class="bp">self</span><span class="o">.</span><span class="n">train_len</span><span class="o">//</span><span class="mi">2</span><span class="p">),</span> <span class="o">-</span><span class="mi">1</span><span class="o">*</span><span class="n">np</span><span class="o">.</span><span class="n">ones</span><span class="p">(</span><span class="bp">self</span><span class="o">.</span><span class="n">train_len</span><span class="o">//</span><span class="mi">2</span><span class="p">)),</span> <span class="n">axis</span><span class="o">=</span><span class="mi">0</span><span class="p">))</span>

  <span class="bp">self</span><span class="o">.</span><span class="n">test_x</span> <span class="o">=</span> <span class="n">np</span><span class="o">.</span><span class="n">concatenate</span><span class="p">((</span><span class="n">np</span><span class="o">.</span><span class="n">concatenate</span><span class="p">((</span><span class="n">test_a</span><span class="p">,</span> <span class="n">test_b</span><span class="p">),</span> <span class="n">axis</span><span class="o">=</span><span class="mi">0</span><span class="p">),</span> <span class="n">np</span><span class="o">.</span><span class="n">asmatrix</span><span class="p">(</span><span class="n">np</span><span class="o">.</span><span class="n">ones</span><span class="p">(</span><span class="bp">self</span><span class="o">.</span><span class="n">test_len</span><span class="p">))</span><span class="o">.</span><span class="n">T</span><span class="p">),</span> <span class="n">axis</span><span class="o">=</span><span class="mi">1</span><span class="p">)</span><span class="o">.</span><span class="n">T</span>
  <span class="bp">self</span><span class="o">.</span><span class="n">test_y</span> <span class="o">=</span> <span class="n">np</span><span class="o">.</span><span class="n">asmatrix</span><span class="p">(</span><span class="n">np</span><span class="o">.</span><span class="n">concatenate</span><span class="p">(</span> <span class="p">(</span><span class="n">np</span><span class="o">.</span><span class="n">ones</span><span class="p">(</span><span class="bp">self</span><span class="o">.</span><span class="n">test_len</span><span class="o">//</span><span class="mi">2</span><span class="p">),</span> <span class="o">-</span><span class="mi">1</span><span class="o">*</span><span class="n">np</span><span class="o">.</span><span class="n">ones</span><span class="p">(</span><span class="bp">self</span><span class="o">.</span><span class="n">test_len</span><span class="o">//</span><span class="mi">2</span><span class="p">)),</span> <span class="n">axis</span><span class="o">=</span><span class="mi">0</span><span class="p">))</span>
</code></pre></div><h2 id="gradiant">Gradiant<a href="#gradiant" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h2>
<p>In two dimensions, a linear classifier is just a line. Let us denote it by \(f\) . We define our dataset as \(x = [x_0, x_1, 1]\) and weight \(w = [w_0, w_1, w_2]\), where \(w\) is the term we want to learn. Though our samples are in two-dimensions, we have \(x\) as three-dimensional vector so as to account for bias.</p>
<p>Since we are using MSE loss, we get:</p>
<p>$$ J = \frac{1}{N} \mid \mid y - \sigma(w.x^T) \mid \mid^2 $$</p>
<p>Where:
\(J\) is the loss function
\(N\) is the number of samples
\(y\) is the correct output
\(\sigma(w.x^t)\) is the predicted output</p>
<p>Now, we can differentiate it as:</p>
<p>$$ \nabla J = \frac{\text{d} J}{\text{d} w} = \frac{1}{N} \times 2 \times (y - \sigma(w.x^T)) \odot \frac{\text{d} (y - \sigma(w.x^T))}{\text{d}w}$$</p>
<p>The symbol $\odot$ symbolises <a href="https://en.wikipedia.org/wiki/Hadamard_product_(matrices)">Hadamard Product</a>.
According to <a href="https://beckernick.github.io/sigmoid-derivative-neural-network/">in-depth proof</a>, we get:</p>
<p>$$ \frac{\text{d} \sigma(x)}{\text{d} x} = \sigma(x) ( 1- \sigma(x) ) $$</p>
<p>Substituting this formula</p>
<p>$$ \nabla J = \frac{2}{N} \times (y - \sigma(w.x^T)) \times \sigma(w.x^T) (1 - \sigma(w.x^T) \times x^T$$</p>
<h2 id="classification">Classification<a href="#classification" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h2>
<p>We have the gradiant, now using the gradiant descent formula we get</p>
<p>$$ w = w - \eta\times \nabla J $$</p>
<p>Continue doing this till the difference in improvement is less than 0.0001 (arbitrary number)</p>
<div class="highlight"><pre class="chroma"><code class="language-Python" data-lang="Python"><span class="k">def</span> <span class="nf">training</span><span class="p">(</span><span class="bp">self</span><span class="p">):</span>
  <span class="n">x</span><span class="p">,</span> <span class="n">y</span> <span class="o">=</span> <span class="bp">self</span><span class="o">.</span><span class="n">g</span><span class="o">.</span><span class="n">getTrain</span><span class="p">()</span>

  <span class="c1"># Pre-defining weights as 1</span>
  <span class="n">w_prev</span> <span class="o">=</span> <span class="n">np</span><span class="o">.</span><span class="n">asmatrix</span><span class="p">(</span><span class="n">np</span><span class="o">.</span><span class="n">zeros</span><span class="p">(</span><span class="mi">3</span><span class="p">))</span>
  <span class="n">w</span> <span class="o">=</span> <span class="n">np</span><span class="o">.</span><span class="n">asmatrix</span><span class="p">(</span><span class="n">np</span><span class="o">.</span><span class="n">ones</span><span class="p">(</span><span class="mi">3</span><span class="p">))</span>

  <span class="n">train_errors</span> <span class="o">=</span> <span class="p">[]</span>
  <span class="n">iteration</span> <span class="o">=</span> <span class="mi">0</span>

  <span class="k">while</span> <span class="n">np</span><span class="o">.</span><span class="n">linalg</span><span class="o">.</span><span class="n">norm</span><span class="p">(</span><span class="n">w</span> <span class="o">-</span> <span class="n">w_prev</span><span class="p">)</span> <span class="o">&gt;</span> <span class="mf">1e-4</span><span class="p">:</span>

      <span class="c1"># Computing the loss via MSE</span>
      <span class="n">loss</span> <span class="o">=</span> <span class="mi">1</span><span class="o">/</span> <span class="bp">self</span><span class="o">.</span><span class="n">train_len</span> <span class="o">*</span> <span class="n">np</span><span class="o">.</span><span class="n">linalg</span><span class="o">.</span><span class="n">norm</span><span class="p">(</span><span class="n">y</span> <span class="o">-</span> <span class="bp">self</span><span class="o">.</span><span class="n">sigmoid</span><span class="p">(</span><span class="n">np</span><span class="o">.</span><span class="n">matmul</span><span class="p">(</span><span class="n">w</span><span class="p">,</span> <span class="n">x</span><span class="p">)))</span><span class="o">**</span><span class="mi">2</span>
      <span class="n">train_errors</span><span class="o">.</span><span class="n">append</span><span class="p">(</span><span class="n">loss</span><span class="p">)</span>

      <span class="n">w_prev</span> <span class="o">=</span> <span class="n">w</span>

      <span class="c1"># w = w - η∇J</span>
      <span class="c1"># @: hadamard product, matmul: dot product</span>
      <span class="n">w</span> <span class="o">=</span> <span class="n">w</span> <span class="o">-</span> <span class="bp">self</span><span class="o">.</span><span class="n">eta</span><span class="o">*</span><span class="mi">2</span> <span class="o">/</span> <span class="bp">self</span><span class="o">.</span><span class="n">train_len</span> <span class="o">*</span> <span class="p">(</span>
          <span class="n">np</span><span class="o">.</span><span class="n">mat</span><span class="p">(</span><span class="n">np</span><span class="o">.</span><span class="n">array</span><span class="p">(</span><span class="o">-</span><span class="n">y</span><span class="p">)</span><span class="o">*</span><span class="p">(</span><span class="n">np</span><span class="o">.</span><span class="n">array</span><span class="p">(</span><span class="bp">self</span><span class="o">.</span><span class="n">sigmoid</span><span class="p">(</span><span class="n">w</span><span class="nd">@x</span><span class="p">))</span><span class="o">*</span><span class="n">np</span><span class="o">.</span><span class="n">array</span><span class="p">((</span><span class="mi">1</span><span class="o">-</span><span class="bp">self</span><span class="o">.</span><span class="n">sigmoid</span><span class="p">(</span><span class="n">w</span><span class="nd">@x</span><span class="p">)))))</span><span class="nd">@x.T</span> 
          <span class="o">+</span> 
          <span class="n">np</span><span class="o">.</span><span class="n">mat</span><span class="p">(</span><span class="n">np</span><span class="o">.</span><span class="n">array</span><span class="p">(</span><span class="bp">self</span><span class="o">.</span><span class="n">sigmoid</span><span class="p">(</span><span class="n">w</span><span class="nd">@x</span><span class="p">))</span><span class="o">*</span><span class="n">np</span><span class="o">.</span><span class="n">array</span><span class="p">(</span><span class="bp">self</span><span class="o">.</span><span class="n">sigmoid</span><span class="p">(</span><span class="n">w</span><span class="nd">@x</span><span class="p">))</span><span class="o">*</span><span class="n">np</span><span class="o">.</span><span class="n">array</span><span class="p">(</span><span class="mi">1</span> <span class="o">-</span> <span class="n">np</span><span class="o">.</span><span class="n">array</span><span class="p">(</span><span class="bp">self</span><span class="o">.</span><span class="n">sigmoid</span><span class="p">(</span><span class="n">w</span><span class="nd">@x</span><span class="p">))))</span><span class="nd">@x.T</span>
      <span class="p">)</span>
      <span class="n">iteration</span> <span class="o">+=</span> <span class="mi">1</span>
  
  <span class="k">return</span> <span class="n">w</span>
</code></pre></div><h2 id="results">Results<a href="#results" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h2>
<p>We use the learned $w$ to create the classification line (this is only possible since we have a 2-dimensional space, for higher dimensions it is impossible to visualise such equations).</p>
<div class="highlight"><pre class="chroma"><code class="language-Python" data-lang="Python"><span class="n">weights</span> <span class="o">=</span> <span class="n">t</span><span class="o">.</span><span class="n">training</span><span class="p">()</span>

<span class="n">m</span> <span class="o">=</span> <span class="o">-</span><span class="n">weights</span><span class="p">[</span><span class="mi">0</span><span class="p">,</span> <span class="mi">0</span><span class="p">]</span> <span class="o">/</span> <span class="n">weights</span><span class="p">[</span><span class="mi">0</span><span class="p">,</span> <span class="mi">1</span><span class="p">]</span>
<span class="n">c</span> <span class="o">=</span> <span class="o">-</span><span class="n">weights</span><span class="p">[</span><span class="mi">0</span><span class="p">,</span> <span class="mi">2</span><span class="p">]</span> <span class="o">/</span> <span class="n">weights</span><span class="p">[</span><span class="mi">0</span><span class="p">,</span> <span class="mi">1</span><span class="p">]</span>

<span class="n">x</span> <span class="o">=</span> <span class="n">np</span><span class="o">.</span><span class="n">linspace</span><span class="p">(</span><span class="o">-</span><span class="mi">15</span><span class="p">,</span> <span class="mi">15</span><span class="p">,</span> <span class="mi">300</span><span class="p">)</span>
<span class="n">y</span> <span class="o">=</span> <span class="n">m</span><span class="o">*</span><span class="n">x</span> <span class="o">+</span> <span class="n">c</span>

<span class="c1"># Plotting the line on train dataset (can do the same for test dataset as well)</span>
<span class="n">plt</span><span class="o">.</span><span class="n">figure</span><span class="p">(</span><span class="n">figsize</span><span class="o">=</span><span class="p">(</span><span class="mi">8</span><span class="p">,</span> <span class="mi">8</span><span class="p">))</span>
<span class="n">plt</span><span class="o">.</span><span class="n">scatter</span><span class="p">([</span><span class="n">point</span><span class="p">[</span><span class="mi">0</span><span class="p">]</span> <span class="k">for</span> <span class="n">point</span> <span class="ow">in</span> <span class="n">train_a</span><span class="p">],</span> <span class="p">[</span><span class="n">point</span><span class="p">[</span><span class="mi">1</span><span class="p">]</span> <span class="k">for</span> <span class="n">point</span> <span class="ow">in</span> <span class="n">train_a</span><span class="p">],</span> <span class="n">color</span> <span class="o">=</span> <span class="s1">&#39;green&#39;</span><span class="p">,</span> <span class="n">label</span> <span class="o">=</span> <span class="s1">&#39;Class 1&#39;</span><span class="p">)</span>
<span class="n">plt</span><span class="o">.</span><span class="n">scatter</span><span class="p">([</span><span class="n">point</span><span class="p">[</span><span class="mi">0</span><span class="p">]</span> <span class="k">for</span> <span class="n">point</span> <span class="ow">in</span> <span class="n">train_b</span><span class="p">],</span> <span class="p">[</span><span class="n">point</span><span class="p">[</span><span class="mi">1</span><span class="p">]</span> <span class="k">for</span> <span class="n">point</span> <span class="ow">in</span> <span class="n">train_b</span><span class="p">],</span> <span class="n">color</span> <span class="o">=</span> <span class="s1">&#39;blue&#39;</span><span class="p">,</span> <span class="n">label</span> <span class="o">=</span> <span class="s1">&#39;Class 2&#39;</span><span class="p">)</span>
<span class="n">plt</span><span class="o">.</span><span class="n">plot</span><span class="p">(</span><span class="n">x</span><span class="p">,</span> <span class="n">y</span><span class="p">,</span> <span class="s1">&#39;-r&#39;</span><span class="p">)</span>
<span class="n">plt</span><span class="o">.</span><span class="n">legend</span><span class="p">()</span>
<span class="n">plt</span><span class="o">.</span><span class="n">show</span><span class="p">()</span>
</code></pre></div><p>We see the learned classifier performing well on the training set:</p>
<figure>
    <img src="https://i.imgur.com/r49wLqC.png"/> 
</figure>

<p>Testing the same on test dataset we find:</p>
<figure>
    <img src="https://i.imgur.com/HvEWPcg.png"/> 
</figure>

<h2 id="references">References:<a href="#references" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h2>
<p>Introduction to DL: <a href="https://github.com/sayarghoshroy/Intro_to_DL_tutorial">https://github.com/sayarghoshroy/Intro_to_DL_tutorial</a></p>
