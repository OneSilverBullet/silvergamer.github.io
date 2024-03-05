---
layout: post
title:  "[Algorithm Design]Algorithm Thinking"
date:   2024-2-27
excerpt: "The basic algorithm design techniques."
tag:
- Algorithm Development
comments: false
blog: true
feature: https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/blogHead/directX12partI.jpg
---


## 1. Dynamic Programming

### 1.1 Conception

Dynamic Programming: making plans of events. Current method is used in finding the optimal value of a solution where several solutions exists.

There are two approaches:

* Memorization(Top-down): Recursion with overlapping subproblems, and a table or a map to store solutions to subproblems.
* Iterative Dynamic Porgramming(Bottom-up): Similar to 1, but often you can **get rid of recursion altogether** and populate the table iteratively.

Create the structure of subproblems
* Find **a structure of subproblems** parameterized by one or more variables.
* **Optimal Substrcuture Property**: Optimal solution to a problem should be reconstrcutable from optimal solutions to subproblems.
* **Overlapping Subproblems Property**: problems rely on subproblems. Many of the sub-sub-...-subproblems must be shared in order to achieve savings.

Four steps of Dynamic Programming:
* Characterize the structure of an optimal solution
* Recursively define the value of an optimal solution
* Compute the value of an optimal solution in bottom-up fashion or by using memorization.
* Construct an optimal solution from computed information.




### 1.2 Questions

#### 1.2.1 Assembly-line Scheduling

A product can be made on line 1 or line 2, each line consisting of several stations.

* e1,x1: the time needed to enter, exit line 1.
* e2,x2: the time needed to enter, exit line 2.
* a1,i: the time needed in station i on line 1.
* a2,i: the time needed in station i on line 2.
* t1,i: the time needed to transfer from line1 to 2.
* t2,i: the time needed to transfer from line2 to 1.

Quetion: Find the shortest time to complete the product?

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/al/al.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/al/al.png" align="center"></a>
    <figcaption>The assembly-line scheduling.</figcaption>
</figure>

Solution: Find the subproblem structure:

$$f_1[j]= min\{f_1[j-1] + a_{1,j}, f_2[j-1] + t_{2,j-1} + a_{1,j}\}$$

$$f_2[j]= min\{f_2[j-1] + a_{2,j}, f_1[j-1] + t_{1,j-1} + a_{2,j}\}$$

$$solution = min\{f_1[n] + x_1, f_2[n]+x_2\}$$

#### 1.2.2 Rod Cutting

Question: Rods are cut and sold. Rods of length are available. A cut is done at no cost. For each length i <= N of rod has a given price. Cut the rod and maximum the prirce.


<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/al/rod.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/al/rod.png" align="center"></a>
    <figcaption>The rod cutting.</figcaption>
</figure>


Solution: 

We express the values v_n for n>=1 in terms of optimal revenues from shorter rods:

$$r_n = max\{p_i + r_{n-i}: 1<=i<=n\}$$

Subproblem:

$$r_k = max\{p_i + r_{k-i}: 1<=i<=k\}$$

$$r_0=0$$

Store the length of the first rod in a seperate table s[1..n]. We modify MEMOIZED-CUT-ROD-AUX(p,n,r) to compute s of optimal first-piece sizes. 


#### 1.2.3 Matrix Multiplication


Question: Consider 4 matrices multiplying:

$$M_1 * M_2 * M_3 * M_4$$

There are several possible orders of evaluation but some orders save on the number of scalar multiplications.

Solution:

Define the matrices:

$$M_1 * M_2 * M_3 * ... * M_n$$


$$p_0 \times p_1, p_1 \times p_2, p_2 \times p_3, ..., p_{n-1} \times p_n$$

Goal: optimal parenthesization to minimize the total cost of multiplying.

So the subproblem is:

$$P(n) = 1, if n = 1$$

$$P(n) = \sum_{k=1}^{n-1}P(k)P(n-k), if n >= 2$$


