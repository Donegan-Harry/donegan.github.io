---
layout: percolation
title: Monte-Carlo Simulation
---

Now we have a computer simulation of percolation with a cluster-identifying sub-routine we are free to investigate quantities of interest to our hearts desire. In general we will be interested in the the $p$-dependence of expectation values $\langle X \rangle$. The expectation value of a quantity $X$ at $p$ can be calculated using the sample mean of a lot of repeated numerical 'experiments'. Thus conducting the percolation multiple times we have

$$\langle X \rangle \approx \overline{X} = \frac{1}{M}\sum_{i} X_{i}, $$

where the result can only be trusted within any statistical errors asscoiated with the variance

$$ \Delta X = \frac{1}{\sqrt{M}} \sqrt{\overline{X^2}-\overline{X}^2}.$$

For example, we can look at the first percolation quantity introduced: the infinite cluster strength $P_{\infty$}. We define $P_{\infty}$ in this instance as the expectation value of $n_{\infty}/N$ where $n_{\infty}$ is the number of sites belonging to the 'infinite' cluster and $N$ is the total number of sites ($L^2$ for the square lattice). Such a quantity requires us to find the 'infinite' cluster which is actually quite simple. We need only find if the opposite edge rows or columns share any common labels and if they do we have find our infinite cluster, the label and the corresponding size. The following functions implement the necessary calculations:

```julia
function isPercolating(HK_M)
    # Retrieve edges of system
    top_edge = HK_M[1, :]
    bottom_edge = Set(HK_M[end, :])
    left_edge = Set(HK_M[:, 1])
    right_edge = HK_M[:, end]

    # Check if cluster spans system top to bottom
    vertical = findfirst(k -> k in bottom_edge, top_edge)
    if vertical !== nothing 
        return true, top_edge[vertical] 
    end 

    # Check if clcuster spans system left to right
    horizontal = findfirst(k -> k in left_edge, right_edge)
    if horizontal !== nothing 
        return true, right_edge[horizontal] 
    else 
        return false, nothing
    end
end
```

