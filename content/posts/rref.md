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
{{< highlight python >}}
def rref(matrix):
  dimension = matrix.shape
  numrows = dimension[0]
  numcols = dimension[1]
  lead = 0 # or pivot
  for r in range(numrows):
      # <END: if you reach last column>

      # <LOOP: to find next pivot>

      # <CONVERT: convert all other rows based on pivot>
{{< /highlight >}}

<p>Now, lets write code for each one by one. The first part is END, which is essentially that if you dont have any more columns, it means that the matrix is already in a RREF state, hence return.</p>
{{< highlight python >}}
# END:
if lead >= numcols:
  print(self.matrix)
  return
{{< /highlight >}}

<p>The loop parts requires you to go through each column in the row to determine if the number is suitable to be a pivot. If you find none, then go to the next column! (this means current column was all zeros and must be put at last). If there are no pivots at all to be found, that means you have already reached RREF</p>
{{< highlight python >}}
# LOOP:
index = r
while matrix[index][lead] = 0:
  index += 1
  if numrows == index:
      # No pivots in my row, all zeros
      lead += 1
      if lead == numcols:
          # No pivots at all
          print(matrix)
          return

# Swap if I had to go to the next column
matrix[[index, r], :] = matrix[[r, index], :]
{{< /highlight >}}

<p>I have my pivot now <code>matrix[r][lead]</code>, which I need to convert to 1 via elementary row operations. Once that is done, everything else in the column needs to be converted to 0s as well</p>
{{< highlight python >}}
# CONVERT:

# R = R/k
matrix[r] = matrix[r] / matrix[r][lead]

for i in range(numrows):
  if i != r:
      # I = I - cR
      matrix[i] = matrix[i] - matrix[i][lead]*matrix[r]
{{< /highlight >}}

<h2 id="walkthrough">🚶Walkthrough:<a href="#walkthrough" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h2>
{{< highlight text >}}
matrix: array([[ -12,  12,   2],
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

since lead >= numcols, exit
{{< /highlight >}}

<h2 id="final-code">Final Code:<a href="#final-code" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h2>
{{< highlight python >}}
import numpy as np

class RREF:
  def __init__(self, matrix):
      self.matrix = matrix

  def rref(self):
      dimension = self.matrix.shape
      numrows = dimension[0]
      numcols = dimension[1]
      lead = 0
      for r in range(0, numrows):
          if numcols <= lead:
              print(self.matrix)
              return

          index = r
          while self.matrix[index][lead] == 0:
              index += 1
              if numrows == index:
                  index = r
                  lead += 1
                  if lead == numcols:
                      print(self.matrix)
                      return

          # swap if my row is all 0s
          self.matrix[[index, r], :] = self.matrix[[r, index], :]

          # make my first number = 1
          self.matrix[r] = self.matrix[r] / self.matrix[r][lead]

          for i in range(numrows):
              if i != r:
                  print(f"Matrix: self.matrix[i] = {self.matrix[i]} - {self.matrix[i][lead]}*{self.matrix[r]}")
                  self.matrix[i] = self.matrix[i] - self.matrix[i][lead]*self.matrix[r]
          lead += 1

      ic(self.matrix)

m = np.array([
  [-12, 12, 2],
  [12, -12, -2],
  [2, -2, -17]
])
r = RREF(m)
r.rref()
{{< /highlight >}}

<h2 id="references">References:<a href="#references" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h2>
<ul>
<li><a href="https://en.wikipedia.org/wiki/Row_echelon_form#Reduced_row_echelon_form">Understanding RREF from wikipedia</a></li>
<li><a href="https://rosettacode.org/wiki/Reduced_row_echelon_form">Psuedocode from Rosetta Code, containing implementation in lots of different languages!</a></li>
</ul>