Let OPT[i, j] denote the minimum cost of parenthesization of 

$$A_i * A_{i+1} * ... * A_j$$

We assume OPT[i, j] consists of performing k th multiplication last for some k between  i and j.

$$OPT[i, j] = OPT[i, k] + OPT[k+1, j] + p_{i-1} * p_{k} * p_{j}$$

Hence, the solution of the whole problem is OPT[1, n].
It is worth to mention that OPT[i, i] = 1. We can start with the Matrices Chain with 1 length. Using an iterative dynamic programming approach, we solve increasing larger subproblemns m[i, i + l - 1].


#### 1.2.4 Longest Common Subsequence

Question: Given two strings of characters:

x1x2x3...xn

y1y2y3...yn

Find the longest substring that in both strings. The substring can be obtained by ommitting some characters.

Solution:

If text1[i] == text2[j]:

$$OPT[i, j] = OPT[i-1, j-1] + 1$$

else:

$$OPT[i, j] = max\{OPT[i-1, j], OPT[i, j-1]\}$$


```
int longestCommonSubsequence(string text1, string text2) {

        vector<vector<int>> mem(text1.size() + 1, vector<int>(text2.size() + 1, 0));

        int i = text1.size();
        int j = text2.size();

        for(int k = 1; k <= i; ++k){
            for(int m = 1; m <= j; ++m){
                if(text1[k-1] == text2[m-1])
                    mem[k][m] = mem[k-1][m-1] + 1;
                else 
                {
                    mem[k][m] = std::max(mem[k-1][m], mem[k][m-1]);
                }
            }
        }
        
        return mem[i][j];
        
    }
```

Obviously, the time complicate rate is θ(mn).


#### 1.2.5 Longest Increasing Subsequence

Question: 

Given a sequence of numbers(a1,a2,a3,...,an) find the longest increasing subsequence(LIS) in it. Example:

sequence: 10, 3, 12, 18, 30, 4, 6, 21, 7, 20

LIS: 3, 4, 6, 7, 20

Solution：We assume l is the max length in current position i.

$$l_i = 1 + max\{l_j, 1 \leq j < i \}$$

```
    int lengthOfLIS(vector<int>& nums) {
        vector<int> mem(nums.size(), 1);

        for(int i = 1; i < nums.size(); ++i)
        {
            for(int j = 0; j < i; ++j){
                if(nums[i] > nums[j])
                    mem[i] = max(mem[j] + 1, mem[i]);                
            }
        }
        int res= 1;
        for(int i = 0; i < mem.size(); ++i){
            res = std::max(res, mem[i]);
        }

        return res;
    }
```

Obviously, the time complicate rate is θ(n^2).

#### 1.2.6 Optimal Binary Search Trees

We have nodes and the probabilities with which the nodes are searched. 

N nodes k1, k2, ..., kn, such that k1 < k2 < ... < kn.

Probabilities p1, p2, ..., pn of searching for these nodes.

probabilities q0, q1, ..., qn of searching for elements between nodes.





#### 1.2.7  Knapasack Problems

Questions:

We have a container of size W and a set of items i1, i2, . . . , in. Each item i
j has weight wj and value vj. Find a subset of items which together has weight ≤ W and that maximizes the total value.

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/al/pack.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/al/pack.png" align="center"></a>
    <figcaption>The knapsack problems.</figcaption>
</figure>

Solution:

Input: 
* w[1 ... n]: each weight of item
* v[1 ... n]: each value of item

Consider items [w1, w2, ... , wj], [v1, v2, ..., vj] and capacity W.

If we does not contain item i_j:
* [w1, . . . , wj−1], [v1, . . . , vj−1] and capacity W

If we contain item i_j:
* [w1, . . . , wj−1], [v1, . . . , vj−1] and capacity W − wj

So we assume the memorization is D[0..n, 0..W];

