---
title: "Fast Multiplication"
date: 2021-11-07T12:35:02+05:30
tags:
  - math
---

<p>Multiplication, generally, is relatively easy to do, understand and code. Even the 2nd standard algorithm of multiple each digit with every other digit is simple enough to write. The issue is that such an algorithm takes O(n^2) time complexity, which is a lot, especially if you want to multiply huge numbers. Hence why you need better algorithms in place to do your job. Note, languages like Python have lots of optimisations on top of specific algorithms used for computing products when the user types in a simple &lsquo;*', but the article is more about understanding these algorithms.</p>
<h2 id="fast-exponentiation">Fast Exponentiation<a href="#fast-exponentiation" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h2>
<p>Most useful in the cases of binary numbers, fast exponentiation allows a user to get the exponent of a number in logN time. One can also use this algorithm for getting modulo of huge powers or for matrix multiplication!
Well, since it&rsquo;s logN time, the first thought which comes to mind is that conversion into binary is required, which is correct. A simple prerequisite to understanding this algorithm is knowing how exponents work, importantly, that \(a^{b + c} = a^b + a^c\). Let&rsquo;s take an example of finding 3*50</p>
<figure>
    <img src="https://i.imgur.com/lW3iZNU.png"/> 
</figure>

<p>Converting the exponent to binary, we get the exponent in this form. Now, this is exciting, since if you notice, all the powers are strictly in powers of 2. It is important because now we do not have to worry about multiplying arbitrary powers with other things; we continue to square logN times, at which we will get our answer!</p>
<figure>
    <img src="https://i.imgur.com/7Jc5gu8.png"/> 
</figure>

