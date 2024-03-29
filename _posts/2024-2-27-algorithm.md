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



### 1.4 Recursion Trees Method

#### Regular Example

Used to generate a guess, then verify by substitution method.

Example: T(n) = 3T(n/4) + θ(n^2). Draw out a recursion tree.


<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/al/rt.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/al/rt.png" align="center"></a>
    <figcaption>Recursion Tree.</figcaption>
</figure>

Subproblem size for nodes at depth i is n/4^i. So the base case

$$n/4^i=1$$

$$n=4^i$$

$$i=log_4n$$

Each level has 3 times as many nodes as the level above. Each internal node at depth i has cost c(n/4^i)^2.

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/al/rt2.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/al/rt2.png" align="center"></a>
    <figcaption>Recursion Tree further proof.</figcaption>
</figure>


Then, we use substitution method to verify O(n^2) upper bound. We assume T(n) <= dn^2.

$$T(n) <= 3T(n/4) + cn^2$$

$$T(n) <= 3d(n/4)^2 + cn^2 = 3/16*dn^2 + cn^2 <= dn^2$$

And the d exist for the correctness of above equation.

#### Irregular Example

T(n) = T(n/3) + T(2n/3) + θ(n).

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/al/rt3.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/al/rt3.png" align="center"></a>
    <figcaption>Irregular Recursion Tree.</figcaption>
</figure>


The leftmost branch reaches n=1 after log_3 n levels. The rightmost branch reaches n=1 after log_{3/2}n levels.

So, we assume that 
* In lower bound guess: 
$$T(n) >= dnlog_3n=Ω(nlgn)$$

* In the upper bound guess:

$$T(n) <= dnlog_{3/2}n=O(nlgn)$$

Then prove in substitution method.


### 1.5 Master Theorem

Let a>=1 and b>1 be constants, let f(n) be a function, and let T(n) be defined on the nonnegative integers by the recurrence.

$$T(n) = aT(n/b) + f(n)$$

(1) If

$$f(n) = O(n^{log_b^{a-β}})$$

for some β>0, then:

$$T(n)= θ(n^{log_b^a})$$

(2) If

$$f(n) = O(n^{log_b^{a}}lg^kn)$$

for some k>0, then:

$$T(n)= θ(n^{log_b^a}lg^{k+1}n)$$

(3) If

$$f(n) = O(n^{log_b^{a+β}})$$

for some β>0, and if

$$af(n/b)<=cf(n)$$

for some constant c < 1 and all sufficiently large n, then T(n) = θ(f(n)).

Examples:

$$T(n) = 7T(n/3) + n^2$$

$$n^{log_3^7}=n^{1.8}$$

$$7(n/3)^2<=cn^2$$

if c>=7/9, the equation is correct, so the run-time is θ(n^2).




## 2. Divide and Conquer

### 2.1 Divide and Conquer

(1) Divide the problem into subproblems.

(2) Conquer each subproblem by solving them recursively.

(3) Combine the solutions to the subproblems into a solution of the original problem.

The run-time of divide and conquer algorithm:

$$T(n) = aT(n/b) + D(n) + C(n)$$

* D(n): the time breaking a problem into subproblems.
* C(n): the time combining subproblems into a solution

#### BinSearch

```
int binSearch(x, A, i, j)
{
    if(i>j) return -1;
    mid = (i+j)/2;
    if(x < A[mid])
        return binSearch(x, A, i, mid-1);
    else if(x > A[mid])
        return binSearch(x, A, mid+1, j);
    return mid;
}
```

So use the master Theorem state 3, the run time of binSearch is θ(lgn).


#### Merge Sort

```
MergeSort(A, p, r):
    //sort A[p] to A[r];
    if(p < r){
        mid = (p + r)/ 2;
        MergeSort(A, p, mid);
        MergeSort(A, mid, r);
        Merge(A, p, mid, r);
    }
```

For merge sort function, the run time:

$$T(n) = 2T(n/2) + θ(n)$$

So the run time is θ(nlgn). Master Theorem state 2.

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/al/merge.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/al/merge.png" align="center"></a>
    <figcaption>Merge Sort.</figcaption>
</figure>

