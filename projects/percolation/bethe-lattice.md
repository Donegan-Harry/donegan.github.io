---
layout: percolation
title: Bethe Lattice
---

Despite the simplicity of the percolation model, it is notoriously difficult to solve exactly on even the simplest lattices (with the trivial exception of one dimension, which we will discuss later). The difficulty is that on a general lattice the presence of loops introduces correlations between sites, destroying statistical independence and placing an exact solution beyond our current means. It should therefore come as no surprise that the lattice on which an exact solution can be derived precludes any loops in its geometry. The Bethe lattice is an infinite connected cycle-free lattice where each vertx has $z$ neighbours ($z$ is known as the coordination number). It can be constructed by starting from a central vertex with $z$ bonds; at the end of each bond, an additional $z-1$ bonds emerge, with the remaining bond connecting back toward the origin. This branching process is continued onwards.

<p style="text-align: center; margin: 1.5rem 0;">
  <img src="/assets/images/percolation/BetheLattice.svg" alt="BetheLattice" style="max-width: 400px;">
</p>

<p style="text-align: center; color: #666; font-size: 0.95em; font-style: italic; margin-top: 0.5rem;">
  Figure 2: Bethe lattice with coordination number $z=3$ and depth $d=3$.
</p>


The first thing we will be interested in solving for is the critical point $p_{c}$ where the infinite cluster emerges. To solve this, we can consider the origin which we call our root. We are then free to choose a neighbour and we will define a branch as the part of the lattice rooted at the neighbour. We let $u$ be the probability that a branch fails, i.e. if it cannot be connected to infinity. Let us now consider the neighbour we we first choose, clearly if it is unoccupied with probability $1-p$ the branch fails at the first step. Now this site has $z-1$ child branches which emenate from on but since all these structures are statistically independent (the Bethe lattice is after all self-similar) the probability that one of them fail if the site is occupied is $u$. We therefore arrive at the recursion relation

$$u = (1-p) + pu^{z-1}.$$

An infinite cluster exists provided that $u<1$. Our criteria for finding the threshold is then a matter of stability analysis of our non-linear recusion relation. In particular, if we let $f(u)=(1-p)+pu^{z-1}$ then we have $u=f(u)$ and we know at a fixed point $u_{0}$ our solution is stable provided that $\partial_{u}f(u_{0})<1$. Thus we consider $\partial_{u}f(1)=p(z-1)$ from which we find the critical point 

$$p_{c}=\frac{1}{z-1}.$$

We mentioned before that one-dimension is trivial and the Bethe lattice contains it, namely the $z=2$ is a one-dimensional chain whose percolation threshold is at unity probability.

With the recursion equation provided, in principle we are free to calculate the quantities of interest. The first of these is the order parameter $P_{\infty}$ which is known as the strength of the infinite cluster. A non-zero value indicates the presence of the infinite cluster and thus siginifies it is in the percolating phase. To define it, we note that $(p-P_{\infty})$ is the probability the origin is occupied but not connected to any of its $z$ branches, this quantity is also equal to $pu^{z}$ and so we therefore have

$$P_{\infty}=p(1-u^{z}),$$

which is evidently vanishing when $p<p_{c}$ and $u=1$ is the stable solution. For general $z$ we will not be able to find a closed-form for $P_{\infty}$ and we will must rely on numerical solutions but if $z\leq5$ then the recurssion relation defines a polynomial equation which we can find closed forms for. We of course, as an example, elect to solve the simplest one for $z=3$ where $u=(1-p)+pu^2$ for which we obtain $u=1$ and $u=(1-p)/p$. The first holds for below the critical point $p<p_{c}=1/2$ with $P_{\infty}=0$ while above the critical point we have

$$\frac{P_{\infty}}{p}=1-\frac{(1-p)^3}{p^3}.$$

We may also be interested in the mean cluster size before the transition (it is obviously diverging for $p>p_{c}$). Let $T$ be the mean cluster size for one branch. If the branch is unnoccupied then $T=0$ while if it is occupied it contributes its own mass and the mass $T$ for its $z-1$ subbranches

$$T = p(1+(z-1)T),$$

with the solution $T=p/(1-(z-1)p)$ for $p<p_{c}$. If the origin is occupied then it will contribute its own mass and that of its $z$ branches leaving

$$ S = 1 + 3T = \frac{(4-z)p+1}{(1-z)p+1}. $$
