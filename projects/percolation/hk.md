---
layout: percolation
title: Hoshen–Kopelman Algorithm
---

Getting percolation to work on a computer is actually rather simple. We need only initialise a zero array (it fact we only need a bit matrix) whose elements correspond to sites on the lattice and then use a random number generator to flip some of these elements to one. And just like that we have a digital implementation of percolation. We, however, need to actually do some number crunching on these results and since we are already using a computer we might as well use its vastly superior number crunching abilities. The most important aspect of this digital framework we need is a cluster identification algorithm. Once we can label the sites into clusters calculating all the quantities of interest is rather trivial in exercise. In principle, we can do this by a rather brute-force method but it should come at no surprise that this sort of problem falls into the heart of computer science and people in years long past have conjured up all sorts of efficient algorithms. The method we present here is a particularly simple and elegant one due to Hoshen and Kopelman, which is based on the union–find algorithm.

The find-union algroithm is all to do with disjointed sets and has two operations (its in the name!):
- Find: Do two elements belong to the same set?
- Union: Merge two sets together

To make the algorithm clear, we'll be a bit more precise in what we are talking about. We have an array $A$ whose elements are integers between 0 and up N (the number of elements in the array) and the elements will act as pointers so for example if $A[3]=4$ this means $A[3]$ and $A[4]$ are connected. For our clusters this constraint is clearly going to hold. Now comes the find operation. Let us now define the root of the index $i$ as $A[A[A...[A[i]]...]]$ until $A[i]=i$. How does such an object help us? Well for that, let us consider an example

| i    | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 |
|------|---|---|---|---|---|---|---|---|---|
| A[i] | 1 | 9 | 4 | 9 | 6 | 6 | 7 | 8 | 9 |

which produces the following chains 3->4->9 and 2->9 thus we have the set {2, 3, 4, 9}. Likewise we find that 5->6 and we have the set {5, 6} and the others are all sets of themselves. And the union operation? Well this is rather easy. Say we have two sets we want to combine with roots $p$ and $q$ we need only set $p=q$ or vice-versa. So for example if we wanted the union of {2, 3, 4, 9} and {5, 6} to get {2, 3, 4, 5, 6, 9} we need only set 6->9.