Disadvantages:
* additional space
* not in-place sorting

Advantage:
* worst case is same as the best
* θ(nlgn) is guaranteed


#### Integer Multiplication

Input: X, Y - two n-digit integers

Output: X * Y

Origin Solution:

```
Multiply(X[1..n], Y[1..n]):
    Z[1..2n] = 0
    for i=1 to n:
        carry = 0;
        for j = n to 1:
            m = Z[i+j] + carry + X[j] * Y[i];
            Z[i+j] = m mod 10;
            carry = m/10;
        Z[i] = carry;
    return Z;
```

As the codes show, the run time is θ(n2). We should use the divide and conquer method on the question:

$$X = 10^{n/2}X_1 + X_2$$

$$Y = 10^{n/2}Y_1 + Y_2$$

So

$$X*Y = (10^{n/2}X_1 + X_2)*(10^{n/2}Y_1 + Y_2)$$

$$=10^nX_1Y_1 + 10^{n/2}(X_1Y_2+X_2Y_1) + X_2Y_2$$

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/al/nmb.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/al/nmb.png" align="center"></a>
    <figcaption>Number Multiplication Solution.</figcaption>
</figure>




### 2.2 Multiplying Square Matrices

Input: Three nxn matrices, A=aij, B=bij, C=cij.

Result: The matrix product A B is added into C.

Solution: Strassen's Algorithm

Perform only 7 recursive multiplications of n/2 x n/2 matrices， rather than 8.

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/al/ss2.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/al/ss2.png" align="center"></a>
    <figcaption>Strassen's Algorithm.</figcaption>
</figure>

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/al/ss3.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/al/ss3.png" align="center"></a>
    <figcaption>Strassen's Algorithm.</figcaption>
</figure>


The recurrence will be:

$$T(n)=7T(n/2) +θ(n^2)$$

By the master theorem, solution is

$$T(n)=θ(n^{lg7})$$


### 2.3 Maximum Subarray Problem

Input: A[1 .. n] array of n integers.

Output: Maximum sum of a contigous subarray, there exists 1 <= i < j <= n 

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/al/msubarray.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/al/msubarray.png" align="center"></a>
    <figcaption>Maximum subarray problem.</figcaption>
</figure>

Solution: We apply the divide and conquer thinking to the question. 
Consider:
* the lefe side
* the right side
* the cross part

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/al/mp.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/al/mp.png" align="center"></a>
    <figcaption>Maximum subarray problem.</figcaption>
</figure>

```
MaxCrossingSubarray(A, low, mid, high):
    L = -∞; R = -∞;
    S = 0;
    for i = mid to low:
        S = S + A[i];
        L = max(L, S);
    S = 0;
    for i= mid+1 to high:
        S = S + A[i];
        L = max(R, S);
    return L+ R;


MaxSubarray(A, low, high):
    if high = low + 1:
        return A[low] + A[high];
    if high <= low:
        return -∞;
    mid = (low + high)/2;
    left = MaxSubarray(A, low, mid);
    right = MaxSubarray(A, mid + 1,high);
    cross = MaxCrossingSubarray(A, low, mid, high);
    return max(left, right, cross);
```

So the running time is:

$$T(n) = 2T(n/2) + O(n) = O(nlgn)$$


### 2.4 Quick Sort

Quicksort is based on the three-step process of divide and conquer.

(1) Divide: Partition A[p .. r] in two subarray A[p .. q-1] and A[q+1 .. r].Such that each element in the first subarray A[p .. q-1] is <= A[q ] and A[q] is < each element in the second subarray A[q+1 .. r].


(2) Conquer: Sort the two subarray by recursive call Quick Sort.

(3) Combine: No work to do. Sort in place.



<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/al/quick.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/al/quick.png" align="center"></a>
    <figcaption>Quick Sort.</figcaption>
</figure>

Worst Case: one of two subproblems is empty and the other is of size n-1. O(n2)

Average Case: O(nlgn)


#### Improvement: Randomized Quicksort

a) Select the pivot randomly.

b) Permutate the elements in random manner.

(1) The expected running time of randomized quicksort on a sequence of size n is O(nlgn)

