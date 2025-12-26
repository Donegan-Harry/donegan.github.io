---
layout: percolation
title: Monte-Carlo Simulation
---

Now we have a computer simulation of percolation with a cluster-idenifying sub-routine we are free to investigate quantities. In general we will be interested in the the $p$-dependence of expectation values $\langle X \rangle$. The expectation value of a quantity $X$ at $p$ can be calculated using the sample mean of a lot of repeated numerical 'experiments'. Thus conducting the percolation multiple times we have

$$\langle X \rangle \approx \overline{X} = \frac{1}{N}\sum_{i} X_{i}, $$

where the result can only be trusted within any statistical errors asscoiated with the variance

$$ \Delta X = \frac{1}{\sqrt{N}} \sqrt{\overline{X^2}-\overline{X}^2}.$$

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