```
vector<vector<int>> dp(n, vector<int>(W, 0));

for(int j = 1; j < n; ++j){
    for(int c = 1; c < W; ++c){
        dp[j][c] = dp[j-1][c];
        if(w[j] <= c)
            dp[j][c] = max(dp[j][c], dp[j-1][c - w[j]] + v[j]);
    }
}
return dp[n][W];
```

So obviously, the running time is O(nW).
* running time O(n · W) is not polynomial
* Polynomial running time refers to polynomial in the input length. Therefore, polynomial running time for Knapsack would be expressed as a polynomial in terms of n and log W.
* n · 2^bitlength(W)








## 2. Greedy Algorithm

### 2.1 Conceptions

It is called **greedy** because when a choice is to be made, it chooses what **looks best at that moment**.

It gives simple approximation algorithms in many cases where **exact solutions** are
too difficult to get. When we use the greedy paradigm, we must verify that it is appropriate for the given problem.

The greedy approach always will get three types results:
* give optimal solutions
* good approximation
* is not suitable

#### 2.1.1 Natural Greedy Algorithm

Indeed, since items are divisible it makes sense to start taking as much of an item
that gives the biggest bang for the buck as possible
* Sort items in non-increasing order of vi/wi (value per unit weight)
* Process items in this order and for each item:
    * If the remaining capacity of the knapsack can accommodate the **entire item**, take the entire item
    * otherwise take **the largest portion** of the item you can filling the knapsack to capacity

#### 2.1.2 Huffman Codes

Instead of representing each character by a fixed-length code (7 or 8 bits typically),
most frequently used characters are given very short (binary character) code while
less frequently used characters are given longer codes... **variable-length code**

**prefix-free code** in which no code is a prefix of another code.
* Prefix-free codes can be **obtained** from **full binary trees** in which the characters are leaves.

Let C be a given alphabet with frequency c.freq defined for each character c.

Let d(c) denot the depth of c's leaf in the tree T.

The number of bits/the cost of the tree required:

$$B(T) = \sum_{c\in C} c.freq * d(c)$$

Solution:

1. order the characters by frequencies (c.freq) starting from smallest to largest.

repeat the following:

2. Join **first two elements of the list** into a tree.

3. Frequency of root of this tree = **sum of frequencies of the elements**,

4. Put the root of the tree back in the list, in order of frequencies.

until only one element remains in the list

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/al/hf.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/al/hf.png" align="center"></a>
    <figcaption>The huffman's algorithm.</figcaption>
</figure>

```
n ← |C|
Q ← C // Initialize min-priority queue, Q
for i ← 1, n − 1 do
    allocate a new node z
    x ← Extract=Min(Q)
    y ← Extract=Min(Q)
    z.left ← x
    z.right ← y
    z.freq ← x.freq + y.freq
    Insert(Q, z) // implementation of Insert determines order of duplicate keys
return Extract=Min(Q) 
```


#### 2.1.3 Proof of Huffman Codes

**Leema**: Greedy Choice Property of Optimal Prefix-free code trees.

Let x and y be two characters in C having the lowest frequencies. Then there exists
an optimal prefix-free code for C in which the codewords for x and y have the same
length and differ only in the last bit.
* x and y are leaves of the **same internal node**
* x and y will be **at maximum depth in the tree**.

**Proof**

