---
layout: percolation
title: Hoshen–Kopelman Algorithm
---

Getting percolation to work on a computer is actually rather simple. We need only initialise a zero array (in fact we only need a bit matrix) whose elements correspond to sites on the lattice and then use a random number generator to flip some of these elements to one. And just like that, we have a digital implementation of percolation. 

However, we need to do some number crunching on these results, and since we are already using a computer, we might as well leverage its vastly superior computational abilities. The most important component of this digital framework is a cluster identification algorithm. Once we can label the sites into clusters, calculating all the quantities of interest becomes a rather trivial exercise. In principle, we can do this by brute force, but this problem falls squarely into the domain of computer science, and researchers have developed highly efficient algorithms. The method we present here is a particularly simple and elegant one due to Hoshen and Kopelman, based on the union–find algorithm.

The union–find algorithm is concerned with disjoint sets and has two operations (it's in the name!):
- **Find**: Do two elements belong to the same set?
- **Union**: Merge two sets together

To make the algorithm clear, let's be more precise about what we're discussing. We have an array $A$ whose elements are integers between 0 and $N$ (the number of elements in the array), and the elements act as pointers. For example, if $A[3]=4$, this means $A[3]$ points to $A[4]$, indicating they are connected. 

Now comes the find operation. We define the **root** of index $i$ as the value obtained by following the chain $A[A[A...[A[i]]...]]$ until we reach an element where $A[i]=i$. How does this help us? Let's consider an example:

| i    | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 |
|------|---|---|---|---|---|---|---|---|---|
| A[i] | 1 | 9 | 4 | 9 | 6 | 6 | 7 | 8 | 9 |

This produces the following chains: 3→4→9 and 2→9, giving us the set {2, 3, 4, 9} with root 9. Likewise, 5→6→6 gives us the set {5, 6} with root 6. Elements 1, 7, and 8 are each their own root, forming singleton sets.

The union operation is straightforward. To combine two sets with roots $p$ and $q$, we simply set $A[p]=q$ (or vice versa). For example, to merge {2, 3, 4, 9} and {5, 6} into {2, 3, 4, 5, 6, 9}, we set $A[6]=9$.

Before implementing this code, we can make one significant improvement to the find algorithm through **path compression**, which flattens the resulting chains. For a large array, very long chains can emerge that are expensive to traverse. The process is simple: as we traverse a chain to find its root, we update each element along the path to point directly to the root. This makes future lookups much faster.

Altogether, the code reads:
```julia
function findAdj(labels, x)
    y = x
    # Find root
    while labels[y] != y 
        y = labels[y]
    end 
    # Path compression
    while labels[x] != x 
        z = labels[x]
        labels[x] = y 
        x = z 
    end 
    return y
end

function unionAdj(labels, x, y)
    labels[findAdj(labels, x)] = findAdj(labels, y)
end
```

