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
{{< highlight python >}}
import ijson
import spacy

class Data:
  def __init__(self):

      # All the variables in uppercase are globally defined constants
      self.path = PATH
      self.limit = LIMIT
      self.window = WINDOW
      self.nlp = spacy.load(DATASET)
      self.word2index = {}
      self.index2word = {}
      self.vocab = []
      self.index = 0
  
  def getSentences(self):
      return self.sentences

  def getVocab(self):
      return self.word2index, self.index2word, self.index, self.vocab

  def readJSON(self, limit, path):

      # Reading sentences line by line
      sentences = []
      f = open(path)
      for item in ijson.items(f, 'reviewText', multiple_reviews=True):
          sentences.append(item)
          if len(sentences) == limit:
              break
      
      return sentences

  def addWords(self, word):

      # Adding words to vocabulary (if they don't exist in it previously)
      if word not in self.word2index:
          self.word2index[word] = self.index
          self.index2word[self.index] = word
          self.vocab.append(word)
          self.index += 1
  
  def createVocab(self, sentences):

      # Going line by line and token by token
      for sent in sentences:
          words = self.nlp(sent)
          for word in words:
              self.addWords(word.text)

  def handle(self):
      self.sentences = self.readJSON(self.limit, self.path)
      self.createVocab(self.sentences)
{{< /highlight >}}

<h2 id="creating-co-occurance-matrix">Creating Co-Occurance Matrix<a href="#creating-co-occurance-matrix" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h2>
<p>Usually, the limit of numpy matrices are around the region of 1e5*1e5. In some cases, the data might exceed this limit, hence I have used sparse matrices (so that all the cases are covered). Sparse matrices are able to hold larger matrices given that they are sparse in nature (mostly 0s).</p>
<p>I have used <a href="https://docs.scipy.org/doc/scipy/reference/generated/scipy.sparse.lil_matrix.html">lil_matrix</a> to create the sparse matrix, you can also use <a href="https://docs.scipy.org/doc/scipy/reference/generated/scipy.sparse.csr_matrix.html">csr_matrix</a> instead.</p>
<p>I have also used <a href="https://docs.scipy.org/doc/scipy/reference/generated/scipy.sparse.linalg.svds.html">svds</a>, which is a function which returns the Singular Value Decomposition matrices given a matrix and is optimised to work on sparse matrices. You can also use <a href="https://scikit-learn.org/stable/modules/generated/sklearn.decomposition.TruncatedSVD.html">truncated_svd</a> in place of it.</p>
{{< highlight python >}}

import numpy as np
from scipy.sparse.linalg import svds
from scipy.sparse import lil_matrix

class Matrix:
  def __init__(self, sentences, nlp, word2index, index2word, index, vocab):

      # Window refers to the window size for calculating co-occurances
      # K refers to the truncated value of svd we will be taking
      self.sentences = sentences
      self.nlp = nlp
      self.window = WINDOW
      self.word2index = word2index
      self.index2word = index2word
      self.index = index
      self.vocab = vocab
      self.k = K

  def getEmbeddings(self):
      return self.embeddings

  def getMatrix(self):
      return self.matrix

  def wordEmbeddings(self, word):
      return self.embeddings[self.word2index[word]]

  def moveWindow(self, matrix, window, sentences):

      # Going window to window to increment all co-occuring words
      for sent in sentences:
          words = self.nlp(sent)
          for i in range(0, len(words) - window + 1):
              for j in range(i + 1, i + window + 1):
                  if j < len(words):
                      i_index = self.word2index[words[i].text]
                      j_index = self.word2index[words[j].text]
                      matrix[i_index, j_index] += 1
                      matrix[j_index, i_index] += 1

      return matrix

  def getSVD(self, matrix):

      # Performing Single Value Decomposition on matrix and taking top k columns
      # The word embeddings is the right matrix, and hence we return only that
      right, sigma, left_t = svds(matrix, self.k)
      return right

  def handle(self):
      self.matrix = lil_matrix((self.index, self.index), dtype=np.float)
      self.matrix = self.moveWindow(self.matrix, self.window, self.sentences)
      self.embeddings = self.getSVD(self.matrix)
{{< /highlight >}}
