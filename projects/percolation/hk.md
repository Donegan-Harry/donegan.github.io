---
layout: percolation
title: Hoshen–Kopelman Algorithm
---

Getting percolation to work on a computer is actually rather simple. We need only initialise a zero array (it fact we only need a bit matrix) whose elements correspond to sites on the lattice and then use a random number generator to flip some of these elements to one. And just like that we have a digital implementation of percolation. We, however, need to actually do some number crunching on these results and since we are already using a computer we might as well use its vastly superior number crunching abilities. The most important aspect of this digital framework we need is a cluster identification algorithm. Once we can label the sites into clusters calculating all the quantities of interest is rather trivial in exercise. In principle, we can do this by a rather brute-force method but it should come at no surprise that this sort of problem falls into the heart of computer science and people in years long past have conjured up all sorts of efficient algorithms. The method we present here is a particularly simple and elegant one due to Hoshen and Kopelman, which is based on the union–find algorithm.
