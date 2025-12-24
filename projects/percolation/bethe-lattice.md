---
layout: percolation
title: Bethe Lattice
---

Despite the simplicity of the percolation model, it is notoriously difficult to solve exactly on even the simplest lattices (with the trivial exception of one dimension, which we will discuss later). The difficulty is that on a general lattice the presence of loops introduces correlations between sites, destroying statistical independence and placing an exact solution beyond our current means. It should therefore come as no surprise that the lattice on which an exact solution can be derived precludes any loops in its geometry. The Bethe lattice is an infinite connected cycle-free lattice where each vertx has $z$ neighbours ($z$ is known as the coordination number). It can be constructed by starting from a central vertex with $z$ bonds; at the end of each bond, an additional $z-1$ bonds emerge, with the remaining bond connecting back toward the origin. This branching process is continued onwards.


The first thing we will be interested in solving for is the critical point $p_{c}$ where the infinite cluster emerges. To solve this, we can consider the origin which we call our root. We are then free to choose a neighbour and we will define a branch as the part of the lattice rooted at the neighbour. We let $u$ be the probability that a branch fails, i.e. if it cannot be connected to infinity. Let us now consider the neighbour we we first choose, clearly if it is unoccupied with probability $1-p$ the branch fails at the first step. Now this site has $z-1$ child branches which emenate from on but since all these structures are statistically independent (the Bethe lattice is after all self-similar) the probability that one of them fail if the site is occupied is $u$. We therefore arrive at the recursion relation

$$u = (1-p) + pu^{z-1}.$$

An infinite cluster exists provided that $u<1$. Our criteria for finding the threshold is then a matter of stability analysis of our non-linear recusion relation. In particular, if we let $f(u)=(1-p)+pu^{z-1}$ then we have $u=f(u)$ and we know at a fixed point $u^{*}$ our solution is stable provided that $|f'(u^*)|<1$. Thus we consider $|f'(1)|=p(z-1)$ from which we find the critical point 

$$p_{c}=\frac{1}{z-1}.$$