Let T be an optimal prefix-free tree. If T does not satisfy the conditions of the Lemma, we show how to modify it to tree T'.
* T' satisfies the conditions of the Lemma.
* B(T') <= B(T)

Let a and b be any two characters that
are sibling leaves of maximum depth in
T. WLOG, assume a.freq ≤ b.freq and
x.freq ≤ y.freq.

x.freq ≤ a.freq and y.freq ≤ b.freq, and
dT (a), dT (b) ≥ dT (x), dT (y)

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/al/hf2.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/al/hf2.png" align="center"></a>
    <figcaption>The huffman's algorithm proof.</figcaption>
</figure>

B(T′) = B(T) + x.freq·dT′(x) + a.freq·dT′(a) + y.freq·dT′(y) + b.freq·dT′(b) − x.freq·dT (x) − a.freq·dT (a) − y.freq·dT (y) − b.freq·dT (b)

= B(T) + x.freq·dT (a) + a.freq·dT (x) + y.freq·dT (b) + b.freq·dT (y) − x.freq·dT (x) − a.freq·dT (a) − y.freq·dT (y) − b.freq·dT (b)

= B(T) − (a.freq − x.freq)(dT (a) − dT (x)) − (b.freq − y.freq)(dT (b) − dT (y))

≤ B(T)

Since T is optimal, we have B(T) ≤ B(T′), which implies B(T′) = B(T). Thus, T′ is an optimal tree in which x and y appear as sibling leaves of maximum depth, from which the lemma follows.


**Theorem**: Huffman’s algorithm produces an optimal prefix-free tree.

**Proof**

Base case 1: obvious.

Inductive Assumption: Huffman’s algorithm produces an optimal
prefix-free tree for all inputs with at most n characters for some n ≥ 2

Inductive step: consider an input C with n + 1 characters with x and y being two least frequent characters

Let T be an optimum tree for C with x and y siblings at the lowest level (exists by the Lemma)

Define a new character z with associated frequency z.freq = x.freq + y.freq.

Let **T′ be the prefix-free tree** for C ∪ {z} − {x, y} obtained by replacing the parent of x, y with z.

B(T′) 

= B(T) − x.freq·dT (x) − y.freq·dT (y) + z.freq·dT′(z)

= B(T) − (x.freq + y.freq)

By induction, Huffman’s algorithm finds **optimal tree H′** for C ∪ {z} − {x, y}.

B(H′) ≤ B(T′)

Also, by the algorithm’s definition tree H for **the original input C** satisfies

B(H) = B(H′) + x.freq + y.freq

≤ B(T′) + x.freq + y.freq

= (B(T) − (x.freq + y.freq)) + x.freq + y.freq

= B(T)

Since T is optimal tree for the whole input C and B(H) ≤ B(T) the tree output by Huffman’s algorithm, H is optimal too.


### 2.2 Questions

#### 2.2.1 Activity Selection Problem

Question:

We have a set of n proposed activities, S = {a1, a2, . . . , an}, for a single lecture hall.
Each activity ai has a start time si and finish time fi. Find the largest set of activities
that can be held in the lecture hall that respect the start - finish times. Two requests are compatible if their time slots do not overlap.

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/al/gre.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/al/gre.png" align="center"></a>
    <figcaption>The activity selection.</figcaption>
</figure>

Solution:

For our optimal solution, we ordered the activities by **earliest finish time**, i.e.,
consider jobs according to non-decreasing fi.

pre-sort all activities by termination time in monotonically increasing order such that
fi ≤ fi+1. O(nlogn)


#### 2.2.2 Scheduling Problems

Question: 

We have activities to schedule as before, each activity has a start time and finish time. Find **the smallest number of rooms** needed to schedule all activities.

Solution A:

List of activities a1, a2, ..., an sorted by start time. 
<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/al/gre1.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/al/gre1.png" align="center"></a>
    <figcaption>The activity scheduling problems.</figcaption>
</figure>


If we increase the number of rooms from m to m + 1 when scheduling activity ai,
we do it because all rooms 1 to m contain events whose
starting times are ≤ si and finish times are > fi.
Thus there are m + 1 requests that are incompatible with one another.

So the run-time is O(n^2).


Solution B: 

So we can as well put the activity in the room that is available earliest, if compatible.

Use a **priority queue** to store information about the **end of the last event in each room**. Implement the priority queue as a heap. The top element in the heap is **the room with earliest termination time**(min heap).

So the run-time is O(nlogn).


#### 2.2.3 Fractional Knapsack Problem

Question:

Find a subset of items or fractions of items which together have weight ≤ W and
that maximizes the total value.

Since we can take fractions, we can always fill-up the knapsack to maximum.

W = 50, {(w1 = 10, v1 = $60),(20, $100),(30, $120)}

Take items or **a fraction of an item** in order of value per unit of weight until knapsack
is full.


Solution: We can replace any part of the knapsack content with items having the same volume.

We just use the greedy method to fill the highest unit price pack.



## 3. Amortized Analysis

### 3.1 Conception


Amortized analysis can be used to show that the average cost of an operation is small even though a single operation within the sequence might be expensive. Cost of the “expensive” operation is 
**“amortized” by the cheap ones**.
* If an “expensive” operation is not happening very often and is in a sequence of “cheap” operations, the average cost can be low.
* It is different from average-case analysis, the probability is not involved.



### 3.2 Aggregate Analysis


We show that for all n, a sequence of n operations takes worst case time T(n) in total. Thus, **cost per operation is T(n)/n.**



<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/al/counter.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/al/counter.png" align="center"></a>
    <figcaption>The counter.</figcaption>
</figure>

#### 3.2.1 Binary Counter

Question:

```
INCREMENT(A, k)
    i ← 0
    while i < k & A[i] = 1 do
        A[i] ← 0
        i ← i + 1
    if i < k then
        A[i] ← 1
```

What is the cost of repeating INCREMENT n times?

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/al/ana.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/al/ana.png" align="center"></a>
    <figcaption>The aggregate analysis.</figcaption>
</figure>

Not every bit flips each time.


<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/al/ana2.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/al/ana2.png" align="center"></a>
    <figcaption>The aggregate analysis.</figcaption>
</figure>

So on average the cost is 2 per operation, i.e. O(1).

### 3.3 Accounting Method 

We assign to operations cost that differ from the actual cost.
* Some are charged more than the actual cost,
* Some are charged less than the actual cost.

The amount charged is called **amortized cost**.


What we need is that everything balances out at the end, i.e. **the total of amortized charges must be at least as high as the actual cost**. The extra is called credit.
* by over-charging for some operations that are easy to count we don’t need to charge anything to operations that are less easy to count.

#### 3.3.1 Stack Operations with Multipop

Now consider **any sequence of n PUSH, POP and MULTIPOPs** on initially **empty stack**.

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/al/mm.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/al/mm.png" align="center"></a>
    <figcaption>The stack operation.</figcaption>
</figure>

Each PUSH is over-charging the cost by 1. Thus, we “pre-pay” for the eventual POP or MULTIPOP when push is done.

If at most n PUSH operations are done, the total amortized cost is ≤ 2n and the total actual cost is O(n).

#### 3.3.2 Binary Counter

Let the **amortized cost of setting a bit from 0 to 1 be 2**.

Thus we **charge 1 to set it and we completely pre-payed the eventual change from 1 to 0**.

Thus the amortized cost of a single
INCREMENT is 2 = O(1) since only
one 0 bit is flipped to a 1 bit.

A sequence of n INCREMENTs is O(n)

### 3.4 Potential Method 

We represent the prepaid work as “potential energy” that can be released later for **the cost of future operations**.

The potential is associated with the data structure as a whole rather than with specific objects.

We have a data structure D. D_i ... the data structure D after applying **ith operation** to it.

Initially, we have D_0. A **potential function Φ** maps the data structure **Di** to a real number **Φ(Di)**, the associated potential.

$$\hat c_i = c_i + \phi(D_i) - \phi(D_{i-1})$$

The total amortized cost of n operations is

$$\sum_{i=1}^{n} \hat c_i = \sum_{i=1}^{n} c_i + \phi(D_n) - \phi(D_{0})$$

If Φ(Dn) ≥ Φ(D0) then the total amortized cost is at least as great as the total actual cost.

$$\sum_{i=1}^n c_i$$

### 3.4.1 Multipop














 