(2) Any comparison-based sorting algorithm requires Ω(nlgn) comparisons to sort n elements in worst case.


## 3. Order Statistics

### 3.1 Conception

The i th order statistic of a set of n elements is the i th smallest element.
* the minimum is the first order statistic. i=1.
* the maximum is the n th order statistic. i=n.
* a median is the halfway point of a set.
    * lower median is n/2 th element.
    * upper median is n/2 + 1 th element.

Question: Give an array A of n elements, find i th smallest element in it.

Input: A[1 .. n] array of n integers.

Output: x ∈ A larger than exactly i-1 elements in it: **Find i th order statistics of A**.

Origin solution: sort and find ith order statistics. O(nlgn).

### 3.2 Simutaneous Minimum and Maximum

Some applications need both the minimum and maximum of a set of elements. 

Origin Solution: A simple algorithm to find the minimum and maximum is to find each on independently. There will be n-1 comparisons for the minimum and n-1 comaprisons for the maximum. θ(n).

In fact, at most 3[n / 2] comparisons suffice to find both the minimum and maximum.
* Maintain the minimum and maximum
* Dont compare each elements to the minimum and maximum separately.
* Process elements in pairs.
* Comapre the elements of a paire to each other, and then compare to the maximum and minimum.

```
if(A[i] > A[i+1])
    if(max < A[i])
        max = A[i];
    if(min > A[i])
        min = A[i];
else
    if(max < A[i+1])
        max = A[i+1];
    if(min > A[i])
        min = A[i];
```

### 3.3 Selection in Expected Linear Time

Randomized-Select differs from Qucksort in that it recurses on only one side of the partition.

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/al/rs.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/al/rs.png" align="center"></a>
    <figcaption>The selection algorithm.</figcaption>
</figure>

After the call to Randomized-Partition, the array is partitioned into two subarrays.
A[p .. q-1] and A[q+1 .. r] along with the pivot element at A[q ].

* The elements of the subarray A[p .. q-1] are all <= A[q ].
* The elements of the subarray A[q+1 .. r] are all > A[q ].
* The pivot element is the k th element of the array A[p .. r], where k = q - p + 1.
* If the pivot element is the i th smallest element. return A[q ].
* Recursively select the appropriate element in one of the two partitions.

The action of Randomize-Select as successive partitioning narrow A[p .. r].

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/al/rsp.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/al/rsp.png" align="center"></a>
    <figcaption>The randomize select.</figcaption>
</figure>

Runtime: 

* The worst-case of Randomize-Select is θ(n^2).

* The expected run-time T(n) of Randomize-Select is linear. T(n)=θ(n).


### 3.4 Optimize Selection in Worst-case Linear Time

Can we make a Select algorithm to be linear in the worst case?

Select Function:
* Divide n elements into n/5 groups of 5 elements, and one group that can have less than 5 elements.
* Find the median of each group.
* Use **Select** recursively on the medians to find the median x of the n/5 median values.
* Partition the entire input A[p .. r] around x, with the index for x being q, as before.
* Continue as in the **Select** algorithm recursively.

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/al/median.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/al/median.png" align="center"></a>
    <figcaption>The median algorithm.</figcaption>
</figure>

* There are at least 3(g/2) >= 3n/10 elements greater than x.
* There are at least 3n/10 elements less than x.


### 3.5 Finding a closest pair of points

Given a set of n points Q in the plane, find a pair of points that is closest. n points form n(n-1)/2 different pairs of points.

An algorithm calculating distances between all pairs needs time O(n^2). 

**Base Idea**: If there are at most 3 points, solve the problem by **calculating all pairs distances**.


<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/al/dvd.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/al/dvd.png" align="center"></a>
    <figcaption>The divide.</figcaption>
</figure>



**Divide**: Given Q, find **a vertical line l** that bisects Q into 2 subsets Q1, Q2 of the same size.
* Select median in O(n) time of the x-value.


**Conquer**: 
* Find the closest pair in Q1.
* Find the closest pair in Q2

**Combine**:

Let θ be the closest distance in Q1 and Q2. Insepect the band around l of size 2θ and see 
if any pair there is at distance < θ. If yes, tha pair is the solution.

So, the recurrence is

