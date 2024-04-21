---
layout: post
title:  "[Algorithm Design]NP Problems"
date:   2024-4-17
excerpt: "The analysis of NP problems."
tag:
- Algorithm Development
comments: false
blog: true
feature: https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/blogHead/directX12partI.jpg
---



## 1. Computational Complexity

### 1.1 Conception

Efficient means polynomial run-time complexity: on outputs of size n, the worst-case running time is O(n^k) for some constant k.

* Tractable: solvable in polynomial time.
    * Finding the shortest simple path between vertices u and v in a given graph.
    * Determine if there is an Eulerian tour in a given graph(**uses every edge exactly once but vist node multiple**)
    * Testing 2 colorability
    * Satisfiability when each clause has two literals
* Intractable: requires superpolynomial time
    * **Finding the longest simple path between vertices u and v in a given graph**.
    * Determine if there is a **Hamiltonian circuit** in a given graph(**use every vertex exactly once**).
    * Testing 3-colorability.
    * Satisfiability when each clause has three literals.

**Computational complexity theory** aims to measure the amount of resources required for various computation problems.

* Modifying the problem
* Restricting inputs to be of special case
* Consider randomized inputs
* Consider approximation algorithms

**Exptime**: Many problems appear not to have a polynomial time algorithm.

**NP-Completeness**: Theory shows **relations and explains behavior of many combinatorial problems** from many field:
* Logic
* Graph
* Databases

**Formal Definitions**:
* **Computation**: Turing machine, Random Access Machine, Lambda-Calculus.
* **Computational Problem**: abstract definition, abstract inputs, abstract outputs.

### 1.2 Formal Languages

**Non-trivial mathematical objects**: functions, sets, graphs, integers, etc.

Each higher-level mathematical object is encoded via bits.
* **Alphabet**$\sum$: a **finite set** of characters/symbols.
* **String or Sentence**: a **finite sequence** of character from $\sum$.
* **Language**: L a set(could be finite or infinite) of strings over $\sum$.

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/al/conca.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/al/conca.png" align="center"></a>
    <figcaption>Language operation.</figcaption>
</figure>

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/al/conca2.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/al/conca2.png" align="center"></a>
    <figcaption>Language operation.</figcaption>
</figure>

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/al/conca3.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/al/conca3.png" align="center"></a>
    <figcaption>Language operation.</figcaption>
</figure>

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/al/star.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/al/star.png" align="center"></a>
    <figcaption>Star operation and Plus operation.</figcaption>
</figure>


### 1.3 Decision Problems

**Decision Problems**: A problem P is a decision problem, if the answer to P is either **yes or no**.

**Optimization Problems**: Answer is **a number maximizing or minimizing a given objective**.

**Search Problems**: Answer is **some object(set of vertices, function) subject to given constraints**.

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/al/problemexample.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/al/problemexample.png" align="center"></a>
    <figcaption>Problems Examples.</figcaption>
</figure>

(1) Observe that solving optimization problem also solves the corresponding decision problem.



### 1.4 The Proof Methods

Reducing from a known NP-complete problem

Step1: L $\in$ NP

Step2: Select a known NP-complete problem L'

Step3: Prove L' $\leq_p L$
* constructing a reduction f from L' to L.
* proving that f is correct and runs in polytime.

Example: CLIQUE $\in$ NPC

**Step1**: CLIQUE $\in$ NP
* the certificate is a set S of vertices
* the verifier A(< G, k>, < S >)
    * check that < G, k > encodes a valid CLIQUE  instance
    * check that < S > encodes a subset of vertices and |S| $\ge$ k

if all checks succeed return yes, otherwise return no


**Step2**: we choose to reduce from 3-CNF-SAT

To show 3-CNF-SAT $\leq_p$ CLIQUE, we need to convert input φ to 3-CNF-SAT into a G, k  to CLIQUE such that.

* φ is satisfiable if and only if G has a clique of size at least k.

**Step3**: construct a reduction  from 3-CNF-SAT to CLIQUE.

* φ: 3-CNF-SAT formula.

* X={x1, x2, ..., xn} - literals of φ.

* C1, ..., Cm: clauses of φ.

Construct graph G as follows:

* For each clause $C_j = x_1^j $



$$\lim_{n \to \inf}\frac{g(n)}{f(n)} = 0$$

Then:

$$g(n)=O(f(n))$$

$$f(n) = Ω(g(n)) $$


(5) If 

$$\lim_{n \to \inf}\frac{g(n)}{f(n)} = c$$

Then:

$$g(n)=θ(f(n))$$

$$f(n) = θ(g(n)) $$



<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/al/ss1.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/al/ss1.png" align="center"></a>
    <figcaption>Substitue into the recurrence.</figcaption>
</figure>













 