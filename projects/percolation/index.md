---
layout: percolation
title: Introduction to Percolation Theory
---

Percolation theory is concerned with the study of clusters in random networks. There are two types of percolation models, fixed points on the network with random linkages or random points but with predfined linkages. The former is known as bond percolation, which is historically the first form studied, and the latter is site percolation. In these notes we will largely concern ourselves with sites.

To make these points clearer, consider a simple square lattice where a site is said to be randomly occupied with probability $p$. For a range of $p$ we have displayed the percolation model in Figure 1. For small probabilities, small clusters begin to appear and as we tune the probability higher the clusters become larger and begin to conglomorate until eventually a cluster spans the system (edge-to-edge). When such a cluster forms, the system is said to be percolating. The transition from non-percolating to percolating is a phase transition. It is perhaps the simplest phase transition and with it we can easily access the knowledge of critical phenomena and renomalisation group.

<p style="text-align: center; margin: 1.5rem 0;">
  <img src="/assets/images/percolation/PercolationIntroClusters.svg" alt="ClustersIntro" style="max-width: 600px;">
</p>

*Figure 1: Formation of percolation clusters at different occupation probabilities $p$.*

Beyond the pedalogical tool of percolation, it is actually a very versatile tool to understanding a variety of phenomena across many fields and has been used to model: 
- Forest fires
- Disease spread
- Gelation in chemistry 
- Porous media flow
- Conductivity in disordered media 