{{< highlight python >}}
def expo(base, power):
    if power == 0:
        return 1
    result = expo(base, power//2)
    base = base*base
    if n&1:
        result = result*base
    return result
{{< /highlight >}}

<h2 id="karatsuba-algorithm">Karatsuba Algorithm<a href="#karatsuba-algorithm" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h2>
<p>Discovered by Antony Karatsuba, this is a fast multiplication algorithm that only slightly improves a typical O(\(n^2\)) case. It is based on dividing numbers into two and multiplying them recursively to answer (also, it has lesser space complexity :p)
It is based on the straightforward thought that addition takes lesser time than multiplication, which, I hope, is not very hard to prove. Here, ideally, we will also be using previously learnt fast exponentiation.</p>
<figure>
    <img src="https://i.imgur.com/9WZcjZT.png"/> 
</figure>

<p>As can be seen, we divide the numbers into two based on powers of 10 (You can also do this based on powers of 2), and we multiple recursively. This algorithm saves time in computing ad + bc, where it computes (a + b)*(c + d) and subtracts previously calculated products to get the required answer. Essentially, instead of computing four products, it computes 3, which is why its complexity is O(\(n^{log_23}\))</p>
<figure>
    <img src="https://i.imgur.com/e9e3LPG.png"/> 
</figure>

{{< highlight python >}}
def karatsuba(x, y):
    if len(str(x)) == 1 or len(str(y)) == 1:
        return x*y
    n = max(len(str(x)), len(str(y)))/2
    a = x / 10**n
    b = x % 10**n
    c = y / 10**n
    d = y % 10**n

    ac = karatsuba(a, c)
    bd = karatsuba(b, d)
    ad_bc = karatsuba(a+b, c+d) - ac -bd
    return 10**(2*n)*ac + 10**n * (ad_bc) + bd
{{< /highlight >}}

<h2 id="fast-fourier-transformation">Fast Fourier Transformation<a href="#fast-fourier-transformation" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h2>
<p>This is the most complex algorithm out of the three so far. It is essentially used for multiplying polynomials, but you can ofcourse write a normal number as a polynomial quite easily. This requires knowledge about complex numbers so its advisable you read up on that before going through this.</p>
<p>Let there be two polynomials A(x) and B(x) of degrees n and m respectively and we want to find C(x) = A(x) * B(x). Before going through best options, lets analyse stuff a bit.
There are 3 traditional ways to represent a polynomial:</p>
<ol>
<li>Coeffecient Vector: F(x) = \(a_0 + a_1x + a_2x^2 .. a_nx^n\)</li>
<li>Roots: F(x) = \( (x-r_1)(x-r_2)&hellip;(x-r_n) \)</li>
<li>Samples: F(x) = \( { (x_1, y_1), (x_2, y_2) &hellip; (x_{n+1}, y_{n+1})}\)</li>
</ol>
<p>Depending on the representaiton, each operation takes different amount of time:</p>
<table>
<thead>
<tr>
<th>Algorithm</th>
<th>Coeffecient</th>
<th>Roots</th>
<th>Samples</th>
</tr>
</thead>
<tbody>
<tr>
<td>Evaluation</td>
<td>O(n)</td>
<td>O(n)</td>
<td>O(n^2)</td>
</tr>
<tr>
<td>Addition</td>
<td>O(n)</td>
<td>Inf</td>
<td>O(n)</td>
</tr>
<tr>
<td>Multiplication</td>
<td>O(n^2)</td>
<td>O(n)</td>
<td>O(n)</td>
</tr>
</tbody>
</table>
<p>As you can see, for multiplication (which is our focus), simply having polynomial in coefficient form is bad, and if we could convert it into either sample or root we would be able to do whole thing in O(n). So we try to convert it into sample form:</p>
<figure>
    <img src="https://i.imgur.com/Q63Ii0I.png"/> 
</figure>

<p>Let Vandermonde Matrix V be a matrix of any random n points where</p>
<p>$$A = \begin{pmatrix}
a_0,\<br>
a_1,\<br>
.,\<br>
.,\<br>
.,\<br>
a_{n-1},\<br>
\end{pmatrix}$$</p>
<p>$$Y = \begin{pmatrix}
y_0,\<br>
y_1,\<br>
.,\<br>
.,\<br>
.,\<br>
y_{n-1}, \<br>
\end{pmatrix} $$</p>
<p>A is a mtrix of Coefficients and Y is a matrix of results of A(x). For vandermonde&rsquo;s matrix V, we know that:
$$det(V) = \prod_{1 \leq j \lt k \leq n} (x_k - x_j)$$</p>
<p>Proof for which is available here: <a href="https://towardsdatascience.com/the-vandermonde-determinant-a-novel-proof-851d107bd728">https://towardsdatascience.com/the-vandermonde-determinant-a-novel-proof-851d107bd728</a></p>
<p>Hence, we find out that V is invertible, and we realise that we can convert to the coefficient form of representing a polynomial if we are given the sample form.</p>
<p>As can be seen from the table, sample form is quite convenient when it comes to addition and multiplication of polynomials. Its main issue is that we have to evaluate the polynomial for a particular given point, for which we end up using Lagrange&rsquo;s Interpolation at O(n^2). Added to this, the degree of resulting polynomial is the sum of the two polynomial which we are multiplying, hence we require 2n + 1 points instead of just n + 1 points.</p>
<p>So what we end up doing instead is that when it comes to individual evaluation of polynomial, we prefer converting it back to coefficient form and evaluate it in O(n) instead.</p>
<figure>
    <img src="https://s3.us-west-2.amazonaws.com/secure.notion-static.com/af05d5a4-cb09-4ebe-babb-ff1f2620f99a/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&amp;X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20211107%2Fus-west-2%2Fs3%2Faws4_request&amp;X-Amz-Date=20211107T095415Z&amp;X-Amz-Expires=86400&amp;X-Amz-Signature=afd3a4925a86f383131e0dc8c4bf2238645bc934985e7a6174829245b71350e5&amp;X-Amz-SignedHeaders=host&amp;response-content-disposition=filename%20%3D%22Untitled.png%22"/> 
</figure>

<p>We can use any points as evaluation points, but we observe that by choosing the points as \(2n^{th}\) roots of unity the conversion time is reduced to O(\(nlogn\)) instead of O(\(n^2\)).</p>
<p>Also, we need some knowledge about complex numbers :p</p>
<h3 id="nth-roots-of-unity">Nth Roots of Unity<a href="#nth-roots-of-unity" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h3>
<p>These are defined as
$$  \omega_n = e^{\frac{2\pi \iota}{n}} $$</p>
<figure>
    <img src="https://i.imgur.com/6CXGPZv.png"/> 
</figure>

<h3 id="cancellation-lemma">Cancellation Lemma:<a href="#cancellation-lemma" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h3>
<p>For any integer \(n\geq 0, d\geq 0, k\geq 0\)
$$
\omega_{dn}^{dk} = \omega_{n}^{k}
$$</p>
<p>Proof:
$$
\omega_{dn}^{dk} = (e^{\frac{2\pi \iota}{dn}})^{dk} = (e^{\frac{2\pi\iota}{n}})^k = \omega_n^k
$$</p>
<h3 id="halving-lemma">Halving Lemma:<a href="#halving-lemma" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h3>
<p>If \(n\) is even, then the squares of the n complex nth roots of unity are the \(n/2\) complex \(n/2\)th roots of unity.</p>
<p>Proof: By Cancellation Lemma we know that
$$
(\omega_n^k)^2 = \omega_{\frac{n}{2}}^k
$$</p>
<p>And we know each term on the LHS will occur twice,  hence proved</p>
<h3 id="summation-lemma">Summation Lemma<a href="#summation-lemma" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h3>
<p>Sum of all nth roots of unity is equal to 0</p>
<p>We need to evaluate the polynomial A(x) at 2nth roots of unity and store its result in Y. This vector Y is called Discrete Fourier Transformation of the coefficient vector A, which is represented as <code>y = DFT_n(a)</code>.</p>
<p>By using a method known as the Fast Fourier Transformation (FFT), which takes advantage of the special properties of complex roots of unity, we can computed <code>DFT_n(a)</code> in time O(\(n logn \)) as opposed to O(\(n^2\)) of the straightforward method. Here, we assume that n is a power of 2 for convenience</p>
<p>FFT employs a divide and conquer technique where it divides the coefficients of A(x) on the bases of even and odd indices as:
$$
A^{[0]}(x) = a_0 + a_2x + . . . + a_{n-2}x^{\frac{n}{2} - 1}
$$
$$
A^{[1]}(x) = a_1 + a_3x + . . . + a_{n-1}x^{\frac{n}{2} - 1}
$$</p>
<p>Where
$$ A(x) = A^{[0]}x^2 + xA^{[1]}x^2$$</p>
<figure>
    <img src="https://s3.us-west-2.amazonaws.com/secure.notion-static.com/8343035f-9c84-495a-b6db-520c2a189a5b/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&amp;X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20211107%2Fus-west-2%2Fs3%2Faws4_request&amp;X-Amz-Date=20211107T100659Z&amp;X-Amz-Expires=86400&amp;X-Amz-Signature=93c06cbac496306eff489229a0c0bc049f204e3584dc16f9751933bc5f41238a&amp;X-Amz-SignedHeaders=host&amp;response-content-disposition=filename%20%3D%22Untitled.png%22"/> 
</figure>

<p>Since \(\omega_n^0\) collapses on squaring, the size is constantly reduced by a factor of 2 as we progress in the recursive tree. Hence, we get time complexity as:
$$ T(n) = 2*T(n/2) + \theta(n)
= \theta(nlgn) $$</p>
<p>Now, for interpolation of the complex roots, we write \(Y = V^{-1}A\) where \(V_{j,k}^{-1} = \frac{\omega^{-kj}_n}{n} \). Since this is of the same type as V, we can use the same procedure of divide and conquer when it comes to interpolating to get back to coefficient form.</p>
{{< highlight cpp >}}
using cd = complex<double>;
const double PI = acos(-1);

void fft(vector<cd> & a, bool invert) {
    int n = a.size();
    if (n == 1)
        return;

    vector<cd> a0(n / 2), a1(n / 2); // divide
    for (int i = 0; 2 * i < n; i++) {
        a0[i] = a[2*i];
        a1[i] = a[2*i+1]; 
    }
    fft(a0, invert); // recursively divide
    fft(a1, invert);

    double ang = 2 * PI / n * (invert ? -1 : 1);
    cd w(1), wn(cos(ang), sin(ang)); // omega 
    for (int i = 0; 2 * i < n; i++) {
        a[i] = a0[i] + w * a1[i];
        a[i + n/2] = a0[i] - w * a1[i];
        if (invert) {
            a[i] /= 2;
            a[i + n/2] /= 2;
        }
        w *= wn;
    }
}

vector<int> multiply(vector<int> const& a, vector<int> const& b) {
    vector<cd> fa(a.begin(), a.end()), fb(b.begin(), b.end()); // defining vectors
    int n = 1;
    while (n < a.size() + b.size()) 
        n <<= 1;
    fa.resize(n);
    fb.resize(n);

    fft(fa, false); // convert into sample form
    fft(fb, false);
    for (int i = 0; i < n; i++)
        fa[i] *= fb[i];
    fft(fa, true); // convert answer back to coeff form

    vector<int> result(n);
    for (int i = 0; i < n; i++)
        result[i] = round(fa[i].real()); // answer is only the real part
    return result;
}
{{< /highlight >}}