$$T(n)=2T(n/2)+O(n)$$

So, the run-time is T(n)=O(nlgn).




## 3. Dynamic Programming

### 3.1 Conception

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


### 3.2 Questions

#### 3.2.1 Assembly-line Scheduling

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

#### 3.2.2 Rod Cutting

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


#### 3.2.3 Matrix Multiplication


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


#### 3.2.4 Longest Common Subsequence

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


#### 3.2.5 Longest Increasing Subsequence

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

#### 3.2.6 Optimal Binary Search Trees

We have nodes and the probabilities with which the nodes are searched. 

N nodes k1, k2, ..., kn, such that k1 < k2 < ... < kn.

Probabilities p1, p2, ..., pn of searching for these nodes.

probabilities q0, q1, ..., qn of searching for elements between nodes.

We will place these probabilities at dummy nodes d0, d1, d2, ..., dn.

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/al/opt.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/al/opt.png" align="center"></a>
    <figcaption>The optimal binary tree.</figcaption>
</figure>

E[T ]: the cost of a tree T expresses **the expected number of comparisons corresponding to the given probabilities of searches**.

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/al/opte.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/al/opte.png" align="center"></a>
    <figcaption>The cost of the optimal binary tree.</figcaption>
</figure>

Question: Find a binary search tree T gives the lowest expected cost.

Theorem: If an optimal tree T has **a subtree T'** with keys ki, ki+1, .., kj and dummy keys di-1, di, .., dj. Then **T' must be also optimal for those keys**.

Let e[1 ... n+1, 0 ... n] with e[i, j], for j>=i-1, be **the expected cost** of an optimal search tree for keys ki, ki+1, ..., kj. We wish to compute e[1, n].

If kr is the root of the subtree with ki, ki+1, ..., kj, so:

$$e[i, j]=e[i,r-1]+e[r+1, j] + w(i, j)$$

$$w(i,j)=\sum_{k=1}^jp_k + \sum_{k=i-1}^jq_k$$

Let w[1 .. n+1, 0 .. n] with

$$w[i,j]=w[i,j-1]+p_j+q_j$$

The lowest cost tree is obtained by minizing over all possible choices of r. r[i, j] for roots of optimal tree with ki, ki+1, ..., kj.

So the solution is:

$$e[i,j]=min_{i<=r<=j}(e[i,r-1]+e[r+1,j])+w[i, j]$$

It gives O(n3) algorithm.



#### 3.2.7  Knapasack Problems

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








## 4. Greedy Algorithm

### 4.1 Conceptions

It is called **greedy** because when a choice is to be made, it chooses what **looks best at that moment**.

It gives simple approximation algorithms in many cases where **exact solutions** are
too difficult to get. When we use the greedy paradigm, we must verify that it is appropriate for the given problem.

The greedy approach always will get three types results:
* give optimal solutions
* good approximation
* is not suitable

#### 4.1.1 Natural Greedy Algorithm

Indeed, since items are divisible it makes sense to start taking as much of an item
that gives the biggest bang for the buck as possible
* Sort items in non-increasing order of vi/wi (value per unit weight)
* Process items in this order and for each item:
    * If the remaining capacity of the knapsack can accommodate the **entire item**, take the entire item
    * otherwise take **the largest portion** of the item you can filling the knapsack to capacity

#### 4.1.2 Huffman Codes

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


#### 4.1.3 Proof of Huffman Codes

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


### 4.2 Questions

#### 4.2.1 Activity Selection Problem

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


#### 4.2.2 Scheduling Problems

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



## 5. Amortized Analysis

### 5.1 Conception


Amortized analysis can be used to show that the average cost of an operation is small even though a single operation within the sequence might be expensive. Cost of the “expensive” operation is 
**“amortized” by the cheap ones**.
* If an “expensive” operation is not happening very often and is in a sequence of “cheap” operations, the average cost can be low.
* It is different from average-case analysis, the probability is not involved.



### 5.2 Aggregate Analysis


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

### 5.3 Accounting Method 

We assign to operations cost that differ from the actual cost.
* Some are charged more than the actual cost,
* Some are charged less than the actual cost.

The amount charged is called **amortized cost**.


