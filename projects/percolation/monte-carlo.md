---
layout: percolation
title: Monte-Carlo Simulation
---

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

