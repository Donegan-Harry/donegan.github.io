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

Now we have our functions, the Hoshen-Kopelman algorithm can be presented. Here, we have a 2D percolation array (we take our system to be 2D) and initialise a zero array of the same size. We start at the top left corner; if it is occupied, we assign it label 1, if not we leave it at zero. Next we look at the cell to the right. If it is occupied, we check the cell to its left (the previous cell). If the left neighbour is labelled, we assign the current cell the root of that label; if not, we create a new label. We continue scanning left to right, row by row (a raster scan).

For each occupied site in the interior of the grid, we check both the left and above neighbours. Four cases arise:
- **Both neighbours empty**: Create a new label
- **Only left occupied**: Inherit the root of the left label
- **Only above occupied**: Inherit the root of the above label  
- **Both neighbours occupied**: This is the key case—we perform a union operation to merge the two clusters (which may have different labels but are now discovered to be connected), then assign the current site the root of the merged cluster

After completing the scan, we make a final pass through the label array, updating each label to point directly to its root using the find operation with path compression. This ensures all sites in the same cluster share the same label, giving us our final cluster identification. In Julia, this can be implemented as

```julia 
function HK(M)
    largest_label = 0
    label = zeros(Int, n_rows, n_columns)
    labels = collect(0:n_rows*n_columns)
    
    for i in 1:n_rows
        for j in 1:n_columns
            if M[i, j] == 1
                # Conditional arguments for boundary cases
                left = (j > 1) ? label[i, j-1] : 0
                above = (i > 1) ? label[i-1, j] : 0
                
                if left == 0 && above == 0
                    largest_label += 1
                    label[i, j] = largest_label
                    labels[largest_label] = largest_label
                elseif left != 0 && above == 0
                    label[i, j] = findAdj(labels, left)
                elseif left == 0 && above != 0
                    label[i, j] = findAdj(labels, above)
                else
                    unionAdj(labels, left, above)
                    label[i, j] = findAdj(labels, left)
                end
            end
        end
    end
    
    # Final path compression
    for i in 1:n_rows
        for j in 1:n_columns
            if label[i, j] != 0
                label[i, j] = findAdj(labels, label[i, j])
            end
        end
    end
    
    return label
end
```