What we need is that everything balances out at the end, i.e. **the total of amortized charges must be at least as high as the actual cost**. The extra is called credit.
* by over-charging for some operations that are easy to count we don’t need to charge anything to operations that are less easy to count.

#### 5.3.1 Stack Operations with Multipop

Now consider **any sequence of n PUSH, POP and MULTIPOPs** on initially **empty stack**.

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/al/mm.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/al/mm.png" align="center"></a>
    <figcaption>The stack operation.</figcaption>
</figure>

Each PUSH is over-charging the cost by 1. Thus, we “pre-pay” for the eventual POP or MULTIPOP when push is done.

If at most n PUSH operations are done, the total amortized cost is ≤ 2n and the total actual cost is O(n).

#### 5.3.2 Binary Counter

Let the **amortized cost of setting a bit from 0 to 1 be 2**.

Thus we **charge 1 to set it and we completely pre-payed the eventual change from 1 to 0**.

Thus the amortized cost of a single
INCREMENT is 2 = O(1) since only
one 0 bit is flipped to a 1 bit.

A sequence of n INCREMENTs is O(n)

### 5.4 Potential Method 

We represent the prepaid work as “potential energy” that can be released later for **the cost of future operations**.

The potential is associated with the data structure as a whole rather than with specific objects.

We have a data structure D. D_i ... the data structure D after applying **ith operation** to it.

Initially, we have D_0. A **potential function Φ** maps the data structure **Di** to a real number **Φ(Di)**, the associated potential.

$$\hat c_i = c_i + \phi(D_i) - \phi(D_{i-1})$$

The total amortized cost of n operations is

$$\sum_{i=1}^{n} \hat c_i = \sum_{i=1}^{n} c_i + \phi(D_n) - \phi(D_{0})$$

If Φ(Dn) ≥ Φ(D0) then the total amortized cost is at least as great as the total actual cost.

$$\sum_{i=1}^n c_i$$

### 5.5 dynamic tables

Table expansion: insert an item x:
* allocate a double size table
* copy data to the new table
* release original table
* insert x into new table.


Table Contraction: delete an item x:
* delete an item x.
* allocate a smaller table
* release original table
* copy data to the new table

Load Factor: 

$$α(T)=num[T]/size[T] ，0≤α≤1$$

#### 5.5.1 Table Expansion

(1) Aggregate Analysis

Cost c_i of i'th insertion is:
* i if i is a power of 2.
* 1 otherwise

$$\sum_{i=1}nc_i \leq n + \sum_{j=0}^{logn}2^j=n+\frac{2^{logn+1}-1}{2-1} \leq n+2n = 3n$$


(2) Accounting Method

Charge amortized cost of $3 per insertion of x.
• $1 pays for x’s insertion.
• $1 pays for x to be moved in the future.
• $1 pays for some other item to be moved.

(3) Potential Method

Over size/2 insertions, potential needs to go from 0 to size.

So, the potential function:

$$Φ(T) = 2(T.num − T.size/2)$$


* The potential equals 0 just after expansion, when T.num = T.size/2.
* The potential equals T.size when the table fills, when T.num = T.size.


Φi = potential after the ith operation,

Φi − Φi−1 = change in potential due the ith operation,

cˆi = ci + (Φi − Φi−1) 


When the i th insertion does not trigger expansion:

$$\hat c_i = c_i + (Φ_i − Φ_{i−1})=1+2=3$$

When the i th insertion does trigger expansion.

* numi = number of items in the table after the ith operation 
* sizei = size of the table after the ith operation

before insertion:

$$Φ_{i-1}=2(num_{i-1}-size_{i-1}/2)=size_{i-1}= i-1$$

After expansion:
$$Φ_i=2(num_i-size_{i}/2) = 2 * 1 = 2$$

$$(Φ_i − Φ_{i−1}) = 2 − (i − 1) = 3 − i$$

$$\hat c_i = c_i + (Φ_i − Φ_{i−1}) = i+ 3-i=3$$

#### 5.5.2 Table Contraction

Shrink the table size to half size when α is 1/4.

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/al/pm.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/al/pm.png" align="center"></a>
    <figcaption>The potential function.</figcaption>
</figure>













 