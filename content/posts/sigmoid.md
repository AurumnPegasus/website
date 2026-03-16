---
title: "Linear Classifier"
date: 2021-09-21T12:00:06+09:00
archived: true
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
{{< highlight python >}}
def getClasses(self):
  # Assumed equal amount of samples in each class, you can take anything
  # covariance matrix = [[12, 0], [0, 12]]

  # mean_a = [-5, 3]
  train_a = np.random.multivariate_normal(self.mean_a, self.covariance, self.train_len//2)
  test_a = np.random.multivariate_normal(self.mean_a, self.covariance, self.test_len//2)

  # mean_b = [3, -5]
  train_b = np.random.multivariate_normal(self.mean_b, self.covariance, self.train_len//2)
  test_b = np.random.multivariate_normal(self.mean_b, self.covariance, self.test_len//2)

  return train_a, test_a, train_b, test_b
{{< /highlight >}}

<p>Once we have samples from the two classes, we append them together (and add 1 for bias) to get train and test dataset.</p>
{{< highlight python >}}
def handle(self):
  train_a, test_a, train_b, test_b = self.getClasses()

  # Assigning 1 label to class A and -1 to class B
  self.train_x = np.concatenate((np.concatenate((train_a, train_b), axis=0), np.asmatrix(np.ones(self.train_len)).T), axis=1).T
  self.train_y = np.asmatrix(np.concatenate( (np.ones(self.train_len//2), -1*np.ones(self.train_len//2)), axis=0))

  self.test_x = np.concatenate((np.concatenate((test_a, test_b), axis=0), np.asmatrix(np.ones(self.test_len)).T), axis=1).T
  self.test_y = np.asmatrix(np.concatenate( (np.ones(self.test_len//2), -1*np.ones(self.test_len//2)), axis=0))
{{< /highlight >}}

<h2 id="gradiant">Gradiant<a href="#gradiant" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h2>
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
{{< highlight python >}}
def training(self):
  x, y = self.g.getTrain()

  # Pre-defining weights as 1
  w_prev = np.asmatrix(np.zeros(3))
  w = np.asmatrix(np.ones(3))

  train_errors = []
  iteration = 0

  while np.linalg.norm(w - w_prev) > 1e-4:

      # Computing the loss via MSE
      loss = 1/ self.train_len * np.linalg.norm(y - self.sigmoid(np.matmul(w, x)))**2
      train_errors.append(loss)

      w_prev = w

      # w = w - η∇J
      # @: hadamard product, matmul: dot product
      w = w - self.eta*2 / self.train_len * (
          np.mat(np.array(-y)*(np.array(self.sigmoid(w@x))*np.array((1-self.sigmoid(w@x)))))@x.T 
          + 
          np.mat(np.array(self.sigmoid(w@x))*np.array(self.sigmoid(w@x))*np.array(1 - np.array(self.sigmoid(w@x))))@x.T
      )
      iteration += 1
  
  return w
{{< /highlight >}}

<h2 id="results">Results<a href="#results" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h2>
<p>We use the learned $w$ to create the classification line (this is only possible since we have a 2-dimensional space, for higher dimensions it is impossible to visualise such equations).</p>
{{< highlight python >}}
weights = t.training()

m = -weights[0, 0] / weights[0, 1]
c = -weights[0, 2] / weights[0, 1]

x = np.linspace(-15, 15, 300)
y = m*x + c

# Plotting the line on train dataset (can do the same for test dataset as well)
plt.figure(figsize=(8, 8))
plt.scatter([point[0] for point in train_a], [point[1] for point in train_a], color = 'green', label = 'Class 1')
plt.scatter([point[0] for point in train_b], [point[1] for point in train_b], color = 'blue', label = 'Class 2')
plt.plot(x, y, '-r')
plt.legend()
plt.show()
{{< /highlight >}}

<p>We see the learned classifier performing well on the training set:</p>
<figure>
    <img src="https://i.imgur.com/r49wLqC.png"/> 
</figure>

<p>Testing the same on test dataset we find:</p>
<figure>
    <img src="https://i.imgur.com/HvEWPcg.png"/> 
</figure>

<h2 id="references">References:<a href="#references" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h2>
<p>Introduction to DL: <a href="https://github.com/sayarghoshroy/Intro_to_DL_tutorial">https://github.com/sayarghoshroy/Intro_to_DL_tutorial</a></p>
