---
layout: post
title:  "[Algorithm Design]Advanced Algorithm Thinking"
date:   2024-3-27
excerpt: "The algorithm design techniques."
tag:
- Algorithm Development
comments: false
blog: true
feature: https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/blogHead/directX12partI.jpg
---



## 1. Linear Programming

### 1.1 Conception






## 1. Algorithm Basis Conception

### 1.1 Asymptotic Notation

Scalability: Asymptotics.

The counting of time is not exact, we express the run-time using the asymptotic notation.
* O: f(n)=O(g(n)) if there exists postive constants c, n0 such that f(n)<=cg(n) for n >= n0.
    * g(n) is an upper bound.
* Ω: f(n)=Ω(g(n)) if there exists postive constants c, n0 such that f(n)>=cg(n) for n >= n0.
    * g(n) is a lower bound.
* θ: f(n)=θ(g(n)) if there exists postive constants c1, c2, n0 such that c2g(n)>=f(n)>=c1g(n) for n >= n0.
    * f(n) grows as fast as g(n).
* o: f(n)=o(g(n)) if there exists postive constants c, n0 such that f(n)< cg(n) for n >= n0.
    * f(n) grows slower than g(n)
* w: f(n)=w(g(n)) if there exists postive constants c, n0 such that f(n)> cg(n) for n >= n0.
    * f(n) grows faster than g(n)

Simplification of expressions in asymptotic analysis.

(1) If c is a constant:

$$O(f(n) + c) = O(f(n))$$

$$O(cf(n)) = O(f(n))$$

(2) If f1=O(f2):

$$O(f1(n) + f2(n)) = O(f2(n))$$

(3) If f1=O(g1(n)) and f2=O(g2(n)) then

$$O(f_1(n)f_2(n))= O(g_1(n)g_2(n))$$

(4) If 

$$\lim_{n \to \inf}\frac{g(n)}{f(n)} = 0$$

Then:

$$g(n)=O(f(n))$$

$$f(n) = Ω(g(n)) $$


(5) If 

$$\lim_{n \to \inf}\frac{g(n)}{f(n)} = c$$

Then:

$$g(n)=θ(f(n))$$

$$f(n) = θ(g(n)) $$



### 1.2 Recurrence

An algorithm that breaks a problem of size n into one subproblem of size n/3 and another of size 2n/3, taking θ(n) time to divide and combine:

$$T(n) = T(n/3) + T(2n/3) + θ(n)$$

Solution:

$$T(n) = θ(nlgn)$$

### 1.3 Substitution Method

Steps:

(1) Guess the Solution.

(2) Use induction to find the constants and show that the solution works.

Example:

Determine an asymptotic upper bound on T(n) = 2T(n/2) + θ(n). Floor function ensures that T(n) is defined over integers.

Solution:

Guess: T(n) = O(nlgn)

Inductive Step:

We assume that T(n) <= cnlgn for all numbers >= n0 and < n. 

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/al/ss1.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/al/ss1.png" align="center"></a>
    <figcaption>Substitue into the recurrence.</figcaption>
</figure>













 